## How to query the most popular query string?

### Title description

The search engine will record all the query strings used by the user each time through the log file, and the length of each query string does not exceed 255 bytes.

Suppose there are currently 1000w records (the repetition of these query strings is relatively high, although the total is 1000w, but after removing the duplication, it will not exceed 300w). Please count the most popular 10 query strings, and the required memory cannot exceed 1G. (The higher the repetition of a query string, the more users who query it, and the more popular it is.)

### Solution ideas

The maximum length of each query string is 255B, and 1000w strings require about 2.55G of memory. Therefore, we cannot read all the strings into the memory for processing.

#### Method One: Divide and Conquer

Divide and conquer is still a very practical method.

Divide into multiple small files to ensure that the strings in a single small file can be directly loaded into the memory for processing, and then find the 10 most frequent strings in each file; finally, count all files through a small top heap The 10 character strings that appear most in the.

The method is feasible, but not the best, other methods are described below.

#### Method 2: HashMap method

Although the total number of strings is relatively large, it does not exceed 300w after deduplication. Therefore, you can consider saving all strings and the number of occurrences in a HashMap, and the space occupied is 300w\*(255+4)≈777M (wherein, 4 means 4 bytes occupied by an integer). It can be seen that 1G of memory space is completely sufficient.

**The idea is as follows**:

First, traverse the string. If it is not in the map, store it directly into the map, and the value is recorded as 1. If it is in the map, add 1 to the corresponding value. The time complexity of this step is O(N).

Then traverse the map to build a small top heap of 10 elements. If the number of occurrences of the traversed string is greater than the number of occurrences of the top string, replace it and adjust the heap to a small top heap.

After the traversal is over, the 10 strings in the heap are the strings with the most occurrences. The time complexity of this step is O(Nlog10).

#### Method Three: Prefix Tree Method

Method two uses HashMap to count the number of times. When these strings have a large number of the same prefix, you can consider using a prefix tree to count the number of occurrences of the string. The node of the tree stores the number of occurrences of the string, and 0 means no occurrence.

**The idea is as follows**:

When traversing the string, search in the prefix tree, if found, add 1 to the number of strings stored in the node, otherwise build a new node for this string, and after the construction is completed, the occurrence of the string in the leaf node The number of times is set to 1.

Finally, the small top heap is still used to sort the number of occurrences of the string.

### Method summary

Prefix trees are often used to count the number of occurrences of strings. Another big use of it is string search, to determine whether there are duplicate strings, etc.