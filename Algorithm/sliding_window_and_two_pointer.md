### 3. Longest Substring Without Repeating Characters

Given a string s, find the length of the longest substring without repeating characters.

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
Example 4:

Input: s = ""
Output: 0
 

Constraints:

0 <= s.length <= 5 * 104
s consists of English letters, digits, symbols and spaces.

```python3
'''
一直维持当前的substring没有重复的字符 一旦遇到 并且大于等于！！当前substring index！！！就将substring的start变为该重复字母上一次的index（记录在hashmap中，每个元素都要保持更新）
res每次与当前substring比较 最后得到最大值
'''
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        usedChar = {}
        res, start = 0, 0
        for i, c in enumerate(s):
            if c in usedChar and usedChar[c] >= start:
                start = usedChar[c] + 1
            usedChar[c] = i
            res = max(res, i - start + 1)
        return res                              
```

### 159. Longest Substring with At Most Two Distinct Characters

Given a string s , find the length of the longest substring t  that contains at most 2 distinct characters.

Example 1:

Input: "eceba"
Output: 3
Explanation: t is "ece" which its length is 3.
Example 2:

Input: "ccaabbb"
Output: 5
Explanation: t is "aabbb" which its length is 5.

```python3
# 考虑清楚再开始写代码 不确定之前不要确定idea
# 一个练习 哪些操作需要放在if条件之内，哪些都需要 的好例子

class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: str) -> int:
        index_map = {}  # {a: 3, b: }
        left = 0  # 2
        right = 0 # 3
        res = 0  # 4

        # "ccaabbbddd" 5

        for i, c in enumerate(s):  # 0, e; ...
            index_map[c] = i
            right = i
            if len(index_map) > 2:                          
                del_idx = min(index_map.values())  
                left = del_idx + 1
                del_c = s[del_idx]
                del index_map[del_c]
            res = max(res, right - left + 1)

        return res
```
how amy ways:
divide array into 3 parts where each previous part is smaller or equal to the next part

```python3
def findCount(arr, n):
    # Stores the prefix sums
    prefix_sum = [0 for x in range(n)]
    prefix_sum[0] = arr[0]

    for i in range(1, n):
        prefix_sum[i] = prefix_sum[i - 1] + arr[i]

        # Stores the suffix sums
    suffix_sum = [0 for x in range(n)]

    suffix_sum[n - 1] = arr[n - 1]

    for i in range(n - 2, -1, -1):
        suffix_sum[i] = suffix_sum[i + 1] + arr[i]

    s = 1
    e = 2
    curr_subarray_sum = arr[s]
    count = 0
    # curr_subarray_sum : include arr[s], not include arr[e]   arr[s:e]
    # Traverse the given array
    while (s <= n - 2 and e <= n - 1):
            #2 3 1 7
            #2 7 1 7
        # Updating curr_subarray_sum until
        # it is less than prefix_sum[s-1]
        while (e <= n - 1 and curr_subarray_sum < prefix_sum[s - 1]):
            curr_subarray_sum += arr[e-1]
            e += 1
        # increasing end
        while e <= n - 1 and curr_subarray_sum <= suffix_sum[e]:
            # Increase count
            count += 1
            curr_subarray_sum += arr[e-1]
            e += 1
        if e == n or curr_subarray_sum > suffix_sum[e]:
            curr_subarray_sum -= arr[e-1]
            e -= 1

        # Decrease curr_subarray_sum by arr[s[]
        curr_subarray_sum -= arr[s]
        s += 1

    return count
```

