## Distributed system CAP theorem What does P stand for

The author had a lot of doubts when looking at the CAP theorem. The definition of the CAP theorem means that in a distributed system, only two of the three can be satisfied, that is, the existence of a distributed CA system. The author has looked up many articles about CAP on the Internet. Although these articles have various explanations of P, in summary, most of these views are that P is indispensable, which means that in a distributed system, it can only be AP or CP. This theory conflicts with the theory I knew before (there is a distributed CA system), so I have doubts.

> This theorem originated from a conjecture proposed by Eric Brewer, a computer scientist at the University of California, Berkeley, at the 2000 Principles of Distributed Computing Symposium (PODC). In 2002, Seth Gilbert and Nancy Lynch of the Massachusetts Institute of Technology (MIT) published a proof of Brewer’s conjecture, making it a theorem.

### What is CAP theorem (CAP theorem)

In theoretical computer science, CAP theorem (CAP theorem), also known as Brewer's theorem, points out that it is impossible for a distributed computing system to satisfy the following three points at the same time:

-Consistency (equivalent to all nodes accessing the same copy of the latest data)
-Availability (Availability can be obtained for every request-but there is no guarantee that the data obtained is the latest data)
-Partition tolerance (in terms of actual effect, partition is equivalent to the time limit of communication. If the system cannot achieve data consistency within the time limit, it means that a partition has occurred, and the current operation must be in C Choose between A and A.)

### Partition tolerance

The easiest way to understand the CAP theory is to imagine two nodes on both sides of the partition. Allowing at least one node to update the state will result in inconsistent data, that is, loss of the C property. If in order to ensure data consistency, the node on one side of the partition is set to be unavailable, then the A property is lost. Unless two nodes can communicate with each other, they can guarantee both C and A, which will lead to the loss of the P property.

-P refers to partition fault tolerance. After partition phenomenon occurs, fault tolerance is required. Fault tolerance refers to choosing between A and C. If the distributed system has no partition phenomenon (no inconsistency or unavailability), there will be no partition itself. Since there is no partition, there will be no partition fault tolerance.
-No matter whether the system I designed is AP or CP system, it is unavailable if there is no inconsistency. Then the system is in the CA state
-The premise of P is that there must be a partition

> Article source: [Wikipedia CAP Theorem](https://zh.wikipedia.org/wiki/CAP%E5%AE%9A%E7%90%86)

## Comparison of several commonly used CAP frameworks

| Frame | Affiliation |
| --------- | ---- |
| Eureka | AP |
| Zookeeper | CP |
| Consul | CP |

### Eureka

> Eureka guarantees availability and achieves ultimate consistency.

All nodes of Eureka are equal, all data is the same, and Eureka can be cross-registered with each other.
Eureka client uses the built-in polling load balancer to register. There is a detection interval. If the heartbeat is not received within a certain period of time, the node registration information will be removed; if the client finds that the current Eureka is not available, it will switch to other If all Eurekas kneel down, the Eureka client will use the last data as a local cache; therefore, each of the above designs does not have the characteristics of `consistency`.

Note: Because of the characteristics of EurekaAP and the request interval synchronization mechanism, when the service is updated, the current service status is generally set to `offline` manually through Eureka's api, and restart after 2 synchronization intervals, so as to ensure that the service update node Impact on the overall system

### Zookeeper

> Strong consistency

Zookeeper will stop the service when electing the leader. Only after the successful election of the leader can it provide services, the election time is longer; the paxos election voting mechanism is used internally, and only if more than half of the votes are obtained can it become the leader. Otherwise, re-vote, so the deployment time is the most Good cluster nodes are not less than an odd number of 3 (but who can guarantee that the nodes will also be the base number after kneeling down); Zookeeper health checks generally use tcp long links, which will become unavailable when the internal network is jittered or when the corresponding node is blocked. It's still more dangerous here;

### Consul

Same data CP as Zookeeper

When Consul is registered, only more than half of the nodes are successfully written before it is considered successful; when the leader hangs, the entire Consul is unavailable during the re-election period, ensuring strong consistency but sacrificing availability
Many blogs say that Consul belongs to ap, and the official has confirmed that it is a CP mechanism. Original address: https://www.consul.io/docs/intro/vs/serf