template
===
```python
dfs():
    if ...
        return
    for ...
        dfs()
```
491. Increasing Subsequences
Medium
Given an integer array, your task is to find all the different possible increasing subsequences of the given array, and the length of an increasing subsequence should be at least 2.

Example:

Input: [4, 6, 7, 7]

Output: [[4, 6], [4, 7], [4, 6, 7], [4, 6, 7, 7], [6, 7], [6, 7, 7], [7,7], [4,7,7]]
Note:
The length of the given array will not exceed 15.
The range of integer in the given array is [-100,100].
The given array may contain duplicates, and two equal integers should also be considered as a special case of increasing sequence.
```python
class Solution(object):
    def findSubsequences(self, nums):
        
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        
        res = []
        self.helper(nums, 0, [], res)
        return res
    
    
    def helper(self, nums, index, temp, res):
        if len(temp) >= 2:
              res.append(temp[:])
                
        used = {}

        for i in xrange(index, len(nums)):
            if len(temp) > 0 and temp[-1] > nums[i]:
                continue
            if nums[i] in used: continue  # to avoid the same search: 4 6 7; 4 6 7 
            
            used[nums[i]] = True
            self.helper(nums, i + 1, temp+[nums[i]], res)
        
```

140. Word Break II
Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, add spaces in s to construct a sentence where each word is a valid dictionary word. Return all such possible sentences.

Note:

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.

Example 1:

Input:
s = "catsanddog"
wordDict = ["cat", "cats", "and", "sand", "dog"]
Output:
[
  "cats and dog",
  "cat sand dog"
]

Example 2:

Input:
s = "pineapplepenapple"
wordDict = ["apple", "pen", "applepen", "pine", "pineapple"]
Output:
[
  "pine apple pen apple",
  "pineapple pen apple",
  "pine applepen apple"
]
Explanation: Note that you are allowed to reuse a dictionary word.


Example 3:

Input:
s = "catsandog"
wordDict = ["cats", "dog", "sand", "and", "cat"]
Output:
[]
```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: List[str]
        """
        
        def helper(s, wordDict, memo):
            
            if s in memo: return memo[s]
            if not s: return []
            res = []
            for word in wordDict:
                if not s.startswith(word):
                    continue
                if len(s) == len(word):
                    res.append(word)
                else:
                    resultOfTheRest = helper(s[len(word):], wordDict, memo)
                    for item in resultOfTheRest:
                        item = word + ' ' + item
                        res.append(item)
            memo[s] = res
            return res
        return helper(s, wordDict, {})
        
```

46. Permutations
Given a collection of distinct integers, return all possible permutations.

Example:

Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        res = []
        self.dfs(nums, [], res)
        return res

    def dfs(self, nums, path, res):
        if not nums:
            res.append(path)
            return # backtracking
        for i in xrange(len(nums)):
            self.dfs(nums[:i]+nums[i+1:], path+[nums[i]], res)
```
47. Permutations II
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

Example:

Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]

```python

class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort()
        res = []
        def dfs(nums, path, res):
            if len(nums) == 0:
                res.append(path)
                return
            for i in range(len(nums)):
                if i > 0 and nums[i-1] == nums[i]:
                    continue
                dfs(nums[:i]+nums[i+1:], path + [nums[i]], res)
        dfs(nums, [], res)
        return res
```

131. Palindrome Partitioning

Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

Example:

Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```python
class Solution(object):
    def partition(self, s):
        """
        :type s: str
        :rtype: List[List[str]]
        """
        res = []
        self.helper(s, res, [])
        return res
    
    def isPal(self, s):
        return s == s[::-1]
    
    def helper(self, s, res, cur):
        if not s:
            res.append(cur)
            return
        for i in xrange(1, len(s) + 1):
            if self.isPal(s[:i]):
                self.helper(s[i:], res, cur + [s[:i]])

```

282. Expression Add Operators

Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

Example 1:
Input: num = "123", target = 6
Output: ["1+2+3", "1*2*3"] 

Example 2:
Input: num = "232", target = 8
Output: ["2*3+2", "2+3*2"]

Example 3:
Input: num = "105", target = 5
Output: ["1*0+5","10-5"]

Example 4:
Input: num = "00", target = 0
Output: ["0+0", "0-0", "0*0"]

Example 5:
Input: num = "3456237490", target = 9191
Output: []

```python
class Solution(object):
    def addOperators(self, num, target):
        """
        :type num: str
        :type target: int
        :rtype: List[str]
        """
        res = []
        self.helper(num, target, res, '', 0, 0, 0)
        return res
    
    def helper(self, num, target, res, path, pos, totalVal, pre_val):
        if pos == len(num):
            if target == totalVal:
                res.append(path)
            return
        
        for i in xrange(pos, len(num)):
            if num[pos] == '0' and i != pos:
                break  #  105 , 0 as a single digit is acceptable but not acceptable as 05
            
            curVal = int(num[pos: i+1])
            if pos == 0:
                self.helper(num, target, res, str(curVal), i + 1, curVal, curVal)
            else:
                self.helper(num, target, res, path + '+' + str(curVal), i + 1, totalVal + curVal, curVal)
                self.helper(num, target, res, path + '-' + str(curVal), i + 1, totalVal - curVal, -curVal)
                self.helper(num, target, res, path + '*' + str(curVal), i + 1, totalVal - pre_val + pre_val*curVal, pre_val*curVal)
```

489. Robot Room Cleaner

Given a robot cleaner in a room modeled as a grid.

Each cell in the grid can be empty or blocked.

The robot cleaner with 4 given APIs can move forward, turn left or turn right. Each turn it made is 90 degrees.

When it tries to move into a blocked cell, its bumper sensor detects the obstacle and it stays on the current cell.

Design an algorithm to clean the entire room using only the 4 given APIs shown below.

interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}

Example:

Input:
room = [
  [1,1,1,1,1,0,1,1],
  [1,1,1,1,1,0,1,1],
  [1,0,1,1,1,1,1,1],
  [0,0,0,1,0,0,0,0],
  [1,1,1,1,1,1,1,1]
],
row = 1,
col = 3

Explanation:
All grids in the room are marked by either 0 or 1.
0 means the cell is blocked, while 1 means the cell is accessible.
The robot initially starts at the position of row=1, col=3.
From the top left corner, its position is one row below and three columns right.
Notes:

The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the mentioned 4 APIs, without knowing the room layout and the initial robot's position.
The robot's initial position will always be in an accessible cell.
The initial direction of the robot will be facing up.
All accessible cells are connected, which means the all cells marked as 1 will be accessible by the robot.
Assume all four edges of the grid are all surrounded by wall.

```python

# """
# This is the robot's control interface.
# You should not implement it, or speculate about its implementation
# """
#class Robot(object):
#    def move(self):
#        """
#        Returns true if the cell in front is open and robot moves into the cell.
#        Returns false if the cell in front is blocked and robot stays in the current cell.
#        :rtype bool
#        """
#
#    def turnLeft(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def turnRight(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def clean(self):
#        """
#        Clean the current cell.
#        :rtype void
#        """

class Solution(object):
    def cleanRoom(self, robot):
        """
        :type robot: Robot
        :rtype: None
        """
        dir = ([-1, 0], [0, 1], [1, 0], [0, -1])  # 只要相对是顺序不变就行 哪个列表在第一个无所谓。表示每次向右转然后走一步的方向/表示向四个不同的方向走一步
        
        def dfs(i, j, cleaned, cur_dir):
            robot.clean()
            cleaned.add((i, j))
            
            for k in xrange(4):
                x, y = dir[(cur_dir+k) % 4]  # 和下面的turnRight 对应， 下面turnRight，所以要记录cur_dir才知道turn_right是哪个方向
                if (i+x, j+y) not in cleaned and robot.move():  # 这里前进了一步 后面就要返回
                    dfs(i+x, j+y, cleaned, (cur_dir+k) % 4)
                    robot.turnLeft()  # 返回原来位置
                    robot.turnLeft()
                    robot.move()
                    robot.turnLeft()
                    robot.turnLeft() 
        
                robot.turnRight()  # 旋转90度 到下一个方向
        
        dfs(0, 0, set(), 0)  # 开始位置设置为0， 0 相对位置

 ```
