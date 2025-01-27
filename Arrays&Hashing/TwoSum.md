# 000. Two Sum
Difficulty: Easy

## QUESTION

Given an array of integers `nums` and an integer `target`, return the indices `i` and `j` such that `nums[i] + nums[j] == target` and `i != j`.

You may assume that every input has exactly one pair of indices `i` and `j` that satisfy the condition.

Return the answer with the smaller index first.

### EXAMPLE

```
Input: nums = [3,4,5,6], target = 7
Output: [0,1]
```

```
Input: nums = [4,5,6], target = 10
Output: [0,2]
```

```
Input: nums = [5,5], target = 10
Output: [0,1]
```
## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'twoSum' to find two indices in the list such that their values sum up to the target.
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Iterate through each element in the list.
        for i in range(len(nums)):
            # Check every element after the current one.
            for j in range(i + 1, len(nums)):
                # If the sum of the two elements equals the target, return their indices.
                if nums[i] + nums[j] == target:
                    return [i, j]
        
        # If no pair is found, return an empty list.
        return []
```

### APPROACH: SORTING

```python
class Solution:
    # Define a method 'twoSum' to find two indices in the list such that their values sum up to the target.
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Initialize an empty list to store pairs of (value, index).
        A = []
        # Iterate through the list, storing each value and its index.
        for i, num in enumerate(nums):
            A.append([num, i])
        
        # Sort the list of pairs by their values (the first element of each pair).
        A.sort()

        # Initialize two pointers: one at the start and one at the end of the sorted list.
        i, j = 0, len(nums) - 1

        # Use the two-pointer technique to find the target sum.
        while i < j:
            # Calculate the sum of the values at the two pointers.
            cur = A[i][0] + A[j][0]
            
            # If the sum equals the target, return the indices of the two elements.
            if cur == target:
                return [min(A[i][1], A[j][1]),  # Return the smaller index first.
                        max(A[i][1], A[j][1])]  # Return the larger index second.
            # If the sum is less than the target, move the left pointer to the right.
            elif cur < target:
                i += 1
            # If the sum is greater than the target, move the right pointer to the left.
            else:
                j -= 1
        
        # If no pair is found, return an empty list.
        return []
```

### APPROACH: HASH TABLE

```python
class Solution:
    # Define a method 'twoSum' to find two indices in the list such that their values sum up to the target.
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # Initialize a dictionary to store previously seen numbers and their indices.
        prevMap = {}  # key: number, value: index

        # Iterate through the list, tracking both the index and the number.
        for i, n in enumerate(nums):
            # Calculate the difference needed to reach the target.
            diff = target - n
            # Check if the difference is already in the hash map.
            if diff in prevMap:
                # If found, return the indices of the current number and the previously seen number.
                return [prevMap[diff], i]
            # Otherwise, store the current number and its index in the hash map.
            prevMap[n] = i
```
