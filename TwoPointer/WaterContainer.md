# 11 - Container With Most Water
Difficulty: Medium

## QUESTION

You are given an integer array `heights` where  `heights[i]` represents the height of the ith bar. You may choose any two bars to form a container. Return the maximum amount of water a container can store.

### EXAMPLE

```
Input: height = [1,7,2,5,4,7,3,6]
Output: 36
```

```
Input: height = [2,2,2]
Output: 4
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'maxArea' that takes a list of integers 'heights' and 
    # returns the maximum area of water a container can store.
    def maxArea(self, heights: List[int]) -> int:
        # Initialize the result variable to store the maximum area found so far.
        res = 0

        # Outer loop iterates over each bar as the left side of the container.
        for i in range(len(heights)):
            # Inner loop iterates over each subsequent bar as the right side of the container.
            for j in range(i + 1, len(heights)):
                # Calculate the area of the container formed by the two bars.
                # The height of the container is determined by the shorter bar,
                # and the width is the distance between the two bars (j - i).
                area = min(heights[i], heights[j]) * (j - i)
                # Update the result with the maximum area found so far.
                res = max(res, area)

        # Return the maximum area found.
        return res
```

### APPROACH: TWO POINTER

```python
class Solution:
    # Define a method 'maxArea' that takes a list of integers 'heights' and 
    # returns the maximum area of water a container can store.
    def maxArea(self, heights: List[int]) -> int:
        # Initialize two pointers: 'l' (left) at the start of the list and 
        # 'r' (right) at the end of the list.
        l, r = 0, len(heights) - 1
        # Initialize the result variable to store the maximum area found so far.
        res = 0

        # Loop until the two pointers meet.
        while l < r:
            # Calculate the area of the container formed by the heights at the two pointers.
            # The height is the smaller of the two heights, and the width is the distance
            # between the two pointers (r - l).
            area = min(heights[l], heights[r]) * (r - l)
            # Update the result with the maximum area found so far.
            res = max(res, area)

            # Move the pointer corresponding to the shorter height inward.
            # This is because the area is limited by the shorter height, and moving
            # the taller pointer inward will not help to increase the area.
            if heights[l] <= heights[r]:
                l += 1
            else:
                r -= 1

        # Return the maximum area found.
        return res
```
