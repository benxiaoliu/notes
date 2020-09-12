union find model
====

```python3

'''
n is the number of nodes
if 1 < value of nodes <= n
Krustal O(ElogE)

rank[node]: the longth depth of node's children
'''

        parent = [node for node in range(n + 1)]
        rank = [0 for node in range(n + 1)]
        
        def find(node):
            if node != parent[node]:
                parent[node] = find(parent[node])
            return parent[node]

        def union(node1, node2):
            parent1, parent2 = find(node1), find(node2)
            #if parent1 == parent2: return 0
            if rank[parent1] > rank[parent2]:
                parent[parent2] = parent1
            elif rank[parent1] == rank[parent2]:
                parent[parent2] = parent1
                rank[parent1] += 1
            else:
                parent[parent1] = parent2 
            
            #return 1
```

1579. Remove Max Number of Edges to Keep Graph Fully Traversable

Alice and Bob have an undirected graph of n nodes and 3 types of edges:

Type 1: Can be traversed by Alice only.
Type 2: Can be traversed by Bob only.
Type 3: Can by traversed by both Alice and Bob.
Given an array edges where edges[i] = [typei, ui, vi] represents a bidirectional edge of type typei between nodes ui and vi, find the maximum number of edges you can remove so that after removing the edges, the graph can still be fully traversed by both Alice and Bob. The graph is fully traversed by Alice and Bob if starting from any node, they can reach all other nodes.

Return the maximum number of edges you can remove, or return -1 if it's impossible for the graph to be fully traversed by Alice and Bob.

Example 1:
![Avator]https://assets.leetcode.com/uploads/2020/08/19/ex1.png
Input: n = 4, edges = [[3,1,2],[3,2,3],[1,1,3],[1,2,4],[1,1,2],[2,3,4]]
Output: 2
Explanation: If we remove the 2 edges [1,1,2] and [1,1,3]. The graph will still be fully traversable by Alice and Bob. Removing any additional edge will not make it so. So the maximum number of edges we can remove is 2.

```python3
'''
n is the number of nodes
if 1 < value of nodes <= n
Krustal O(ElogE)

rank[node]: the longth depth of node's children
'''

class Solution:
    def maxNumEdgesToRemove(self, n, edges):
        # Union find
        def find(node):
            if node != parent[node]:
                parent[node] = find(parent[node])
            return parent[node]

        def union(node1, node2):
            parent1, parent2 = find(node1), find(node2)
            if parent1 == parent2: return 0
            if rank[parent1] > rank[parent2]:
                parent[parent2] = parent1
            elif rank[parent1] == rank[parent2]:
                parent[parent2] = parent1
                rank[parent1] += 1
            else:
                parent[parent1] = parent2 
            
            return 1

        res = union_times_A = union_times_B = 0

        # Alice and Bob
        parent = [node for node in range(n + 1)]
        rank = [0 for node in range(n + 1)]
        
        for t, node1, node2 in edges:
            if t == 3:
                if union(node1, node2):
                    union_times_A += 1
                    union_times_B += 1
                else:
                    res += 1
        parent0 = parent[:]  # Alice union will change the parent array, keep origin for Bob

        # only Alice
        for t, node1, node2 in edges:
            if t == 1:
                if union(node1, node2):
                    union_times_A += 1
                else:
                    res += 1

        # only Bob
        parent = parent0
        for t, node1, node2 in edges:
            if t == 2:
                if union(node1, node2):
                    union_times_B += 1
                else:
                    res += 1
# only if Alice and Bob both union n-1 times, the graph is connected for both of them
        return res if union_times_A == union_times_B == n - 1 else -1
```
