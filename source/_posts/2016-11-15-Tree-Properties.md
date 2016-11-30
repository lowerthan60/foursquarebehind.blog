---
title: Tree Properties
tags:
- Algorithm
- Tree
---


* **Depth**: The number of lines you pass through when you travel from the root until you reach a particular node is the depth of that node in the tree, or is the number of edges from the node to the tree's root node. A root node will have a depth of 0.
* **Height**: The height of the tree is the maximum depth of any node in the tree, or is the number of edges on the longest path from the node to a leaf. A leaf node will have a height of 0.
* **Degree**: The number of children emanating from a given node is referred to as its degree.
* **Diameter (or width)** of a tree is the number of nodes on the longest path between any two leaf nodes. 

E.g. In Figure 1, the tree has a height of 3, node **G** has a depth of 2, node **A** has a degree of 3 and node **H** has a degree of 1. In Figure 2, the tree has a **diameter** of 6 nodes.

| &nbsp; | &nbsp; |
| :---   | ---:   | 
| {% asset_img tree_properties_1.jpg "Figure 1" "A tree "%} | {% asset_img tree_height_depth.png "Figure 2" "Tree height and depth "%} |


-------------------------------------------------------------------------------------------------------------------------------------------------------------------

In Binary Tree, **Inorder Successor** of a node is the next node in Inorder traversal of the Binary Tree. **Inorder Successor** is NULL for the last node in Inoorder traversal. 
In Binary Search Tree, **Inorder Successor** of an input node can also be defined as the node with the smallest key greater than the key of input node.


-------------------------------------------------------------------------------------------------------------------------------------------------------------------


| **Q**        | **How can you find successors and predecessors in a binary search tree in order?** |
| :---         | :--- 																			  |
| **Case 1**   | **The node has a right subtree**                                                   |
| &nbsp;       | _If the given node has a right subtree then by the BST property the next larger key must be in the right subtree._ |
| &nbsp;       | _Since all keys in a right subtree are larger than the key of the given node, the successor must be the smallest of all those keys in the right subtree._ |
| **Case 2**   | **The node does not have a right subtree**                                         |
| &nbsp;       | _In this case we will have to look up the tree since that's the only place we might find the next larger key._ | 
| &nbsp;       | _There is no point looking at the left subtree as all keys in the left subtree are guaranteed to be smaller than the key in the given tree._ |
| &nbsp;       | _When we look up from the given node, there can be two cases:_                     |
| **Case 2.1** | _The current node is the left child of its parent. In this case the parent is the successor node. This is because the parent always comes next in inorder traversal if you are done with left subtree (rooted at the current node)._ |
| **Case 2.2** | _The current node is the right child of the parent. This is an interesting case. In this case, as you keep going up the ancestor chain you encounter smaller values if you are going up but larger values if you are going right. The successor node will be the first node up the ancestor chain that you encounter on the right chain._ |

##### References ######
* http://stackoverflow.com/questions/2603692/what-is-the-difference-between-tree-depth-and-height
* https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-binary-search-and-red-black-trees/