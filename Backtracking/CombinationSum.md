# 39. Combination Sum
Difficulty: Medium

## QUESTION

You are given an array of distinct integers `nums` and a target integer `target`. Your task is to return a list of all unique combinations of `nums` where the chosen numbers sum to `target`.

The same number may be chosen from nums an unlimited number of times. Two combinations are the same if the frequency of each of the chosen numbers is the same, otherwise they are different.

You may return the combinations in any order and the order of the numbers in each combination can be in any order.

### EXAMPLE

```
Input: 
nums = [2,5,6,9] 
target = 9

Output: [[2,2,5],[9]]
```

```
Input: 
nums = [3,4,5]
target = 16

Output: [[3,3,3,3,4],[3,3,5,5],[4,4,4,4],[3,4,4,5]]
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class Solution:
    def combinationSum(self, nums: List[int], target: int) -> List[List[int]]:
        """
        Finds all unique combinations in nums where the numbers sum up to the target.
        Each number in nums can be used an unlimited number of times.

        :param nums: List of candidate numbers.
        :param target: The target sum to achieve.
        :return: A list of unique combinations that sum to the target.
        """
        res = []  # List to store valid combinations
        nums.sort()  # Sort the numbers to enable pruning

        def dfs(i, cur, total):
            """
            Recursive function to generate valid subsets.

            :param i: Current index in nums.
            :param cur: Current subset being built.
            :param total: Sum of the current subset.
            """
            if total == target:  # If the subset sum matches target, add it to results
                res.append(cur.copy())  # Append a copy of the current subset
                return
            
            for j in range(i, len(nums)):  
                if total + nums[j] > target:  # Prune the search if the sum exceeds target
                    return
                
                cur.append(nums[j])  # Include the current candidate
                dfs(j, cur, total + nums[j])  # Allow reuse of the same element (j, not j+1)
                cur.pop()  # Backtrack to explore other possibilities
        
        dfs(0, [], 0)  # Start DFS with index 0
        return res  # Return list of unique valid combinations
```

