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
3（9，20）
20（15，7）

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
   ```
   
   
###  1008. Construct Binary Search Tree from Preorder Traversal

Return the root node of a binary search tree that matches the given preorder traversal.

(Recall that a binary search tree is a binary tree where for every node, any descendant of node.left has a value < node.val, and any descendant of node.right has a value > node.val.  Also recall that a preorder traversal displays the value of the node first, then traverses node.left, then traverses node.right.)

It's guaranteed that for the given test cases there is always possible to find a binary search tree with the given requirements.

Example 1:

Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]

 ![Avator](https://assets.leetcode.com/uploads/2019/03/06/1266.png)

Constraints:


1 <= preorder.length <= 100

1 <= preorder[i] <= 10^8

The values of preorder are distinct.


```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:
        inorder = sorted(preorder)
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
  
  
17. Letter Combinations of a Phone Number

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.
![Avator](https://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)

Example:

Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.
        
```python3
'''
234 -> 
c1: a b c:
   c2: d e f:
      c3:  g h i:
            res.append(c1c2c3)
    

'''
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        dic = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'}
        res = []
        
        if not digits:
            return []
        
        def addChar(pre, idx):
            if idx == len(digits):
                res.append(pre)
                return
            for c in dic[digits[idx]]:
                addChar(pre + c, idx + 1)
                
        addChar("", 0)
        return res                            
```


### 297. Serialize and Deserialize Binary Tree

Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Clarification: The input/output format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

 
Example 1:

![Avator](https://assets.leetcode.com/uploads/2020/09/15/serdeser.jpg)
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
Example 2:

Input: root = []
Output: []
Example 3:

Input: root = [1]
Output: [1]
Example 4:

Input: root = [1,2]
Output: [1,2]
 
Constraints:

The number of nodes in the tree is in the range [0, 104].
-1000 <= Node.val <= 1000

```python3
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
# preorder
class Codec:

    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """    
        def flat(root):
            if not root:
                stream.append(None)
                return 
            stream.append(root.val)
            flat(root.left)
            flat(root.right)
            
        stream = []
        flat(root)
        return stream
        

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """     
        deq = collections.deque(data)
        
        def build():
            cur = deq.popleft()
            if cur == None:
                return None
            root = TreeNode(cur)
            root.left = build()
            root.right = build()
            return root
        return build()
        
        
        
# Your Codec object will be instantiated and called as such:
# ser = Codec()
# deser = Codec()
# ans = deser.deserialize(ser.serialize(root))
```
