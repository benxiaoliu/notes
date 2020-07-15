Charpter one: sort 
quick sort (give logn space at most)
not stable [O(nlogn), get by master theory(used in recursive algorithm); space >= logn to store all partition positions] at about 2:00:00 of the vidoe

time complexity/Big O:
describes the amount of time it takes to run an algorithm. Time complexity is commonly estimated by counting the number of elementary operations(has no relationship with input size, like an add manipution a+b...) performed by the algorithm, then ignore lower..

stable: sequence among elements with the same value won't change  [why need: like sort by age first, and then sort by name. we need to sort based on keeping the first order]

swap two inegerters a, b efficiently:
a = a^b
b = a^b
a = a^b

mid position without overflow: mid = l + (r-l)/2

if x is a positive integer, a>>1 == a/2

get a random number among M ~ N:  
M + int((N-M+1)*Math.random())
Math.random() return a double number between 0-1



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
When need to cut down time complexity, always thinking about binary search method.
