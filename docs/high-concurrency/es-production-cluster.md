## Interview Questions

What is the deployment architecture of the ES production cluster? What is the approximate amount of data for each index? How many shards are there for each index?

## Interviewer psychoanalysis

This question, including the following redis, talks about es, redis, mysql sub-database sub-table and other technologies, the interview must be asked! How did you deploy it in your production environment? To put it bluntly, this question has no technical content, it just depends on whether you have done it in a real production environment!

Some students may have never done it in a production environment, have not actually deployed an es cluster with an online machine, nor have they actually played it, nor have they imported tens or even hundreds of millions of data into the es cluster. , Maybe you don’t know the details of some of the production projects inside.

If you have played the demo by yourself and have not touched a real es cluster, then you may be confused at this time. Don't be stunned, you must answer this question calmly, indicating that you have indeed done this.

## Analysis of Interview Questions

In fact, this question is nothing. If you have done es, then you must know the actual situation of your production es cluster. How many machines have you deployed? How many indexes are there? How much data does each index have? How many shards are given for each index? You must know!

But if you haven't done it before, don't be frustrated. I will tell you a basic version. Then you can just talk about it briefly.

-For the es production cluster, we deployed 5 machines, each machine is 6-core 64G, and the total memory of the cluster is 320G.
-The daily incremental data of our es cluster is about 20 million, the daily incremental data is about 500MB, and the monthly incremental data is about 600 million, 15G. The system has been running for several months, and the total amount of data in the es cluster is about 100G.
-There are currently 5 indexes online (combined with your own business to see which data you have can put es), the data volume of each index is about 20G, so within this data volume, we allocate each index There are 8 shards, which is 3 shards more than the default 5 shards.

Probably just say that.