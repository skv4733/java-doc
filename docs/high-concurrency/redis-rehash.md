## Interview Questions

Have you understood the process of Redis rehash?

## Interviewer psychoanalysis

This knowledge counts the relatively low-frequency interview sites in redis, but when you introduce the rehash of HashMap or the rehash of ConcurrentHashMap, you can take the initiative to mention to the interviewer that you not only understand these, but also understand the rehash process in Redis.

Redis is known for its fast speed and good performance. We know that Redis has a limited capacity at the beginning. When the capacity is insufficient, it needs to be expanded. What is the way to expand? Will the data be transferred all at once? When the amount of data is tens of millions to hundreds of millions, this will definitely block Redis's execution of commands. Therefore, it is very necessary to understand the rehash process in Redis.

## Analysis of Interview Questions

As we all know, Redis is mainly used to store key-value pairs (`Key-Value Pair`), and the storage of key-value pairs is implemented by a dictionary, and the bottom layer of the dictionary in Redis is implemented by a hash table. Save the key-value pairs in the dictionary through the nodes in the hash table. Similar to the HashMap in Java, the Key is mapped to the node position of the hash table through a hash function.

The data structure of the dictionary in Redis is as follows:

```c
// The data structure corresponding to the dictionary. For the structure of the hash table, please refer to the redis source code, which will not be described again
typedef struct dict {
    dictType *type; // dictionary type
    void *privdata; // private data
    dictht ht[2]; // 2 hash tables, which are also an important data structure for rehashing. From this it can also be seen that the bottom layer of the dictionary is implemented by hash tables.
    long rehashidx; // An important sign of the rehash process, a value of -1 indicates that the rehash has not been performed
    int iterators; // The number of iterators currently being iterated
} dict;
```

When expanding or shrinking the hash table, the program needs to rehash all the key-value pairs contained in the existing hash table into the new hash table. The specific process is as follows:

### 1. Allocate space for the alternate hash table of the dictionary.

If the expansion operation is performed, the size of the backup hash table is 2" (2 to the power of n) of the first hash table that needs to be expanded and the number of key-value pairs *2; [`5*2 =10,`So the capacity of the standby hash table is the first 2" greater than 10, which is 16]

If the shrinking operation is performed, the size of the backup hash table is 2" that is greater than or equal to the number of key-value pairs of the hash table that needs to be expanded (`ht[0] .used`).

### 2. Progressive rehash

The rehash process is not completed at once when the amount of data is very large (tens of millions, billions), but is done gradually **. The advantage of **progressive rehash** is to avoid affecting the server.

The essence of progressive rehash:

1. With the help of rehashidx, the calculation work required for rehash key-value pairs is equally distributed to each addition, deletion, search, and update operation of the dictionary, thereby avoiding the huge amount of calculation caused by centralized rehash.
2. During the rehash process, every time the dictionary is added, deleted, searched or updated, in addition to performing the specified operation, the program will incidentally rehash all the key-value pairs on the rehashidx index of the original hash table to the backup Hash table, when the rehash work is completed, the program adds 1 to the value of the rehashidx attribute.