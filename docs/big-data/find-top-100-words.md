## How to find high-frequency words from a large amount of data?

### Title description

There is a 1GB file, each line in the file is a word, the size of each word does not exceed 16B, the memory size limit is 1MB, and the 100 words with the highest frequency (Top 100) are required to be returned.

### Solution ideas

Due to memory limitations, we still cannot directly read all the words of a large file into the memory at once. Therefore, you can also use the **divide and conquer strategy** to decompose a large file into multiple small files to ensure that the size of each file is less than 1MB, and then directly read a single small file into the memory for processing.

**The idea is as follows**:

First traverse the large file, execute `hash(x)% 5000` for each word x traversed, and store the word with the result i in the file a<sub>i</sub>. After the traversal is over, we can get 5000 small files. The size of each small file is about 200KB. If the size of some small files still exceeds 1MB, continue the decomposition in the same way.

Then count the 100 most frequent words in each small file. The easiest way is to use HashMap to achieve. The key is the word, and the value is the frequency of the word. The specific method is: for the traversed word x, if it does not exist in the map, execute `map.put(x, 1)`; if it exists, execute `map.put(x, map.get(x)+ 1)`, add 1 to the word frequency.

Above we have counted the frequency of occurrence of each small file word. Next, we can find the 100 most frequent words among all words by maintaining a **small top heap**. The specific method is to traverse each small file in turn to construct a small top heap with a heap size of 100. If the number of occurrences of the traversed word is greater than the number of occurrences of the top word, replace the word at the top of the heap with the new word, and then readjust to **small top heap**. After the traversal is over, the word on the small top heap will appear The 100 most frequent words.

### Method summary

1. Divide and conquer, and take the remainder of the hash;
2. Use HashMap to count the frequency;
3. To solve the **largest** TopN, use **small top heap**; to solve the **smallest** TopN, use **large top heap**.