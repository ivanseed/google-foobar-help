# Ion Flux Relabeling

### The challenge specification
Oh no! Commander Lambda's latest experiment to improve the efficiency of her LAMBCHOP doomsday device has backfired spectacularly. She had been improving the structure of the ion flux converter tree, but something went terribly wrong and the flux chains exploded. Some of the ion flux converters survived the explosion intact, but others had their position labels blasted off. She's having her henchmen rebuild the ion flux converter tree by hand, but you think you can do it much more quickly - quickly enough, perhaps, to earn a promotion!

Flux chains require perfect binary trees, so Lambda's design arranged the ion flux converters to form one. To label them, she performed a post-order traversal of the tree of converters and labeled each converter with the order of that converter in the traversal, starting at 1. For example, a tree of 7 converters would look like the following:

   7
 3   6
1 2 4 5

Write a function answer(h, q) - where h is the height of the perfect tree of converters and q is a list of positive integers representing different flux converters - which returns a list of integers p where each element in p is the label of the converter that sits on top of the respective converter in q, or -1 if there is no such converter.  For example, answer(3, [1, 4, 7]) would return the converters above the converters at indexes 1, 4, and 7 in a perfect binary tree of height 3, which is [3, 6, -1].

The domain of the integer h is 1 <= h <= 30, where h = 1 represents a perfect binary tree containing only the root, h = 2 represents a perfect binary tree with the root and two leaf nodes, h = 3 represents a perfect binary tree with the root, two internal nodes and four leaf nodes (like the example above), and so forth.  The lists q and p contain at least one but no more than 10000 distinct integers, all of which will be between 1 and 2^h-1, inclusive.

### Test cases

`1`  
Inputs:  

(int) h = 3  
(int list) q = [7, 3, 5, 1]  

Output:  

(int list) [-1, 7, 6, 3]  

`2`  
Inputs:  

(int) h = 5
(int list) q = [19, 14, 28]  

Output:  

(int list) [21, 15, 29]

### Understanding
The challenge asks to you return the parent of a node in a perfect binary tree whose nodes are labelled in postfix order. This is quite a simple challenge once you realize that you don't have to create entire tree in memory to find parent of a node.

Also a node with label **n** will always(for a tree of any height) have same parent given that it is not the root node and it exists in the tree of given height.

### A Solution
1. If I can find out the height of the node and whether the node is left child or right child, I can easily find the parent of the node.
2. For left children, `parent = node_label + 2**node_height`.
3. For right children, `parent = node_label + 1`.
4. If height on node matches height of tree then return -1.

**Calculating Height**  
1. For any given tree of `height = h`, the root will be `2^h -1`.
2. Thus if `node + 1` is perfect power of 2 then we can find out the height easily. (No need to take log base 2, just see the number of bits after converting to binary).
3. If its not perfect power of 2, then calculate `x = 2^i - 1`. Choose `i = floor(log_base_2(node))`.
4. Subtract `x` from `node`.
5. Repeat *step 3 and 4* until `node + 1` becomes perfect power of 2.
6. Once `node + 1` is perfect power of 2, calculate `height = log_base_2(node + 1)`.

**Left or Right Child**
1. If `node + 1` was already a perfect power of 2, then it was left child.
2. Just before becoming a perfect power of 2, if node was equal to `2 * x`, then it was right child.
3. Otherwise it was left child.

### Summary

In summary it is a fairly straight forward challenge, steps include:

* Calculate height of the node and whether it is left or right child.

* For left child `parent = node_label + 2**node_height`.

* For right child `parent = node_label + 1`.

* When height matches tree height then return -1.

Some tips:

* To calculate `floor(log_base_2(node))` just find out the number of bits in binary and subtract 1 from it.  
eg in python: `floor(log_base_2(node)) = node.bit_length() - 1`
