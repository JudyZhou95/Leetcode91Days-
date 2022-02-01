# Shortest-Cycle-Containing-Target-Node
https://binarysearch.com/problems/Shortest-Cycle-Containing-Target-Node

## Solution
```python
class Solution:
    def solve(self, graph, target):
        visited = set()
        q = [target]
        level = 0

        while q:
            l = len(q)
            for i in range(l):
                cur = q.pop(0)
                visited.add(cur)
                for neighbor in graph[cur]:
                    if neighbor not in visited:
                        q.append(neighbor)
                    elif neighbor == target:
                        return level+1
            level += 1

        return -1
```

## Analysis

Starting from target node, using BFS to traverse in the graph. When first time the path goes back to target node. It is the shortest path containing target, because BFS guarantees the first path found is the shortest one.\
Time Complexity: O(V+E)\
Space Complexity: O(V)
