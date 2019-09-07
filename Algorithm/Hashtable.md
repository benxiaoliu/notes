939. Minimum Area Rectangle

Given a set of points in the xy-plane, determine the minimum area of a rectangle formed from these points, with sides parallel to the x and y axes.

If there isn't any rectangle, return 0.

 

Example 1:

Input: [[1,1],[1,3],[3,1],[3,3],[2,2]]
Output: 4

Example 2:

Input: [[1,1],[1,3],[3,1],[3,3],[4,1],[4,3]]
Output: 2
 

Note:

1 <= points.length <= 500
0 <= points[i][0] <= 40000
0 <= points[i][1] <= 40000
All points are distinct.
```python

# Put all the points in a set. For each pair of points, if the associated rectangle are 4 distinct points all in the set, then take the area of this rectangle as a candidate answer.

class Solution(object):
    def minAreaRect(self, points):
        S = set(map(tuple, points))
        ans = float('inf')
        for j, p2 in enumerate(points):
            for i in xrange(j):  # 罗列可能的对角线上的点的组合
                p1 = points[i]
                if (p1[0] != p2[0] and p1[1] != p2[1] and  # 对角线上两个点的坐标包含了构成长方形的四个点可能出现的所有坐标值
                        (p1[0], p2[1]) in S and (p2[0], p1[1]) in S):
                    ans = min(ans, abs(p2[0] - p1[0]) * abs(p2[1] - p1[1]))
        return ans if ans < float('inf') else 0
 ```
 
 380. Insert Delete GetRandom O(1)

Design a data structure that supports all following operations in average O(1) time.

insert(val): Inserts an item val to the set if not already present.
remove(val): Removes an item val from the set if present.
getRandom: Returns a random element from current set of elements. Each element must have the same probability of being returned.
Example:

// Init an empty set.
RandomizedSet randomSet = new RandomizedSet();

// Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomSet.insert(1);

// Returns false as 2 does not exist in the set.
randomSet.remove(2);

// Inserts 2 to the set, returns true. Set now contains [1,2].
randomSet.insert(2);

// getRandom should return either 1 or 2 randomly.
randomSet.getRandom();

// Removes 1 from the set, returns true. Set now contains [2].
randomSet.remove(1);

// 2 was already in the set, so return false.
randomSet.insert(2);

// Since 2 is the only number in the set, getRandom always return 2.
randomSet.getRandom();

```python
class RandomizedSet(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.dataList = []
        self.dataMap = {}

    def insert(self, val):
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        :type val: int
        :rtype: bool
        """
        if val in self.dataMap:
            return False
        self.dataMap[val] = len(self.dataList)
        self.dataList.append(val)
        return True
    
        

    def remove(self, val):
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        :type val: int
        :rtype: bool
        """
        if val not in self.dataMap:
            return False
        
        idx = self.dataMap[val]
        tail = self.dataList.pop()
        if idx < len(self.dataList):  # 当要删除的刚好是最后一个元素时 不需要以下操作
            self.dataList[idx] = tail
            self.dataMap[tail] = idx
        self.dataMap.pop(val)
        return True
        

    def getRandom(self):
        """
        Get a random element from the set.
        :rtype: int
        """
        
        return random.choice(self.dataList)  # random time complexity O(1)
        


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```
49. Group Anagrams

Given an array of strings, group anagrams together.

Example:

Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
Note:

All inputs will be in lowercase.
The order of your output does not matter.

```python

class Solution(object):
    def groupAnagrams(self, strs):
        d = {}
        for s in strs:
            key = tuple(sorted(s))  # 注意这里有tuple
            d[key] = d.get(key, []) + [s]
        return d.values()
```

582. Kill Process

Given n processes, each process has a unique PID (process id) and its PPID (parent process id). 
Each process only has one parent process, but may have one or more children processes. This is just like a tree structure. Only one process has PPID that is 0, which means this process has no parent process. All the PIDs will be distinct positive integers.
We use two list of integers to represent a list of processes, where the first list contains PID for each process and the second list contains the corresponding PPID. 
Now given the two lists, and a PID representing a process you want to kill, return a list of PIDs of processes that will be killed in the end. You should assume that when a process is killed, all its children processes will be killed. No order is required for the final answer.
Example 1:
Input: 
pid =  [1, 3, 10, 5]
ppid = [3, 0, 5, 3]
kill = 5
Output: [5,10]
Explanation: 
           3
         /   \
        1     5
             /
            10
Kill 5 will also kill 10.

Note:
The given kill id is guaranteed to be one of the given PIDs.
n >= 1.

```python
class Solution(object):
    def killProcess(self, pid, ppid, kill):
        res = []
        childs = collections.defaultdict(list)
        for i in xrange(len(ppid)):
            childs[ppid[i]].append(pid[i])
        q = [kill]
        while q:
            p = q.pop(0)
            res.append(p)
            for child in childs[p]:
                q.append(child)
        return res

```
895. Maximum Frequency Stack

Implement FreqStack, a class which simulates the operation of a stack-like data structure.
FreqStack has two functions:
push(int x), which pushes an integer x onto the stack.
pop(), which removes and returns the most frequent element in the stack. 
If there is a tie for most frequent element, the element closest to the top of the stack is removed and returned.
 
Example 1:
Input: 
["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
Output: [null,null,null,null,null,null,null,5,7,5,4]
Explanation:
After making six .push operations, the stack is [5,7,5,7,4,5] from bottom to top.  Then:

pop() -> returns 5, as 5 is the most frequent.
The stack becomes [5,7,5,7,4].

pop() -> returns 7, as 5 and 7 is the most frequent, but 7 is closest to the top.
The stack becomes [5,7,5,4].

pop() -> returns 5.
The stack becomes [5,7,4].

pop() -> returns 4.
The stack becomes [5,7].
 
Note:
Calls to FreqStack.push(int x) will be such that 0 <= x <= 10^9.
It is guaranteed that FreqStack.pop() won't be called if the stack has zero elements.
The total number of FreqStack.push calls will not exceed 10000 in a single test case.
The total number of FreqStack.pop calls will not exceed 10000 in a single test case.
The total number of FreqStack.push and FreqStack.pop calls will not exceed 150000 across all test cases.

```python
class FreqStack(object):
    def __init__(self):
        self.num2freq = collections.defaultdict(int)
        self.freq2num = collections.defaultdict(list)
        self.maxfreq = 0
        
    def push(self, x):
        self.num2freq[x] += 1
        self.freq2num[self.num2freq[x]].append(x)
        self.maxfreq = max(self.maxfreq, self.num2freq[x])

    def pop(self):
        num = self.freq2num[self.maxfreq].pop()
        if not self.freq2num[self.maxfreq]:
            self.maxfreq -= 1
        self.num2freq[num] -= 1
        return num
```

299. Bulls and Cows

You are playing the following Bulls and Cows game with your friend: You write down a number and ask your friend to guess what the number is. Each time your friend makes a guess, you provide a hint that indicates how many digits in said guess match your secret number exactly in both digit and position (called "bulls") and how many digits match the secret number but locate in the wrong position (called "cows"). Your friend will use successive guesses and hints to eventually derive the secret number.
Write a function to return a hint according to the secret number and friend's guess, use A to indicate the bulls and B to indicate the cows. 
Please note that both secret number and friend's guess may contain duplicate digits.
Example 1:
Input: secret = "1807", guess = "7810"

Output: "1A3B"

Explanation: 1 bull and 3 cows. The bull is 8, the cows are 0, 1 and 7.
Example 2:
Input: secret = "1123", guess = "0111"

Output: "1A1B"

Explanation: The 1st 1 in friend's guess is a bull, the 2nd or 3rd 1 is a cow.
Note: You may assume that the secret number and your friend's guess only contain digits, and their lengths are always equal.

```python
class Solution(object):
    def getHint(self, secret, guess):
        """
        :type secret: str
        :type guess: str
        :rtype: str
        """
        bulls = cows = 0
        dic_sec = collections.defaultdict(int)
        for s, g in zip(secret, guess):
            if s == g:
                bulls += 1
            else:
                dic_sec[s] += 1
        for i, g in enumerate(guess):
            if secret[i] != g and dic_sec[g] > 0:
                cows += 1
                dic_sec[g] -= 1
        return str(bulls) + 'A' + str(cows) + 'B'
```
