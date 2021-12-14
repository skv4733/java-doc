## How to find the top 500 numbers?

### Title description

There are 20 arrays, each array has 500 elements, and they are arranged in order. How to find the top 500 among these 20\*500 numbers?

### Solution ideas

For TopK problems, the most common method is to use heap sort. For this question, assuming that the array is arranged in descending order, the following methods can be used:

First, create a large top heap, the size of the heap is the number of arrays, which is 20, and store the largest value of each array in the heap.

Then delete the top element of the heap, save it to another array of size 500, and then insert the next element of the array where the deleted element is located into the large top heap.

Repeat the above steps until the 500th element has been deleted, that is, the largest first 500 numbers have been found.

> In order to get a piece of data from the heap, you can know from which array it was taken, so that you can get a value from this array, you can store the pointer of the array in the heap, and provide a way to compare the size of this pointer .

```java
import lombok.Data;

import java.util.Arrays;
import java.util.PriorityQueue;

/**
 * @author https://github.com/yanglbme
 */
@Data
public class DataWithSource implements Comparable<DataWithSource> {
    /**
     * Value
     */
    private int value;

    /**
     * Array for recording the source of the value
     */
    private int source;

    /**
     * Record the index of the value in the array
     */
    private int index;

    public DataWithSource(int value, int source, int index) {
        this.value = value;
        this.source = source;
        this.index = index;
    }

    /**
     *
     * Since PriorityQueue is implemented using a small top heap, here is modified
     * The comparison logic of two integers makes PriorityQueue a big top heap
     */
    @Override
    public int compareTo(DataWithSource o) {
        return Integer.compare(o.getValue(), this.value);
    }
}

class Test {
    public static int[] getTop(int[][] data) {
        int rowSize = data.length;
        int columnSize = data[0].length;

        // Create an array of columnSize size and store the result
        int[] result = new int[columnSize];

        PriorityQueue<DataWithSource> maxHeap = new PriorityQueue<>();
        for (int i = 0; i <rowSize; ++i) {
            // Put the largest element of each array into the heap
            DataWithSource d = new DataWithSource(data[i][0], i, 0);
            maxHeap.add(d);
        }

        int num = 0;
        while (num <columnSize) {
            // Delete the top element of the heap
            DataWithSource d = maxHeap.poll();
            result[num++] = d.getValue();
            if (num >= columnSize) {
                break;
            }

            d.setValue(data[d.getSource()][d.getIndex() + 1]);
            d.setIndex(d.getIndex() + 1);
            maxHeap.add(d);
        }
        return result;

    }

    public static void main(String[] args) {
        int[][] data = {
                {29, 17, 14, 2, 1},
                {19, 17, 16, 15, 6},
                {30, 25, 20, 14, 5},
        };

        int[] top = getTop(data);
        System.out.println(Arrays.toString(top)); // [30, 29, 25, 20, 19]
    }
}
```

### Method summary

For TopK, why not consider heap sort?