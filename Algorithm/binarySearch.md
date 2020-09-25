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
        return upper_bound
      
```
