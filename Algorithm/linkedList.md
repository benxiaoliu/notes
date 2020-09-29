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

### 460. LFU Cache

Design and implement a data structure for Least Frequently Used (LFU) cache. It should support the following operations: get and put.

get(key) - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
put(key, value) - Set or insert the value if the key is not already present. When the cache reaches its capacity, it should invalidate the least frequently used item before inserting a new item. For the purpose of this problem, when there is a tie (i.e., two or more keys that have the same frequency), the least recently used key would be evicted.

Note that the number of times an item is used is the number of calls to the get and put functions for that item since it was inserted. This number is set to zero when the item is removed.

 

Follow up:
Could you do both operations in O(1) time complexity?

 

Example:

LFUCache cache = new LFUCache( 2 /* capacity */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // returns 1
cache.put(3, 3);    // evicts key 2
cache.get(2);       // returns -1 (not found)
cache.get(3);       // returns 3.
cache.put(4, 4);    // evicts key 1.
cache.get(1);       // returns -1 (not found)
cache.get(3);       // returns 3
cache.get(4);       // returns 4
```python3
'''
frequency:  doublely linkedlist (newly come, insert to the head)
hashMap:    key: Node
Node{key, val, fre, pre, next}


get:
key in cacheMap: ***delete from original fre_dblist(if == min_fre and fre_dblist becomes None after delete: min_fre + 1) , append to the head of fre+1_dblist***

key not in cache: return -1


put:
if key in cacheMap: ***delete from original fre_dblist(if == min_fre and fre_dblist becomes None after delete: min_fre + 1) , change the val and append to the head of fre+1_dblist***

if key not in cacheMap: add new Node to fre_1_dblist and  % cacheMap %, change min_fre = 1
    if len(cacheMap) > capacity: delete first node in minfreq_dblist (if == min_fre and fre_dblist becomes None after delete: min_fre + 1)
    
corner case: capacity == 0 : cannot put


common operations:
1. insert a node to the head of a dblinked list
2. delete a node from a dblinked list
3. *** update ***


["LFUCache","put","put","get","put","get","get","put","get","get","get"]
[[2],       [1,1], [2,2],[1],  [3,3],[2],  [3],  [4,4],[1],  [3],  [4]]

Output:     [null, null, 1,    null,  2,    3,   null,  -1,   3,    4]
Expected:   [null, null, 1,    null, -1,    3,   null,  -1,   3,    4]
'''


class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.freq = 1
        self.pre = None
        self.next = None
        
class DLinkedList:
    def __init__(self):
        self.dummyHead = Node(-1, -1)
        self.dummyTail = Node(-1, -1)
        self.dummyHead.next = self.dummyTail
        self.dummyHead.pre = self.dummyTail
        self.dummyTail.next = self.dummyHead
        self.dummyTail.pre = self.dummyHead
        
        
        # dummyHead - ... - dummyTail
        # dummyHead - node - ... - dummyTail
    def addToHead(self, node):
        node.next = self.dummyHead.next
        node.pre = self.dummyHead
        
        node.next.pre = node
        self.dummyHead.next = node
        
    def delete(self, node):
        pre = node.pre
        next = node.next
        
        pre.next = next
        next.pre = pre       
        
        return node  # so we can delete from cacheMap
        
            
class LFUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.key_to_node = {}
        self.freq_to_dbList = collections.defaultdict(DLinkedList)
        self.min_freq = 0
    # ***delete from original fre_dblist(if == min_fre and fre_dblist becomes None after delete: min_fre + 1) , change the val and append to the head of fre+1_dblist***
    def update(self, node):
        freq = node.freq
        self.freq_to_dbList[freq].delete(node)
        
        if self.min_freq == freq and self.freq_to_dbList[freq].dummyHead.next == self.freq_to_dbList[freq].dummyTail:
            self.min_freq += 1  # visit one more time
        
        node.freq += 1
        

        self.freq_to_dbList[node.freq].addToHead(node)
        
    def get(self, key: int) -> int:
        if key not in self.key_to_node:
            return -1
        node  = self.key_to_node[key]
        self.update(node)
        return node.val

    def put(self, key: int, value: int) -> None:
        if self.capacity == 0:
            return
        if key in self.key_to_node:
            node = self.key_to_node[key]
            self.update(node)
            node.val = value
        else:            
            # delete node when reach capacity
            if len(self.key_to_node) == self.capacity:
                tail_node = self.freq_to_dbList[self.min_freq].dummyTail.pre
                node = self.freq_to_dbList[self.min_freq].delete(tail_node)
                
                del self.key_to_node[node.key]
                
                if self.freq_to_dbList[self.min_freq].dummyHead.next == self.freq_to_dbList[self.min_freq].dummyTail:
                    del self.freq_to_dbList[self.min_freq]
                    
            node = Node(key, value)
            self.key_to_node[key] = node
            
            
            self.freq_to_dbList[1].addToHead(node)
            self.min_freq = 1
        
                


# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```
