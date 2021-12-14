## Interview Questions

What is the problem of concurrent competition in Redis? how to solve this problem? Do you know the CAS scheme of Redis transactions?

## Interviewer psychoanalysis

This is also a very common problem online, that is, if multiple clients write a key concurrently at the same time, the data that should have arrived first arrives later, causing the data version to be wrong; or multiple clients get a key at the same time, Write back after modifying the value. As long as the sequence is wrong, the data is wrong.

And Redis has its own CAS-type optimistic locking scheme that naturally solves this problem.

## Analysis of Interview Questions

At a certain moment, multiple system instances update a certain key. Distributed locks can be implemented based on zookeeper. Each system obtains a distributed lock through zookeeper to ensure that only one system instance can operate a key at the same time, and no one else is allowed to read or write.

![zookeeper-distributed-lock](./images/zookeeper-distributed-lock.png)

The data you want to write to the cache is all detected from mysql, and must be written to mysql. When writing to mysql, a timestamp must be saved. When it is found from mysql, the timestamp is also found.

Before writing each time, first judge whether the timestamp of the current value is newer than the timestamp of the value in the cache. If so, you can write, otherwise, you cannot overwrite the new data with the old data.