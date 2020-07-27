Charpter one: sort 
quick sort (give logn space at most)
not stable [O(nlogn), get by master theory(used in recursive algorithm); space >= logn to store all partition positions] at about 2:00:00 of the vidoe

time complexity/Big O:
describes the amount of time it takes to run an algorithm. Time complexity is commonly estimated by counting the number of elementary operations(has no relationship with input size, like an add manipution a+b...) performed by the algorithm, then ignore lower..
if just use logn space, partition can't make sure quick sort is stable.

stable: sequence among elements with the same value won't change  [why need: like sort by age first, and then sort by name. we need to sort based on keeping the first order]

when data size is large, choose quick sort. 
when data size is small, like < 60, since other sort methods like insert sort has less elementary operations, we choose to use insert sort or ...

Master theorem: used to compute time complexity of recurssion algorithm
T(n) = aT(n/b) + O(n ** d)

merge sort: TC: nlogn; SC: n (原地缓存法可以实现常数级别 论文级别不要求掌握) 
In java, when you pass an array of primitive numbers to sort, it uses quick sort; but if you pass an array of objects, it will use merge sort for stability.

Chartpter2 30:00
Chartpter2 37:00  recurrsion to iteration
41:00 why not use recerrsion in actual work: three reasons

Charpter two 01:16:00 
heap and heap sort
TC: nlogn, SC: O(1) use index computation, left child = index*2+1, right child = index*2+2, parent = (index-1)/2
elemental operations is bigger than quick sort

swap two inegerters a, b efficiently:
a = a^b
b = a^b
a = a^b

mid position without overflow: mid = l + (r-l)/2

if x is a positive integer, a>>1 == a/2

get a random number among M ~ N:  
M + int((N-M+1)*Math.random())
Math.random() return a double number between 0-1

Charpter two 02:16:00 comparator (tell how to compare)； problems based on Bucket sort.  (watch again after finish java videos!!)

Charpter two 03:00:00  KMP algorithm: if string1 contain string2, return the begin index in string1
每个子字符串 前缀跟后缀的最大匹配

charpter three 17:36     manacher algorithm 在一个字符串中 找它最长的回文子串
子串，子数组必须连续 子序列可以不连续
在每个字符两边加入特殊字符串 解决奇偶回文问题 #1#2#3#2#1#
记录最又回文边界来加速

charpter three 01:46:15 bfprt算法 一个数组中O(n)求第k小/大的数
先选中位数的中位数作为划分值

charpter three 02:35:06 基数排序  0，1，2...9十个先进先出队列桶，按个位，十位，百位...各自入桶出桶，最后结果就是有序的
charpter three 03:00:06 希尔排序： 先选择比较大的步长 递减到1 以实现步长为1时需要交换的次数较小  O(nlogn)
插入排序是步长位1的希尔排序

charpter three 03:10:00 关于栈跟队列的题
charpter three 3:34:00用两个栈实现队列的两个原则： 1. 倒push栈的时候必须全部倒完 2.只有pop栈为空的时候才能往里倒
charpter three 3:35:00 用两个队列实现栈， 每次要pop的时候，把前面所有元素入另外一个队列，最后一个不出，返回， 然后swap queue跟help数组的引用。每次都这样。
charpter four  00:07:00  hash函数
hash函数 当数据量大的时候 会均分
一致性哈希：即均匀分布 又自由增删结点
虚拟节点+路由表（机器对应的虚拟结点， 用来确定放在哪台机器上） 环上放虚拟结点。 解决增减机器负载均衡问题
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
