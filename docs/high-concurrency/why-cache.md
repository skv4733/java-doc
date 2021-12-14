## Interview Questions

How is the cache used in the project? Why use caching? What are the consequences of improper use of the cache?

## Interviewer psychoanalysis

Internet companies must ask this question. If a person doesn’t even know the cache, it’s really embarrassing.

As long as you ask about caching, the first question that comes up is to ask where your project uses caching first? Why use it? Doesn't it work? What are the possible undesirable consequences if you use it?

This is to see if you have any thoughts behind caching. If you are using it stupidly and can’t give the interviewer a reasonable answer, then the interviewer must have a bad impression of you and think that you usually think too little. Just know work.

## Analysis of Interview Questions

### How is the cache used in the project?

This needs to be combined with the business of your own project.

### Why use cache?

There are two main uses for caching: **high performance** and **high concurrency**.

#### High performance

Assuming such a scenario, you have an operation, a request comes, you babble all kinds of messy operations mysql, a result is found in half a day, and it takes 600ms. But this result may not change in the next few hours, or it does not need to be reported to users immediately. So what should I do now?

Cache, toss 600ms to find the result, throw it in the cache, a key corresponds to a value, someone will check it next time, don't use mysql to toss 600ms, directly from the cache, find a value through a key, and it will be done in 2ms. Performance improved by 300 times.

That is to say, for some results that require complex operations and time-consuming to find out, and it is determined that there will be no changes later, but there are many read requests, then the results of the query are directly placed in the cache, and the cache is read directly later.

#### High concurrency

MySQL is such a heavy database, it is not designed to allow you to play with high concurrency, although it can be played, but the natural support is not good. The mysql stand-alone support to `2000QPS` also starts to report to the police easily.

So if you have a system with 10,000 requests per second during the peak period, that single mysql server will definitely die. You can only use the cache at this time, put a lot of data in the cache, don't put mysql. The caching function is simple. To put it bluntly, it is a `key-value` operation. The concurrency supported by a single machine is easily tens of thousands per second, and it supports high concurrency so easy. The amount of concurrency carried by a single machine is dozens of times that of a mysql stand-alone machine.

> Cache is memory, and memory naturally supports high concurrency.

### What are the undesirable consequences after using the cache?

Common caching problems are as follows:

-[Cache and database double write inconsistency](/docs/high-concurrency/redis-consistence.md)
-[Cache avalanche, cache penetration, cache breakdown](/docs/high-concurrency/redis-caching-avalanche-and-caching-penetration.md)
-[Cache Concurrency Competition](/docs/high-concurrency/redis-cas.md)

Click on the hyperlink to directly view the cache-related problems and solutions.