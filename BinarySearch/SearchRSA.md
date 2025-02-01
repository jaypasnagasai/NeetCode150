# 000. Search in Rotated Sorted Array
Difficulty: Medium

## QUESTION

You are given an array of length `n` which was originally sorted in ascending order. It has now been rotated between `1` and `n` times. For example, the array `nums = [1,2,3,4,5,6]` might become:

- `[3,4,5,6,1,2]` if it was rotated `4` times.
- `[1,2,3,4,5,6]` if it was rotated `6` times.

Given the rotated sorted array `nums` and an integer `target`, return the index of `target` within `nums`, or `-1` if it is not present.

You may assume all elements in the sorted rotated array `nums` are unique,

A solution that runs in `O(n)` time is trivial, can you write an algorithm that runs in `O(log n) time`?

### EXAMPLE

```
Input: nums = [3,4,5,6,1,2], target = 1
Output: 4
```

```
Input: nums = [3,5,6,0,1,2], target = 4
Output: -1
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def search(self, nums: List[int], target: int) -> int:        
        # Iterate through the array
        for i in range(len(nums)):
            if nums[i] == target:  # If the target is found, return the index
                return i
        
        return -1  # Return -1 if the target is not found in the array
```

### APPROACH: BINARY SEARCH

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1  # Initialize left and right pointers

        # Step 1: Find the pivot (the index of the smallest element)
        while l < r:
            m = (l + r) // 2  # Find the middle index
            if nums[m] > nums[r]:  
                l = m + 1  # The minimum must be in the right half
            else:
                r = m  # The minimum could be at mid or in the left half
        
        pivot = l  # The index of the smallest element (pivot point)
        
        # Step 2: Perform binary search in the left and right subarrays
        def binary_search(left: int, right: int) -> int:
            while left <= right:
                mid = (left + right) // 2  # Find the middle index
                if nums[mid] == target:
                    return mid  # Target found
                elif nums[mid] < target:
                    left = mid + 1  # Search in the right half
                else:
                    right = mid - 1  # Search in the left half
            return -1  # Target not found
        
        # Step 3: Search in the two sorted halves
        result = binary_search(0, pivot - 1)  # Search in the left part (before pivot)
        if result != -1:
            return result  # If found, return the index
        
        return binary_search(pivot, len(nums) - 1)  # Otherwise, search in the right part (after pivot)
```

### APPROACH: BINARY SEARCH (TWO PASS)

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1  # Initialize left and right pointers

        # Step 1: Find the pivot (the index of the smallest element)
        while l < r:
            m = (l + r) // 2  # Find the middle index
            if nums[m] > nums[r]:  
                l = m + 1  # The minimum must be in the right half
            else:
                r = m  # The minimum could be at mid or in the left half
        
        pivot = l  # The index of the smallest element (pivot point)

        # Step 2: Reset left and right pointers
        l, r = 0, len(nums) - 1

        # Step 3: Determine which half to search in
        if nums[pivot] <= target <= nums[r]:  # Target is in the right half
            l = pivot
        else:  # Target is in the left half
            r = pivot - 1

        # Step 4: Perform binary search in the chosen half
        while l <= r:
            m = (l + r) // 2  # Find the middle index
            if nums[m] == target:
                return m  # Target found
            elif nums[m] < target:
                l = m + 1  # Search in the right half
            else:
                r = m - 1  # Search in the left half

        return -1  # Target not found
```

### APPROACH: BINARY SEARCH (ONE PASS)

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        l, r = 0, len(nums) - 1  # Initialize left and right pointers

        while l <= r:
            mid = (l + r) // 2  # Find the middle index
            
            # Step 1: Check if the middle element is the target
            if target == nums[mid]:
                return mid  # Target found at index mid

            # Step 2: Determine which half is sorted
            if nums[l] <= nums[mid]:  # Left half is sorted
                # Check if target lies within the sorted left half
                if target > nums[mid] or target < nums[l]:  
                    l = mid + 1  # Search in the right half
                else:
                    r = mid - 1  # Search in the left half

            else:  # Right half is sorted
                # Check if target lies within the sorted right half
                if target < nums[mid] or target > nums[r]:  
                    r = mid - 1  # Search in the left half
                else:
                    l = mid + 1  # Search in the right half

        return -1  # Target not found
```
