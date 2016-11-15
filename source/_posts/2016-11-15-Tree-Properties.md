---
title: Tree Properties
tags:
	Algorithm
	Tree
---


* **Depth**: The number of lines you pass through when you travel from the root until you reach a particular node is the depth of that node in the tree, or is the number of edges from the node to the tree's root node. A root node will have a depth of 0.
* **Height**: The height of the tree is the maximum depth of any node in the tree, or is the number of edges on the longest path from the node to a leaf. A leaf node will have a height of 0.
* **Degree**: The number of children emanating from a given node is referred to as its degree.
* **Diameter (or width)** of a tree is the number of nodes on the longest path between any two leaf nodes. 

E.g. In Figure 1, the tree has a height of 3, node **G** has a depth of 2, node **A** has a degree of 3 and node **H** has a degree of 1. In Figure 2, the tree has a **diameter** of 6 nodes.

| &nbsp; | &nbsp; |
| :---   | ---:   | 
| {% asset_img tree_properties_1.jpg " " " "%} | {% asset_img tree_height_depth.png " " "Figure 2"%} |


Refrences:
* http://stackoverflow.com/questions/2603692/what-is-the-difference-between-tree-depth-and-height
* https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-binary-search-and-red-black-trees/