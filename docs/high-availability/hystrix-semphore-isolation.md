## Implement resource isolation based on Hystrix semaphore mechanism

One of the core functions in Hystrix is ​​actually the so-called **resource isolation**. The most core problem to be solved is to isolate multiple dependent service calls into their respective resource pools. Avoid calling a certain dependent service, because the delay or failure of the interface call of the dependent service causes all the thread resources of the service to be consumed on the interface call of this service. Once the thread resources of a certain service are exhausted, it may cause the service to crash, and even the failure will continue to spread.

Hystrix implements resource isolation, there are two main technologies:

- Thread Pool
- signal

By default, Hystrix uses thread pool mode.

The thread pool technology has been mentioned before, this section will talk about the semaphore mechanism to achieve resource isolation, as well as the difference between the two technologies and specific application scenarios.

### Semaphore mechanism

The resource isolation of the semaphore only serves as a switch. For example, the semaphore size of service A is 10, which means that it only allows 10 tomcat threads to access service A at the same time, and other requests will be rejected, thus achieving The role of resource isolation and current limiting protection.

![hystrix-semphore](./images/hystrix-semphore.png)

### The difference between thread pool and semaphore

The thread pool isolation technology does not mean to control the threads of web containers like tomcat. In a more strict sense, Hystrix's thread pool isolation technology controls the execution of tomcat threads. After the Hystrix thread pool is full, it will ensure that the tomcat thread will not be hanged due to the delay or failure of the interface call that depends on the service. The other tomcat threads will not be stuck and can return quickly, and then support other things.

Thread pool isolation technology uses Hystrix's own threads to execute calls; and semaphore isolation technology directly allows Tomcat threads to call dependent services. Semaphore isolation is just a checkpoint. How many tomcat threads are allowed to pass through the semaphore and then execute.

![hystrix-semphore-thread-pool](./images/hystrix-semphore-thread-pool.png)

**Applicable scene**:

-**Thread pool technology**, suitable for most scenarios, such as our call and access to service-dependent network requests, and need to control the call timeout (capture timeout timeout exceptions).
-**Semaphore technology**, it is suitable to say that your access is not an access to external dependencies, but an access to some internal business logic, and the internal code of the system does not actually involve any network requests, then Just do the normal current limit of the semaphore, because there is no need to capture timeout similar problems.

### Simple Semaphore Demo

In the business context, what scenarios are more suitable for semaphores?

For example, in general, the cache service may put some data that is particularly small and frequently accessed in its own pure memory.

Give a chestnut. Generally, after obtaining the product data, we have to obtain the geographic location, province, city, seller, etc. of the product, which may be in our own pure memory, such as a Map. For this kind of logic that directly accesses local memory, it is more suitable to use semaphores to do a simple isolation.

The advantage is that you don’t need to manage the thread pool yourself, you don’t need care timeout, and you don’t need to perform thread context switching. If the semaphore is isolated, the performance will be relatively higher.

If this is a local cache, we can get cityName through cityId.

```java
public class LocationCache {
    private static Map<Long, String> cityMap = new HashMap<>();

    static {
        cityMap.put(1L, "Beijing");
    }

    /**
     * Get cityName by cityId
     *
     * @param cityId city id
     * @return city name
     */
    public static String getCityName(Long cityId) {
        return cityMap.get(cityId);
    }
}
```

Write a GetCityNameCommand and set the strategy to **semaphore**. Get the local cache in the run() method. Our purpose is to isolate resources from the code that gets the local cache.

```java
public class GetCityNameCommand extends HystrixCommand<String> {

    private Long cityId;

    public GetCityNameCommand(Long cityId) {
        // Set semaphore isolation strategy
        super(Setter.withGroupKey(HystrixCommandGroupKey.Factory.asKey("GetCityNameGroup"))
                .andCommandPropertiesDefaults(HystrixCommandProperties.Setter()
                        .withExecutionIsolationStrategy(HystrixCommandProperties.ExecutionIsolationStrategy.SEMAPHORE)));

        this.cityId = cityId;
    }

    @Override
    protected String run() {
        // Code that needs semaphore isolation
        return LocationCache.getCityName(cityId);
    }
}
```

At the interface layer, by creating GetCityNameCommand, passing in the cityId, and executing the execute() method, the code for obtaining the local cityName cache will isolate the resources of the semaphore.

```java
@RequestMapping("/getProductInfo")
@ResponseBody
public String getProductInfo(Long productId) {
    HystrixCommand<ProductInfo> getProductInfoCommand = new GetProductInfoCommand(productId);

    // Execute through command to get the latest product data
    ProductInfo productInfo = getProductInfoCommand.execute();

    Long cityId = productInfo.getCityId();

    GetCityNameCommand getCityNameCommand = new GetCityNameCommand(cityId);
    // The code for obtaining local memory (cityName) will be resource-isolated by the semaphore
    String cityName = getCityNameCommand.execute();

    productInfo.setCityName(cityName);

    System.out.println(productInfo);
    return "success";
}
```