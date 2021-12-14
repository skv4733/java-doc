## Interview Questions

How to design an RPC framework similar to Dubbo?

## Interviewer psychoanalysis

To be honest, this question is actually the same as asking you how to design an MQ yourself. Just two tests:

-Do you have a very in-depth understanding of the principle of a certain rpc framework.
-Can you think about how to design an rpc framework and test your system design ability.

## Analysis of Interview Questions

In fact, when you ask you this question, you can't at least recognize it, because it is knowledge literacy, then I can't give you an in-depth explanation of kafka source code analysis, dubbo source code analysis, and even if I talk about it, you must really digest, understand and absorb. , At least a month later.

So let me give you a suggestion. When encountering such problems, at least start with the principles of similar frameworks that you know, and talk about it by referring to the principles of dubbo. For example, don't dubbo have so many layers? And what each layer does, do you probably know? Then let's talk about it roughly according to this line of thinking. At least you can't be stunned, it's better than those who come up and can't say anything.

For example, let me give you the simplest answer:

-When you come up, you have to go to the registration center to register. Do you have to have a registration center to keep the information of each service, you can use zookeeper to do it, right?
-Then your consumers need to go to the registration center to get the corresponding service information, right, and each service may exist on multiple machines.
-Then it's time for you to make a request, how do you make it? Of course it is based on a dynamic proxy. You get a dynamic proxy for the interface. This dynamic proxy is a local proxy for the interface, and then the proxy will find the machine address corresponding to the service.
-Which machine to send the request to? Then there must be a load balancing algorithm, for example, the simplest one can be random polling.
-Then you find a machine and you can send a request to it. How to send the first question? You can say to use netty, nio way; second question, what format is sent? You can say that the hessian serialization protocol is used, or something else, right. Then the request passed.
-The server is the same, you need to generate a dynamic proxy for your own service, listen to a certain network port, and then proxy your local service code. When a request is received, the corresponding service code is called, right.

This is one of the most basic ideas of the rpc framework. Let’s not talk about how powerful you are in technical skills, even if you can give the simplest idea first?