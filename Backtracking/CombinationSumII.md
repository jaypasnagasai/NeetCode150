# 40. Combination Sum II
Difficulty: Medium

## QUESTION

You are given an array of integers `candidates`, which may contain duplicates, and a `target` integer target. Your task is to return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`.

Each element from `candidates` may be chosen at most once within a combination. The solution set must not contain duplicate combinations.

You may return the combinations in any order and the order of the numbers in each combination can be in any order.

### EXAMPLE

```
Input: candidates = [9,2,2,4,6,1,5], target = 8
Output: [
  [1,2,5],
  [2,2,4],
  [2,6]
]
```

```
Input: candidates = [1,2,3,4,5], target = 7

Output: [
  [1,2,4],
  [2,5],
  [3,4]
]
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def combinationSum2(self, candidates, target):
        """
        Finds all unique combinations in candidates where the numbers sum up to the target.
        Each number in candidates may only be used once in the combination.

        :param candidates: List of candidate numbers (can contain duplicates).
        :param target: The target sum to achieve.
        :return: A list of unique combinations that sum to the target.
        """
        res = set()  # Use a set to store unique combinations
        candidates.sort()  # Sort to handle duplicates effectively

        def generate_subsets(i, cur, total):
            """
            Recursive function to generate subsets that sum up to the target.

            :param i: Current index in candidates.
            :param cur: Current subset being built.
            :param total: Sum of the current subset.
            """
            if total == target:  # If the subset sum matches target, add it to results
                res.add(tuple(cur))  # Convert to tuple to ensure uniqueness in the set
                return
            if total > target or i == len(candidates):  # Stop if sum exceeds target or end of list
                return

            # Include the current candidate
            cur.append(candidates[i])
            generate_subsets(i + 1, cur, total + candidates[i])  # Move to the next index
            cur.pop()  # Backtrack

            # Exclude the current candidate and move to the next unique element
            generate_subsets(i + 1, cur, total)

        generate_subsets(0, [], 0)  # Start the recursive process
        return [list(combination) for combination in res]  # Convert unique tuples back to lists
```

### APPROACH: BACKTRACKING

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        """
        Finds all unique combinations in candidates where the numbers sum up to the target.
        Each number in candidates may only be used once in the combination.

        :param candidates: List of candidate numbers (can contain duplicates).
        :param target: The target sum to achieve.
        :return: A list of unique combinations that sum to the target.
        """
        res = []  # List to store valid combinations
        candidates.sort()  # Sort to handle duplicates and enable pruning

        def dfs(idx, path, cur):
            """
            Recursive function to generate valid subsets.

            :param idx: Current index in candidates.
            :param path: Current subset being built.
            :param cur: Sum of the current subset.
            """
            if cur == target:  # If the subset sum matches target, add it to results
                res.append(path.copy())  # Append a copy of the current subset
                return

            for i in range(idx, len(candidates)):
                # Skip duplicate numbers to ensure unique combinations
                if i > idx and candidates[i] == candidates[i - 1]:
                    continue
                
                # If adding the current candidate exceeds target, break (prune)
                if cur + candidates[i] > target:
                    break

                path.append(candidates[i])  # Include the current candidate
                dfs(i + 1, path, cur + candidates[i])  # Move to the next index
                path.pop()  # Backtrack to explore other possibilities

        dfs(0, [], 0)  # Start DFS with index 0
        return res  # Return list of unique valid combinations
```

