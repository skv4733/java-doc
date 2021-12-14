## How to find unique integers in a large amount of data?

### Title description

Find non-repeating integers among 250 million integers. Note: There is not enough memory to hold these 250 million integers.

### Solution ideas

#### Method One: Divide and Conquer

Similar to the previous topic method, first divide 250 million numbers into multiple small files, use HashSet/HashMap to find the integers that are not repeated in each small file, and then merge each sub-result to get the final result.

#### Method 2: Bitmap method

**Bitmap**, is to use one or more bits to mark the value corresponding to an element, and the key is the element. Using bits as a unit to store data can greatly save storage space.

Bitmaps use bit arrays to indicate whether certain elements exist. It can be used for quick search, heavy judging, sorting, etc. not very clear? Let me give a small example first.

Suppose we want to sort the 5 elements (6, 4, 2, 1, 5) in `[0,7]`, we can use the bitmap method. There are a total of 8 numbers in the range of 0~7, and only 8 bits are needed, that is, 1 byte. First set each bit to 0:

```
0 0 0 0 0 0 0 0
```

Then traverse 5 elements, first encounter 6, then set the 0 of the bit with subscript 6 to 1; then when encounter 4, set the 0 of the bit with subscript 4 to 1:

```
0 0 0 0 1 0 1 0
```

Traverse in turn, after the end, the bit array looks like this:

```
0 1 1 0 1 1 1 0
```

For each bit that is 1, its subscript represents a number:

```
for i in range(8):
    if bits[i] == 1:
        print(i)
```

In this way, we have actually achieved sorting.

For solving integer-related algorithms, **bitmap method** is a very practical algorithm. Assuming that the int integer occupies 4B, which is 32bit, then the number of integers we can represent is 2<sup>32</sup>.

**Then for this question**, we use 2 bits to represent the status of each number:

-00 means this number has never appeared;
-01 means that this number has appeared once (that is, the unique integer found in the question);
-10 means that this number appears multiple times.

Then these 2<sup>32</sup> integers, the total required memory is 2<sup>32</sup>\*2b=1GB. Therefore, when the available memory exceeds 1GB, the bitmap method can be used. Assuming that the memory meets the requirements of the bitmap method, perform the following operations:

Traverse 250 million integers and check the corresponding bits in the bitmap. If it is 00, it becomes 01, if it is 01, it becomes 10, and if it is 10, it remains unchanged. After the traversal is over, check the bitmap and output the integer whose corresponding bit is 01.

Of course, it is specifically stated in this question: **Memory is not enough to hold these 250 million integers**. The memory size of 250 million integers is: 2.5e8/1024/1024/1024=0.93G, which means that the memory is not enough to 1G. The memory size required by the bitmap method is 1G, so this problem is not suitable for solving with the bitmap method.

### Method summary

**The problem of judging whether a number is repeated**, the bitmap method is a very efficient method, of course, the premise is: the memory must meet the storage space required by the bitmap method.