## How to count the number of different phone numbers?

### Title description

It is known that a certain file contains some telephone numbers, each number is 8 digits, and the number of different numbers is counted.

### Solution ideas

The essence of this question is to solve the problem of **data duplication**. For this kind of problem, the bitmap method is generally considered first.

For this question, the number of numbers that can be represented by an 8-digit phone number is 10<sup>8</sup>, that is, 100 million. We use a bit to represent each number, and a total of 100 million bits are needed, and the memory takes up about 100M.

**The idea is as follows**:

Apply for a bitmap array with a length of 100 million and initialize it to 0. Then traverse all the phone numbers and set the position in the bitmap corresponding to the number to 1. After the traversal is completed, if the bit is 1, it means that the phone number exists in the file, otherwise it does not exist. The number with a bit value of 1 is the number of different phone numbers.

### Method summary

To solve the data duplication problem, remember to consider the bitmap method.