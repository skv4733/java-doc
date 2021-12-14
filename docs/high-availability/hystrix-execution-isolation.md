## Hystrix isolation strategy fine-grained control

Hystrix implements resource isolation, there are two strategies:

-Thread pool isolation
-Semaphore isolation

For resource isolation, some fine-grained control can actually be done.

### execution.isolation.strategy

The resource isolation strategy of HystrixCommand.run() is specified: `THREAD` or `SEMAPHORE`, one based on thread pool and one based on semaphore.

```java
// to use thread isolation
HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.THREAD)

// to use semaphore isolation
HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.SEMAPHORE)
```

Thread pool mechanism, each command runs in a thread, and the current limit is controlled by the size of the thread pool; semaphore mechanism, command runs in the calling thread (that is, Tomcat’s thread pool), through the capacity of the semaphore To limit the current.

How to choose between thread pool and semaphore?

**The default strategy** is the thread pool.

**Thread pool** In fact, the biggest advantage is that for network access requests, if there is a timeout, the calling thread can be prevented from being blocked.

The use of semaphore scenarios is usually for large concurrency scenarios, each service instance has a few hundred `QPS` per second, then if you use the thread pool at this time, there are generally not too many threads, which may not be able to support it. With such a high concurrency, if you want to sustain it, it may consume a lot of thread resources, then semaphores are used for current limiting protection. Generally, semaphores are commonly used in some business logic services based on pure memory, and do not involve any network access requests.

### command key & command group

We use thread pool isolation, how do we divide the **dependency service**, **dependence service interface**, and **thread pool**?

For each command, you can set its own name, command key, and also set its own command group.

```java
private static final Setter cachedSetter = Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("ExampleGroup"))
                                                 .andCommandKey(HystrixCommandKey.Factory.asKey("HelloWorld"));

public CommandHelloWorld(String name) {
    super(cachedSetter);
    this.name = name;
}
```

Command group is a very important concept. By default, a thread pool is defined through command group, and some monitoring and alarm information will be aggregated through command group. Requests in the same command group will all enter the same thread pool.

### command thread pool

ThreadPoolKey represents a HystrixThreadPool for unified monitoring, statistics, and caching. The default ThreadPoolKey is the name of the command group. Each command will be bound to the ThreadPool corresponding to its ThreadPoolKey.

If you don't want to use the command group directly, you can also manually set the name of the ThreadPool.

```java
private static final Setter cachedSetter = Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("ExampleGroup"))
                                                 .andCommandKey(HystrixCommandKey.Factory.asKey("HelloWorld"))
                                                 .andThreadPoolKey(HystrixThreadPoolKey.Factory.asKey("HelloWorldPool"));

public CommandHelloWorld(String name) {
    super(cachedSetter);
    this.name = name;
}
```

### command key & command group & command thread pool

**command key**, represents a type of command, generally speaking, represents an interface of downstream dependent services.

**command group** represents a certain downstream dependent service, which is very reasonable. A dependent service may expose multiple interfaces, and each interface is a command key. The command group logically counts the number of calls to a bunch of command keys, the number of successes, the number of timeouts, the number of failures, etc., and you can see the overall access status of a certain service. **Generally speaking, it is recommended to divide a thread pool based on a service area, and the command keys belong to the same thread pool by default. **

For example, there is a service A. You estimate that the total QPS of all interfaces of service A per second is about 100. You have a service B to call service A. Your service B deploys 10 instances, and on each instance, use the command group to correspond to the downstream service A. Given a thread pool, the amount is about 10, so that the overall QPS of service B's access to service A is about 100 per second.

However, if the command group corresponds to a service, and the several interfaces exposed by this service have very different visits, the difference is very big. You may wish to include command keys corresponding to multiple interfaces within the command group corresponding to this service, and do some fine-grained resource isolation. **In other words, I hope to use different thread pools for different interfaces of the same service. **

```
command key -> command group

command key -> own thread pool key
```

Logically speaking, multiple command keys belong to a command group, and they will be put together for statistics when doing statistics. Each command key has its own thread pool, and each interface has its own thread pool for resource isolation and current limiting.

To put it plainly, if you want to use your own thread pool for your command key, you can define your own thread pool key, and it’s ok.

### coreSize

Set the size of the thread pool, the default is 10. Generally speaking, this default 10 thread size is sufficient.

```java
HystrixThreadPoolProperties.Setter().withCoreSize(int value);
```

### queueSizeRejectionThreshold

If all 10 threads in the thread pool are working, and there are no idle threads to do other things, if there are any more requests at this time, they will enter the queue backlog first. If the queue is full and another request comes over, it will reject directly, reject the request, execute the logic of fallback downgrade, and return quickly.

![hystrix-thread-pool-queue](./images/hystrix-thread-pool-queue.png)

Control the threshold of reject after the queue is full, because maxQueueSize does not allow hot modification, so this parameter can be hot modified to control the maximum size of the queue.

```java
HystrixThreadPoolProperties.Setter().withQueueSizeRejectionThreshold(int value);
```

### execution.isolation.semaphore.maxConcurrentRequests

Set the maximum concurrent access allowed when using the SEMAPHORE isolation strategy. If this maximum concurrency is exceeded, the request will be rejected directly.

The setting of this concurrency should be similar to the setting of the thread pool size, but based on the semaphore, the performance will be much better, and the overhead of the Hystrix framework itself will be much smaller.

The default value is 10, try to set it as small as possible, because once it is set too large and there is a delay, it may instantly cause the thread resources of tomcat itself to be full.

```java
HystrixCommandProperties.Setter().withExecutionIsolationSemaphoreMaxConcurrentRequests(int value);
```