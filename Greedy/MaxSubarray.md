# 53. Maximum Subarray
Difficulty: Medium

## QUESTION

Given an array of integers `nums`, find the subarray with the largest sum and return the sum.

A subarray is a contiguous non-empty sequence of elements within an array.

### EXAMPLE

```
Input: nums = [2,-3,4,-2,2,1,-1,4]
Output: 8
```

```
Input: nums = [-1]
Output: -1
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Initialize the result with the first element
        n, res = len(nums), nums[0]

        # Iterate over all possible starting points of subarrays
        for i in range(n):
            cur = 0  # Reset current sum for each new subarray start
            for j in range(i, n):
                cur += nums[j]  # Add current element to the running sum
                res = max(res, cur)  # Update the maximum subarray sum

        return res  # Return the maximum subarray sum found
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def maxSubArray(self, nums):
        # Initialize a DP array with the same values as nums
        dp = [*nums]

        # Iterate through the array, updating dp[i] to store the maximum subarray sum ending at index i
        for i in range(1, len(nums)):
            dp[i] = max(nums[i], nums[i] + dp[i - 1])

        # Return the maximum value found in the DP array, which represents the maximum subarray sum
        return max(dp)
```

### APPROACH: DIVIDE AND CONQUER

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Divide and Conquer approach to find the maximum subarray sum
        def dfs(l, r):
            # Base case: If left index exceeds right index, return negative infinity
            if l > r:
                return float("-inf")

            # Find the middle index
            m = (l + r) >> 1

            # Initialize left and right maximum sums
            leftSum = rightSum = curSum = 0

            # Compute the maximum sum of the left half including the middle element
            for i in range(m - 1, l - 1, -1):
                curSum += nums[i]
                leftSum = max(leftSum, curSum)

            # Reset curSum and compute the maximum sum of the right half including the middle element
            curSum = 0
            for i in range(m + 1, r + 1):
                curSum += nums[i]
                rightSum = max(rightSum, curSum)

            # Return the maximum of:
            # 1. The max subarray sum in the left half
            # 2. The max subarray sum in the right half
            # 3. The max crossing sum (left + middle + right)
            return max(dfs(l, m - 1), 
                       dfs(m + 1, r), 
                       leftSum + nums[m] + rightSum)

        # Start the recursive divide and conquer approach
        return dfs(0, len(nums) - 1)
```
