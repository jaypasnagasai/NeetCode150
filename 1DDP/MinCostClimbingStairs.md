# 746. Min Cost Climbing Stairs
Difficulty: Easy

## QUESTION

You are given an array of integers `cost` where `cost[i]` is the cost of taking a step from the `ith` floor of a staircase. After paying the cost, you can step to either the `(i + 1)th` floor or the `(i + 2)th` floor.

You may choose to start at the index `0` or the index `1` floor.

Return the minimum cost to reach the top of the staircase, i.e. just past the last index in `cost`.

### EXAMPLE

```
Input: cost = [1,2,3]
Output: 2
```

```
Input: cost = [1,2,1,2,1,1,1]
Output: 4
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        # Recursive DFS function to calculate minimum cost from index i
        def dfs(i):
            # Base case: If index is beyond the last step, no additional cost
            if i >= len(cost):
                return 0
            # Current cost plus the minimum of taking 1 or 2 steps ahead
            return cost[i] + min(dfs(i + 1), dfs(i + 2))

        # Start from either step 0 or step 1 and take the minimum
        return min(dfs(0), dfs(1))
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        # Iterate from the third-last element to the first
        for i in range(len(cost) - 3, -1, -1):
            # Update the current cost by adding the minimum of the next two steps
            cost[i] += min(cost[i + 1], cost[i + 2])

        # Return the minimum of starting from step 0 or step 1
        return min(cost[0], cost[1])
```


