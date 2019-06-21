Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.  

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).”  

Given binary search tree:  root = [6,2,8,0,4,7,9,null,null,3,5]  
```
            6
         /     \
        2       8
      /   \    /  \
     0     4  7    9
          / \
         3   5
```
Example 1:  

Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8  
Output: 6  
Explanation: The LCA of nodes 2 and 8 is 6.  
Example 2:  

Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4  
Output: 2  
Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.  
 

Note:  

All of the nodes' values will be unique.  
p and q are different and both values will exist in the BST.  
给定一个搜索二叉树 BST，对于给定的两个数值，返回它们的最小公共根节点  
BST 具有如下特点：根节点左边的节点值均小于根节点，根节点右边的节点值均大于根节点，所以感觉用递归更简洁
```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
        if not root:
            return False
        if p.val < root.val and q.val < root.val:
            return self.lowestCommonAncestor(root.left, p, q)
        if p.val > root.val and q.val > root.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```
