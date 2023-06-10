# LC-Python19

## 235. Lowest Common Ancestor of a Binary Search Tree

June 09, 2023  4h

Congratulations!\
This is the nineteenth day for leetcode python study. Today we will learn more about the Binary Tree!\
The challenges today are about **binary search tree搜索树** ~~need to delete later~~.


## 235. Lowest Common Ancestor of a Binary Search Tree
[Reading link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.md)\
[video](https://www.bilibili.com/video/BV1Zt4y1F7ww/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
[leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)\
二叉搜索树的最近公共祖先, 相对于普通二叉树的最近公共祖先,本题就简单一些了，因为可以利用二叉搜索树的特性。 
```python
# ways 1: recursion
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def traversal(self, cur, p, q):
        if cur is None:
            return cur
        if cur.val > p.val and cur.val > q.val:     #left
            left = self.traversal(cur.left, p, q)
            if left is not None:
                return left
        if cur.val < p.val and cur.val < q.val:     #right
            right = self.traversal(cur.right, p, q)
            if right is not None:
                return right
        return cur

    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        return self.traversal(root, p, q)
```
```python
# ways 1: recursion simple version
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        if root.val > p.val and root.val > q.val:
            return self.lowestCommonAncestor(root.left, p, q)
        elif root.val < p.val and root.val < q.val:
            return self.lowestCommonAncestor(root.right, p, q)
        else:
            return root
```
```python
# ways 2: 迭代
class Solution:
    def lowestCommonAncestor(self, root, p, q):
        while root:
            if root.val > p.val and root.val > q.val:
                root = root.left
            elif root.val < p.val and root.val < q.val:
                root = root.right
            else:
                return root
        return None
```


## 701.
二叉搜索树中的插入操作, simple




## 450.
删除二叉搜索树中的节点，相对于插入操作，本题就有难度了，涉及到改树的结构。
