1229. Meeting Scheduler

Given the availability time slots arrays slots1 and slots2 of two people and a meeting duration duration, return the earliest time slot that works for both of them and is of duration duration.

If there is no common time slot that satisfies the requirements, return an empty array.

The format of a time slot is an array of two elements [start, end] representing an inclusive time range from start to end.  

It is guaranteed that no two availability slots of the same person intersect with each other. That is, for any two time slots [start1, end1] and [start2, end2] of the same person, either start1 > end2 or start2 > end1.

 
Example 1:

Input: slots1 = [[10,50],[60,120],[140,210]], slots2 = [[0,15],[60,70]], duration = 8
Output: [60,68]
Example 2:

Input: slots1 = [[10,50],[60,120],[140,210]], slots2 = [[0,15],[60,70]], duration = 12
Output: []

```python
class Solution:
    def minAvailableDuration(self, slots1: List[List[int]], slots2: List[List[int]], duration: int) -> List[int]:
        slots1.sort()
        slots2.sort()
        i, j = 0, 0
        while i < len(slots1) and j < len(slots2):
            s = max(slots1[i][0], slots2[j][0])
            e = min(slots1[i][1], slots2[j][1])
            
            if s + duration <= e:
                return [s, s + duration]
            elif slots1[i][1] < slots2[j][1]:
                i += 1
            else:
                j += 1
        return []
        
        
```

2. Add Two Numbers

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example:

Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

```python3
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
'''注意随时把carry带上，注意每次cur = cur.next,之前还要确保cur不是None'''
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        cur = head = ListNode()
        over = 0
        while l1 or l2 or over:
            a = l1.val if l1 else 0
            b = l2.val if l2 else 0
            if a+b+over >= 10:
                value = (a+b+over) % 10 
                over = int((a+b+over) / 10)
            else:
                value = a+b+over
                over = 0
            cur.next = ListNode(value, None)
            cur = cur.next
            if l1: l1 = l1.next 
            if l2: l2 = l2.next 
        return head.next
```
