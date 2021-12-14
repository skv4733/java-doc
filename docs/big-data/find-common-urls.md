## How to find the same URL from a large number of URLs?

### Title description

Given two files a and b, each stores 5 billion URLs, each URL occupies 64B, and the memory limit is 4G. Please find the common URL of the two files a and b.

### Solution ideas

#### 1. Divide and conquer strategy

Each URL occupies 64B, so the space occupied by 5 billion URLs is about 320GB.

> 5, 000, 000, 000 _ 64B ≈ 5GB _ 64 = 320GB

Since the memory size is only 4G, it is impossible for us to load all URLs into the memory at once for processing. For this type of topic, a **divide and conquer strategy** is generally adopted, that is: the URL in a file is divided into multiple small files according to a certain feature, so that the size of each small file does not exceed 4G, so that you can This small file is read into memory for processing.

**The idea is as follows**:

First traverse file a, ask for `hash(URL)% 1000` for the traversed URL, and store the traversed URL in a<sub>0</sub>, a<sub>1</sub>, according to the calculation result, a<sub>2</sub>, ..., a<sub>999</sub>, so each is about 300MB in size. Use the same method to traverse file b, and store the URLs in file b into files b<sub>0</sub>, b<sub>1</sub>, b<sub>2</sub>, .. ., b<sub>999</sub>. After this process, all URLs that may be the same are in the corresponding small files, that is, a<sub>0</sub> corresponds to b<sub>0</sub>, ..., a<sub>999</sub > Corresponding to b<sub>999</sub>, small files that do not correspond cannot have the same URL. So next, we only need to ask for the same URL in these 1000 pairs of small files.

Then iterate over a<sub>i</sub>( `i∈[0,999]`) and store the URL in a HashSet collection. Then traverse each URL in b<sub>i</sub> to see if it exists in the HashSet collection. If it exists, it means that this is a common URL. You can save this URL in a separate file.

#### 2. Prefix tree

Generally speaking, the length of the URL will not be small, and the first few characters are mostly the same. In this case, it is very suitable to use the trie tree (trie tree) data structure for storage, which reduces storage costs and improves query efficiency.

> Feedback from [@ChunelFeng](https://github.com/ChunelFeng). [#212](https://github.com/doocs/advanced-java/issues/212)

### Method summary

#### Divide and Conquer Strategy

1. Divide and conquer, and take the remainder of the hash;
1. Perform HashSet statistics on each sub-file.

#### Prefix tree

1. Use the common prefix of strings to reduce storage costs and improve query efficiency.