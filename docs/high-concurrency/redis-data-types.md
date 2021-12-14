## Interview Questions

What data types does Redis have? In which scenarios is it appropriate to use?

## Interviewer psychoanalysis

Unless the interviewer feels that he is looking at your resume, he is a relatively junior classmate who has been working for less than 3 years, and may not have very in-depth research on technology, the interviewer will ask such questions. Otherwise, during the precious interview time, the interviewer really doesn't want to ask too much.

In fact, there are two main reasons for asking this question:

-See if you have a comprehensive understanding of what functions Redis has, how to use it in general, and what to use in any scenario, I am afraid that you will not be the easiest KV operation;
-See how you have played Redis in actual projects.

If you didn't answer well, didn't mention several data types, and didn't mention any scenes, the interviewer must have a bad impression of you when you are done, and think that you usually do simple set and get.

## Analysis of Interview Questions

Redis mainly has the following data types:

-Strings
-Hashes
-Lists
-Sets
-Sorted Sets

> In addition to these 5 data types, Redis also has Bitmaps, HyperLogLogs, Streams, etc.

### Strings

This is the simplest type, that is, ordinary set and get, for simple KV caching.

```bash
set college szu
```

### Hashes

This is a structure similar to map. This generally means that structured data, such as an object (provided that this object is not nested with other objects), can be cached in Redis, and then read and write the cache every time When, you can just manipulate **a field** in the hash.

```bash
hset person name bingo
hset person age 20
hset person id 1
hget person name
```

```json
(person = {
  "name": "bingo",
  "age": 20,
  "id": 1
})
```

### Lists

Lists are ordered lists, which can be played with many tricks.

For example, you can store some list-type data structures through list, such as fan lists and article comment lists.

For example, you can use the lrange command to read the elements in a closed interval, and you can implement paging query based on the list. This is a great feature. Based on Redis to implement simple high-performance paging, you can do drop-down and continuous paging similar to Weibo. The things with high performance, just go page by page.

```bash
# 0 start position, -1 end position, when the end position is -1, it means the last position of the list, that is, view all.
lrange mylist 0 -1
```

For example, you can set up a simple message queue, go in from the head of the list, and get it out from the tail of the list.

```bash
lpush mylist 1
lpush mylist 2
lpush mylist 3 4 5

# 1
rpop mylist
```

### Sets

Sets are unordered collections, with automatic deduplication.

Throw in the data that needs to be deduplicated in the system directly based on the set, and it will be automatically deduplicated. If you need to perform rapid global deduplication on some data, you can of course also deduplicate based on the HashSet in the jvm memory, but if Is one of your systems deployed on multiple machines? It is necessary to perform global set deduplication based on Redis.

You can play the operations of intersection, union, and difference based on set. For example, intersection. You can put the fan lists of two people into an intersection to see who their mutual friends are? Right.

Put the fans of the two big Vs in two sets, and make the intersection of the two sets.

```bash
#-------Operate a set-------
# Add element
sadd mySet 1

# View all elements
smembers mySet

# Determine whether it contains a certain value
sismember mySet 3

# Delete some/some elements
srem mySet 1
srem mySet 2 4

# View the number of elements
scard mySet

# Randomly delete an element
spop mySet

#-------Operate multiple sets-------
# Move the elements of one set to another set
smove yourSet mySet 2

# Find the intersection of two sets
sinter yourSet mySet

# Find the union of two sets
sunion yourSet mySet

# Find the elements in yourSet but not in mySet
sdiff yourSet mySet
```

### Sorted Sets

Sorted Sets are sorted sets, deduplicated but can be sorted. When writing in, a score is given and it is automatically sorted according to the score.

```bash
zadd board 85 zhangsan
zadd board 72 lisi
zadd board 96 wangwu
zadd board 63 zhaoliu

# Get the top three users (the default is ascending order, so you need to change rev to descending order)
zrevrange board 0 3

# Get the ranking of a user
zrank board zhaoliu
```