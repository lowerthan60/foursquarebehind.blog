---
title: Red-Black Tree
date: 2016-11-16 16:04:28
tags:
- Algorithm
- Tree
---

# Preface #

##### BST(Binary Search Tree) Retrieval #####

Retrieving an element from **BST** requires simple navigation, starting from the root and going left, if the current node is larger than the node we are looking for, or going right otherwise.

**Any of these primitive operations on BST run in $O(h)$ time, where $h$ is the tree height, so the smaller the tree height the better running time operations will achieve. Which means the time complexity of BST Retrieval is related to the height of the BST!**

The problem with **BST** is that, depending on the order of inserting elements in the tree, the tree shape can vary. In the worst cases (such as inserting elements in order) the tree will look like a **linked list** in which each node has only a right child. This yields $O(n)$ for primitive operations on the **BST**, with $n$ the number of nodes in the tree, in the other words, the **BST** is turned to an **ordered linked list**.

To solve this problem many variations of **BST** exist. Of these variations, **Red-Black tree provides a well-balanced **BST** that guarantees a logarithmic bound on primitive operations**.  

-----------------------------------------------------------------------------------------------------------------------------------------------------
# Introduction #
**Red-Black Tree**, an evolution of **BST** that aim to **keep the tree balanced without affecting the complexity of the primitive operations**. This is done by coloring each node in the tree with either **RED** or **BLACK** and preserving a set of properties that guarantee that **the deepest path in the tree is not longer than twice the shortest one**.

<a name="Properties"></a>
### Properties ###
1. Every node is colored with either **RED** or **BLACK**;
2. All leaf (NULL Pointer) nodes are colored with **BLACK**; 
	**Note:** _If a node’s child is missing then we will assume that it has a nil child(NULL Pointer) in that place and this nil child(NULL Pointer) is always colored **BLACK**._
3. Every path from a node $x$ to a descendent leaf has the same number of **BLACK** nodes (**_not counting node $x$_**). We call this number the **Black-Height** of node $x$, which is denoted by $bh(x)$;
4. Both children of a **RED** node must be **BLACK** nodes;
	**Note:** _So it's impossible to have 2 consecutive reds on a path, so it ensures that the deepest path in the tree is not longer than twice the shortest one. Thus, the **Red-Black Tree** is a relatively balanced binary tree._
5. The root is always **BLACK**;


{% asset_img rbt_1.jpg "Figure 1 " "Red-Black Tree" %}



-----------------------------------------------------------------------------------------------------------------------------------------------------
# Application of Red-Black Tree #
**Red-Black Tree** is used widely, it is mainly used to store sorted/ordered data, its time complexity is $O(log_2(n))$ which has high efficiency. 
For example, the Java collection classes **TreeSet** and **TreeMap** , the C++ STL classes **set**, **map**, and Linux virtual memory management, all of these are based on **Red-Black Tree**.

-----------------------------------------------------------------------------------------------------------------------------------------------------
| ** Basis **   | &nbsp;                                            |
| :---          | :---                                              |
| $h$           | Height of the tree;                               |
| $h(x)$        | Height of the node $x$;                           |
| $bh$          | **Black-Height** of the tree;                     |
| $bh(x)$       | **Black-Height** of the node $x$;                 |
| $n$           | Number of nodes in the tree;                      |
| $n(x)$        | Number of the nodes of the node $x$;              |
If tree height is $h$, then its $bh >= h/2$; (**_Why?_** -- _According to the 4<sup>th</sup> property above as each **RED** node strictly requires **BLACK** children_)

-----------------------------------------------------------------------------------------------------------------------------------------------------
# Theorem #
A **Red-Black Tree** with $n$ internal nodes has height $h<=2 * log_2(n + 1)$, which means the time complexity is $O(log_2(n))$


# Proof by induction #

**Proof:** A __Red-Black Tree__ with $n$ internal nodes has height $h<=2 * log_2(n + 1)$. 
The __contrapositive__ is "the height $h$ of the __Red-Black Tree__ has at least $2^{h/2} – 1$ internal nodes". That is $n >= 2^{h/2} - 1$.
So to proof the original __theorem(or the proposition)__ is true, we only need to prove whether the __contrapositive__ is true, which means just to prove "the height $h$ of the __Red-Black Tree__ has at least $2^{h/2} – 1$ internal nodes". That is $n >= 2^{h/2} - 1$.

Starting from a node $x$ (not including the node) to reach a leaf node of any path, the number of **BLACK** nodes is __Black-Height__ of the node $x$, or $x$'s __Black-Height__, that is $bh(x)$. There are two points as below regarding the $bh(x)$:

**Point 1**: According to the 3<sup>rd</sup> **Red-Black Tree** properties "Every path from a node $x$ to a descendent leaf has the same number of **BLACK** nodes (**_not counting node $x$_**)", so the $bh(x)$ is unique of the node $x$!
**Point 2**: According to the 4<sup>th</sup> **Red-Black Tree** properties, "Both children of a **RED** node must be **BLACK** nodes", we can say starting from the node x to leaf node, the number of **BLACK** nodes >= the number the **RED** nodes, which means $bh(x) >= h(x)/2$. Assuming that $x$ is the root node, we can come to the conclusion that $bh >= h/2$. 

Thus, according to the above two points, we conclude that to proof $n >= 2^{h/2} - 1$, we just need to proof $n >= 2^{bh} - 1$, which means the **Red-Black Tree** with height $h$ should contain at least ($2^{bh} - 1$) internal nodes.

So far, we turned the theorem needs to be proofed 
"A __Red-Black Tree__ with $n$ internal nodes has height $h<=2 * log_2(n + 1)$"
to
"A __Red-Black Tree__ with height $h$ contains at least ($2^{bh} - 1$) internal nodes, that is $n >= 2^{bh} - 1$".
	
	


-----------------------------------------------------------------------------------------------------------------------------------------------------
**Base case**: $h(x) = 0$, which means that **x** is a leaft node and therefore $bh(x) = 0$ and the subtree rooted at node **x** has $2^{bh(x)} -1 = 2^0 - 1 = 1 - 1 = 0$ nodes.

**Induction step**:
1. $x$ has positive height and 2 children, that is $h(x) > 0, so each of its child has __Black-Height__ of $bh(x)$ or ($bh(x) - 1$); 
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
# Structure #
Every node of **Red-Black Tree** has $5$ attributes:

```c
struct t_red_black_node {
    enum { 
        RED, BLACK 
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
# Rotations #
How does **inserting** or **deleting** nodes affect a **Red-Black Tree**? To ensure that its color scheme and properties don’t get thrown off, we can recolor or rotate the tree, that is modify the color or the structure of the corresponding nodes, to ensure after the tree modifying operations like **inserting** or **deleting**, the **Red-Black Tree** is continue to keep the its properties and balance.

**Rotation** is a basic operation for changing **Black-Red Tree** structure, it's a binary operation, between a **parent** node and one of its children, that swaps nodes and modifies their pointers while preserving the inorder traversal of the tree (so that elements are still sorted).

There are two types of rotations: **Left Rotation** and **Right Rotation**. 
**Left Rotation** swaps the **parent** node with its right child, 
**Right Rotation** swaps the **parent** node with its left child. 

| **Left Rotation**                                           | &nbsp; |**Right Rotation**                                            |
| :---:                                                       | ---    | :---:                                                        |
| {% asset_img rbt_left_rotation_1.jpg " " "Left Rotation" %} | &nbsp; |{% asset_img rbt_right_rotation_1.jpg " " "Right Rotation" %} |


-----------------------------------------------------------------------------------------------------------------------------------------------------

### Left Rotation ###

Here are the steps involved in for **Left Rotation** (for **Right Rotation** just change "Left" to "Right" below):

When do the **Left Rotation** on a **pivot**, assume its right child is not **NIL[T]**. **pivot** is a left child of any nodes but not **NIL[T]**
**Left Rotation** is based on the axis between the **pivot** node and **y** node, the steps are:
1. Let node **y** be the **parent** of the **pivot** node;
2. Let the **pivot** node be the left child of node **y**;
3. Let the left child of node **y** be the rigth child of the **pivot** node;


The C++ code for **Left Rotation** is below(use **x** as the **pivot** node which is mentioned above):

```C++
void Left_Rotate(Node **T,Node * x) {   // T: tree, x: pivot node
  Node *y = x->right;  
   
  x->right = y->left;                   // Turn y's left subtree into x's right subtree
  if (y->left != T_NIL)                 // If y->left is not null, set x as the parent of y's left child
      y->left->p = x;  
  y->p = x->p;                          // Link x's parent to y
  if(x->p == T_NIL)                     // Means x is root, so its parent is nil
     *T = y;                            // Case 1： first see whether we're at the root, set root as y if we are
  else if (x == x->p->left)             // Case 2： x was on the left of its parent
     x->p->left = y;                    //         set y as the left child of x's parent
  else                                  // Case 3: x must have been on the right
     x->p->right = y;                   //         so set y as the right child of x's parent;
  y->left = x;                          // Set x as y's left child 
  x->p = y;                             // Set y as x's parent;
}  
```

So as the figure below, **Left Rotation** on node **X** means make node **X**'s right child as node **X**'s **parent** node, that is make node **X** as a left child node (node **X** becomes the left child of node **Z**)!
So **Left** means the **Rotated** node will be turned to a left child node.
{% asset_img rbt_left_rotation_example_1.png " " "Left Rotation" %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------

### Right Rotation ###

The C++ code for **Right Rotation** is below(use **y** as the **pivot** node which is mentioned above):

```C++
void Right_Rotate(Node **T,Node *y) {   // T: tree, y: pivot node
  Node *x = y->left;  
   
  y->left = x->right;                  // Turn x's right subtree into y's left subtree
  if (x->right != T_NIL)               // If x->right is not null, set y as the parent of x's right child
      x->right->p = y;  
  x->p = y->p;                         // Link y's parent to x
  if(y->p == T_NIL)                    // Means y is root, so its parent is nil
     *T = x;                           // Case 1： first see whether we're at the root, set root as x if we are
  else if (y == y->p->left)            // Case 2： y was on the left of its parent
     y->p->left = x;                   //         set x as the left child of y's parent
  else                                 // Case 3: y must have been on the right
     y->p->right = x;                  //         so set x as the right child of y's parent
  x->right = y;                        // Set y as x's right child 
  y->p = x;                            // Set x as y's parent
}  
```

So as the figure below, **Reft Rotation** on **x** means make **x**'s left child as **x**'s **parent**, that is make **x** as a right child node (**x** becomes the right child of **y**)!
So **Right** means the **Rotated** node will be turned to a right child node.
{% asset_img rbt_right_rotation_example_1.png " " "Right Rotation" %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------

So **Left Rotation** and **Right Rotation** are a pair of **invertible** operations.
{% asset_img rbt_invertible_rotation.png " " "Invertible Operations between Left/Right Rotation" %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------


# Operations #
### Insertion ###

| **An Example for Insertion**                                                                                                                                                             | &nbsp;                                           |
| :---                                                                                                                                                                                     | ---:                                             |
| **Color this tree**                                                                                                                                                                      | {% asset_img rbt_insertion_1.jpg "Step 1" " " %} | 
| **Insert 8**<br/>Where does it go?<br/>What color shoult it be?                                                                                                                          | {% asset_img rbt_insertion_2.jpg "Step 2" " " %} | 
| **Insert 8**<br/>Color the inserted node as **RED**                                                                                                                                      | {% asset_img rbt_insertion_3.jpg "Step 3" " " %} | 
| **Insert 11**<br/>Where does it go?<br/>What color shoult it be?<br/>Can't be **RED**!(break [property](#Properties) #4)<br/>Can't be **BLACK**!(break [property](#Properties) #3)       | {% asset_img rbt_insertion_4.jpg "Step 4" " " %} | 
| **Insert 11**<br/>Where does it go?<br/>Solution: recolor the tree                                                                                                                       | {% asset_img rbt_insertion_5.jpg "Step 5" " " %} | 
| **Insert 10**<br/>Where does it go?<br/>What color shoult it be?<br/><br/>**Answer**: no color! Tree is too imbalanced!<br/>Must change tree structure to allow recoloring<br/>**Goal**: Restructure tree in $O(log_2(n))$ time                                                                                      | {% asset_img rbt_insertion_6.jpg "Step 6" " " %}      | 


Based on the above example, the **Red-Black Tree Insertion operation** is to do a regular BST insertion, then do the operations (recolor or rotate) to make the new tree satisfy with all of the **Red-Black Tree** [properties](#Properties).
A special case is required for an empty tree. If the tree is empty, replace it with a single **BLACK** node containing the inserted value. This ensures that the root property is satisfied.

So assume we are trying to insert one new node **x** to **T** (a nonempty **Red-Black Tree**)

| **Steps**:  | &nbsp;                                                                                                       |
| :---                  | :---                                                                                                         |
| **1**:           | Use the BST insert algorithm to insert **x** to **T** (**_Note: every insertion take places at a leaf)_**    |
| **2**:           | Color the node **x** **RED**                                                                                 |
| **3**:           | Restore **Red-Black Tree** [properties](#Properties) by **recolor** or **rotate** operations (if necessary)  |


| &nbsp;            | &nbsp;                                                            |
| :---              | :---                                                              |
| **_Question_**:   | At **Step 2**, why color the new inserted node **x** as **RED**?  |
| **_Answer_**:     | Recall the **Red-Black Tree** [properties](#Properties), color the new inserted node **x** as **RED** won't violate the [property](#Properties) #3, so the **Black-Height** will be same as before the insertion |
| &nbsp;            | which means the less cases we have to handle to keep maintaining the **Red-Black Tree** [properties](#Properties)                                                                                                |
  


Back to the step #6 in the example above, after insert **10** into the **Red-Black Tree**, first based on the regular BST insert algorithm to find the proper location, where is the left child of node **11**, then we should color the node **10** as **RED**, so now we get a tree like below:

{% asset_img rbt_insertion_7.jpg "Insert 10 and then color the new inserted node as RED" " " %} 




| **_Properties again_**            | &nbsp; | &nbsp;                                                         |
| :---              | :---:  | :---                                                                           |
| **_Question_**:   | &nbsp; | Which **Red-Black Tree** [properties](#Properties) does the above case break?  |
| 1. Every node is colored with either **RED** or **BLACK**                                  | &nbsp; | We don't break this because the new inserted node is colored as **RED**     | 
| 2. All leaf (NULL Pointer) nodes are colored with **BLACK**                                | &nbsp; | We don't break this because the new inserted node is a nonempty node, it won't impact the leaf nodes which are all nil(NULL Pointer) nodes |
| 3. Every path from a node $x$ to a descendent leaf has the same number of **BLACK** nodes  | &nbsp; | We don't break this because the new inserted node is colored as **RED**, so the count of **BLACK** nodes should keep same as before |
| 4. Both children of a **RED** node must be **BLACK** nodes                                 | &nbsp; | **We might break property #4!!!** |
| 5. The root is always **BLACK**                                                            | &nbsp; | We don't break this because BST insertion will only insert the new node to the leaf nodes, the root node won't be impacted |

So now we need to figure out a way to meet the property #4 in order to reconstruct the new BST to a **Red-Black Tree**

The C++ code for **Red-Black Tree Insertion** is below:

```C++
// Red-Black Tree Insertion
// param : z, the node is going to be inserted 
// return : root node
Node *RB_Insert(Node *Root,Node * z) { 
  Node * y=T_NIL;  
  Node * x=Root;  
  while(x != T_NIL) {               // Find the proper place in T to insert the new node z, y will be the parent node
    y=x;  
    if (z->key < x->key)  
      x = x->left;  
    else  
      x = x->right;  
  }  
  z->p = y;  
  if (y == T_NIL)                   // Case 1: if y is nil, then set z as root
    Root = z;  
  else if (z->key < y->key)         // Case 2: if z < y, then set z as y's left child
    y->left = z;  
  else                              // Case 3: if z >= y, then set z as y's right child
    y->right = z;  
  
  z->left = T_NIL;                  // Set z's left child as nil
  z->right = T_NIL;                 // Set z's right child as nil. So far done with BST insertion for the new node z
  z->color = RED;                   // Color z as RED
  Root = RB_Insert_Fixup(Root,z);   // Reconstruct(recolor or rotate) T via function RB_Insert_Fixup to meet all of the **Red-Black Tree** properties
  return Root;   
}  
```


The C++ code for **Red-Black Tree RB_Insert_Fixup** is below:

```C++
Node* RB_Insert_Fixup(Node *T,Node *z)  {  
  Node * y=NULL;  
  while(z->p->color == RED) {    // z's parent is RED breaks the property #4, so fix it!
    if (z->p == z->p->p->left) { // if z's parent is the left child of z's grampa
      y = z->p->p->right;        // set y as z's uncle (the right child of z's grampa)  
      if (y->color == RED) {     // Case 1: y (z's uncle) is RED
        z->p->color = BLACK;     // Case 1: set the color of z's parent as BLACK
        y->color = BLACK;        // Case 1: set the color of z's uncle as BLACK
        z->p->p->color = RED;    // Case 1: set z's grampa as RED
        z = z->p->p;             // Case 1: Iterate up, point z to z's grampa (the color of z is RED)  
      } else if (z == z->p->right) { // Case 2: z's uncle is BLACK, and z is the right child of z's parent
        z = z->p;                 // Case 2: point z to z's parent
        Left_Rotate(&T,z);        // Case 2: LEFT-ROTATE on z (z is the pivot)
        z->p->color = BLACK;      // Case 2 turns to Case 3: NOW z's uncle node is BLACK, and z is the left child of z's parent! Set z's parent as BLACK
        z->p->p->color = RED;     // Case 3: set z's grampa as RED
        Right_Rotate(&T,z->p->p); // Case 3: RIGHT-ROTATE on z's grampa (z's grampa is the pivot). [while loop is end]
      } else {                   // case 3: z's uncle is BLACK and z is the left child of z's parent
        z->p->color = BLACK;     // Case 3: set z's parent as BLACK
        z->p->p->color = RED;    // Case 3: set z's grampa as RED
        Right_Rotate(&T,z->p->p);// Case 3: RIGHT-ROTATE on z's grampa (z's grampa is the pivot). [while loop is end]
      } 
    } else {                     //Symmetric processing
      y = z->p->p->left;  
      if (y->color == RED) {     // Case 1: z's uncle is RED
        z->p->color = BLACK;  
        y->color = BLACK;  
        z->p->p->color = RED;  
        z = z->p->p;  
  	  } else if (z == z->p->left) { // Case 2: z's uncle is BLACK and z is the left child of z's parent
        z = z->p;  
        Right_Rotate(&T,z);  
        z->p->color = BLACK;  
        z->p->p->color = RED;  
        Left_Rotate(&T,z->p->p);   
      } else {                      // Case 3: z's uncle is BLACK and z is the right child of z's parent
        z->p->color = BLACK;  
        z->p->p->color = RED;  
        Left_Rotate(&T,z->p->p);   
      }
    }  
  }    
  T->color = BLACK;          // make sure don't break the property #5, color the root as BLACK
  return T;  
}  
```

According to **z**'s **parent**'s state, when we insert **z** into a **Red-Black Tree** and then color **z** as **RED**, there will be 3 cases to deal with:

| **Case**                                   | &nbsp;          | &nbsp; |
| :---                                       | :---:           | :---   |
| **1**: **z** is the root node              | &nbsp;---&nbsp; | Color node **z** as **BLACK** |
| **2**: **z**'s **parent** is the root node | &nbsp;---&nbsp; | Do nothing, since **T** is still a **Red-Black Tree** after inserted **z** |
| **3**: **z**'s **parent** is **RED**       | &nbsp;---&nbsp; | This case breaks the [property](#Properties) #4, in this situation, **z** must have **grampa** node, further more|
| &nbsp;                                     | &nbsp;---&nbsp; | **z** must have an **uncle** node (even it's NIL, we will still say it exists and NIL node is **BLACK**). |
| &nbsp;                                     | &nbsp;---&nbsp; | According to **z**'s uncle's state, we will have another **3 sub-cases** as below: |


| **Case** |  &nbsp;                               | &nbsp; |
| :---     | :---                                                    | :---   |
| **3.1**: | **z**'s **parent** is **RED**, **z**'s uncle is **RED** | (01) color **z**'s **parent** as **BLACK**                                       |
| &nbsp;   | &nbsp;                                                  | (02) color **z**'s uncle as **BLACK**                                        |
| &nbsp;   | &nbsp;                                                  | (03) color **z**'s grampa as **RED**                                         |
| &nbsp;   | &nbsp;                                                  | (04) point **z** to **z**'s grampa, then keep the processing iteratively |
| **3.2**: | **z**'s **parent** is **RED**, **z**'s uncle is **BLACK**,      | (01) point **z** to **z**'s **parent**                                       |
| &nbsp;   | and **z** is the **right child** of **z**'s **parent**  | (02) **LEFT-ROTATE** on **z**                                            |
| **3.3**: | **z**'s **parent** is **RED**, **z**'s uncle is **BLACK**,      | (01) color **z**'s **parent** as **BLACK**                                       |
| &nbsp;   | and **z** is the **left child** of **z**'s **parent**   | (02) color **z**'s grampa as **RED**                                         |
| &nbsp;   | &nbsp;                                                  | (03) **RIGHT-ROTATE** on **z**'s grampa                                  |

**_The core idea about the 3 Sub-Cases above is : "Move the RED node to root, then color the root as BLACK"_**


| **Why** |  &nbsp;                                |
| :---    | :---                                   |
| **3.1** | **z** and **z**'s **parent** are **RED**, it breaks [property](#Properties) #4, so color **z**'s **parent** as **BLACK** to resolve this problem. |
| **Q**:  | In this situation, **z** must have a **BLACK grampa**! **Why?** |
| **A**:  | _Check **[property](#Properties) #4 and #5!**_ |
| &nbsp;  | But when recolor **z**'s **parent** from **RED** to **BLACK**, [property](#Properties) #3 is broken, because the **Black-Height** of the subtree where **z**'s **parent** exists increased by 1. |
| **Q**:  | How to resolve this problem? |
| **A**:  | _Recolor **grampa** from **BLACK** to **RED**, and recolor uncle from **RED** to **BLACK**._ |
| **Q**:  | Why this problem got resolved by this way? |
| **A**:  | _Because the **Black-Height** of **z**'s **parent** subtree increased by 1, means the **Black-Height** of **z**'s **grampa** substree increased_ |
| &nbsp;  | _by 1 as well, so recolor **grampa** from **BLACK** to **RED** to resolve the problem that the **Black-Height** of **grampa** subtree increased_ |
| &nbsp;  | _by 1, but it will introduce another problem that the **Black-Height** of **uncle** subtree decreased by 1, because in this case **uncle** is **RED**, so recolor **uncle** from **RED** to **BLACK** can resolve this problem._ |
| &nbsp;  | _According to the above steps: node **z**, **z**'s **parent** and **uncle** will not violate the **Red-Black Tree** [property](#Properties), but **z**'s **grampa** might violate!_ |
| &nbsp;  | _If **z**'s **grampa** is root, then color **z**'s grampa as **BLACK** will resolve the problem completely._ |
| &nbsp;  | _If **z**'s **grampa** is not root, then point the current node (pointer) to **z**'s **grampa** as the **"new" current node**, then analyzes the **"new" current node**_ |

{% asset_img rbt_insertion_case_1.jpg "Red-Black Tree Intertion Case 1" " " %} 


-----------------------------------------------------------------------------------------------------------------------------------------------------

# References #
http://www.cs.virginia.edu/~luebke/cs332.fall00/lecture10/index.htm
http://pages.cs.wisc.edu/~paton/readings/Red-Black-Trees/
https://www.cs.auckland.ac.nz/software/AlgAnim/red_black.html
https://www.cs.auckland.ac.nz/software/AlgAnim/red_black_op.html