## How to sort by query frequency?

### Title description

There are 10 files, each of which is 1G in size. Each line of each file stores the user's query, and each file's query may be repeated. It is required to sort by the frequency of query.

### Solution ideas

If the repetition of the query is relatively large, you can consider reading all the queries into the memory for processing at one time; if the repetition rate of the query is not high, then the available memory is not enough to accommodate all the queries, then you need to use the divide and conquer method or other Method to solve.

#### Method 1: HashMap method

If the query repetition rate is high, it means that the total number of different queries is relatively small. You can consider loading all the queries into the HashMap in memory. Then you can sort by the number of occurrences of the query.

#### Method 2: Divide and Conquer

The divide and conquer method needs to determine the scale of the problem based on the amount of data and the size of available memory. For this question, you can sequentially traverse the queries in 10 files, and divide these queries into 10 small files through the Hash function `hash(query)% 10`. After that, HashMap is used to count the number of occurrences of query for each small file, sorted according to the number of times, and written to a separate file outside of zero.

Then sort all files according to the number of queries, and merge sorting can be used here (because it is impossible to read all queries into memory, external sorting is required).

### Method summary

-If there is enough memory, read it directly for sorting;
-If the memory is not enough, divide it into small files first, and sort the small files after sorting them and use the outer sorting to merge them.