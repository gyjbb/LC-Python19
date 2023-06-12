# LC-Python19

## 235. Lowest Common Ancestor of a Binary Search Tree, 701. Insert into a Binary Search Tree, 450. Delete Node in a BST

June 09, 2023  4h

Congratulations!\
This is the nineteenth day for leetcode python study. Today we will learn more about the Binary Tree!\
The challenges today are about **binary search tree搜索树**. How to get the Lowest Common Ancestor of two elements in a binary search tree. What's the difference with a general binary tree? How to add and delete elements in a binary search tree? ~~need to delete later~~.


## 235. Lowest Common Ancestor of a Binary Search Tree
[Reading link](https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0235.%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E7%9A%84%E6%9C%80%E8%BF%91%E5%85%AC%E5%85%B1%E7%A5%96%E5%85%88.md)\
[video](https://www.bilibili.com/video/BV1Zt4y1F7ww/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
[leetcode](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)\
Compared to the lowest common ancestor of general binary tree, binary search tree is more simple since we can take advantage of the characteristic of binary search tree. When traverse inorder, the array we can get is monotonically increasing.
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
# ways 2: iteration迭代
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


## 701. Insert into a Binary Search Tree
[leetcode](https://leetcode.com/problems/insert-into-a-binary-search-tree/)\
This is a simple question. Any new node can be added as a leaf node, and the tree's original structure remains unchanged.\
The root in termination condition means the node that we currently traverse. In this question the termination condition is: root is null, which means we traverse and find the blank place where we can add the new element. Then at the new place, we define a new tree node, and add the new value. And finally return the new node to the upper node, as a left/right branch of the upper level node. So the new node is connected with its father node and added to the binary tree!\
In conclusion, the returned element(a new tree node) in the termination condition is given to the root.left / root.right in the left/right branches part.
```python
# ways 1: recurson
class Solution:
    def insertIntoBST(self, root, val):
        if root is None:
            node = TreeNode(val)
            return node             #**returned value**

        if root.val > val:
            root.left = self.insertIntoBST(root.left, val)    #**the left subtree of current level equals the returned value of next recursion**
        if root.val < val:
            root.right = self.insertIntoBST(root.right, val)

        return root
```
```python
# ways 1: iteration
class Solution:
    def insertIntoBST(self, root, val):
        if root is None:  # if the root is null, create a new node as root and return it.
            node = TreeNode(val)
            return node

        cur = root
        parent = root  # record the last node, used to connect the new node
        while cur is not None:
            parent = cur
            if cur.val > val:
                cur = cur.left
            else:
                cur = cur.right

        node = TreeNode(val)
        if val < parent.val:
            parent.left = node  # connect the new node to the left subtree of parent node将新节点连接到父节点的左子树
        else:
            parent.right = node  # connect the new node to the right subtree of parent node将新节点连接到父节点的右子树

        return root
```


## 450. Delete Node in a BST
[leetcode](https://leetcode.com/problems/delete-node-in-a-bst/)\
[worth watching again](https://www.bilibili.com/video/BV1tP41177us/?spm_id_from=pageDriver&vd_source=63f26efad0d35bcbb0de794512ac21f3)\
Compared to inserting into a binary search tree, deleting from a binary search tree is more complicated, because the tree's structure will be changed.\
This question can be divided into 5 categories. The deleted tree node can't be found in the tree, or the deleted tree node is a leaf node, or a node with only left subtree but no right subtree, or node with only right subtree but no left subtree, or with both left and right subtree. The final situation is the most complicated one. We need to delete the node, and move its whole left subtree under the most left 
node of its whole right subtree.\
The termination condition is when we find the node we want to delete, and delete it. 
```python
# ways 1: recursion
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def deleteNode(self, root: Optional[TreeNode], key: int) -> Optional[TreeNode]:
        if root is None:
            return root
        if root.val == key:
            if root.left is None and root.right is None:
                return None
            elif root.left is None:
                return root.right
            elif root.right is None:
                return root.left
            else:
                cur = root.right
                while cur.left is not None:
                    cur = cur.left      #get the most left node of the right subtree
                cur.left = root.left   #put the left subtree of the deleted node as the left subtree of the most left node of the deleted node's right subtree.
                return root.right
        if root.val > key:
            root.left = self.deleteNode(root.left, key)
        if root.val < key:
            root.right = self.deleteNode(root.right, key)
        return root
```







