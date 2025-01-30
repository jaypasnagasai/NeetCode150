# 704. Binary Search
Difficulty: Easy

## QUESTION

You are given an array of distinct integers `nums`, sorted in ascending order, and an integer `target`.

Implement a function to search for `target` within `nums`. If it exists, then return its index, otherwise, return `-1`.

Your solution must run in O(logn) time.

### EXAMPLE

```
Input: nums = [-1,0,2,4,6,8], target = 4
Output: 3
```

```
Input: nums = [-1,0,2,4,6,8], target = 3
Output: -1
```

## SOLUTION


### APPROACH: RECURSIVE BINARY SEARCH

```python
class Solution:
    # Define a recursive binary search method.
    def binary_search(self, l: int, r: int, nums: List[int], target: int) -> int:
        # Base case: If left index exceeds right index, the target is not found.
        if l > r:
            return -1

        # Compute the middle index to divide the search space.
        m = l + (r - l) // 2

        # If the middle element is the target, return its index.
        if nums[m] == target:
            return m

        # If the target is greater, search in the right half.
        if nums[m] < target:
            return self.binary_search(m + 1, r, nums, target)

        # Otherwise, search in the left half.
        return self.binary_search(l, m - 1, nums, target)

    # Define a wrapper function that calls the recursive binary search.
    def search(self, nums: List[int], target: int) -> int:
        return self.binary_search(0, len(nums) - 1, nums, target)
```

### APPROACH: ITERATIVE BINARY SEARCH

```python
class Solution:
    # Define a method 'search' to perform binary search on a sorted list.
    def search(self, nums: List[int], target: int) -> int:
        # Initialize left and right pointers.
        l, r = 0, len(nums) - 1

        # Continue searching while the left pointer does not cross the right pointer.
        while l <= r:
            # Compute the middle index to avoid potential integer overflow.
            m = l + ((r - l) // 2)  

            # If the middle element is greater than the target, search in the left half.
            if nums[m] > target:
                r = m - 1
            # If the middle element is smaller than the target, search in the right half.
            elif nums[m] < target:
                l = m + 1
            # If the middle element is the target, return its index.
            else:
                return m
        
        # If the target is not found, return -1.
        return -1
```

### APPROACH: BUILT-IN FUNCTION

```python
import bisect

class Solution:
    # Define a method 'search' to find the target index using the 'bisect' module.
    def search(self, nums: List[int], target: int) -> int:
        # Use 'bisect_left' to find the index where 'target' should be inserted.
        index = bisect.bisect_left(nums, target)

        # If the index is within bounds and matches the target, return the index.
        return index if index < len(nums) and nums[index] == target else -1
```
