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
