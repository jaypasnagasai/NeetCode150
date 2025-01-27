# 84. Largest Rectangle In Histogram
Difficulty: Hard

## QUESTION

You are given an array of integers `heights` where `heights[i]` represents the height of a bar. The width of each bar is `1`.

Return the area of the largest rectangle that can be formed among the bars.

### EXAMPLE

```
Input: heights = [7,1,7,2,2,4]
Output: 8
```

```
Input: heights = [1,3,7]
Output: 7
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'largestRectangleArea' to compute the largest rectangular area in a histogram.
    def largestRectangleArea(self, heights: List[int]) -> int:
        # Get the number of bars in the histogram.
        n = len(heights)
        # Initialize the variable to track the maximum area.
        maxArea = 0

        # Iterate through each bar to calculate the largest rectangle with the current bar as the smallest height.
        for i in range(n):
            # The height of the rectangle is determined by the height of the current bar.
            height = heights[i]

            # Find the rightmost boundary where the height is at least 'height'.
            rightMost = i + 1
            while rightMost < n and heights[rightMost] >= height:
                rightMost += 1

            # Find the leftmost boundary where the height is at least 'height'.
            leftMost = i
            while leftMost >= 0 and heights[leftMost] >= height:
                leftMost -= 1

            # Adjust the boundaries to make them inclusive.
            rightMost -= 1
            leftMost += 1

            # Calculate the area of the rectangle using the width and height.
            maxArea = max(maxArea, height * (rightMost - leftMost + 1))
        
        # Return the maximum rectangle area found.
        return maxArea
```

### APPROACH: STACK

```python
class Solution:
    # Define a method 'largestRectangleArea' to compute the largest rectangular area in a histogram.
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)  # Number of bars in the histogram.
        stack = []        # Stack to store indices for calculating boundaries.

        # Array to store the leftmost index where the height is less than the current bar.
        leftMost = [-1] * n
        for i in range(n):
            # Pop elements from the stack while the top of the stack is greater than or equal to the current bar.
            while stack and heights[stack[-1]] >= heights[i]:
                stack.pop()
            # If the stack is not empty, the top of the stack is the left boundary.
            if stack:
                leftMost[i] = stack[-1]
            # Push the current index onto the stack.
            stack.append(i)

        # Reset the stack for calculating the rightmost boundaries.
        stack = []
        # Array to store the rightmost index where the height is less than the current bar.
        rightMost = [n] * n
        for i in range(n - 1, -1, -1):
            # Pop elements from the stack while the top of the stack is greater than or equal to the current bar.
            while stack and heights[stack[-1]] >= heights[i]:
                stack.pop()
            # If the stack is not empty, the top of the stack is the right boundary.
            if stack:
                rightMost[i] = stack[-1]
            # Push the current index onto the stack.
            stack.append(i)

        # Initialize the maximum area to zero.
        maxArea = 0
        for i in range(n):
            # Adjust left and right boundaries to include the current bar.
            leftMost[i] += 1  # Convert leftMost[i] from exclusive to inclusive.
            rightMost[i] -= 1 # Convert rightMost[i] from exclusive to inclusive.
            # Calculate the area for the current bar as the smallest height.
            maxArea = max(maxArea, heights[i] * (rightMost[i] - leftMost[i] + 1))

        # Return the maximum area found.
        return maxArea
```
