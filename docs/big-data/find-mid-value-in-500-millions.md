## How to find the median from 500 million numbers?

### Title description

Find the median from 500 million numbers. After the data is sorted, the number in the middle is the median. When the number of samples is odd, the median is `(N+1)/2`th; when the number of samples is even, the median is `N/2`th and `1+N/ 2` The mean value of the number.

### Solution ideas

If there is no memory size limit for this question, you can read all the numbers into the memory and sort them to find the median. But the time complexity of the best sorting algorithms is O(NlogN). Other methods are used here.

#### Method 1: Double-stack method

Maintain two piles, a large top pile and a small top pile. The largest number in the big top heap ** is less than or equal to the smallest number in the small top heap; ensure that the difference between the number of elements in the two heaps does not exceed 1.

If the total number of data is **even**, when the two piles are built, the **median is the average value of the top elements of the two piles**. When the total number of data is **odd**, according to the size of the two heaps, the **median must be at the top of the heap with more data**.

```java
class MedianFinder {

    private PriorityQueue<Integer> maxHeap;
    private PriorityQueue<Integer> minHeap;

    /** initialize your data structure here. */
    public MedianFinder() {
        maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
        minHeap = new PriorityQueue<>(Integer::compareTo);
    }

    public void addNum(int num) {
        if (maxHeap.isEmpty() || maxHeap.peek()> num) {
            maxHeap.offer(num);
        } else {
            minHeap.offer(num);
        }

        int size1 = maxHeap.size();
        int size2 = minHeap.size();
        if (size1-size2> 1) {
            minHeap.offer(maxHeap.poll());
        } else if (size2-size1> 1) {
            maxHeap.offer(minHeap.poll());
        }
    }

    public double findMedian() {
        int size1 = maxHeap.size();
        int size2 = minHeap.size();

        return size1 == size2
            ? (maxHeap.peek() + minHeap.peek()) * 1.0 / 2
            : (size1> size2? maxHeap.peek(): minHeap.peek());
    }
}
```

> See LeetCode No.295: https://leetcode.com/problems/find-median-from-data-stream/

In the above method, all data needs to be loaded into memory. When the amount of data is large, this cannot be done. Therefore, this method is **applicable to the case of small amounts of data**. 500 million numbers, each digit occupies 4B, a total of 2G memory is required. If the available memory is less than 2G, this method cannot be used. Another method is introduced below.

#### Method 2: Divide and Conquer

The idea of ​​divide and conquer is to gradually transform a large problem into a smaller problem to solve.

For this question, read the 500 million numbers in sequence. For the number num that is read, if the highest bit in the corresponding binary is 1, then write this number to f1, otherwise write it to f0. Through this step, the 500 million numbers can be divided into two parts, and the number in f0 is greater than the number in f1 (the highest bit is the sign bit).

After the division, it is very easy to know whether the median is in f0 or f1. Suppose there are 100 million numbers in f1, then the median must be in f0, and it is the average of the 150 millionth number in f0, from small to large, and the next number after it.

> **Hint**, the median of 500 million is the 250 millionth and the next number to the right to average. If f1 has 100 million numbers, then the median is the average of the two numbers in f0 starting from the 150 millionth number.

For f0, you can continue to divide the file into two with the next highest binary, and then divide it until the divided file can be loaded into the memory. After the data is loaded into the memory, you can sort directly to find the median.

> **Note**, when the total number of data is even, if the data in the two files have the same number after division, then the median is the maximum value in the file with the smaller data and the minimum in the file with the larger data The average value of the value.

### Method summary

Divide and conquer, really fragrant!