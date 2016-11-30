---
title: Red-Black Tree Deletion
date: 2016-11-29 15:29:09
tags:
- Algorithm
- Tree
---

# Deletion #

The delete operation is similar in feel to the insert operation, but more complicated.

To delete a node from a **Red-Black Tree**, first do a **BST** **Deletion** to delete the node from the **Red-Black Tree**, then via **Rotation** and **Recolor** operations to restore the tree to meet all of the **Red-Black Tree** properties again.

# Detail steps #
| **Step 1** | Delete the node via **BST Deletion** |
| :---       | :---                                                                    |
| **Case 1** | The deleting node is a leaf node, then just delete it and the rest of the tree is exactly as it was, so it is still a BST              |
| **Case 2** | The deleting node has 1 subtree, then just "link past" the node, i.e. connect the parent of the node directly to the node's only subtree. This always works, whether the one subtree is on the left or on the right. |
| **Case 3** | The deleting node has 2 subtree, then just "link past" the node, i.e. connect the parent of the node directly to the node's only subtree. This always works, whether the one subtree is on the left or on the right. |
 
 
       ③ 被删除节点有两个儿子。那么，先找出它的后继节点；然后把“它的后继节点的内容”复制给“该节点的内容”；之后，删除“它的后继节点”。在这里，后继节点相当于替身，在将后继节点的内容复制给"被删除节点"之后，再将后继节点删除。这样就巧妙的将问题转换为"删除后继节点"的情况了，下面就考虑后继节点。 在"被删除节点"有两个非空子节点的情况下，它的后继节点不可能是双子非空。既然"的后继节点"不可能双子都非空，就意味着"该节点的后继节点"要么没有儿子，要么只有一个儿子。若没有儿子，则按"情况① "进行处理；若只有一个儿子，则按"情况② "进行处理。



