315. Count of Smaller Numbers After Self

You are given an integer array nums and you have to return a new counts array. The counts array has the property where counts[i] is the number of smaller elements to the right of nums[i].

Example 1:

Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
 

Constraints:

0 <= nums.length <= 10^5
-10^4 <= nums[i] <= 10^4

```python3
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        self.count = 1  # 该结点的左子树以及本身结点个数
 
class Solution(object):  # O（nlogn）
    def countSmaller(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if not nums:
            return []
        
        root = Node(nums[-1])
        res = [0]  # 最后一个肯定是0
        
        for i in range(len(nums)-2, -1, -1):  # O（n）
            res.append(self.insert_node(nums[i], root))  # O(logn)
        
        return res[::-1]
        
    
    def insert_node(self, val, root):
        count = 0  #要插入的结点 在当前树中有多少个比他小的结点
        
        while True:
            if val <= root.val:
                root.count += 1
                if not root.left:
                    root.left = Node(val)
                    break  # break了 root还是上一个值，不是当前的val
                root = root.left
            else:
                count += root.count
                if not root.right:
                    root.right = Node(val)
                    break
                root = root.right
        return count
```
