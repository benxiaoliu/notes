739. Daily Temperatures
=====

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
