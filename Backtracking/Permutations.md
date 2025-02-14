# 46. Permutations
Difficulty: Medium

## QUESTION

Given an array `nums` of unique integers, return all the possible permutations. You may return the answer in any order.

### EXAMPLE

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

```
Input: nums = [7]
Output: [[7]]
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        # If the list is empty, return a list containing an empty permutation
        if len(nums) == 0:
            return [[]]
        
        # Recursively compute permutations of the sublist excluding the first element
        perms = self.permute(nums[1:])
        
        # This will hold the final permutations
        res = []
        
        # Insert the first element (nums[0]) into every possible position of each permutation
        for p in perms:
            for i in range(len(p) + 1):
                p_copy = p.copy()
                p_copy.insert(i, nums[0])
                res.append(p_copy)
        
        # Return all the generated permutations
        return res
```

### APPROACH: BACKTRACKING

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        # Store all permutations
        self.res = []
        # Begin recursion from the first index
        self.backtrack(nums, 0)
        # Return the collected permutations
        return self.res

    def backtrack(self, nums: List[int], idx: int):
        # If we've used all numbers, add the current permutation to the result
        if idx == len(nums):
            self.res.append(nums[:])
            return
        # Swap each element with the current index and backtrack
        for i in range(idx, len(nums)):
            nums[idx], nums[i] = nums[i], nums[idx]
            self.backtrack(nums, idx + 1)
            # Swap back to restore the original order
            nums[idx], nums[i] = nums[i], nums[idx]
```
