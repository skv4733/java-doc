## How to find the IP that visits Baidu website the most on a certain day?

### Title description

Existing massive log data is stored in a very large file, which cannot be directly read into the memory, and it is required to extract the IP that has the most access to Baidu on a certain day.

### Solution ideas

This question only cares about the IP that visits Baidu the most on a certain day. Therefore, you can first traverse the file and record the relevant information about accessing Baidu's IP on that day in a single large file. The next method is the same as the previous question, which is roughly to hash the IP first, then use the HashMap to count the number of repeated IPs, and finally calculate the IP with the most repeated times.

> Note: You only need to find out the IP with the most occurrences, you don't need to use the heap, just use a variable max.

### Method summary

1. Divide and conquer, and take the remainder of the hash;
2. Use HashMap to count the frequency;
3. To solve the **largest** TopN, use **small top heap**; to solve the **smallest** TopN, use **large top heap**.