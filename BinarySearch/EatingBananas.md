# 875. Koko Eating Bananas
Difficulty: Medium

## QUESTION

You are given an integer array `piles` where `piles[i]` is the number of bananas in the `ith` pile. You are also given an integer `h`, which represents the number of hours you have to eat all the bananas.

You may decide your bananas-per-hour eating rate of `k`. Each hour, you may choose a pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, you may finish eating the pile but you can not eat from another pile in the same hour.

Return the minimum integer `k` such that you can eat all the bananas within `h` hours.

### EXAMPLE

```
Input: piles = [1,4,3,2], h = 9
Output: 2
```

```
Input: piles = [25,10,23,4], h = 4
Output: 25
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'minEatingSpeed' to determine the minimum eating speed for Koko.
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        # Start with the smallest possible speed.
        speed = 1

        # Infinite loop to test increasing speeds until a valid one is found.
        while True:
            totalTime = 0  # Track the total time required for the current speed.

            # Iterate through each pile to compute the time taken at the current speed.
            for pile in piles:
                totalTime += math.ceil(pile / speed)  # Compute hours required for this pile.

            # If the total time is within the allowed hours, return the current speed.
            if totalTime <= h:
                return speed

            # If the current speed is too slow, increase it and try again.
            speed += 1

        # This return statement is redundant because we always return inside the loop.
        return speed
```

### APPROACH: BINARY SEARCH

```python
import math

class Solution:
    # Define a method 'minEatingSpeed' to determine the minimum eating speed for Koko.
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        # The lowest possible speed is 1 (slowest), and the highest is the largest pile (fastest).
        l, r = 1, max(piles)
        # Initialize the result with the maximum possible speed.
        res = r

        # Perform binary search to find the minimum valid speed.
        while l <= r:
            # Calculate the middle speed (candidate eating speed).
            k = (l + r) // 2

            # Calculate the total hours needed to eat all bananas at speed 'k'.
            totalTime = 0
            for p in piles:
                totalTime += math.ceil(float(p) / k)

            # If the total time is within the allowed hours, try a smaller speed.
            if totalTime <= h:
                res = k  # Update the result (potential minimum speed).
                r = k - 1  # Reduce the search space to find a smaller valid speed.
            else:
                l = k + 1  # Increase speed as it takes too long.

        # Return the minimum speed found.
        return res
```

