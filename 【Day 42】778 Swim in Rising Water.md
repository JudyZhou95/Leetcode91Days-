# 778. Swim in Rising Water

You are given an n x n integer matrix grid where each value grid[i][j] represents the elevation at that point (i, j).

The rain starts to fall. At time t, the depth of the water everywhere is t. You can swim from a square to another 4-directionally adjacent square if and only if the elevation of both squares individually are at most t. You can swim infinite distances in zero time. Of course, you must stay within the boundaries of the grid during your swim.

Return the least time until you can reach the bottom right square (n - 1, n - 1) if you start at the top left square (0, 0).

**example 1**\
Input: grid = [[0,2],[1,3]]\
Output: 3\
Explanation:\
At time 0, you are in grid location (0, 0).\
You cannot go anywhere else because 4-directionally adjacent neighbors have a higher elevation than t = 0.\
You cannot reach point (1, 1) until time 3.\
When the depth of water is 3, we can swim anywhere inside the grid.

## Solution
```python
class Solution:
    def swimInWater(self, grid: List[List[int]]) -> int:                           
        m = len(grid)
        n = len(grid[0])
        
        def valid(time, x, y):
            if x < 0 or y < 0 or x >= m or y >= n:
                return False
            if (x, y) in visited:
                return False
            
            if grid[x][y] > time:
                return False
            
            return True
        
        def enough(time, x, y):
            
            if not valid(time, x, y):
                #print("not valid")
                return False
            
            if x == m-1 and y == n-1:
                return True

            visited.add((x, y))
            for xx, yy in [(x+1, y), (x-1, y), (x, y+1), (x, y-1)]:            
                if enough(time, xx, yy):
                    return True                    
            
            return False

        l = 0
        r = max([max(vec) for vec in grid])
        
        while l <= r:
            mid = l + (r-l)//2
            visited = set()
            if enough(mid, 0, 0):
                r = mid - 1
            else:
                l = mid + 1
        return l
```     
## Analysis
Use binary search to search through [0, max elevation in grid], and each time test if the current time can make it to the right bottom corner. The trick part is how to check the curerent time is possible to make it. We use a helper function which uses DFS try to traverse from (0, 0) to (m-1, n-1) whenever the elevation of node in searching path is larger than time, we break this path. \
**Time Complexity**: O(M x N x logMAX(grid)) \
**Space Complexity**: O(M x N) visited set

