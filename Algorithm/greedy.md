
621. Task Scheduler

Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer n that represents the cooldown period between two same tasks (the same letter in the array), that is that there must be at least n units of time between any two same tasks.

Return the least number of units of times that the CPU will take to finish all the given tasks.

Example 1:

Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: 
A -> B -> idle -> A -> B -> idle -> A -> B
There is at least 2 units of time between any two same tasks.
Example 2:

Input: tasks = ["A","A","A","B","B","B"], n = 0
Output: 6
Explanation: On this case any permutation of size 6 would work since n = 0.
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
And so on.
Example 3:

Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
Output: 16
Explanation: 
One possible solution is
A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A
 

Constraints:

1 <= task.length <= 104
tasks[i] is upper-case English letter.
The integer n is in the range [0, 100].


```python3
'''
算出frequency最高的task, 假设没有其他的任务给他间隔 那CPU最多会idle (max_freq - 1) * colddown_period
然后greedy地把剩下的任务按frequency高到低依次放到 空闲时间中, 每个任务只能在某间隔中放一次（除非最高frequency有多个并列的 不然不会出现需要放超过一次的情况）
依次放到这之间的任务相隔一定会大于等于colddown period 
如
A[][]A[][]A
A[B][]A[B][]A

如果max frequency的colddown period不够放其他的任务，增大cold period就可以了
比如 AAABBBCCCDDD n = 2
A[B][C]A[B][C]ABC
没地方放D->增大colddown period(不会违反题目要求)
A[B][C][]A[B][C][]ABC -> A[B][C][D]A[B][C][D]ABCD
tasks = ["A","A","A","B","B","B"], n = 2
A[][]A[][]A
A[B][]A[B][]A
A[B][]A[B][]A

max = (3-1) * 2 = 4
idle_time = 4 - 2  = 2
return 6 + 2
output : 8
'''
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        frequencies = [0] * 26  # 26个字母
        
        for task in tasks:
            frequencies[ord(task) - ord('A')] += 1
        frequencies.sort()
        max_f = frequencies.pop()
        max_idle = (max_f - 1) * n
        
        while frequencies and max_idle > 0:
            max_idle -= min(max_f - 1, frequencies.pop())
        return len(tasks) + max(max_idle, 0)
               
```



### 787. Cheapest Flights Within K Stops

There are n cities connected by m flights. Each flight starts from city u and arrives at v with a price w.

Now given all the cities and flights, together with starting city src and the destination dst, your task is to find the cheapest price from src to dst with up to k stops. If there is no such route, output -1.

Example 1:
Input: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 1
Output: 200
Explanation: 
The graph looks like this:

![Avator](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.
Example 2:
Input: 
n = 3, edges = [[0,1,100],[1,2,100],[0,2,500]]
src = 0, dst = 2, k = 0
Output: 500
Explanation: 
The graph looks like this:

![Avator](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png)

The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.
 

Constraints:

The number of nodes n will be in range [1, 100], with nodes labeled from 0 to n - 1.
The size of flights will be in range [0, n * (n - 1) / 2].
The format of each flight will be (src, dst, price).
The price of each flight will be in the range [1, 10000].
k is in the range of [0, n - 1].
There will not be any duplicated flights or self cycles.


```python
# notice this is a directed graph
# greedy 

'''
def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        graph = collections.defaultdict(list)
        min_heap = [[0, src, -1]]  # cost, node, stops
        for src, dst, price in flights:  # !!!!!注意命名规范 这样不对 会把输入的dst改变 导致整个要求的东西都改变了...
            graph[src].append([dst, price])
        while min_heap:
            cost, node, stops = heapq.heappop(min_heap)
            if node == dst and stops <= K:
                return cost
            
            stops += 1
            if stops <= K and node in graph:
                for nei, price in graph[node]:
                    heapq.heappush(min_heap, [cost + price, nei, stops])
        return -1
'''
class Solution:
    
    def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, K: int) -> int:
        graph = collections.defaultdict(list)
        min_heap = [[0, src, -1]]  # cost, node, stops
        for frm, to, price in flights:
            graph[frm].append([to, price])
        while min_heap:
            cost, node, stops = heapq.heappop(min_heap)
            if node == dst and stops <= K:
                return cost
            
            stops += 1
            if stops <= K:
                for nei, price in graph[node]:
                    heapq.heappush(min_heap, [cost + price, nei, stops])
        return -1
                
```
