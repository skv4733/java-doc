## Interview Questions

How is Redis deployed in the production environment?

## Interviewer psychoanalysis

See if you understand the deployment architecture of your company's Redis production cluster. If you don't understand, then you are indeed negligent. Is your Redis a master-slave architecture? Cluster architecture? Which cluster solution was used? Is there any guarantee of high availability? Is there a persistence mechanism to ensure data recovery? How many gigabytes of memory does online Redis give? What parameters are set? After the stress test, how many QPS does your Redis cluster carry?

Brother, you must be clear about these, otherwise you really haven't thought about it.

## Analysis of Interview Questions

Redis cluster, 10 machines, 5 machines are deployed with Redis master instances, and the other 5 machines are deployed with Redis slave instances. Each master instance has a slave instance. 5 nodes provide external read and write services. The peak read and write QPS may reach 50,000 per second, and 5 machines can receive up to 250,000 read and write requests per second.

What is the configuration of the machine? 32G memory + 8 core CPU + 1T disk, but 10g of memory is allocated to the Redis process. In general online production environments, the Redis memory should not exceed 10g as much as possible. If it exceeds 10g, there may be problems.

Five machines provide external reading and writing, with a total of 50g of memory.

Because each master instance has a slave instance, it is highly available. If any master instance goes down, it will automatically fail migration, and the Redis slave instance will automatically become the master instance and continue to provide read and write services.

What data are you writing to memory? What is the size of each piece of data? Commodity data, each piece of data is 10kb. 100 pieces of data are 1mb, and 100,000 pieces of data are 1g. The resident memory is 2 million pieces of product data, and the occupied memory is 20g, which is only less than 50% of the total memory. The current peak period is about 3,500 requests per second.

In fact, large companies will have an infrastructure team responsible for the operation and maintenance of the cache cluster.