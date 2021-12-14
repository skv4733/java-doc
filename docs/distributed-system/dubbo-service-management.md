## Interview Questions

How to perform service governance, service degradation, failure retry and timeout retry based on dubbo?

## Interviewer psychoanalysis

Service governance, if you ask you this question, it is actually to see if you have the idea of ​​**service governance**, because this is a problem that people who have done complex microservices will definitely encounter.

**Service degradation**, this is a must-have topic in complex distributed systems, because distributed systems call each other back and forth, and any system fails, if you don’t downgrade, it will just crash completely? That would be too cheating.

**Failure retry**, network requests are so frequent in a distributed system. If it accidentally fails once due to network problems, should I try again?

**Timeout retry**, as above, if the network is a little slower accidentally and it times out, how do I try again?

## Analysis of Interview Questions

### Service governance

#### 1. Call link automatically generated

A large-scale distributed system, or to use the current popular microservice architecture, **distributed system consists of a large number of services**. So how do these services call each other? What is the call link? To be honest, almost no one can figure it out later, because there are too many services, maybe hundreds or even thousands of services.

Then you need to automatically record the calls between various services in a distributed system based on dubbo, and then automatically generate the dependencies and call links between each service to make a picture, Show it, everyone can see it, right.

![dubbo-service-invoke-road](./images/dubbo-service-invoke-road.png)

#### 2. Service access pressure and duration statistics

Need to automatically count the number of calls and access delay between each interface and service, and it should be divided into two levels.

-One level is the interface granularity, which is how many times each interface of each service is called per day, TP50/TP90/TP99, and what are the request delays of the three levels;
-The second level starts from the source entry. After a complete request link passes through dozens of services, one request is completed, how many times the full link goes every day, and the full link request delay TP50/TP90/TP99, respectively how many.

After all these things are settled, we can then see where the current system pressure is mainly and how to expand and optimize it.

#### 3. Other

-Service layering (avoid circular dependencies)
-Call link failure monitoring and alarm
-Service authentication
-Monitoring of the availability of each service (Interface call success rate? How many 9? 99.99%, 99.9%, 99%)

### Service downgrade

For example, service A calls service B, and service B hangs up. Service A retries to call service B several times, but it still fails. Then downgrade directly, go through a backup logic, and return a response to the user.

For example, we have the interface `HelloService`. `HelloServiceImpl` has a concrete implementation of this interface.

```java
public interface HelloService {
   void sayHello();
}

public class HelloServiceImpl implements HelloService {
    public void sayHello() {
        System.out.println("hello world......");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <dubbo:application name="dubbo-provider" />
    <dubbo:registry address="zookeeper://127.0.0.1:2181" />
    <dubbo:protocol name="dubbo" port="20880" />
    <dubbo:service interface="com.zhss.service.HelloService" ref="helloServiceImpl" timeout="10000" />
    <bean id="helloServiceImpl" class="com.zhss.service.HelloServiceImpl" />

</beans>

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <dubbo:application name="dubbo-consumer" />

    <dubbo:registry address="zookeeper://127.0.0.1:2181" />

    <dubbo:reference id="fooService" interface="com.test.service.FooService" timeout="10000" check="false" mock="return null">
    </dubbo:reference>

</beans>

```

When we fail to call the interface, we can uniformly return null through `mock`.

The value of mock can also be modified to true, and then implement a Mock class under the same path as the interface. The naming rule is "interface name + `Mock` "suffix. Then implement your own downgrade logic in the Mock class.

```java
public class HelloServiceMock implements HelloService {
    public void sayHello() {
        // Downgrade logic
    }
}
```

### Failure retry and timeout retry

The so-called failed retry is that if the consumer fails to call the provider, such as throwing an exception, it should be retryable at this time, or the call can also be retryed if the call times out. The configuration is as follows:

```xml
<dubbo:reference id="xxxx" interface="xx" check="true" async="false" retries="3" timeout="2000"/>
```

Give a chestnut.

The interface of a certain service takes 5s. You can't wait here. After you configure the timeout here, I wait 2s. If it doesn't return, I will withdraw it directly. I can't wait for you.

You can talk about how you set these parameters based on the specific scenarios of your company:

-`timeout`: Generally set to `200ms`, we think it cannot exceed `200ms` and it has not returned yet.
-`retries`: Set retries, usually when reading a request. For example, if you want to query data, you can set a retries. If you don't read it the first time, report an error, retry the specified number of times, and try to read it again.