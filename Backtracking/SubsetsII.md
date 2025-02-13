# 90. Subsets II
Difficulty: Medium

## QUESTION

You are given an array `nums` of integers, which may contain duplicates. Return all possible subsets.

The solution must not contain duplicate subsets. You may return the solution in any order.

### EXAMPLE

```
Input: nums = [1,2,1]
Output: [[],[1],[1,2],[1,1],[1,2,1],[2]]
```

```
Input: nums = [7,7]
Output: [[],[7], [7,7]]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        """
        Generates all unique subsets of a given list, accounting for duplicate elements.

        :param nums: List of integers, possibly containing duplicates.
        :return: A list of unique subsets.
        """
        res = set()  # Using a set to store unique subsets

        def backtrack(i, subset):
            """
            Recursive backtracking function to generate subsets.

            :param i: Current index in nums.
            :param subset: Current subset being built.
            """
            if i == len(nums):  # Base case: reached the end of the list
                res.add(tuple(subset))  # Convert list to tuple to store in set
                return

            # Include the current element
            subset.append(nums[i])
            backtrack(i + 1, subset)

            # Exclude the current element
            subset.pop()
            backtrack(i + 1, subset)

        nums.sort()  # Sort the list to ensure duplicate elements are adjacent
        backtrack(0, [])  # Start backtracking from index 0
        return [list(s) for s in res]  # Convert tuples back to lists for the result
```

### APPROACH: BACKTRACKING

```python
class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        """
        Generates all unique subsets of a given list, accounting for duplicate elements.

        :param nums: List of integers, possibly containing duplicates.
        :return: A list of unique subsets.
        """
        nums.sort()  # Sort to ensure duplicate elements are adjacent
        res = []  # List to store all unique subsets

        def backtrack(i, subset):
            """
            Recursive backtracking function to generate subsets.

            :param i: Current index in nums.
            :param subset: Current subset being built.
            """
            res.append(subset[::])  # Append a copy of the current subset

            for j in range(i, len(nums)):
                # Skip duplicates to avoid generating duplicate subsets
                if j > i and nums[j] == nums[j - 1]:
                    continue
                
                subset.append(nums[j])  # Include the current element
                backtrack(j + 1, subset)  # Recur with the next index
                subset.pop()  # Backtrack by removing the last element

        backtrack(0, [])  # Start backtracking from index 0
        return res  # Return the list of unique subsets
```
