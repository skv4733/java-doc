## Interview Questions

If you are asked to write a message queue, how to design the architecture? Tell me about your thoughts.

## Interviewer psychoanalysis

In fact, when talking about this issue, the interviewer generally has to look at two things:

-Have you ever had a more in-depth understanding of the principles of a certain message queue, or grasped the architectural principles of a message queue from an overall understanding.
-Take a look at your design ability, give you a common system, the message queuing system, and see if you can grasp the overall architecture design from a global perspective, and give you some key points.

To be honest, when asking similar questions, most people are basically confused, because they have never thought about similar questions. **Most people just bury their heads and never think about some of the things behind**. Similar questions, for example, what would you do if you were asked to design a Spring framework? What would you do if you were asked to design a Dubbo framework? What would you do if you were asked to design a MyBatis framework?

## Analysis of Interview Questions

In fact, to answer this kind of question, to put it plainly, I don’t ask you to read the source code of the technology. At least you have to know the basic principles, core components, and basic structure of the technology, and then design a system with reference to some open source technologies. Just talk about the idea.

For example, for this message queuing system, let's consider it from the following perspectives:

-First of all, this mq must support scalability, that is, if you need to quickly expand the capacity, you can increase the throughput and capacity, then how to do it? To design a distributed system, refer to the design concept of Kafka, broker -> topic -> partition, each partition puts a machine and stores part of the data. If the resources are not enough now, it's simple, add partitions to the topic, then do data migration, add more machines, can you store more data and provide higher throughput?

-Secondly, you have to consider whether this mq data should land on disk, right? It must be necessary. Only by dropping the disk can the data be lost if other processes fail. How do you drop the disk when you drop it? Sequential writing, so there is no addressing overhead of random disk read and write, and the performance of disk sequential read and write is very high. This is the idea of ​​Kafka.

-Secondly, do you think about the availability of your mq? For this matter, please refer to Kafka's high-availability guarantee mechanism explained in the previous section on availability. Multiple copies -> leader & follower -> the broker hangs up and re-elects the leader to serve externally.

-Can you support data 0 loss? Yes, refer to the Kafka zero data loss solution we mentioned earlier.

mq is definitely very complicated. The interviewer asks you this question, which is actually an open question. He is to see if you have the overall thinking and ability to conceive and design from an architectural perspective. It is true that this problem can wipe out a large number of people, because most people don't think about these things at ordinary times.