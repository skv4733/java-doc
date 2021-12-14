## Interview Questions

Tell me about the working principle of dubbo? Can the communication be continued after the registration center is hung up? Tell me about the process of rpc request?

## Interviewer psychoanalysis

MQ, ES, Redis, Dubbo, come up and ask you some **thinking questions** and **principles**, such as the principle of kafka high-availability architecture, the principle of es distributed architecture, the principle of redis threading model, and the working principle of Dubbo; Then there are some problems that may be encountered in the production environment, because after the introduction of each technology, the production environment may encounter some problems; the more comprehensive one is the system design, such as letting you design an MQ, design a search engine, Design a cache, design an rpc framework, and so on.

Now that we have started to talk about distributed systems, we naturally focus on talking about dubbo first. After all, dubbo is the rpc framework standard for distributed systems in most companies. Based on dubbo, a complete set of microservice architecture can also be built. But you need to develop it yourself.

Of course, spring cloud started to be very popular last year, and now a large number of companies have begun to turn to spring cloud. After all, spring cloud is a family bucket of microservice architecture. But because many companies are still using dubbo, dubbo will definitely be the focus of the current interview. What's more, dubbo has restarted the open source community for maintenance and donated it to apache. It should still have a certain market and position in the future.

Now that we talk about dubbo, we must start with the principle of dubbo. You first talk about the architecture of dubbo supporting rpc distributed calls, and then talk about how an rpc request dubbo is done for you, right.

## Analysis of Interview Questions

### How dubbo works

-The first layer: service layer, interface layer, for service providers and consumers to achieve
-The second layer: config layer, configuration layer, mainly for various configurations of dubbo
-The third layer: proxy layer, service proxy layer, whether it is a consumer or a provider, dubbo will generate a proxy for you, and network communication between the proxy
-The fourth layer: registry layer, service registration layer, responsible for service registration and discovery
-The fifth layer: cluster layer, cluster layer, encapsulate routing and load balancing of multiple service providers, combine multiple instances into one service
-The sixth layer: monitor layer, monitoring layer, to monitor the number of calls and call time of the rpc interface
-Seventh layer: protocal layer, remote call layer, encapsulation rpc call
-The eighth layer: exchange layer, information exchange layer, package request response mode, synchronous to asynchronous
-The ninth layer: transport layer, network transport layer, abstract mina and netty as a unified interface
-Tenth layer: serialize layer, data serialization layer

### work process

-The first step: the provider registers with the registration center
-Step 2: The consumer subscribes to the service from the registry, and the registry will notify the consumer of the registered service
-Step 3: The consumer calls the provider
-Step 4: Both consumer and provider notify the monitoring center asynchronously

![dubbo-operating-principle](./images/dubbo-operating-principle.png)

### Can the communication be continued after the registration center is hung up?

Yes, because the consumer will pull the provider's address and other information ** to the local cache** when initializing, so the registry can continue to communicate after hanging up.