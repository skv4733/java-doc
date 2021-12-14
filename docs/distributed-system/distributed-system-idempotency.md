## Interview Questions

How to design the idempotence of the distributed service interface (for example, no repeated deductions)?

## Interviewer psychoanalysis

From this question, the interviewer has entered the interview of **actual production problems**.

How to ensure idempotence for a certain interface in a distributed system? This matter is actually a technical issue of the production environment that you must consider when doing distributed systems. What do you mean?

You see, if you have a service that provides some interfaces for external calls, this service is deployed on 5 machines, and the next interface is the payment interface. Then when other users are operating on the front-end, they don’t know why. In short, an order **accidentally initiated two payment requests**, and these two requests are scattered on different machines deployed by this service. , As a result, an order deduction is deducted twice.

Or the order system called the payment system to make the payment, and the result was accidental because the **network timed out**, and then the order system went through the retry mechanism we saw earlier, and clicked to give you a try again. Okay, the payment system Received a payment request twice, and because the load balancing algorithm fell on different machines, it was embarrassing. . .

So you must know this, otherwise the distributed system you build may be easy to bury the hole.

## Analysis of Interview Questions

This is not a technical problem. There is no universal method for this. This should be combined with business to ensure idempotence.

The so-called **idempotence** means that an interface initiates the same request multiple times. Your interface must ensure that the result is accurate. For example, no more deductions, no more data can be inserted, and no more statistical values ​​can be added by 1 . This is idempotence.

In fact, there are three main points to ensure idempotence:

-For each request, there must be a unique identifier. For example, an order payment request must include the order id. An order id can be paid at most once, right.
-After each request is processed, there must be a record indicating that the request has been processed. A common solution is to record a status in mysql, such as recording a payment flow for this order before payment.
-Every time a request is received, it needs to be judged to judge whether it has been processed before. For example, if an order has been paid, there is already a payment flow. Then if the request is sent repeatedly, the payment flow will be inserted first. The orderId already exists, the unique key constraint is valid, and an error message cannot be inserted. Then you don't have to deduct any more.

In the actual operation process, you should combine your own business, such as using Redis, and use orderId as the unique key. Only by successfully inserting this payment stream can the actual payment deduction be executed.

The requirement is to pay for an order, a payment stream must be inserted, and order_id is a unique key `unique key`. Before you pay for an order, insert a payment stream first, and the order_id will already be entered. You can write a logo into Redis, `set order_id payed`, the next time you repeat the request, first check the value corresponding to the order_id of Redis, if it is `payed`, it means you have already paid, so don’t pay again. Up.