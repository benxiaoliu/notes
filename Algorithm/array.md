54. Spiral Matrix
====

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example 1:

Input:
[

 [ 1, 2, 3 ],
 
 [ 4, 5, 6 ],
 
 [ 7, 8, 9 ]
]

Output: [1,2,3,6,9,8,7,4,5]

```python3
'''
命名很重要，有助于写代码时逻辑清晰，少写bug
注意下标范围以及定位
'''

class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        if not matrix or not matrix[0]: return matrix
        row_start, col_start = 0, 0
        row_end, col_end= len(matrix), len(matrix[0])
        res = []
        while row_start < row_end and col_start < col_end:
            if col_start == col_end:
                break
            for j in range(col_start, col_end):            
                res.append(matrix[row_start][j])
            row_start += 1
            
            if row_start == row_end:
                break
            for i in range(row_start, row_end):
                res.append(matrix[i][col_end-1])
            col_end -= 1
            
            if col_end == col_start:
                break
            for j in range(col_end-1, col_start-1, -1):
                res.append(matrix[row_end-1][j])
            row_end -= 1
            
            if row_end == row_start:
                break
            for i in range(row_end-1, row_start-1, -1):
                res.append(matrix[i][col_start])
            col_start += 1
        return res
    
        
```
