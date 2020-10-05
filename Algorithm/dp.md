### 85. Maximal Rectangle

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

Example:

Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6

```java
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix: return 0

        m = len(matrix)
        n = len(matrix[0])

        left = [0] * n # initialize left as the leftmost boundary possible
        right = [n] * n # initialize right as the rightmost boundary possible
        height = [0] * n

        maxarea = 0

        for i in range(m):

            cur_left, cur_right = 0, n
            # update height
            for j in range(n):
                if matrix[i][j] == '1': height[j] += 1
                else: height[j] = 0
            # update left
            for j in range(n):
                if matrix[i][j] == '1': left[j] = max(left[j], cur_left)
                else:
                    left[j] = 0  # # don't include row above, calculate as wide as possible for this row
                    cur_left = j + 1
            # update right
            for j in range(n-1, -1, -1):
                if matrix[i][j] == '1': right[j] = min(right[j], cur_right)
                else:
                    right[j] = n  # don't include row above, calculate as wide as possible for this row
                    cur_right = j
            # update the area
            for j in range(n):
                maxarea = max(maxarea, height[j] * (right[j] - left[j]))

        return maxarea

```

### 10. Regular Expression Matching

Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

Note:

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like . or *.
Example 1:

Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
Example 2:

Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
Example 3:

Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
Example 4:

Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
Example 5:

Input:
s = "mississippi"
p = "mis*is*p*."
Output: false

```java
class Solution {
public boolean isMatch(String s, String p) {

    if (s == null || p == null) {
        return false;
    }
    boolean[][] dp = new boolean[s.length()+1][p.length()+1];
    dp[0][0] = true;
    for (int i = 0; i < p.length(); i++) {
        if (p.charAt(i) == '*' && dp[0][i-1]) {
            dp[0][i+1] = true;
        }
    }

    

    
    for (int i = 0 ; i < s.length(); i++) {
        for (int j = 0; j < p.length(); j++) {
            if (p.charAt(j) == '.') {
                dp[i+1][j+1] = dp[i][j];
            }
            if (p.charAt(j) == s.charAt(i)) {
                dp[i+1][j+1] = dp[i][j];
            }
            if (p.charAt(j) == '*') {
                if (p.charAt(j-1) != s.charAt(i) && p.charAt(j-1) != '.') {
                    dp[i+1][j+1] = dp[i+1][j-1];
                } else {
                    dp[i+1][j+1] = (dp[i+1][j] || dp[i][j+1] || dp[i+1][j-1]);
                }
            }
            
        }
    }
    return dp[s.length()][p.length()];
}
}
```

### 1143. Longest Common Subsequence


Given two strings text1 and text2, return the length of their longest common subsequence.

A subsequence of a string is a new string generated from the original string with some characters(can be none) deleted without changing the relative order of the remaining characters. (eg, "ace" is a subsequence of "abcde" while "aec" is not). A common subsequence of two strings is a subsequence that is common to both strings.

 

If there is no common subsequence, return 0.

 

Example 1:

Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
Example 2:

Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
Example 3:

Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
 

Constraints:

1 <= text1.length <= 1000
1 <= text2.length <= 1000
The input strings consist of lowercase English characters only.

```python3
'''
     text2 a c e
         0 1 2 3
text1 0  0 0 0 0
    a 1  0 1 1 1
    b 2  0 1 1 1
    c 3  0 1 2 2
    d 4  0 1 2 2
    e 5  0 1 2 3
'''

class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        # dp[i][j] reprents the length of the longest common subsequence for text1[:i] and text2[:j]
        m, n = len(text1) + 1, len(text2) + 1
        dp = [[0] * n for _ in range(m)]
        res = 0
        for i in range(1, m):  # start from one char
            for j in range(1, n):
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                else:
                    dp[i][j] = max(dp[i][j-1], dp[i-1][j])
                res = max(res, dp[i][j])                                                        
        return res
```



### 718. Maximum Length of Repeated Subarray

Given two integer arrays A and B, return the maximum length of an subarray that appears in both arrays.

Example 1:

Input:
A: [1,2,3,2,1]
B: [3,2,1,4,7]
Output: 3
Explanation: 
The repeated subarray with maximum length is [3, 2, 1].
 

Note:

1 <= len(A), len(B) <= 1000
0 <= A[i], B[i] < 100
```python3
'''
当A[I] == b[j] 时， 因为需要subarray，所以必须上一个字符相等 可匹配 ，当前case才能叠加上一步的common array

注意变量的声明 如果申明 dp = cur_dp = [0] * len_A, 修改dp[2] = 20, cur_dp[2]也会变成20，所以不能一起申明赋值
    A 1 0 0 0 1
    0 1 2 3 4 5
B 0 0 0 0 0 0 0
1 1 0 1 0 0 0 1
0 2 0 0 2 1 1 0
0 3 0 0 1 3 2 0
1 4 0 1 0 0 0 3
1 5 0 1 0 0 0 1

O(n) space, 7000ms...   
def findLength(self, A: List[int], B: List[int]) -> int:
        len_A = len(A) + 1
        len_B = len(B) + 1
        cur_dp = [0] * len_A
        dp = [0] * len_A
        res = 0
      
        for i in range(1, len_B):
            for j in range(1, len_A):
                if B[i-1] == A[j-1]:                   
                    cur_dp[j] = dp[j-1] + 1
                else:
                    cur_dp[j] = 0  # 此处也要修改，不然会保留原始值                
                res = max(res, cur_dp[j])           
            dp = cur_dp[:]
        return res
'''
# 6248ms
class Solution:
    def findLength(self, A: List[int], B: List[int]) -> int:
        len_A = len(A) + 1
        len_B = len(B) + 1
        res = 0
        dp = [[0] * len_A for _ in range(len_B)]
        for i in range(1, len_B):
            for j in range(1, len_A):
                if B[i-1] == A[j-1]:
                    dp[i][j] = dp[i-1][j-1] + 1
                    res = max(res, dp[i][j])
        return res
        
        
```


### 416. Partition Equal Subset Sum

Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

Note:

Each of the array element will not exceed 100.
The array size will not exceed 200.
 

Example 1:

Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
 

Example 2:

Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.

```python3
'''
0 1 2 3 4 5 6 7 8 9 10 11
  T
          T T
                        T
                       

'''
# 1176 ms
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        if sum(nums) % 2 != 0 or max(nums) > sum(nums) // 2:
            return False
        target = sum(nums) // 2
        
        dp = [False] * (target + 1)
        
        for num in nums:
            next_dp = dp[:]
            next_dp[num] = True
            for i in range(target):
                if dp[i] and i + num <= target:
                    next_dp[i + num] = True
            dp = next_dp[:]
            
        return dp[target]
    
    '''
    优化： 从后往前 不用担心改写掉需要使用的上一层dp, 避免了赋值来赋值去 
    但是需要考虑的点更多 不那么容易想到
    # 748ms
class Solution(object):
    def canPartition(self, nums):
        if sum(nums) % 2 != 0:
            return False
        target = sum(nums) / 2
        dp = [False for i in range(target + 1)]
        dp[0] = True
        for num in nums:
            for t in range(target,num-1, -1):
                    dp[t] = dp[t] or dp[t - num]
        
        return dp[target]
    '''
```


### 494. Target Sum

You are given a list of non-negative integers, a1, a2, ..., an, and a target, S. Now you have 2 symbols + and -. For each integer, you should choose one from + and - as its new symbol.

Find out how many ways to assign symbols to make sum of integers equal to target S.

Example 1:

Input: nums is [1, 1, 1, 1, 1], S is 3. 
Output: 5
Explanation: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

There are 5 ways to assign symbols to make the sum of nums be target 3.
 

Constraints:

The length of the given array is positive and will not exceed 20.
The sum of elements in the given array will not exceed 1000.
Your output answer is guaranteed to be fitted in a 32-bit integer.

```python3
'''
  -5 -4 -3 -2 -1 0 1 2 3 4 5
1              1   1
1           1    2   1
1        1     2   2   1
1       

因为不能存在index为-5的位置 所以整个数组整体往后移sum位
min = - sum(arr)
max = sum(arr)
w[i][j] 
index,target 


'''
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        
        Sum = sum(nums)
        # 注意考虑到这个corner case
        if S > Sum:
            return 0
        l = Sum * 2 + 1
        dp = [0 for _ in range(l)]
        dp[0 + Sum] = 1  # 为了让第一个数加入时 可以让dp[-num+Sum] 和dp[num] 等于1  
        for num in nums:
            next_dp = [0 for _ in range(l)]            
            for i, times in enumerate(dp):
                if times != 0:
                    next_dp[i-num] +=  times
                    next_dp[i+num] +=  times
                dp = next_dp  # 这里可以这么赋值 因为每一次都重新开辟了一个next_dp数组 不会操作覆盖
        return dp[S+Sum] 
       
```

### 1027. Longest Arithmetic Subsequence

Given an array A of integers, return the length of the longest arithmetic subsequence in A.

Recall that a subsequence of A is a list A[i_1], A[i_2], ..., A[i_k] with 0 <= i_1 < i_2 < ... < i_k <= A.length - 1, and that a sequence B is arithmetic if B[i+1] - B[i] are all the same value (for 0 <= i < B.length - 1).

Example 1:

Input: A = [3,6,9,12]
Output: 4
Explanation: 
The whole array is an arithmetic sequence with steps of length = 3.
Example 2:

Input: A = [9,4,7,2,10]
Output: 3
Explanation: 
The longest arithmetic subsequence is [4,7,10].
Example 3:

Input: A = [20,1,15,3,10,5,8]
Output: 4
Explanation: 
The longest arithmetic subsequence is [20,15,10,5].
 

Constraints:

2 <= A.length <= 1000
0 <= A[i] <= 500


```python
'''
抽两个数 得到等差 计算所有的组合
dp[(j, dif)]指的是以dif为等差， 截至（包含）A[j], 的最长等差序列
'''
class Solution:
    def longestArithSeqLength(self, A: List[int]) -> int:
        dp = {}
        for i in range(len(A)):
            for j in range(i + 1, len(A)):
                dif = A[j] - A[i]
                dp[(j, dif)] = dp.get((i, dif), 1) + 1
        return max(dp.values())
```

eBay OA 题库里的
maxArithmeticLength
Suppose we have array a and b (no duplicates & sorted) a = [0,4,8,20]
b = [5,7,12,16,22]
Suppose u can pick any number of element from b (could be 0), and u want to insert them into array a such that all elements in a are increasing by certain number,
so in this example u can pick "12, 16" from b and append into a such that a = [0,4,8,12,16,20], which increase by 4 for each elem​​​​​​​​​​​​​​​​​​​ent
write a function to return the maximum number of element in a after appending elements from b (in the exmaple above the result is 6), if there is no such case, return -1

```python 3

def merge(a, b):
   c = []
   while a or b:
       if not a:
           c += b
           break
       if not b:
           c += a
           break

       if a[0] < b[0]:
           c.append(a.pop(0))
       elif a[0] == b[0]:  # remove duplicate
           b.pop(0)
       else:
           c.append(b.pop(0))
   return c

def longestArithSeqLength(a, b):
   original_a = set(a)
   # merge a, b
   A = merge(a, b)
   dp = {}  # (index,dif):(length,{Arithmetic subarray}
   for i in range(len(A)):  # O(n)
       for j in range(i+1, len(A)):  # O(n)
           count, arithmetic_subarray = dp.get((i, A[j] - A[i]), (1, {A[i]}))
           _arithmetic_subarray = arithmetic_subarray.copy()  # 对象，防止修改之前的set
           _arithmetic_subarray.add(A[j])  # O(1)
           dp[(j, A[j] - A[i])] = (count + 1, _arithmetic_subarray)
           if A[j] in original_a:  # can not skip any number in original_a
               break
   re = -1
   for count, arithmetic_subarray in dp.values():
       if original_a.issubset(arithmetic_subarray):
           re = max(re, count)

   return re
```
