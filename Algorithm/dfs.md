1559. Detect Cycles in 2D Grid
=====

Share
Given a 2D array of characters grid of size m x n, you need to find if there exists any cycle consisting of the same value in grid.

A cycle is a path of length 4 or more in the grid that starts and ends at the same cell. From a given cell, you can move to one of the cells adjacent to it - in one of the four directions (up, down, left, or right), if it has the same value of the current cell.

Also, you cannot move to the cell that you visited in your last move. For example, the cycle (1, 1) -> (1, 2) -> (1, 1) is invalid because from (1, 2) we visited (1, 1) which was the last visited cell.

Return true if any cycle of the same value exists in grid, otherwise, return false.

Example 1:
![avatar](https://assets.leetcode.com/uploads/2020/07/15/11.png)

Input: grid = [["a","a","a","a"],["a","b","b","a"],["a","b","b","a"],["a","a","a","a"]]
Output: true
Explanation: There are two valid cycles shown in different colors in the image below:

```python3
'''
用dfs判断环

有点类似于number of islands
'''

class Solution:
    def containsCycle(self, grid: List[List[str]]) -> bool:
        def dfs(node, parent):
            if node in visited: return True
            visited.add(node)
            nx,ny = node
            childs = [(cx,cy) for cx,cy in [[nx+1,ny],[nx-1, ny],[nx,ny+1],[nx,ny-1]] 
                      if 0 <= cx < m and 0 <= cy < n 
                      and grid[cx][cy] == grid[nx][ny] and (cx,cy) != parent]
            for x in childs:
                if dfs(x, node): return True 
            return False  
    
        m, n = len(grid), len(grid[0])
        visited = set()
        for i in range(m):
            for j in range(n):
                if (i,j) in visited: continue 
                if dfs((i,j), None): return True
        return False 
```

#### DFS + Stack

### 772. Basic Calculator III

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

The expression string contains only non-negative integers, +, -, *, / operators , open ( and closing parentheses ) and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of [-2147483648, 2147483647].

Some examples:

"1 + 1" = 2
" 6-4 / 2 " = 4
"2*(5+5*2)/3+(6/2+8)" = 21
"(2+6* 3+5- (3*14/7+2)*5)+3"=-12
 

Note: Do not use the eval built-in library function.

```python 3
class Solution:
    def calculate(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        s = s + "$"  # 为了防止最后的数无法参与运算， 因为只有遇到数字，...之外的 才会去计算
        def dfs(stack, i):
            num = 0  # 将要操作的数
            operator = '+'  # 将要操作的数之前的运算符，最初默认为 +
            while i < len(s):
                c = s[i]
                if c == " ":
                    i += 1
                    continue  
                if c.isdigit():
                    num = 10 * num + int(c)
                    i += 1
                elif c == '(':
                    num, i = dfs([], i+1)  # 计算括号里的
                else:  # 碰到运算符时才计算先前的num,使用的也是旧的，num之前那个operator
                    if operator == '+':  # 这里是operator 不是c 一定要注意！！！！！！！
                        stack.append(num)  # stack 里面只放 用来计算和的数，遇到乘或除的时候 pop一个计算出 最后用来计算和的数
                    if operator == '-':
                        stack.append(-num) 
                    if operator == '*':
                        stack.append(stack.pop() * num)
                    if operator == '/':
                        stack.append(int(float(stack.pop()) / num))
                        
                    num = 0  # 遇到运算符 把当前数计算完之后， 将下一个要计算的数重置为0
                    i += 1
                    operator = c  # 将下一个要操作的数的operator修改为
                    
                    if c == ')':
                        return sum(stack), i  # 只有碰到 ）才同时返回index, i 在上面已经加一了！！
                    
                    
                    
            return sum(stack)  # 整个结束计算 返回sum
        return dfs([], 0)
```

### 227. Basic Calculator II (与dfs 无关，为了比较放在这)

Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

Example 1:

Input: "3+2*2"
Output: 7
Example 2:

Input: " 3/2 "
Output: 1
Example 3:

Input: " 3+5 / 2 "
Output: 5
Note:

You may assume that the given expression is always valid.
Do not use the eval built-in library function.

```python3
class Solution:
    def calculate(self, s: str) -> int:
        stack = []
        operator = '+'
        num = 0
        s += '#'
        for c in s:
            if c == ' ':
                continue
            elif c.isdigit():
                num = num * 10 + int(c)
            else:
                if operator == '+':
                    stack.append(num)
                elif operator == '-':
                    stack.append(-num)
                elif operator == '*': 
                    stack.append(stack.pop() * num) 
                else:
                    stack.append(int(float(stack.pop()) / num) )
                num = 0
                operator = c
        return sum(stack)

```

### 224. Basic Calculator

Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

Example 1:

Input: "1 + 1"
Output: 2
Example 2:

Input: " 2-1 + 2 "
Output: 3
Example 3:

Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
Note:
You may assume that the given expression is always valid.
Do not use the eval built-in library function.

```python3
class Solution:
    def calculate(self, s: str) -> int:
                
        def dfs(stack, i):
            num = 0
            operator = '+'
            while i < len(s):
                c = s[i]
                if c == ' ':
                    i += 1
                    continue
                if c.isdigit():
                    num = num * 10 + int(c)
                    i += 1
                elif c == '(':
                    num, i = dfs([], i + 1)
                else:
                    if operator == '+':
                        stack.append(num)                      
                    elif operator == '-':
                        stack.append(-num)      
                        
                    i += 1
                    num = 0
                    operator = c  
                    
                    if c == ')':
                        return sum(stack), i
            return sum(stack)
                    
        s += '#'
        return dfs([], 0)
                
            
```

### 785. Is Graph Bipartite?

Given an undirected graph, return true if and only if it is bipartite.

Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: graph[i] is a list of indexes j for which the edge between nodes i and j exists.  Each node is an integer between 0 and graph.length - 1.  There are no self edges or parallel edges: graph[i] does not contain i, and it doesn't contain any element twice.

Example 1:
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation: 
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.
Example 2:
Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: false
Explanation: 
The graph looks like this:
0----1
| \  |
|  \ |
3----2
We cannot find a way to divide the set of nodes into two independent subsets.
 

Note:

graph will have length in range [1, 100].
graph[i] will contain integers in range [0, graph.length - 1].
graph[i] will not contain i or duplicate values.
The graph is undirected: if any element j is in graph[i], then i will be in graph[j].

```python3
# dfs node and assign to one set, return false when any connect node in the same set, else return true
# input: node as index, conn_nodes as a list
#  题目说两个group 不一定就要用两个set作为数据结构。就方便代码实现来说，用一个dic来记录color更合适
# 还有就是面试的时候不要被面试官带歪了 面试官反驳你的时候 要冷静思考反例 不要被带着走
# 面试时 只要确定了一种解法 就要自信地写下去 不要怀疑这种解法不对 或者不是最优
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        
        def dfs(node):
            for nei in graph[node]:   
                if nei in color_dic :
                    if color_dic[nei] ==  color_dic[node]:
                        return False
                else:
                    color_dic[nei] = 1 - color_dic[node]
                    if not dfs(nei):
                        return False                
            return True
        
        
        color_dic = {}
        for node in range(len(graph)):
            if node not in color_dic:
                color_dic[node] = 0
                if not dfs(node):
                    return False
        return True
```
