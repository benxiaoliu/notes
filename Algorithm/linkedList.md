146. LRU Cache
=====

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.

Follow up:
Could you do both operations in O(1) time complexity?

Example:

LRUCache cache = new LRUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.put(4, 4);    // evicts key 1
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4

```python3

'''
data structure:
CachedMap
can we change capacity or not? capacity 不直接操作capacity，通过与len(cachedMap)比较判断是否需删除头节点
firstNodeInDBLinkedList: use dummyhead, dummytail for easy manipulate

functions:

delete a node somewhere from the Doubly linked list

add to the end of the Doubly linked list
直接操作node 而不是key, value;这样才能操作时只new一个Node，并且拿到这个Node的引用来操作

get(key):
if key in CachedMap: delete(key), add(key, val)
else: return -1

put(key):
if key in CachedMap: delete(key) 
add(node), cachedMap[key] = node
if capacity < len(cachedMap): delete(firstNodeInDBLinkedList)

一定要时刻注意到几个变量之间的相互影响
'''
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.pre = None
        self.next = None
        
class LRUCache:
    def __init__(self, capacity):
        self.dummyHead = Node(-1, -1)
        self.dummyTail = Node(-1, -1)
        self.capacity = capacity
        
        self.dummyHead.next = self.dummyTail
        self.dummyTail.pre = self.dummyHead
        
        self.dummyTail.next = self.dummyHead
        self.dummyHead.pre = self.dummyTail
        self.cachedMap = dict()  # key to node
    
    def delete(self, node):
        pre = node.pre                  
        next = node.next
        pre.next = next
        next.pre = pre
        
        
    def add_to_end(self, node):
        pre = self.dummyTail.pre
        node.pre = pre
        node.next = self.dummyTail
        
        pre.next = node
        self.dummyTail.pre = node
        
    # just need to manipulate the linkedList
    def get(self, key):
        if key in self.cachedMap:
            node = self.cachedMap[key]
            self.delete(self.cachedMap[key])
            self.add_to_end(node)
            return node.value
        else:
            return -1
   
    def put(self, key, value):
        if key in self.cachedMap:
            self.delete(self.cachedMap[key]) 
        node = Node(key, value)                
        self.add_to_end(node)
        self.cachedMap[key] = node
        if self.capacity < len(self.cachedMap):
            head_node = self.dummyHead.next
            self.delete(head_node)
            del self.cachedMap[head_node.key]
```
