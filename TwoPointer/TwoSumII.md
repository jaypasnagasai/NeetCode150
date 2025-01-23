# 167. Two Sum II - Input Array Is Sorted
Difficulty: Medium

## QUESTION

Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where
`1 <= index1 < index2 <= numbers.length`.

Return the indices of the two numbers, `index1` and `index2`, added by one as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

### EXAMPLE

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'twoSum' that takes a sorted list of integers 'numbers'
    # and a target value 'target', and returns the indices of two numbers
    # that add up to the target.
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        # Outer loop iterates through each number in the list.
        for i in range(len(numbers)):
            # Inner loop starts from the next index after 'i' to avoid duplicates
            # and checks all subsequent numbers in the list.
            for j in range(i + 1, len(numbers)):
                # If the sum of the two numbers equals the target, return their
                # indices as a 1-based index (add 1 to both indices).
                if numbers[i] + numbers[j] == target:
                    return [i + 1, j + 1]
        
        # If no pair of numbers adds up to the target, return an empty list.
        return []
```

### APPROACH: TWO POINTER

```python
class Solution:
    # Define a method 'twoSum' that takes a sorted list of integers 'numbers'
    # and a target value 'target', and returns the indices of two numbers
    # that add up to the target.
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        # Initialize two pointers: 'l' (left) at the start of the list 
        # and 'r' (right) at the end of the list.
        l, r = 0, len(numbers) - 1

        # Loop until the two pointers meet.
        while l < r:
            # Calculate the sum of the numbers at the two pointers.
            curSum = numbers[l] + numbers[r]

            # If the current sum is greater than the target, 
            # move the right pointer one step to the left to reduce the sum.
            if curSum > target:
                r -= 1
            # If the current sum is less than the target, 
            # move the left pointer one step to the right to increase the sum.
            elif curSum < target:
                l += 1
            # If the current sum equals the target, 
            # return the indices of the two numbers (1-based index).
            else:
                return [l + 1, r + 1]
        
        # If no such pair is found, return an empty list.
        return []
```
