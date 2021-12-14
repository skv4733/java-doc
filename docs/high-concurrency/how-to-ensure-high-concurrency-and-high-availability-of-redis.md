## Interview Questions

How to ensure the high concurrency and high availability of redis? Can you introduce the principle of redis master-slave replication? Can you introduce the principle of redis sentry?

## Interviewer psychoanalysis

In fact, to ask this question is mainly to test you, how high concurrency can a single machine of redis carry? How to expand the capacity to carry more concurrency if the single machine cannot handle it? Will redis hang? Since redis will hang, how to ensure that redis is highly available?

In fact, they are all about some issues that you must consider in the project. If you haven't considered them, then you really don't think too much about the issues in the production system.

## Analysis of Interview Questions

If you use redis caching technology, you must consider how to use redis to add multiple machines to ensure that redis is highly concurrent, and how to make redis ensure that it does not die immediately after it hangs, that is, redis is highly available.

Since this section contains a lot of content, it will be divided into two sections for explanation.

-[redis master-slave architecture](/docs/high-concurrency/redis-master-slave.md)
-[redis achieves high availability based on sentinel](/docs/high-concurrency/redis-sentinel.md)

Redis achieves **high concurrency** mainly relying on **master-slave architecture**, one master with multiple slaves. Generally speaking, many projects are actually enough. A single master is used to write data, a single machine is tens of thousands of QPS, and multiple slaves are used. To query data, multiple slave instances can provide 10w QPS per second.

If you want to accommodate a large amount of data while achieving high concurrency, you need a redis cluster. After using the redis cluster, you can provide hundreds of thousands of read and write concurrency per second.

Redis is highly available. If it is a master-slave deployment, then a sentinel can be added, and it can be realized. If any instance is down, the master-slave switch can be performed.