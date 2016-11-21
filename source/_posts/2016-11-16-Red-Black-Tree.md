---
title: Red-Black Tree
date: 2016-11-16 16:04:28
tags:
- Algorithm
- Tree
---

### Preface ###

##### BST(Binary Search Tree) Retrieval #####

Retrieving an element from **BST** requires simple navigation, starting from the root and going left, if the current node is larger than the node we are looking for, or going right otherwise.

**Any of these primitive operations on BST run in $O(h)$ time, where $h$ is the tree height, so the smaller the tree height the better running time operations will achieve. Which means the time complexity of BST Retrieval is related to the height of the BST!**

The problem with **BST** is that, depending on the order of inserting elements in the tree, the tree shape can vary. In the worst cases (such as inserting elements in order) the tree will look like a **linked list** in which each node has only a right child. This yields $O(n)$ for primitive operations on the **BST**, with $n$ the number of nodes in the tree, in the other words, the **BST** is turned to an **ordered linked list**.

To solve this problem many variations of **BST** exist. Of these variations, **Red-Black tree provides a well-balanced **BST** that guarantees a logarithmic bound on primitive operations**.  

-----------------------------------------------------------------------------------------------------------------------------------------------------
### Introduction ###
**Red-Black Tree**, an evolution of **BST** that aim to **keep the tree balanced without affecting the complexity of the primitive operations**. This is done by coloring each node in the tree with either red or black and preserving a set of properties that guarantee that **the deepest path in the tree is not longer than twice the shortest one**.

### Properties ###
1. Every node is colored with either red or black;
2. All leaf (NULL Pointer) nodes are colored with black; 
	**Note:** _If a node’s child is missing then we will assume that it has a nil child(NULL Pointer) in that place and this nil child(NULL Pointer) is always colored black._
3. Every path from a node $x$ to a descendent leaf has the same number of black nodes (**_not counting node $x$_**). We call this number the **black height** of node $x$, which is denoted by $bh(x)$;
4. Both children of a red node must be black nodes;
	**Note:** _So it's impossible to have 2 consecutive reds on a path, so it ensures that the deepest path in the tree is not longer than twice the shortest one. Thus, the **Red-Black Tree** is a relatively balanced binary tree._
5. The root is always black.


{% asset_img rbt_1.jpg "Figure 1 " "Red-Black Tree" %}



-----------------------------------------------------------------------------------------------------------------------------------------------------
### Application of Red-Black Tree ###
**Red-Black Tree** is used widely, it is mainly used to store sorted/ordered data, its time complexity is $O(log_2(n))$ which has high efficiency. 
For example, the Java collection classes **TreeSet** and **TreeMap** , the C++ STL classes **set**, **map**, and Linux virtual memory management, all of these are based on **Red-Black Tree**.

-----------------------------------------------------------------------------------------------------------------------------------------------------
| ** Basis **   | &nbsp;                                            |
| :---          | :---                                              |
| $h$           | Height of the tree;                               |
| $h(x)$        | Height of the node $x$;                           |
| $bh$          | Black height of the tree;                         |
| $bh(x)$       | Black height of the node $x$;                     |
| $n$           | Number of nodes in the tree;                      |
| $n(x)$        | Number of the nodes of the node $x$;              |
If tree height is $h$, then its $bh >= h/2$; (**_Why?_** -- _According to the 4<sup>th</sup> property above as each red node strictly requires black children_)

-----------------------------------------------------------------------------------------------------------------------------------------------------
### Theorem ### 
A **Red-Black Tree** with $n$ internal nodes has height $h<=2 * log_2(n + 1)$, which means the time complexity is $O(log_2(n))$


### Proof by induction ###

**Proof:** A __Red-Black Tree__ with $n$ internal nodes has height $h<=2 * log_2(n + 1)$. 
The __contrapositive__ is "the height $h$ of the __Red-Black Tree__ has at least $2^{h/2} – 1$ internal nodes". That is $n >= 2^{h/2} - 1$.
So to proof the original __theorem(or the proposition)__ is true, we only need to prove whether the __contrapositive__ is true, which means just to prove "the height $h$ of the __Red-Black Tree__ has at least $2^{h/2} – 1$ internal nodes". That is $n >= 2^{h/2} - 1$.

Starting from a node $x$ (not including the node) to reach a leaf node of any path, the number of black nodes is __black height__ of the node $x$, or $x$'s __black height__, that is $bh(x)$. There are two points as below regarding the $bh(x)$:

**Point 1**: According to the 3<sup>rd</sup> **Red-Black Tree** properties "Every path from a node $x$ to a descendent leaf has the same number of black nodes (**_not counting node $x$_**)", so the $bh(x)$ is unique of the node $x$!
**Point 2**: According to the 4<sup>th</sup> **Red-Black Tree** properties, "Both children of a red node must be black nodes", we can say starting from the node x to leaf node, the number of black nodes >= the number the red nodes, which means $bh(x) >= h(x)/2$. Assuming that $x$ is the root node, we can come to the conclusion that $bh >= h/2$. 

Thus, according to the above two points, we conclude that to proof $n >= 2^{h/2} - 1$, we just need to proof $n >= 2^{bh} - 1$, which means the **Red-Black Tree** with height $h$ should contain at least ($2^{bh} - 1$) internal nodes.

So far, we turned the theorem needs to be proofed 
"A __Red-Black Tree__ with $n$ internal nodes has height $h<=2 * log_2(n + 1)$"
to
"A __Red-Black Tree__ with height $h$ contains at least ($2^{bh} - 1$) internal nodes, that is $n >= 2^{bh} - 1$".
	
	


-----------------------------------------------------------------------------------------------------------------------------------------------------
**Base case**: $h(x) = 0$, which means that **x** is a leaft node and therefore $bh(x) = 0$ and the subtree rooted at node **x** has $2^{bh(x)} -1 = 2^0 - 1 = 1 - 1 = 0$ nodes.

**Induction step**:
1. $x$ has positive height and 2 children, that is $h(x) > 0, so each of its child has __black height__ of $bh(x)$ or ($bh(x) - 1$); 
2. The height of a child $= h(x) - 1$, so the subtrees rooted at each child contain at least ($2^{bh(x) - 1} -1$) internal nodes;   
3. Thus subtree at node $x$ contains $(2^{bh(x) - 1}) + (2^{bh(x) - 1}) + 1 = 2 * 2^{bh(x) - 1} - 1 = 2^{bh(x)} - 1$ nodes, that is $n(x) >= 2^{bh(x)} - 1$.
4. So consider $x$ as the root node, then $n >= 2^{bh} - 1$.

| ** Thus **     | &nbsp; | &nbsp;               | &nbsp; |
| :---           | :---   | :---                 | :---   |
| $n$            | >=     | $2^{bh} - 1$         | &nbsp; |
| $n$            | >=     | $2^{h/2} - 1$        | &nbsp; |
| $log_2(n + 1)$ | >=     | $h/2$                | &nbsp; |
| $h$            | <=     | $2 * log_2(n + 1)$   | &nbsp; |

**Conclusion**: 
A **Red-Black Tree** with $n$ internal nodes has height $h<=2 * log_2(n + 1)$, which means the time complexity is $O(log_2(n))$

**Corollary**:
All operations of a **Red-Black Tree** take $O(log_2(n))$ time complexity, e.g. Minimum(), Maximum(), Successor(), Predecessor(), Search();
Insert() and Delete() will also take $O(log_2(n))$ time complexity, but will need special care since they modify the tree;


-----------------------------------------------------------------------------------------------------------------------------------------------------
### Structure ###
Every node of **Red-Black Tree** has $5$ attributes:

```c
struct t_red_black_node {
    enum { 
        red, black 
    } color;
    void *key;
    struct t_red_black_node *left;
    struct t_red_black_node *right;
    struct t_red_black_node *parent;
	void *value;
	int numLeft;      //optional
	int numRight;     //optional
}
```

A **Red-Black Tree** node has 6 mandatory instance variables, which are color，key，left，right，parent and value. For the pointers left, right and parent, these values are assigned to nil when a node is instantiated. 
Of course we can add more variables like the count of its children nodes according to the requirements.


-----------------------------------------------------------------------------------------------------------------------------------------------------
### Operations and Rotations ###
How does **inserting** or **deleting** nodes affect a **Red-Black Tree**? To ensure that its color scheme and properties don’t get thrown off, we can recolor or rotate the tree, that is modify the color or the structure of the corresponding nodes, to ensure after the tree modifying operations like **inserting** or **deleting**, the **Red-Black Tree** is continue to keep the its properties and balance.

**Rotation** is a binary operation, between a parent node and one of its children, that swaps nodes and modifies their pointers while preserving the inorder traversal of the tree (so that elements are still sorted).

There are two types of rotations: **left rotation** and **right rotation**. 
**Left rotation** swaps the parent node with its right child, 
**Right rotation** swaps the parent node with its left child. 

| **Left rotation**                                           | &nbsp; |**Right rotation**                                            |
| :---:                                                       | ---    | :---:                                                        |
| {% asset_img rbt_left_rotation_1.jpg " " "Left rotation" %} | &nbsp; |{% asset_img rbt_right_rotation_1.jpg " " "Right rotation" %} |


Here are the steps involved in for left rotation (for right rotations just change “left” to “right” below):

When do the left rotation on a **pivot**, assume its right child is not **NIL[T]**. **pivot** is a left child of any nodes but not **NIL[T]**
**Left rotation** is based on the axis between the **pivot** node and **Y** node, the steps are:
1. Let node **Y** be the parent of the **pivot** node;
2. Let the **pivot** node be the left child of node **Y**;
3. Let the left child of node **Y** be the rigth child of the **pivot** node;

The pesudo code for **left rotation** is below(use **X** as the **pivot** node which is mentioned above):

```
LEFT-ROTATE(T, X)  
 Y ← right[X]            
 right[X] ← left[Y]      // Turn Y's left subtree into X's right subtree;
 p[left[Y]] ← X          // Set X as the parent of Y's left child;
 p[Y] ← p[X]             // Link X's parent to Y;
 if p[X] = nil[T]        // Means X is root, so its parent is nil
 then root[T] ← Y        // Case 1： first see whether we're at the root ;
 else if X = left[p[X]]  
 then left[p[X]] ← Y     // Case 2： X was on the left of its parent;
 else right[p[X]] ← Y    // Case 3: X must have been on the right, so set Y as the right child of X's parent;
 left[Y] ← X             // Set X as Y's left child;
 p[X] ← Y                // Set Y as X's parent;
```


The pesudo code for **right rotation** is below(use **X** as the **pivot** node which is mentioned above):

```
RIGHT-ROTATE(T, X)  
 Y ← left[X]            
 left[X] ← right[Y]      // * The differences aginst LEFT-ROTATE
 p[right[Y]] ← X         // * The differences aginst LEFT-ROTATE
 p[Y] ← p[X]             
 if p[X] = nil[T]        
 then root[T] ← Y        
 else if X = left[p[X]]  
 then left[p[X]] ← Y     
 else right[p[X]] ← Y    
 right[Y] ← X            // * The differences aginst LEFT-ROTATE 
 p[X] ← Y                
```
