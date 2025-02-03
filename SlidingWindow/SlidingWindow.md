# 239. Sliding Window Maximum
Difficulty: Hard

## QUESTION

You are given an array of integers `nums` and an integer `k`. There is a sliding window of size `k` that starts at the left edge of the array. The window slides one position to the right until it reaches the right edge of the array.

Return a list that contains the maximum element in the window at each step.

### EXAMPLE

```
Input: nums = [1,2,1,0,4,2,6], k = 3
Output: [2,2,4,4,6]

Explanation: 
Window position            Max
---------------           -----
[1  2  1] 0  4  2  6        2
 1 [2  1  0] 4  2  6        2
 1  2 [1  0  4] 2  6        4
 1  2  1 [0  4  2] 6        4
 1  2  1  0 [4  2  6]       6
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        output = []  # List to store the maximum values for each window

        # Iterate over all possible windows of size k
        for i in range(len(nums) - k + 1):
            maxi = nums[i]  # Initialize maxi with the first element of the window
            
            # Iterate over the k elements in the current window to find the maximum
            for j in range(i, i + k):
                maxi = max(maxi, nums[j])  # Update maxi if a larger element is found
            
            output.append(maxi)  # Append the maximum value of the window to the output list

        return output  # Return the list of maximum values for each window
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        
        # Arrays to store the max values from left and right for each partition
        leftMax = [0] * n
        rightMax = [0] * n

        # Initialize the first and last elements of leftMax and rightMax respectively
        leftMax[0] = nums[0]
        rightMax[n - 1] = nums[n - 1]

        # Populate leftMax and rightMax arrays
        for i in range(1, n):
            # Start a new block when i is a multiple of k
            if i % k == 0:
                leftMax[i] = nums[i]
            else:
                leftMax[i] = max(leftMax[i - 1], nums[i])  # Take max from the left side

            # Start a new block when (n-1-i) is a multiple of k
            if (n - 1 - i) % k == 0:
                rightMax[n - 1 - i] = nums[n - 1 - i]
            else:
                rightMax[n - 1 - i] = max(rightMax[n - i], nums[n - 1 - i])  # Take max from the right side

        # Compute the maximums for each sliding window
        output = [0] * (n - k + 1)
        for i in range(n - k + 1):
            output[i] = max(leftMax[i + k - 1], rightMax[i])  # Get max from leftMax and rightMax

        return output  # Return the list containing max values for each window
```

### APPROACH: DEQUE

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        output = []  # List to store the maximum values for each window
        q = deque()  # Deque to store indices of elements in the current window
        l = r = 0  # Left and right pointers for the sliding window

        while r < len(nums):
            # Remove elements from the back of the deque if they are smaller than the current element
            while q and nums[q[-1]] < nums[r]:
                q.pop()
            q.append(r)  # Add the current index to the deque

            # Remove the leftmost element if it is outside the window
            if l > q[0]:
                q.popleft()

            # Once the window size reaches k, append the max element (front of deque) to output
            if (r + 1) >= k:
                output.append(nums[q[0]])  # Max element of the window
                l += 1  # Move left pointer forward
            
            r += 1  # Move right pointer forward

        return output  # Return the list containing max values for each window
```
