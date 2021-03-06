### 模板：

```python3
nums = [1,1,1,2,2,2,3,3,3]
theFirstEqualTo = 2
theLastEqualTo = 2

def lowerBound (nums, theFirstEqualTo):
    if not nums:
        return None
    left, right = 0, len(nums) - 1  # 现在还不确定left, right 跟 theFirstEqualTo的大小关系 下面会判断 并且不会错过判断任何一个数
    while left < right:
        mid = (left + right) // 2  # 整除 向下取整 总是取偏小的一个
        if nums[mid] < theFirstEqualTo:  # 只有发现比要找的数小了 才移动左边界 这样保证不会错过第一个与target相等的数
            left = mid + 1
        else:  # nums[mid] >= theFirstEqualTo
            right = mid
    return left if nums[left] == theFirstEqualTo else -1  # 或者return right if nums[right] == theFirstEqualTo else -1

def upperBound(nums, theLastEqualTo):
    if not nums:
        return None
    left, right = 0, len(nums) - 1
    while left < right:
        mid = (left + right + 1) // 2  # 求最后一个等于target的，得总是取较大的一个index了
        if nums[mid] > theLastEqualTo:  # 只有当发现比要找的数大了 才移动右边界 这样保证不会错过最后一个与target相等的数
            right = mid - 1
        else:
            left = mid
    return left if nums[left] == theLastEqualTo else -1  # 或者return right if nums[right] == theFirstEqualTo else -1

# if the array is sorted and each element in the array is unique, or it's not unique but you can return anyone
def findInUnique(nums, target):
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = (left + right) // 2
        if nums[mid] == target:
            return mid
        elif target > nums[mid] :
            left = mid + 1
        else:
            right = mid - 1
    return -1
    
print(lowerBound(nums, theLastEqualTo))  # 3
print(upperBound(nums, theLastEqualTo))  # 5
print(findInUnique([0,1,2,3,4,5,6,7],5))  # 5
print(findInUnique([0,1,2,3,4,5,5,5,6,7],5))  # 7



```
### 704. Binary Search
只是找target 不管上下界 用哪一个模板都可以
Given a sorted (in ascending order) integer array nums of n elements and a target value, write a function to search target in nums. If target exists, then return its index, otherwise return -1.


Example 1:

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4

Example 2:

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
 

Note:

You may assume that all elements in nums are unique.
n will be in the range [1, 10000].
The value of each element in nums will be in the range [-9999, 9999].

```python3
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left < right:
            mid = (left + right) // 2
            if nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        return left if nums[left] == target else -1
        
```
或者
```python3
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        
        while left < right:
            mid = (left + right + 1) // 2
            if nums[mid] > target:
                right = mid - 1
            else:
                left = mid
        return left if nums[left] == target else -1
```

### 875. Koko Eating Bananas

Koko loves to eat bananas.  There are N piles of bananas, the i-th pile has piles[i] bananas.  The guards have gone and will come back in H hours.

Koko can decide her bananas-per-hour eating speed of K.  Each hour, she chooses some pile of bananas, and eats K bananas from that pile.  If the pile has less than K bananas, she eats all of them instead, and won't eat any more bananas during this hour.

Koko likes to eat slowly, but still wants to finish eating all the bananas before the guards come back.

Return the minimum integer K such that she can eat all the bananas within H hours.

 

Example 1:

Input: piles = [3,6,7,11], H = 8
Output: 4
Example 2:

Input: piles = [30,11,23,4,20], H = 5
Output: 30
Example 3:

Input: piles = [30,11,23,4,20], H = 6
Output: 23
 

Constraints:

1 <= piles.length <= 10^4
piles.length <= H <= 10^9
1 <= piles[i] <= 10^9

```python3
class Solution:
    def minEatingSpeed(self, piles: List[int], H: int) -> int:
        if len(piles) > H:
            return False
        upper_bound = max(piles)
        lower_bound = 1
        
        
        def hoursNeeded(eat_per_hour):
            hours = 0
            for pile in piles:
                hours += math.ceil(pile / eat_per_hour)  # 取小数的上界
            return hours
        
        
        while upper_bound > lower_bound:
            mid = (upper_bound + lower_bound) // 2
            need_hour = hoursNeeded(mid)
            if need_hour > H:
                lower_bound = mid + 1
            else:
                upper_bound = mid
        return lower_bound
        
        
```


1011. Capacity To Ship Packages Within D Days

A conveyor belt has packages that must be shipped from one port to another within D days.

The i-th package on the conveyor belt has a weight of weights[i].  Each day, we load the ship with packages on the conveyor belt (in the order given by weights). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within D days.

 

Example 1:

Input: weights = [1,2,3,4,5,6,7,8,9,10], D = 5
Output: 15
Explanation: 
A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed. 
Example 2:

Input: weights = [3,2,2,4,1,4], D = 3
Output: 6
Explanation: 
A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4
Example 3:

Input: weights = [1,2,3,1,1], D = 4
Output: 3
Explanation: 
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
 

Constraints:

1 <= D <= weights.length <= 50000
1 <= weights[i] <= 500

```python3
class Solution:
    def shipWithinDays(self, weights: List[int], D: int) -> int:
        def daysNeeded(convey_per_day):
            days = 0
            cur_sum = 0
            for weight in weights:
                if cur_sum < convey_per_day and cur_sum + weight <= convey_per_day:
                    cur_sum += weight
                else:
                    days += 1
                    cur_sum = weight
            return days + 1  # 最后一天没有算
        
        
        
        lower_bound = max(weights)
        upper_bound = sum(weights)
        
        while lower_bound < upper_bound:
            mid = lower_bound + (upper_bound - lower_bound) // 2
            need_day = daysNeeded(mid)
          
            if need_day > D:
                lower_bound = mid + 1
            else:
                upper_bound = mid
        return lower_bound
      
```


### eBay phone screen 

find a string in an sorted array with "" here and there
["", apple, "", "", "", box, "", "", "card", ""]
target = box
return 5


```python3

'''
if find "" :
    move right and left at the same time to find the first non-empty string as mid position
if words[mid] == target: return mid
elif words[mid] > target: right = mid - 1
else: left = mid + 1
'''

def searchStr(words, target):
    l, r = 0, len(words) - 1  # 0, 9
    pre_l, pre_r = 0, len(words) - 1
    while l <= r:  
        mid = (l + r) // 2  # 4, 2
        left, right = mid - 1, mid + 1
        if words[mid] == "":
            # 同时判断left和right, 哪个先不为空就把哪个设为mid!!!!  注意这种逻辑怎么写
            while right <= r and left >= l:
                if words[right] != "":
                    mid = right
                    break
                if words[left] != "":
                    mid = left
                    break
                right += 1
                left -= 1

        if words[mid]  == target:
            return mid
        elif words[mid]  < target:
            l = mid + 1
        else:
            r = mid - 1  # 4
    return -1
```

1552. Magnetic Force Between Two Balls

In universe Earth C-137, Rick discovered a special form of magnetic force between two balls if they are put in his new invented basket. Rick has n empty baskets, the ith basket is at position[i], Morty has m balls and needs to distribute the balls into the baskets such that the minimum magnetic force between any two balls is maximum.

Rick stated that magnetic force between two different balls at positions x and y is |x - y|.

Given the integer array position and the integer m. Return the required force.

 

Example 1:

![Avatar](https://assets.leetcode.com/uploads/2020/08/11/q3v1.jpg)

Input: position = [1,2,3,4,7], m = 3
Output: 3
Explanation: Distributing the 3 balls into baskets 1, 4 and 7 will make the magnetic force between ball pairs [3, 3, 6]. The minimum magnetic force is 3. We cannot achieve a larger minimum magnetic force than 3.
Example 2:

Input: position = [5,4,3,2,1,1000000000], m = 2
Output: 999999999
Explanation: We can use baskets 1 and 1000000000.
 

Constraints:

n == position.length
2 <= n <= 10^5
1 <= position[i] <= 10^9
All integers in position are distinct.
2 <= m <= position.length


```python3
# 求能放下m个球的最大的距离（force）---> 求upper bound

class Solution:
    def maxDistance(self, position: List[int], m: int) -> int:
        position.sort()
        left, right = 0, position[-1]
        
        def canPutAll(force):
            count = 1  # fill the first basket at first
            pre = position[0]
            for f in position[1:]:
                if f - pre >= force:
                    count += 1
                    pre = f
            return True if count >= m else False
                
            
            
        while left < right:
            mid = (left + right + 1) // 2
            if not canPutAll(mid):
                right = mid - 1
            else:
                left = mid
        return left

```

### 33. Search in Rotated Sorted Array

You are given an integer array nums sorted in ascending order, and an integer target.

Suppose that nums is rotated at some pivot unknown to you beforehand (i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

If target is found in the array return its index, otherwise, return -1.

Example 1:

Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
Example 2:

Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
Example 3:

Input: nums = [1], target = 0
Output: -1
 

Constraints:

1 <= nums.length <= 5000
-10^4 <= nums[i] <= 10^4
All values of nums are unique.
nums is guranteed to be rotated at some pivot.
-10^4 <= target <= 10^4

```python3
'''
find the side which is sorted first, then decide which part to search, combine same operation in else condition
'''

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        n = len(nums) - 1
        left, right = 0, len(nums) - 1  
        while left <= right:          
            mid = (left + right) // 2  
            if nums[mid] == target:
                return mid
            if nums[mid] > nums[right]:  # mid is in not sorted part                
                if nums[left] <= target < nums[mid]: # in this case, if we want a smaller one, it can be in both parts, but if we want a bigger one, it must be in the right part
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return -1

```


### 81. Search in Rotated Sorted Array II

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,0,1,2,2,5,6] might become [2,5,6,0,0,1,2]).

You are given a target value to search. If found in the array return true, otherwise return false.

Example 1:

Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
Example 2:

Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
Follow up:

This is a follow up problem to Search in Rotated Sorted Array, where nums may contain duplicates.
Would this affect the run-time complexity? How and why?

```python3
'''
[1,1,1,1,2,2,3,3,3,0,0,1,1]  有重复元素的时候 只有这一种特殊情况需要考虑怎么处理 重复元素在其他地方不影响
'''

class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return True
            if nums[mid] > nums[right]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1
            elif nums[mid] == nums[right]:
                temp = mid
                while temp < right and nums[temp] == nums[right]:
                    temp += 1
                if temp == right:
                    right = mid - 1
                else:
                    left = mid + 1          
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return False
```

### 153. Find Minimum in Rotated Sorted Array

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

You may assume no duplicate exists in the array.

Example 1:

Input: [3,4,5,1,2] 
Output: 1
Example 2:

Input: [4,5,6,7,0,1,2]
Output: 0

```python3
'''
最小的数的特点: 比前一个数小
'''
class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if mid - 1 > -1 and nums[mid] < nums[mid - 1]:
                return nums[mid]
            if nums[mid] > nums[-1]:
                left = mid + 1
            else:
                right = mid - 1
        return nums[0]  # 如果没有pivot或者只有一个数
```

154. Find Minimum in Rotated Sorted Array II

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

The array may contain duplicates.

Example 1:

Input: [1,3,5]
Output: 1
Example 2:

Input: [2,2,2,0,1]
Output: 0
Note:

This is a follow up problem to Find Minimum in Rotated Sorted Array.
Would allow duplicates affect the run-time complexity? How and why?

```python3
'''
[3,1,1,1,1]
[2,2,2,0,0,1,2,2,2]
'''

'''
[3,1,1,1,1]
[2,2,2,0,0,1,2,2,2]
'''

class Solution:
    def findMin(self, nums: List[int]) -> int:
        left, right = 0, len(nums) - 1  # 0，0
        while left < right:  # 因为不一定存在，并且里面没有跳出条件 所以这边不能等于，以防死循环
            mid = (left + right) // 2  # 0
            if mid - 1 > -1 and nums[mid-1] > nums[mid]:
                return nums[mid]
            if nums[mid] > nums[-1]:
                left = mid + 1
            elif nums[mid] == nums[-1]:
                temp = mid
                while temp < len(nums) and nums[temp] == nums[-1]:
                    temp += 1
                if temp == len(nums):
                    right = mid  # 不能等于mid - 1,有可能会跳过答案 如 [3,1,1,1,1]
                else:
                    left = temp - 1
            else:
                right = mid
        if left - 1 >= 0 and nums[left-1] > nums[left]:
            return nums[left]
        else:
            return nums[0]
        
        
```
