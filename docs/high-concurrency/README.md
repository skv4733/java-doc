# High concurrent architecture

## [Message queue](/docs/high-concurrency/mq-interview.md)

-[Why use message queues? What are the advantages and disadvantages of message queues? What are the advantages and disadvantages of Kafka, ActiveMQ, RabbitMQ, RocketMQ? ](/docs/high-concurrency/why-mq.md)
-[How to ensure the high availability of the message queue? ](/docs/high-concurrency/how-to-ensure-high-availability-of-message-queues.md)
-[How to ensure that messages will not be re-consumed? (How to ensure the idempotence of message consumption)](/docs/high-concurrency/how-to-ensure-that-messages-are-not-repeatedly-consumed.md)
-[How to ensure the reliable transmission of messages? (How to deal with the problem of message loss)](/docs/high-concurrency/how-to-ensure-the-reliable-transmission-of-messages.md)
-[How to ensure the order of messages? ](/docs/high-concurrency/how-to-ensure-the-order-of-messages.md)
-[How to solve the problem of delay and expiration of message queue? What should I do when the message queue is full? There are millions of news backlogged for several hours, talk about how to solve it? ](/docs/high-concurrency/mq-time-delay-and-expired-failure.md)
-[If you are asked to write a message queue, how to design the architecture? Tell me about your thoughts. ](/docs/high-concurrency/mq-design.md)

## [Search Engine](/docs/high-concurrency/es-introduction.md)

-[Can you explain the principle of ES's distributed architecture (how does ES realize distributed architecture)? ](/docs/high-concurrency/es-architecture.md)
-[What is the working principle of ES writing data? What is the working principle of ES query data? What about Lucene at the bottom? Do you understand the inverted index? ](/docs/high-concurrency/es-write-query-search.md)
-[ES How to improve query efficiency when the amount of data is large (billions)? ](/docs/high-concurrency/es-optimizing-query-performance.md)
-[What is the deployment architecture of the ES production cluster? What is the approximate amount of data for each index? How many shards are there for each index? ](/docs/high-concurrency/es-production-cluster.md)

## Cache

-[How is the cache used in the project? What are the consequences if the cache is used incorrectly? ](/docs/high-concurrency/why-cache.md)
-[What is the difference between Redis and Memcached? What is the threading model of Redis? Why is single-threaded Redis much more efficient than multi-threaded Memcached? ](/docs/high-concurrency/redis-single-thread-model.md)
-[What data types does Redis have? In which scenarios is it appropriate to use? ](/docs/high-concurrency/redis-data-types.md)
-[What are the expiration strategies of Redis? Write the LRU code by hand? ](/docs/high-concurrency/redis-expiration-policies-and-lru.md)
-[How to ensure Redis high concurrency and high availability? Can you introduce the principle of Redis master-slave replication? Can you introduce the principle of Redis sentinel? ](/docs/high-concurrency/how-to-ensure-high-concurrency-and-high-availability-of-redis.md)
-[How many ways are there to persist Redis? What are the advantages and disadvantages of different persistence mechanisms? How is the specific bottom layer of the persistence mechanism implemented? ](/docs/high-concurrency/redis-persistence.md)
-[Can you tell me about the working principle of Redis cluster mode? In cluster mode, how are Redis keys addressed? What are the algorithms for distributed addressing? Do you understand the consistent hash algorithm? How to dynamically add and delete a node? ](/docs/high-concurrency/redis-cluster.md)
-[Understand what is the avalanche, penetration and breakdown of redis? What happens after Redis crashes? How should the system respond to this situation? How to deal with Redis penetration? ](/docs/high-concurrency/redis-caching-avalanche-and-caching-penetration.md)
-[How to ensure double-write consistency between the cache and the database? ](/docs/high-concurrency/redis-consistence.md)
-[What is the problem of concurrent competition in Redis? how to solve this problem? Do you know the CAS scheme of Redis transactions? ](/docs/high-concurrency/redis-cas.md)
-[How is Redis deployed in the production environment? ](/docs/high-concurrency/redis-production-environment.md)
-[Have you learned about the Redis rehash process? ](/docs/high-concurrency/redis-rehash.md)

## Sub-library and sub-table

-[Why sub-database and sub-table (how to design at the database level when designing a high-concurrency system)? Which sub-database and sub-table middleware have been used? What are the advantages and disadvantages of different database and table middleware? How do you specifically split the database vertically or horizontally? ](/docs/high-concurrency/database-shard.md)
-[Now there is a system that does not have sub-databases and sub-meters. In the future, it will need to sub-databases and sub-meters. How to design so that the system can dynamically switch from sub-databases and sub-meters to sub-databases and sub-meters? ](/docs/high-concurrency/database-shard-method.md)
-[How to design a sub-database and sub-table solution that can dynamically expand and shrink? ](/docs/high-concurrency/database-shard-dynamic-expand.md)
-[After sub-database sub-table, how to deal with id primary key? ](/docs/high-concurrency/database-shard-global-id-generate.md)

## Read and write separation

-[How to achieve MySQL's read-write separation? What is the principle of MySQL master-slave replication? How to solve the delay problem of MySQL master-slave synchronization? ](/docs/high-concurrency/mysql-read-write-separation.md)

## High Concurrency System

-[How to design a high concurrency system? ](/docs/high-concurrency/high-concurrency-design.md)

---

## the public

GitHub Technical Community [Doocs](https://github.com/doocs) owns the only official account "**Doocs Open Source Community**"​, welcome to scan the code and follow, **Focus on sharing relevant knowledge in the technical field and the latest industry information* *. Of course, you can also add my personal WeChat (note: GitHub) to pull you into the technical exchange group.

<table>
  <tr>
    <td align="center" style="width: 200px;">
      <a href="https://github.com/doocs">
        <img src="https://cdn.jsdelivr.net/gh/doocs/advanced-java@main/images/qrcode-for-doocs.jpg" style="width: 400px;"><br>
        <sub>Public platform</sub>
      </a><br>
    </td>
    <td align="center" style="width: 200px;">
      <a href="https://github.com/yanglbme">
        <img src="https://cdn.jsdelivr.net/gh/doocs/advanced-java@main/images/qrcode-for-yanglbme.jpg" style="width: 400px;"><br>
        <sub>Personal WeChat</sub>
      </a><br>
    </td>
  </tr>
</table>

Follow the official account of "**Doocs Open Source Community**" and reply to **PDF** to get the offline PDF document (283 pages of the essence) of this project, making learning more convenient!

<img src="https://cdn.jsdelivr.net/gh/doocs/advanced-java@main/images/pdf.png" style="width: 600px;"><br>