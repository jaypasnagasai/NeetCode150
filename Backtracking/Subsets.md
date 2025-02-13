# 78. Subsets
Difficulty: Medium

## QUESTION

Given an array `nums` of unique integers, return all possible subsets of `nums`.

The solution set must not contain duplicate subsets. You may return the solution in any order.

### EXAMPLE

```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

```
Input: nums = [7]
Output: [[],[7]]
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        """
        Generates all possible subsets (the power set) of a given list.

        :param nums: List of distinct integers.
        :return: A list of all subsets.
        """
        res = []  # List to store all subsets
        subset = []  # Temporary list to build subsets

        def dfs(i):
            """
            Depth-First Search (DFS) to generate subsets.

            :param i: Current index in nums.
            """
            if i >= len(nums):  # Base case: If index exceeds length, add subset to result
                res.append(subset.copy())  # Append a copy of the subset to results
                return

            # Include nums[i] in the subset
            subset.append(nums[i])
            dfs(i + 1)

            # Exclude nums[i] from the subset
            subset.pop()
            dfs(i + 1)

        dfs(0)  # Start DFS from index 0
        return res  # Return the list of all subsets
```

### APPROACH: ITERATION

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        """
        Generates all possible subsets (the power set) of a given list using an iterative approach.

        :param nums: List of distinct integers.
        :return: A list of all subsets.
        """
        res = [[]]  # Initialize with the empty subset
        
        for num in nums:
            # For each number, generate new subsets by adding it to existing subsets
            res += [subset + [num] for subset in res]
        
        return res  # Return the list of all subsets
```

### APPROACH: BIT MANIPULATION

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        """
        Generates all possible subsets (the power set) of a given list using a bitwise approach.

        :param nums: List of distinct integers.
        :return: A list of all subsets.
        """
        n = len(nums)  # Get the length of nums
        res = []  # List to store all subsets

        # Iterate through all possible binary representations from 0 to (2^n - 1)
        for i in range(1 << n):  # 1 << n is equivalent to 2^n
            # Generate subset by selecting elements where the corresponding bit is set
            subset = [nums[j] for j in range(n) if (i & (1 << j))]
            res.append(subset)  # Append generated subset to the result list

        return res  # Return the list of all subsets
```
