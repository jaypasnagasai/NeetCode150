# 287. Find the Duplicate Number
Difficulty: Medium

## QUESTION

You are given an array of integers `nums` containing `n + 1` integers. Each integer in `nums` is in the range `[1, n]` inclusive.

Every integer appears exactly once, except for one integer which appears two or more times. Return the integer that appears more than once.

### EXAMPLE

```
Input: nums = [1,2,3,2,2]
Output: 2
```

```
Input: nums = [1,2,3,4,4]
Output: 4
```

## SOLUTION


### APPROACH: SORTING

```python
from typing import List  # Import List for type hinting

class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        """
        Finds the duplicate number in an array containing n+1 integers 
        where each integer is in the range [1, n] inclusive.

        This implementation **sorts** the array and checks adjacent elements for duplicates.

        :param nums: List of integers with one duplicate.
        :return: The duplicate number.
        """
        
        nums.sort()  # Step 1: Sort the array (O(n log n) time complexity)
        
        # Step 2: Scan through the sorted list to find the duplicate
        for i in range(len(nums) - 1):
            if nums[i] == nums[i + 1]:  # If two consecutive numbers are the same, it's the duplicate
                return nums[i]
        
        return -1  # In case no duplicate is found (this should never happen per problem constraints)
```

### APPROACH: NEGATIVE MARKING

```python
from typing import List  # Import List for type hinting

class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        """
        Finds the duplicate number in an array containing n+1 integers 
        where each integer is in the range [1, n] inclusive.

        This implementation modifies the input array by marking visited indices as negative.

        :param nums: List of integers with one duplicate.
        :return: The duplicate number.
        """
        
        for num in nums:
            idx = abs(num) - 1  # Convert value to index (1-based index converted to 0-based)
            
            if nums[idx] < 0:  # If already negative, the index has been visited -> Duplicate found
                return abs(num)
            
            nums[idx] *= -1  # Mark the index as visited by negating the value
        
        return -1  # If no duplicate is found (shouldn't happen per problem constraints)
```

### APPROACH: FAST AND SLOW POINTERS

```python
from typing import List  # Import List for type hinting

class Solution:
    def findDuplicate(self, nums: List[int]) -> int:
        """
        Finds the duplicate number in an array containing n+1 integers 
        where each integer is in the range [1, n] inclusive.

        This implementation uses **Floyd’s Cycle Detection Algorithm** (Tortoise and Hare approach).

        :param nums: List of integers with one duplicate.
        :return: The duplicate number.
        """
        
        # Step 1: Detect cycle using slow and fast pointers
        slow, fast = 0, 0
        while True:
            slow = nums[slow]  # Move slow pointer one step
            fast = nums[nums[fast]]  # Move fast pointer two steps
            
            if slow == fast:  # If they meet, a cycle is detected
                break

        # Step 2: Find the entrance to the cycle (duplicate number)
        slow2 = 0  # Start a new pointer from index 0
        while True:
            slow = nums[slow]  # Move both slow and slow2 one step
            slow2 = nums[slow2]
            
            if slow == slow2:  # The meeting point is the duplicate number
                return slow
```
