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
### 105. Construct Binary Tree from Preorder and Inorder Traversal
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given

preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
Return the following binary tree:

  3
  
9,  20

   15,7
   
   
 ```python3
 
 # Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

'''
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]

no duplicates, 记录inorder中数的index，根据这个index确定左右子树 在preoder inorder中的index
preorder第一个元素就是根结点

递归找root的左子树右子树 
'''python3
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        inoder_idx_map = {}
        for i, num in enumerate(inorder):
            inoder_idx_map[num] = i
            
            
        def build(in_start, in_end, pre_start, pre_end):
            if in_start > in_end or pre_start > pre_end:
                return None
            root = TreeNode(preorder[pre_start])
            root_idx_in_inorder = inoder_idx_map[preorder[pre_start]]
            root.left = build(in_start, root_idx_in_inorder-1, pre_start+1, pre_start+(root_idx_in_inorder-in_start))
            root.right = build(root_idx_in_inorder+1, in_end, pre_start+root_idx_in_inorder-in_start + 1, pre_end)
            return root
        
        
        return build(0, len(inorder)-1, 0, len(preorder)-1)
        
 ```
 
 
### 106. Construct Binary Tree from Inorder and Postorder Traversal

Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given

inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
Return the following binary tree:

    3
   / \
  9  20
    /  \
   15   7
```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        inorder_idx_map = {}
        for i, num in enumerate(inorder):
            inorder_idx_map[num] = i
        
        def build(in_s, in_e, pos_s, pos_e):
            if in_s > in_e or pos_s > pos_e:
                return None
            root = TreeNode(postorder[pos_e])
            root_idx_in_inoder = inorder_idx_map[root.val]
            root.left = build(in_s, root_idx_in_inoder-1, pos_s, pos_s + (root_idx_in_inoder - in_s) - 1)
            root.right = build(root_idx_in_inoder + 1, in_e, pos_s + (root_idx_in_inoder - in_s), pos_e - 1)
            
            return root
        return build(0, len(inorder)-1, 0, len(postorder)-1)
        

python
