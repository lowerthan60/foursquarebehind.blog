---
title: Red-Black Tree Deletion
date: 2016-11-29 15:29:09
tags:
- Algorithm
- Tree
---

<a name="Properties"></a>
# Red-Black Tree Properties #
1. Every node is colored with either **RED** or **BLACK**;
2. All leaf (NULL Pointer) nodes are colored with **BLACK**; 
	**Note:** _If a node’s child is missing then we will assume that it has a nil child(NULL Pointer) in that place and this nil child(NULL Pointer) is always colored **BLACK**._
3. Every path from a node $x$ to a descendent leaf has the same number of **BLACK** nodes (**_not counting node $x$_**). We call this number the **Black-Height** of node $x$, which is denoted by $bh(x)$;
4. Both children of a **RED** node must be **BLACK** nodes;
	**Note:** _So it's impossible to have 2 consecutive reds on a path, so it ensures that the deepest path in the tree is not longer than twice the shortest one. Thus, the **Red-Black Tree** is a relatively balanced binary tree._
5. The root is always **BLACK**;


# Deletion #

The delete operation is similar in feel to the insert operation, but more complicated.

To delete a node from a **Red-Black Tree**, first do a **BST** **Deletion** to delete the node from the **Red-Black Tree**, then via **Rotation** and **Recolor** operations to restore the tree to meet all of the **Red-Black Tree** properties again.

# Detail steps #

| **Step 1** | **Delete the node via BST Deletion** |
| :---       | :---                                 |
| **Case 1** | The deleting node is a leaf node, then just delete it.  **Black-Height change is possible!!!**|
| **Case 2** | The deleting node has 1 subtree, then just "link past" the node. **Black-Height change is possible!!!** |
| &nbsp;     | i.e. connect the parent of the node directly to the node's only subtree. |
| &nbsp;     | This always works, whether the one subtree is on the left or on the right. |
| **Case 3** | The deleting node has 2 subtree, then follow the below 3 steps:  |
| &nbsp;     | &nbsp;&nbsp; **3.1** First find the successor of this node. |
| &nbsp;     | &nbsp;&nbsp; **3.2** Then copy the value of the successor to this node. |
| &nbsp;     | &nbsp;&nbsp; The idea is move the value from the successor to the deleting node, then delete the successor.|
| &nbsp;     | &nbsp;&nbsp; Now considering the successor, **it's impossible that the successor has two non-empty child nodes!** |
| &nbsp;     | &nbsp;&nbsp; so either the successor is a leaf node so process as the **Case 1**; |
| &nbsp;     | &nbsp;&nbsp; or the successor only has 1 child node so process as the **Case 2**. |

-----------------------------------------------------------------------------------------------------------------------------------------------------

| **Deletion Cases** | &nbsp; |
| :---               | :---                                 |
| The deleting node is a **RED** leaf, then just delete it. | {% asset_img rbt_deletion_case_1.png "Figure 1" "Red-Black Tree Delete RED leaf" %} |
| The deleting node is a **BLACK** leaf, **Black-Height** changed!!!<br/>We need the step 2. | {% asset_img rbt_deletion_case_2.png "Figure 2" "Red-Black Tree Delete BLACK leaf" %} |
| The deleting node is a **BLACK** node with 1 **RED** child,<br/>link past the node, then recolor the **RED** child to **BLACK** to keep the **Black-Height**. | {% asset_img rbt_deletion_case_3.png "Figure 3" "Red-Black Tree Delete RED non-empty node" %} |


**The deleting node is a node with 2 child nodes,it will be a little complexity.** 

-----------------------------------------------------------------------------------------------------------------------------------------------------


| **_Properties again_**            | &nbsp; | &nbsp;                                                         |
| :---              | :---:  | :---                                                                           |
| **_Question_**:   | &nbsp; | Which **Red-Black Tree** [properties](#Properties) does the above case break?  |
| 1. Every node is colored with either **RED** or **BLACK**                                  | &nbsp; | We don't break this because the new inserted node is colored as **RED**;     | 
| 2. All leaf (NULL Pointer) nodes are colored with **BLACK**                                | &nbsp; | We don't break this because the new inserted node is a nonempty node, it won't impact the leaf nodes which are all nil(NULL Pointer) nodes; |
| 3. Every path from a node $x$ to a descendent leaf has the same number of **BLACK** nodes  | &nbsp; | We don't break this because the new inserted node is colored as **RED**, so the count of **BLACK** nodes should keep same as before; |
| 4. Both children of a **RED** node must be **BLACK** nodes                                 | &nbsp; | **We might break property #4!!!** |
| 5. The root is always **BLACK**                                                            | &nbsp; | We don't break this because BST insertion will only insert the new node to the leaf nodes, the root node won't be impacted; |

-----------------------------------------------------------------------------------------------------------------------------------------------------

**After step 1, delete one node via BST deletion, the new tree might break Red-Black Tree [properties](#Properties) 3,4,5, it's what we want to resolve at step 2.**

| **Step 2** | **Rotation or Recolor to restore the tree to meet all of the Red-Black Tree [properties](#Properties) again.** | 
| :---       | :---                                 |
| &nbsp;     | Because after step 1, the new tree might not meet all of the **Red-Black Tree** [properties](#Properties). |

-----------------------------------------------------------------------------------------------------------------------------------------------------

**Pseudo code --- RB-DELETE**
```
RB-DELETE(T, z)
if left[z] = nil[T] or right[z] = nil[T]         
   then y ← z                                  // if "z" has unique child, then assign "z" to "y"
   else y ← TREE-SUCCESSOR(z)                  // otherwise assign "z"'s successor to "y" 

if left[y] ≠ nil[T]
   then x ← left[y]                            // if "y"'s left child is not NIL, then assign it to "x"
   else x ← right[y]                           // otherwise assign "y"'s right child to "x" (because "y" is from "z", it must have at least one child)

p[x] ← p[y]                                    // assign "y"'s parent to "x"'s parent
if p[y] = nil[T]                               
   then root[T] ← x                            // Case 1: if "y"'s parent is NIL, then set "root" as "x"
   else if y = left[p[y]]                    
           then left[p[y]] ← x                 // Case 2: if "y" is the left child of "y"'s parent, then set "x" as the left child of "y"'s parent
           else right[p[y]] ← x                // Case 3: if "y" is the right child of "y"'s parent, then set "x" as the right child of "y"'s parent
if y ≠ z                                    
   then key[z] ← key[y]                        // if "y" not equal to "z", then only copy "y"'s satellite data to "z", don't copy "y"'s color!
        copy y's satellite data into z         
		
if color[y] = BLACK                            
   then RB-DELETE-FIXUP(T, x)                  // if "y" is BLACK, then call RE-DELETE-FIXUP function
return y

```

{% asset_img rbt_deletion_case_4.png "Figure 4" "Red-Black Tree Deletion" %} 

So according to the above image _Figure 4_, let's say the deleting node is **z**, then 

If **z**'s children is less than 2, then just need to point **y** to **z**;
If **y** is **RED**, then just delete it, because it won't break **Red-Black Tree**'s [properties](#Properties);
If **y** is **BLACK**, then we have to call the $fixup$ function, also just delete it, because it won't break **Red-Black Tree**'s [properties](#Properties); 

If **z** has two child nodes, then find **z**'s successor **y**, use **y** to replace **z**, recolor **y** as **z**'s color, so it won't break **Red-Black Tree**'s [properties](#Properties);

In order to facilitate analysis, we assume that 







删除操作同二叉树的删除操作也是类似，也是同样分为：少于两个子结点、有两个子结点的情况，红色标注不同；
1.z的子节点数少于两个：（令y指向z的位置）如果结点y颜色为红色直接删除，因为不会破坏红黑的性质；若为y黑色，调用颜色修复函数,并令其子树x代替z的位置，并把颜色也改变成z的颜色。
2.z的子节点数有两个：找到z的后继y,用y代替z的位置，并把y的颜色换成z的颜色，这样不会破坏红黑的性质。但是如果y在之前的位置是黑色，现在由于转移走了，y的右子树x代替了y的位置，此时破坏了这个支树的红黑性质，少了一个黑色结点，需要调用颜色修复函数；

当删除一个黑色节点D时，把D的黑色“下推”至其子节点C，也就是说C除了本身的颜色外多了一重额外的黑色，然后不断把这重额外的黑色沿树上移，直到碰到一个红色节点，把其变为黑色以保证路径上黑色节点数目不变，或者移到树的根部，这样所有路径上的黑色节点数目都减一，保持相等。上移过程中可能需要旋转和修改一些节点的颜色，以保证路径上黑色节点数目不变。

-----------------------------------------------------------------------------------------------------------------------------------------------------


The C++ code for **Red-Black Tree RB_Transplant** is below:

```C++
void RB_Transplant(Node **T,Node * u,Node * v) {  //It's like copy u to v
  if (u->p == T_NIL)  
    *T = v;  
  else if (u == u->p->left)  
    u->p->left = v;  
  else  
    u->p->right = v;  
  v->p = u->p;            //Mandatory step even v's value is T_NIL, it's for the color fix up needed
}  
```

The C++ code for **Red-Black Tree RB_Delete** is below:

```C++
Node * RB_Delete(Node *T ,Node *z) {  //Same as BST Deletion, the only difference is check y's color
  Node * x = NULL;  
  Node * y = z;                             //assign z to y
  enum colors y_original_color = y->color;  //save z's original color before deleting
  if (z->left == T_NIL) {                   //if z doesn't have left child
    x = z->right;  
    RB_Transplant(&T,z,z->right);  
  } else if (z->right == T_NIL) {           //if z doesn't have right child
    x = z->left;  
    RB_Transplant(&T,z,z->left);  
  } else {                                  //if z has two child nodes
    y = Tree_Minimum(z->right);             //find z's successor   
    y_original_color = y->color;            //save y's original color
    x = y->right;  
    if (y->p == z) {                        //if y's parent is z
      x->p = y;  
    } else {  
        RB_Transplant(&T,y,y->right);       //if y's parent is not z, replace y with y's right child
        y->right = z->right;  
        y->right->p = y;  
    }  
      
    RB_Transplant(&T,z,y);                  //replace z with y, no matter if y's value is T_NIL
    y->left = z->left;  
    y->left->p = y;  
    y->color = z->color;                    //change y's color to z's color
  }  
  
  if (y_original_color == black)            //if y's color is BLACK, then fix up the tree
    RB_Delete_Fixup(T,x); 
	
  return T;  
}  
```

Here let's take about the function **RB_Delete_Fixup(T,x)**. Please notice that the input parameter is **x**, it becauses that either **y** is deleted or transplanted, **x** replaces **y**'s position.
If **x** is **RED**, then recolor **x** as **BLACK** will fix up the tree.
If **x** is **BLACK**, then: