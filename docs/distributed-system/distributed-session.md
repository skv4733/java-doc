## Interview Questions

How to implement distributed sessions in cluster deployment?

## Interviewer psychoanalysis

The interviewer asked you how a bunch of Dubbo are played. If you know how to play Dubbo, you can turn a monolithic system into a distributed system. After the distributed system, there will be a bunch of problems. The biggest problem is ** Distributed transaction**, **interface idempotence**, **distributed lock**, and the last one is **distributed Session**.

Of course, the problems in distributed systems are more than just that, there are so many, and the complexity is very high. Here are just a few common questions, which are also frequently asked during interviews.

## Analysis of Interview Questions

What is Session? The browser has a cookie, and this cookie exists for a period of time, and every time a request is sent, it will bring a special `jsessionid cookie`. Based on this, a corresponding Session domain can be maintained on the server side. Put some data.

Generally speaking, as long as you do not close the browser and the cookie is still there, then the corresponding session is there, but if the cookie is gone, the session is gone. Commonly found in shopping carts and the like, as well as login status preservation and the like.

Not much to say about this, anyone who knows Java should know this.

It's okay to play sessions like this in a monolithic system, but if you are a distributed system, where is the session state maintained for so many services?

In fact, there are many methods, but the following are commonly used:

### No Session at all

Use JWT Token to store user identity, and then obtain other information from the database or cache. So it doesn't matter which server the request is assigned to.

### Tomcat + Redis

This is actually quite convenient, it is to use the Session code, as before, it is still based on Tomcat's native Session support, and then a thing called `Tomcat RedisSessionManager` is used to let all the Tomcats we deploy store Session data Just go to Redis.

Configure in the Tomcat configuration file:

```xml
<Valve className="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve" />

<Manager className="com.orangefunction.tomcat.redissessions.RedisSessionManager"
         host="{redis.host}"
         port="{redis.port}"
         database="{redis.dbnum}"
         maxInactiveInterval="60"/>
```

Then specify the host and port of Redis to be ok.

```xml
<Valve className="com.orangefunction.tomcat.redissessions.RedisSessionHandlerValve" />
<Manager className="com.orangefunction.tomcat.redissessions.RedisSessionManager"
sentinelMaster="mymaster"
sentinels="<sentinel1-ip>:26379,<sentinel2-ip>:26379,<sentinel3-ip>:26379"
maxInactiveInterval="60"/>
```

You can also use the above method to save session data based on the Redis high-availability cluster supported by the Redis sentinel, all of which are ok.

### Spring Session + Redis

The second method mentioned above will be re-coupled with the Tomcat container. If I want to migrate the Web container to Jetty, do I have to configure Jetty again?

Because the above Tomcat + Redis method is easy to use, but it will **heavily depend on the web container**, it is not easy to port the code to other web containers, especially if you change the technology stack? For example, replace it with Spring Cloud or Spring Boot?

So the better one-stop solution based on Java is Spring. People Spring basically contracted most of the frameworks we need to use, Spirng Cloud for microservices, and Spring Boot for scaffolding, so Spring Session is a good choice.

Configure in pom.xml:

```xml
<dependency>
  <groupId>org.springframework.session</groupId>
  <artifactId>spring-session-data-redis</artifactId>
  <version>1.2.1.RELEASE</version>
</dependency>
<dependency>
  <groupId>redis.clients</groupId>
  <artifactId>jedis</artifactId>
  <version>2.8.1</version>
</dependency>
```

Configure in the Spring configuration file:

```xml
<bean id="redisHttpSessionConfiguration"
     class="org.springframework.session.data.redis.config.annotation.web.http.RedisHttpSessionConfiguration">
    <property name="maxInactiveIntervalInSeconds" value="600"/>
</bean>

<bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
    <property name="maxTotal" value="100" />
    <property name="maxIdle" value="10" />
</bean>

<bean id="jedisConnectionFactory"
      class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" destroy-method="destroy">
    <property name="hostName" value="${redis_hostname}"/>
    <property name="port" value="${redis_port}"/>
    <property name="password" value="${redis_pwd}" />
    <property name="timeout" value="3000"/>
    <property name="usePool" value="true"/>
    <property name="poolConfig" ref="jedisPoolConfig"/>
</bean>
```

Configure in web.xml:

```xml
<filter>
    <filter-name>springSessionRepositoryFilter</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
    <filter-name>springSessionRepositoryFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

Sample code:

```java
@RestController
@RequestMapping("/test")
public class TestController {

    @RequestMapping("/putIntoSession")
    public String putIntoSession(HttpServletRequest request, String username) {
        request.getSession().setAttribute("name", "leo");
        return "ok";
    }

    @RequestMapping("/getFromSession")
    public String getFromSession(HttpServletRequest request, Model model){
        String name = request.getSession().getAttribute("name");
        return name;
    }
}
```

The above code is ok. Configure Spring Session to store Session data based on Redis, and then configure a Spring Session filter. In this case, Session related operations will be handled by Spring Session. Then in the code, use the native Session operation, which is directly based on Spring Session to get data from Redis.

There are many ways to implement a distributed session. I'm just talking about the more common ways. Tomcat + Redis was more commonly used in the early days, but it will be re-coupled to Tomcat; in recent years, it has been implemented through Spring Session.