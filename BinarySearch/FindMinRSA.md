# 000. Name
Difficulty: Medium

## QUESTION

You are given an array of length `n` which was originally sorted in ascending order. It has now been rotated between `1` and `n` times. For example, the array `nums = [1,2,3,4,5,6]` might become:

- `[3,4,5,6,1,2]` if it was rotated `4` times.
- `[1,2,3,4,5,6]` if it was rotated `6` times.
- Notice that rotating the array `4` times moves the last four elements of the array to the beginning. Rotating the array `6` times produces the original array.

Assuming all elements in the rotated sorted array `nums` are unique, return the minimum element of this array.

A solution that runs in `O(n)` time is trivial, can you write an algorithm that runs in `O(log n) time?`

### EXAMPLE

```
Input: nums = [3,4,5,6,1,2]
Output: 1
```

```
Input: nums = [4,5,0,1,2,3]
Output: 0
```

```
Input: nums = [4,5,6,7]
Output: 4
```
## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List  # Importing List type hint from typing module

class Solution:
    def findMin(self, nums: List[int]) -> int:
        return min(nums)  # Using Python's built-in min() function to find and return the smallest element
```

### APPROACH: BINARY SEARCH

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def findMin(self, nums: List[int]) -> int:        
        res = nums[0]  # Initialize the result with the first element of the array
        l, r = 0, len(nums) - 1  # Set left and right pointers
        
        while l <= r:
            # If the subarray is already sorted, the leftmost element is the minimum
            if nums[l] < nums[r]:
                res = min(res, nums[l])
                break  # No need to search further
            
            m = (l + r) // 2  # Find the middle index
            res = min(res, nums[m])  # Update the minimum value found so far
            
            # If the middle element is greater than or equal to the left element,
            # it means the smallest value is in the right half
            if nums[m] >= nums[l]:
                l = m + 1  # Move the left pointer to the right half
            else:
                r = m - 1  # Otherwise, search in the left half
                
        return res  # Return the minimum element found
```

### APPROACH: BINARY SEARCH (LOWER BOUND)

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def findMin(self, nums: List[int]) -> int:
        """
        Function to find the minimum element in a rotated sorted array using binary search.

        :param nums: List[int] - A list of integers representing the rotated sorted array
        :return: int - The minimum element in the array
        """
        
        l, r = 0, len(nums) - 1  # Initialize left and right pointers
        
        while l < r:  # Continue searching until l and r converge
            m = l + (r - l) // 2  # Find the middle index
            
            # If the middle element is smaller than the rightmost element,
            # the minimum must be in the left half (including m)
            if nums[m] < nums[r]:
                r = m  # Move right pointer to m
            else:
                # If nums[m] >= nums[r], it means the minimum is in the right half
                l = m + 1  # Move left pointer to m + 1
        
        return nums[l]  # The left pointer now points to the minimum element
```
