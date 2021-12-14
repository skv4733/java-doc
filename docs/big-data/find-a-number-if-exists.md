## How to judge whether a number exists in a large amount of data?

### Title description

Given 4 billion unsigned unsigned int integers that are not repeated and unordered, and then a number is given, how to quickly determine whether this number is among the 4 billion integers?

### Solution ideas

#### Method One: Divide and Conquer

Divide and conquer can still be used to solve the problem. The method is similar to the previous one, so I won’t repeat it again.

#### Method 2: Bitmap method

Since the range of unsigned int numbers is `[0, 1 << 32)`, we use `1<<32=4,294,967,296` bits to represent each number. The initial bits are all 0, so a total of memory is required: 4,294,967,296b≈512M.

We read these 4 billion integers and set the corresponding bit to 1. Then read the number to be queried and check whether the corresponding bit is 1, if it is 1, it means it exists, if it is 0 it means it does not exist.

### Method summary

**The problem of judging whether a number exists and whether a number is repeated**, the bitmap method is a very efficient method.