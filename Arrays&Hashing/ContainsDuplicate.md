# 000. Contains Duplicate
Difficulty: Easy

## QUESTION

Given an integer array `nums`, return `true` if any value appears more than once in the array, otherwise return `false`.

### EXAMPLE

```
Input: nums = [1, 2, 3, 3]
Output: true
```

```
Input: nums = [1, 2, 3, 4]
Output: false
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'hasDuplicate' to check if the input list contains duplicate elements.
    def hasDuplicate(self, nums: List[int]) -> bool:
        # Iterate through each element in the list.
        for i in range(len(nums)):
            # Compare the current element with all subsequent elements in the list.
            for j in range(i + 1, len(nums)):
                # If a duplicate is found, return True immediately.
                if nums[i] == nums[j]:
                    return True
        # If no duplicates are found after all comparisons, return False.
        return False
```

### APPROACH: SORTING

```python
class Solution:
    # Define a method 'hasDuplicate' to check if the input list contains duplicate elements.
    def hasDuplicate(self, nums: List[int]) -> bool:
        # Sort the input list to bring duplicate elements next to each other.
        nums.sort()
        # Iterate through the sorted list, starting from the second element.
        for i in range(1, len(nums)):
            # Check if the current element is equal to the previous element.
            if nums[i] == nums[i - 1]:
                # If a duplicate is found, return True immediately.
                return True
        # If no duplicates are found after checking all elements, return False.
        return False
```

### APPROACH: HASH SET

```python
class Solution:
    # Define a method 'hasDuplicate' to check if the input list contains duplicate elements.
    def hasDuplicate(self, nums: List[int]) -> bool:
        # Initialize an empty set to store unique elements.
        seen = set()
        # Iterate through each element in the list.
        for num in nums:
            # If the current element is already in the set, a duplicate is found.
            if num in seen:
                return True
            # Otherwise, add the current element to the set.
            seen.add(num)
        # If no duplicates are found after checking all elements, return False.
        return False
```
### APPROACH: HASH SET LENGTH

```python
class Solution:
    # Define a method 'hasDuplicate' to check if the input list contains duplicate elements.
    def hasDuplicate(self, nums: List[int]) -> bool:
        # Convert the input list to a set, which automatically removes duplicates.
        # Compare the length of the set with the length of the original list.
        # If the set's length is less, it means there are duplicates in the list.
        return len(set(nums)) < len(nums)
```
