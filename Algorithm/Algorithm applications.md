
Kadane's Algorithm (Maximum sub array problem)
===
```python
def max_subarray(A):
    max_ending_here = max_so_far = A[0]
    for x in A:
        max_ending_here = max(x, max_ending_here + x)
        max_so_far = max(max_so_far, max_ending_here)
    return max_so_far
```


```python
local_max = global_max = nums[0]
        for i in range(1,len(nums)):
            local_max = max(local_max + nums[i], nums[i])
            global_max = max(global_max, local_max)
        return global_max
```        

Binary search
===
When need to cut down time complexity, also thinking about binary search method.
