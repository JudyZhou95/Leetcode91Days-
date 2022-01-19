# Minimum Light Radius

https://binarysearch.com/problems/Minimum-Light-Radius

## Description

You are given a list of integers nums representing coordinates of houses on a 1-dimensional line. You have 3 street lights that you can put anywhere on the coordinate line and a light at coordinate x lights up houses in [x - r, x + r], inclusive. Return the smallest r required such that we can place the 3 lights and all the houses are lit up.

**Constraints**\
n ≤ 100,000 where n is the length of nums

**Example 1**\
Input: nums = [3, 4, 5, 6]\
Output: 0.5

**Explanation**\
If we place the lamps on 3.5, 4.5 and 5.5 then with r = 0.5 we can light up all 4 houses.

## Solution
```python
class Solution:
    def solve(self, nums):
        nums.sort()

        N = len(nums)
        if N <= 3:
            return 0

        LIGHTS = 3

        def enough(diameter):
            start = nums[0]
            end = start + diameter

            for i in range(LIGHTS):
                #bisect_right: the rightmost place in the sorted list to insert the given element
                idx = bisect_right(nums, end)
                if idx >= N:
                    return True               
                start = nums[idx]
                end = start + diameter
            return False

        l, r = 0, nums[-1] - nums[0]
        while l <= r:
            mid = l + (r-l)//2
            if enough(mid):
                r = mid-1
            else:
                l = mid + 1
            
        return l/2
```
## Analysis 
It ask for the minimum possible radius. One way to solve it is that binary search among the possible radius space, define a function to test if the input radius is enough to cover all the houses, and find the minimum radius which is enough. The difficult part is how to define this helper function. It "bisect_right" each light's cover range one by one into the sorted array of houses to see which houses are covered by each light. The cover range always starts with the next to be covered house coordinate and ends with the coordinate+diameter. 

**Time Complexity**: Sort the nums array is O(nlogn). Main binary search loop is O(log(max(nums)-min(nums))), bisect_right inside the helper function is O(logn). Overall is O(nlogn + logn*log(max(nums)-min(nums))\
**Space Complexity**: O(n) (Depends on the space used in sorting algorithm).

