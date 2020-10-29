### 329. Longest Increasing Path in a Matrix

Given an integer matrix, find the length of the longest increasing path.

From each cell, you can either move to four directions: left, right, up or down. You may NOT move diagonally or move outside of the boundary (i.e. wrap-around is not allowed).

Example 1:

Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
Output: 4 
Explanation: The longest increasing path is [1, 2, 6, 9].
Example 2:

Input: nums = 
[
  [3,4,5],
  [3,2,6],
  [2,2,1]
] 
Output: 4 
Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.

```python3
'''
dfs each cell to get the longest increasing path 
key point: will time out if no memorization.
why memo work?: 
Input: nums = 
[
  [9,9,4],
  [6,6,8],
  [2,1,1]
] 
when you dfs nums[2][1], you will dfs 1->2->->6->9, in this way, you have calculated the longest increasing path for cell 2,6,9 too, so you don't need to calculate again. that why we will skip if find memo[i][j] != 0
'''

class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:
        memo = [[0] * len(matrix[0]) for _ in range(len(matrix))]
        if not matrix or not matrix[0]:return 0 
        
        def dfs(i, j):
            if memo[i][j] == 0:
                memo[i][j] = max(
                    1 + dfs(i, j-1) if j-1 >= 0 and matrix[i][j-1] > matrix[i][j] else 1,
                    1 + dfs(i, j+1) if j+1 < len(matrix[0]) and matrix[i][j+1] > matrix[i][j] else 1,
                    1 + dfs(i-1, j) if i-1 >= 0 and matrix[i-1][j] > matrix[i][j] else 1,
                    1 + dfs(i+1, j) if i+1 < len(matrix) and matrix[i+1][j] > matrix[i][j] else 1
                )
            return memo[i][j]
        
        
        return max(dfs(i, j) for i in range(len(matrix)) for j in range(len(matrix[0])))
                
        
                
```
