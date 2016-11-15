---
title: Height of a Complete Binary Tree
date: 2016-11-09 00:28:17
tags:
	Algorithm
	Tree
---

The height of a complete binary tree is $O(log n)$

### Theorem ###
The height of a complete, balanced tree of  $n$ nodes is $log(n+1)$ .

### Intuition ###
| Tree | Nodes($n$) | Height $log(n+1)$ |
| :--- | :---: | :---: | 
| {% asset_img min_tree1.gif " " " " %} | 1 | 1 |
| {% asset_img min_tree2.gif " " " " %} | 3 | 2 |
| {% asset_img min_tree3.gif " " " " %} | 7 | 3 |

### Proof ###

$n$ : Number of nodes in the tree.
$h$ : Height of the tree.

##### Precondition #####
Since the tree is complete, so the subtrees of the same parent should have the same number of nodes and same height.
So $n = 1 + n$<sub>left</sub> $+$ $n$<sub>right</sub>
And $n$<sub>left</sub> $=$ $n$<sub>right</sub>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;$h$<sub>left</sub> $=$ $h$<sub>right</sub>

##### Basis #####
$n$ = 1, $log(n+1)$ = $log2$ = 1, $h$ = $log(n+1)$ = 1

##### Inductive Hypothesis #####
Assume that the theorem is **true** for heights <= $k$ .
	
##### Inductive Step #####
Prove that the inductive hypothhesis is **true** for height $k + 1$
	
Let $h$ as the height of the Complete Binary Tree with $n$ nodes, 
and let $h = k + 1$ .

So $k$ is the height of the direct subtree of the Complete Binary Tree with $n$ nodes, 

so $k = log_2(n$<sub>left</sub> $+$ $1)$ --------- *by the inductive hypothesis*
 
Note that the theorem is **true** (by the inductive hypothesis) of the subtrees of the root, since they have height $k$

| &nbsp;   | &nbsp;              | &nbsp; | &nbsp;                                         | &nbsp; |
| :---     | ---:                | :---:  | :---                                           | ---: |
| &nbsp;   | $n$                 | $=$    | $1 + n$<sub>left</sub> $+$ $n$<sub>right</sub> | &nbsp; |
| &nbsp;   | &nbsp;              | $=$    | $1 + 2n$<sub>left</sub>                        | *since the tree is complete* |
| **BUT**  | $k$                 | $=$    | $log_2(n$<sub>left</sub> $+$ $1)$              | *by the inductive hypothesis* |
| **SO**   | $n$<sub>left</sub>  | $=$    | $2^k - 1$                                      | &nbsp; |
| **THEN** | $n$                 | $=$    | $1 + 2(2^k - 1)$                               | &nbsp; |
| &nbsp;   | &nbsp;              | $=$    | $2^{k + 1} - 1$                                | &nbsp; |
| &nbsp;   | $log_2(n+1)$        | $=$    | $log_2(2^{k + 1})$                             | &nbsp; |
| &nbsp;   | &nbsp;              | $=$    | $k + 1$                                        | &nbsp; |
| &nbsp;   | &nbsp;              | $=$    | $h$                                            | &nbsp; |



Thus, the inductive hypothesis is **true** for height $k + 1$ and, hence (by induction), **true** for all heights. A ccomplete binary tree of $n$ nodes has height $log_2(n+1)$ .