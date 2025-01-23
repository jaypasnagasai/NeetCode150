# 15. 3SUM
Difficulty: Medium

## QUESTION

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` where `nums[i] + nums[j] + nums[k] == 0`, and the indices `i`, `j` and `k` are all distinct.

The output should not contain any duplicate triplets. You may return the output and the triplets in any order.

### EXAMPLE

```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

```
Input: nums = [0,1,1]
Output: []
```

```
Input: nums = [0,0,0]
Output: [[0,0,0]]
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'threeSum' that takes a list of integers 'nums' and
    # returns a list of all unique triplets that sum up to zero.
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # Use a set to store unique triplets, avoiding duplicates.
        res = set()
        # Sort the input list to make it easier to detect duplicates and ensure order.
        nums.sort()
        
        # Iterate through each number in the list as the first element of the triplet.
        for i in range(len(nums)):
            # Iterate through all subsequent numbers as the second element of the triplet.
            for j in range(i + 1, len(nums)):
                # Iterate through all numbers after the second element as the third element of the triplet.
                for k in range(j + 1, len(nums)):
                    # If the sum of the three numbers is zero, create a triplet.
                    if nums[i] + nums[j] + nums[k] == 0:
                        tmp = [nums[i], nums[j], nums[k]]
                        # Add the triplet as a tuple to the set to ensure uniqueness.
                        res.add(tuple(tmp))
        
        # Convert the set of tuples back to a list of lists before returning.
        return [list(i) for i in res]
```


### APPROACH: TWO POINTER

```python
class Solution:
    # Define a method 'threeSum' that takes a list of integers 'nums' and returns
    # a list of all unique triplets that sum up to zero.
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # Initialize an empty list to store the resulting triplets.
        res = []
        # Sort the input list to enable the two-pointer technique and handle duplicates easily.
        nums.sort()

        # Iterate through the sorted list, using 'a' as the first number in the triplet.
        for i, a in enumerate(nums):
            # Since the list is sorted, if 'a' is positive, further numbers cannot sum to zero.
            if a > 0:
                break

            # Skip duplicates for the first number to avoid duplicate triplets in the result.
            if i > 0 and a == nums[i - 1]:
                continue

            # Initialize two pointers: 'l' (left) and 'r' (right) to find the remaining two numbers.
            l, r = i + 1, len(nums) - 1
            while l < r:
                # Calculate the sum of the current triplet.
                threeSum = a + nums[l] + nums[r]
                
                # If the sum is greater than zero, move the right pointer to the left to decrease the sum.
                if threeSum > 0:
                    r -= 1
                # If the sum is less than zero, move the left pointer to the right to increase the sum.
                elif threeSum < 0:
                    l += 1
                # If the sum equals zero, add the triplet to the result list.
                else:
                    res.append([a, nums[l], nums[r]])
                    # Move both pointers inward to continue searching for other triplets.
                    l += 1
                    r -= 1
                    # Skip duplicates for the second number in the triplet to avoid repeated results.
                    while nums[l] == nums[l - 1] and l < r:
                        l += 1
        
        # Return the list of unique triplets that sum to zero.
        return res
```
