###739. Daily Temperatures

Given a list of daily temperatures T, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put 0 instead.

For example, given the list of temperatures T = [73, 74, 75, 71, 69, 72, 76, 73], your output should be [1, 1, 4, 2, 1, 1, 0, 0].

Note: The length of temperatures will be in the range [1, 30000]. Each temperature will be an integer in the range [30, 100].

```java

class Solution {
    public int[] dailyTemperatures(int[] temperatures) {
        Stack<Integer> stack = new Stack<>();
        int[] res = new int[temperatures.length];
        for (int i = 0; i < temperatures.length; i++){
            int cur = temperatures[i];
            while (!stack.isEmpty() && cur > temperatures[stack.peek()]){
                int idx = stack.pop();
                res[idx] = i - idx;
            }
            stack.push(i);
        }
    return res;
    }
}
```

496. Next Greater Element I
===

You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

```java

class Solution {
    
    public int[] nextGreaterElement(int[] findNums, int[] nums) {
        Stack<Integer> stack = new Stack<>();
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums){
            while(!stack.isEmpty() && stack.peek() < num){
                map.put(stack.pop(), num);
            }
            stack.push(num);
        }
        for (int i = 0; i < findNums.length; i++){
            findNums[i] = map.getOrDefault(findNums[i], -1);
        }
        return findNums;
    }
}
```

150. Evaluate Reverse Polish Notation
====

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Note:

Division between two integers should truncate toward zero.
The given RPN expression is always valid. That means the expression would always evaluate to a result and there won't be any divide by zero operation.
Example 1:

Input: ["2", "1", "+", "3", "*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

```python3
'''
不能直接判断数字是不是0-9之间
小技巧，判断是不是操作符先

注意除跟减 两个数之间的的先后顺序
用python的话整数或者负整数相除/相整除得到的结果 想得到整数什么的 有一定的规则 不想记忆的话 在结果前加个int
'''
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        for string in tokens:
            if string not in ["+", "-", "*", "/"]:
                stack.append(int(string))
            else:
                second = stack.pop()
                first = stack.pop()
                if string == "+":
                    stack.append(first + second)
                elif string == "-":
                    stack.append(first - second)
                elif string == "*":
                    stack.append(first * second)
                else:
                    stack.append(int(first / second))
            
                    
        return stack[0]
                    
```


636. Exclusive Time of Functions
=====

On a single threaded CPU, we execute some functions.  Each function has a unique id between 0 and N-1.

We store logs in timestamp order that describe when a function is entered or exited.

Each log is a string with this format: "{function_id}:{"start" | "end"}:{timestamp}".  For example, "0:start:3" means the function with id 0 started at the beginning of timestamp 3.  "1:end:2" means the function with id 1 ended at the end of timestamp 2.

A function's exclusive time is the number of units of time spent in this function.  Note that this does not include any recursive calls to child functions.

The CPU is single threaded which means that only one function is being executed at a given time unit.

Return the exclusive time of each function, sorted by their function id.

 

Example 1:

![Avator](https://assets.leetcode.com/uploads/2019/04/05/diag1b.png)

Input:
n = 2
logs = ["0:start:0","1:start:2","1:end:5","0:end:6"]
Output: [3, 4]
Explanation:
Function 0 starts at the beginning of time 0, then it executes 2 units of time and reaches the end of time 1.
Now function 1 starts at the beginning of time 2, executes 4 units of time and ends at time 5.
Function 0 is running again at the beginning of time 6, and also ends at the end of time 6, thus executing for 1 unit of time. 
So function 0 spends 2 + 1 = 3 units of total time executing, and function 1 spends 4 units of total time executing.
 

Note:

1 <= n <= 100
Two functions won't start or end at the same time.
Functions will always log when they exit.


```python3
'''  
注意start 指该时间起始， end指该时间结束
巧用result数组 结合Id来记录，用pretime记录每次需要的上一次function时间
'''
class Solution(object):
    def exclusiveTime(self, n, logs):
        """
        :type n: int
        :type logs: List[str]
        :rtype: List[int]
        """
        res = [0] * n  # 因为结果要按Id排序，而且Each function has a unique id between 0 and N-1
        preTime = 0  # 用来
        stack = []  # 只放start的index
        
        for log in logs:
            indx, type, time = log.split(':')
            indx = int(indx)
            time = int(time)
            if type == 'start':  
                if stack:  # 前一个Id被挂起了 累加之前的时间并记录在result里面
                    res[stack[-1]] += time - preTime
                stack.append(indx)
                preTime = time
            else:
                res[stack[-1]] += time - preTime + 1  # 一定是该Id的一整段时间，或者最后一段时间
                stack.pop()
                preTime = time + 1
        return res
```
