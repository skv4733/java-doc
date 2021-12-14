## Message queue interview scene

**Interviewer**: Hello.

**Candidate**: Hello.

(The interviewer saw it on your resume, Yo, there is a bright spot, you have used `MQ` in the project, for example, you have used `ActiveMQ`)

**Interviewer**: Have you used message queues in the system? (The interviewer started the interview in an easy-going tone)

**Candidate**: Used (it feels okay at this time)

**Interviewer**: Then tell me how you use message queues in your project?

**Candidate**: Barabala, "Our system sends a message to the queue, and other systems consume it. For example, we have an order system. Every time the order system places a new order, A message will be sent to `ActiveMQ`, and there is an inventory system in the background that is responsible for obtaining the message and updating the inventory."

(Some students will enter a misunderstanding here, that is, you just know and answer how you use this message queue, what have you done with this message queue?)

**Interviewer**: Then why do you use message queues? Your order system does not send messages to `MQ`, but the order system directly calls an interface of the inventory system, click, and the call is successful, and the inventory is not updated.

**Candidate**: Um. . . (Stunned, why? I didn't think about it carefully, the boss just let it be used), bit the bullet and babbled a few gibberish.

(The interviewer was stunned when he heard you, and then listened to your gibberish, and started to feel a little bit of it in his heart, suspecting that you had never thought about this problem before)

**Interviewer**: So what are the advantages and disadvantages of using message queues?

(What the interviewer is thinking at this time is, why your `MQ` is used in the project, you haven't thought about it, then I'll be a bit simpler, I will ask you whether you have considered using the message queue before. What are the advantages and disadvantages?)

**Candidate**: This. . . (It's true that I didn't think about this issue very much... I'm talking nonsense)

(At this time, the interviewer feels even more that your buddies are not good, and they don't usually think about it)

**Interviewer**: What is the difference between `Kafka`, `ActiveMQ`, `RabbitMQ`, and `RocketMQ`?

(The interviewer asks you this question, that is to say, bypass the more fictitious topics and directly see if you understand various `MQ` middleware, whether you have done your homework, whether you have done research)

**Candidate**: We have used `ActiveMQ`, so we haven't used anything else. . . The difference is not very clear. . .

(At this time, the interviewer feels that your buddies are just using it for nonsense, and they don’t think much at all, so they don’t think it works.)

**Interviewer**: How do you ensure the high availability of the message queue?

**Candidate**: This. . . I usually simply call the API, and I don't know how the message queue is deployed. . .

**Interviewer**: How to ensure that the message is not re-consumed? How to ensure that consumption is idempotent?

**Candidate**: What? (Isn't `MQ` just write & consume, why are there so many problems)

**Interviewer**: How to ensure the reliable transmission of messages? What if the message is lost?

**Candidate**: We haven't lost much news. . .

**Interviewer**: How to ensure the order of the messages?

**Candidate**: Sequential? What's the meaning? Why should I guarantee the order of messages? Doesn't it have an order?

**Interviewer**: How to solve the problem of delay and expiration of the message queue? What should I do when the message queue is full? There are millions of news backlogged for several hours, talk about how to solve it?

**Candidate**: No, I haven't encountered any of these problems. I just use it easily and know some functions of `MQ`.

**Interviewer**: If you are asked to write a message queue, how do you design the architecture? Tell me about your thoughts.

**candidate**:. . . . . I still go. . . .

---

This is actually an interview style of the interviewer, which means that the interviewer's questions are not divergent, but slowly spread out from a small point. For example, the interviewer may talk to you about high-concurrency topics. In this topic, I will talk to you about caching, `MQ`, etc., **from the shallower to the deeper, step by step**.

In fact, the above is a very typical technical investigation process about message queues. A good interviewer must start from a certain point you have done, and then conduct in-depth investigations layer by layer, asking one by one, until the technical point is thoroughly questioned. Asked to the bottom.