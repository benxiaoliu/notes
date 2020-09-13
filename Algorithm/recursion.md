450. Delete Node in a BST

Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.
Follow up: Can you solve it with time complexity O(height of tree)?

![avatar](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

Example 1:

![avatar](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)

Input: root = [5,3,6,2,4,null,7], key = 3
Output: [5,4,6,2,null,null,7]
Explanation: Given key to delete is 3. So we find the node with value 3 and delete it.
One valid answer is [5,4,6,2,null,null,7], shown in the above BST.
Please notice that another valid answer is [5,2,6,null,4,null,7] and it's also accepted.

```python3

'''
怎么巧妙地利用递归，省去记录parent以及left or right, 以及让删掉的那个节点的parent对应left/right变为None
'''

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution(object):
    def deleteNode(self, root, key):
        if not root: return None
        
        # when we find the target node
        if root.val == key:
            # target node is a leave node
            if not root.right and not root.left: return None
            
            # has left or right tree
            if not root.right: return root.left
            if not root.left: return root.right
            
            # has left and right tree
            if root.left and root.right:
                temp = root.right
                while temp.left: temp = temp.left
                root.val = temp.val
                root.right = self.deleteNode(root.right, temp.val)

        elif root.val > key:
            root.left = self.deleteNode(root.left, key)
        else:
            root.right = self.deleteNode(root.right, key)
            
        return root
        
```
