---
title: Red-Black Tree Insertion
date: 2016-11-29 14:54:43
tags:
- Algorithm
- Tree
---

# Insertion #

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

-----------------------------------------------------------------------------------------------------------------------------------------------------

According to **z**'s **parent**'s state, when we insert **z** into a **Red-Black Tree** and then color **z** as **RED**, there will be 3 cases to deal with:

| **Case**                                   | &nbsp;          | &nbsp; |
| :---                                       | :---:           | :---   |
| **1**: **z** is the root node              | &nbsp;---&nbsp; | Color node **z** as **BLACK** |
| **2**: **z**'s **parent** is the root node | &nbsp;---&nbsp; | Do nothing, since **T** is still a **Red-Black Tree** after inserted **z** |
| **3**: **z**'s **parent** is **RED**       | &nbsp;---&nbsp; | This case breaks the [property](#Properties) #4, in this situation, **z** must have **grampa** node, further more|
| &nbsp;                                     | &nbsp;---&nbsp; | **z** must have an **uncle** node (even it's NIL, we will still say it exists and NIL node is **BLACK**). |
| &nbsp;                                     | &nbsp;---&nbsp; | According to **z**'s uncle's state, we will have another **3 sub-cases** as below: |


| **Case** | &nbsp;                                                          | &nbsp; |
| :---     | :---                                                            | :---   |
| **3.1**: | **z**'s **parent** is **RED**, **z**'s uncle is **RED**         | (01) color **z**'s **parent** as **BLACK**                               |
| &nbsp;   | &nbsp;                                                          | (02) color **z**'s uncle as **BLACK**                                    |
| &nbsp;   | &nbsp;                                                          | (03) color **z**'s grampa as **RED**                                     |
| &nbsp;   | &nbsp;                                                          | (04) point **z** to **z**'s grampa, then keep the processing iteratively |
| **3.2**: | **z**'s **parent** is **RED**, **z**'s uncle is **BLACK**,      | (01) point **z** to **z**'s **parent**                                   |
| &nbsp;   | and **z** is the **right child** of **z**'s **parent**  		 | (02) **LEFT-ROTATE** on **z**                                            |
| **3.3**: | **z**'s **parent** is **RED**, **z**'s uncle is **BLACK**,      | (01) color **z**'s **parent** as **BLACK**                               |
| &nbsp;   | and **z** is the **left child** of **z**'s **parent**           | (02) color **z**'s grampa as **RED**                                     |
| &nbsp;   | &nbsp;                                                          | (03) **RIGHT-ROTATE** on **z**'s grampa                                  |

**_The core idea about the 3 Sub-Cases above is : "Move the RED node to root, then color the root as BLACK"_**


| **3.1** | &nbsp;&nbsp;**z**'s **parent** is **RED**, **z**'s uncle is **RED** |
| :---    | :---                                                    |
| &nbsp;  | &nbsp;&nbsp;**z** and **z**'s **parent** are **RED**, it breaks [property](#Properties) #4, so color **z**'s **parent** as **BLACK** to resolve this problem. |
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

{% asset_img rbt_insertion_case_1_full.jpg "Red-Black Tree Intertion Case 1 another example" " " %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------

| **3.2** | &nbsp;&nbsp;**z**'s **parent** is **RED**, **z**'s uncle is **BLACK**, and **z** is the **right child** of **z**'s **parent**        |
| :---    | :---                                                                                                                                 |
| &nbsp;  | &nbsp;&nbsp;point **z** to **z**'s **parent**, then **LEFT-ROTATE** on **z**                                                         |
| **Q**:  | **Why point z to z's parent?**                                                                                                       |
| **A**:  | _Becase the new node will be always inserted as the leaf, so we should keep processing on the subtree ascend to the root to ensure satisfy with all of the **Red-Black Tree** [property](#Properties)_ |
| **Q**:  | **Why LEFT-ROTATE on z?**                                                                                                |
| **A**:  | _The core idea of **Red-Black Tree** is "**Move the RED node to root, then color the RED node to BLACK**"". Further more, because **z** is the **right child**, so we need **LEFT-ROTATE** to move **z** up!_ |


{% asset_img rbt_insertion_case_2.jpg "Red-Black Tree Intertion Case 2" " " %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------

{% asset_img rbt_insertion_case_2_full.jpg "Red-Black Tree Intertion Case 2 another example" " " %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------

| **3.3** | &nbsp;&nbsp;**z**'s **parent** is **RED**, **z**'s uncle is **BLACK**, and **z** is the **left child** of **z**'s **parent**            |
| :---    | :---                                                                                                                                    |
| &nbsp;  | &nbsp;&nbsp;recolor **z**'s **parent** as **BLACK**, recolor **z**'s **grampa** as **RED**, then **RIGHT-ROTATE** on **z**'s **grampa** |
| **Q**:  | **Why recolor z and z's parent?** |
| **A**:  | _Both of the current node **z** and **z**'s parent are **RED**, it breaks [property](#Properties) #4, so recolor **z**'s parent as **BLACK** to resolve this issue._ |
| &nbsp;  | _After above step, [property](#Properties) #3 is broken, because the **Black-Height** of the subtree where **z**'s **parent** exists increased by 1__  |
| &nbsp;  | _To resolve this problem, we need to recolor **z**'s **grampa** from **BLACK** to **RED**, and then do **RIGHT-ROTATE** on **z**'s **grampa**._  |

{% asset_img rbt_insertion_case_3_full.jpg "Red-Black Tree Intertion Case 3 another example" " " %} 

-----------------------------------------------------------------------------------------------------------------------------------------------------

| **Steps** | **A Complete Red-Black Tree Insertion Process*               |
| :---      | :---                                                         |
| &nbsp     | {% asset_img rbt_insertion_complete_1.png "Step 1" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_2.png "Step 2" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_3.png "Step 3" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_4.png "Step 4" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_5.png "Step 5" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_6.png "Step 6" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_7.png "Step 7" " " %}        |
| &nbsp     | {% asset_img rbt_insertion_complete_8.png "Step 8" " " %}        |
