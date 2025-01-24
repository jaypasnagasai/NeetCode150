# 42. Trapping Rain Water
Difficulty: Hard

## QUESTION

You are given an array non-negative integers `height` which represent an elevation map. Each value `height[i]` represents the height of a bar, which has a width of `1`.

Return the maximum area of water that can be trapped between the bars.


### EXAMPLE

```
Input: height = [0,2,0,3,1,0,1,3,2,1]
Output: 9
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'trap' that takes a list of integers 'height' and 
    # returns the total amount of water that can be trapped after rainfall.
    def trap(self, height: List[int]) -> int:
        # If the list is empty, no water can be trapped.
        if not height:
            return 0
        
        # Get the length of the height list.
        n = len(height)
        # Initialize the result variable to store the total water trapped.
        res = 0

        # Loop through each bar in the list to calculate the water trapped above it.
        for i in range(n):
            # Initialize leftMax and rightMax to the current height, as a baseline.
            leftMax = rightMax = height[i]

            # Find the maximum height to the left of the current bar (inclusive).
            for j in range(i):
                leftMax = max(leftMax, height[j])
            
            # Find the maximum height to the right of the current bar (exclusive).
            for j in range(i + 1, n):
                rightMax = max(rightMax, height[j])
            
            # The water trapped above the current bar is determined by the
            # smaller of the two max heights (leftMax and rightMax), minus the current height.
            res += min(leftMax, rightMax) - height[i]
        
        # Return the total water trapped.
        return res
```

### APPROACH: TWO POINTER

```python
class Solution:
    # Define a method 'trap' that calculates the total amount of water that can be trapped.
    def trap(self, height: List[int]) -> int:
        # If the input list is empty, no water can be trapped.
        if not height:
            return 0

        # Initialize two pointers: 'l' (left) starting at the beginning, and
        # 'r' (right) starting at the end of the list.
        l, r = 0, len(height) - 1

        # Initialize variables to track the maximum heights encountered so far
        # from the left and right sides.
        leftMax, rightMax = height[l], height[r]

        # Initialize a variable to store the total trapped water.
        res = 0

        # Loop until the two pointers meet.
        while l < r:
            # If the maximum height on the left is less than that on the right,
            # the water trapped depends on the left side.
            if leftMax < rightMax:
                # Move the left pointer one step to the right.
                l += 1
                # Update the maximum height seen on the left.
                leftMax = max(leftMax, height[l])
                # Add the water trapped at the current position (if any).
                res += leftMax - height[l]
            else:
                # If the maximum height on the right is less than or equal to that on the left,
                # the water trapped depends on the right side.
                # Move the right pointer one step to the left.
                r -= 1
                # Update the maximum height seen on the right.
                rightMax = max(rightMax, height[r])
                # Add the water trapped at the current position (if any).
                res += rightMax - height[r]

        # Return the total amount of trapped water.
        return res
```
