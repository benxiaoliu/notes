### 1526. Minimum Number of Increments on Subarrays to Form a Target Array
 
Given an array of positive integers target and an array initial of same size with all zeros.

Return the minimum number of operations to form a target array from initial if you are allowed to do the following operation:

Choose any subarray from initial and increment each value by one.
The answer is guaranteed to fit within the range of a 32-bit signed integer.
 

Example 1:

Input: target = [1,2,3,2,1]
Output: 3
Explanation: We need at least 3 operations to form the target array from the initial array.
[0,0,0,0,0] increment 1 from index 0 to 4 (inclusive).
[1,1,1,1,1] increment 1 from index 1 to 3 (inclusive).
[1,2,2,2,1] increment 1 at index 2.
[1,2,3,2,1] target array is formed.
Example 2:

Input: target = [3,1,1,2]
Output: 4
Explanation: (initial)[0,0,0,0] -> [1,1,1,1] -> [1,1,1,2] -> [2,1,1,2] -> [3,1,1,2] (target).
Example 3:

Input: target = [3,1,5,4,2]
Output: 7
Explanation: (initial)[0,0,0,0,0] -> [1,1,1,1,1] -> [2,1,1,1,1] -> [3,1,1,1,1] 
                                  -> [3,1,2,2,2] -> [3,1,3,3,2] -> [3,1,4,4,2] -> [3,1,5,4,2] (target).
Example 4:

Input: target = [1,1,1,1]
Output: 1
 

Constraints:

1 <= target.length <= 10^5
1 <= target[i] <= 10^5


```python3
'''
            [1,2,3,2,1]

diff array: [1,1,1,-1,-1]

presum: [1,3,6,8,9]

差分数组：a[i] - a[i-1]


对数组A[i:j]区间每个数都加k -> diff[i] + k,   diff[j] - k （j之后的数并不想+k, 所以要减回去）  -> 这样就能还原出一个符合要求的数组了
对[1,2,3,2,1] 区间[1:4] 每个元素都+1 ： 
diff [1,2,1,-1,-2]
还原数组 [1,3,4,3,1]


target = [1,2,3,2,1] -> 求diff -> [1,1,1,-1,-1]

若此题转化为求diff数组
[0,0,0,0,0] -> [1,1,1,-1,-1]

因为每次在一个区间连续+1 相当于在这个区间的第一个diff元素加一，结尾后面一个diff元素-1，此题中-1可选 因为我们最多可以把这个区间扩大到n， 所以 求diff里面positive数的和 就是答案



'''

class Solution:
    def minNumberOperations(self, target: List[int]) -> int:
        diff = [0] * len(target)
        diff[0] = target[0]
        for i, num in enumerate(target[1:], 1):
            diff[i] = target[i] - target[i-1]
            
        opr = 0
        for d in diff:
            if d > 0:
                opr += d
        return opr
            
```
