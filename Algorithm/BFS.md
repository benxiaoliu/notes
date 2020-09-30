1197. Minimum Knight Moves
In an infinite chess board with coordinates from -infinity to +infinity, you have a knight at square [0, 0].

A knight has 8 possible moves it can make, as illustrated below. Each move is two squares in a cardinal direction, then one square in an orthogonal direction.
![image](https://assets.leetcode.com/uploads/2018/10/12/knight.png)
Return the minimum number of steps needed to move the knight to the square [x, y].  It is guaranteed the answer exists.

Example 1:

Input: x = 2, y = 1
Output: 1
Explanation: [0, 0] → [2, 1]
Example 2:

Input: x = 5, y = 5
Output: 4
Explanation: [0, 0] → [2, 1] → [4, 2] → [3, 4] → [5, 5]
 

Constraints:

|x| + |y| <= 300

```python
# [[1,2], [2,1], [2,-1], [1,-2], [-1,-2], [-2,-1], [-2,1], [-1,2]]
class Solution:
    def minKnightMoves(self, x: int, y: int) -> int:
        x, y = abs(x), abs(y)
        step, level = -1, [[0,0]]
        memo = set()
        while level:
            next_level = []
            step += 1
            for xx, yy in level:
                if xx == x and yy == y:
                        return step                
                for i, j in [[1,2], [2,1], [2,-1], [1,-2], [-1,-2], [-2,-1], [-2,1], [-1,2]]:
                    if xx + i >= -1 and yy + j >= -1 and (xx + i, yy + j) not in memo:
                        memo.add((xx + i, yy + j))
                        next_level.append([xx + i, yy + j])
            level = next_level
```

### 863. All Nodes Distance K in Binary Tree

We are given a binary tree (with root node root), a target node, and an integer value K.

Return a list of the values of all nodes that have a distance K from the target node.  The answer can be returned in any order.

Example 1:

Input: root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, K = 2

Output: [7,4,1]

Explanation: 
The nodes that are a distance 2 from the target node (with value 5)
have values 7, 4, and 1.


![image](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/06/28/sketch0.png)


Note that the inputs "root" and "target" are actually TreeNodes.
The descriptions of the inputs above are just serializations of these objects.
 

Note:

The given tree is non-empty.
Each node in the tree has unique values 0 <= node.val <= 500.
The target node is a node in the tree.
0 <= K <= 1000.

```python3
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def distanceK(self, root: TreeNode, target: TreeNode, K: int) -> List[int]:
        graph = collections.defaultdict(set)
        level = collections.deque([root])
        
        '''
        3:(5,1)
        5:(3, 6, 2)
        1:(3)
        6:(5)
        2: (5)
        '''
        while level:  # [5, 1]
            node = level.popleft()
            if node.left:
                graph[node.val].add(node.left.val)
                graph[node.left.val].add(node.val)
                level.append(node.left)
            if node.right:
                graph[node.val].add(node.right.val)
                graph[node.right.val].add(node.val)
                level.append(node.right)
        
        
        visited = set([target.val])
        level = [target.val]
        while K != 0:
            print(level)
            next_level = []
            for node in level:
                for nei in graph[node]:
                    if nei not in visited:
                        next_level.append(nei)
                        visited.add(nei)
            level = next_level[:]
            K -= 1
        print(level)    
        return level
        
```
