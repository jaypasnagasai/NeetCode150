# 853. Car Fleet
Difficulty: Medium

## QUESTION

There are `n` cars traveling to the same destination on a one-lane highway.

You are given two arrays of integers `position` and `speed`, both of length `n`.

- `position[i]` is the position of the `ith car` (in miles)
- `speed[i]` is the speed of the `ith car` (in miles per hour)

The destination is at position `target` miles.

A car can not pass another car ahead of it. It can only catch up to another car and then drive at the same speed as the car ahead of it.

A car fleet is a non-empty set of cars driving at the same position and same speed. A single car is also considered a car fleet.

If a car catches up to a car fleet the moment the fleet reaches the destination, then the car is considered to be part of the fleet.

Return the number of different car fleets that will arrive at the destination.

### EXAMPLE

```
Input: target = 10, position = [1,4], speed = [3,2]
Output: 1
```

```
Input: target = 10, position = [4,1,0,7], speed = [2,2,1,1]
Output: 3
```

## SOLUTION

### APPROACH: STACK

```python
class Solution:
    # Define a method 'carFleet' to calculate the number of car fleets that reach the target.
    def carFleet(self, target: int, position: List[int], speed: List[int]) -> int:
        # Create pairs of position and speed for each car, then sort them in descending order of position.
        pair = [(p, s) for p, s in zip(position, speed)]
        pair.sort(reverse=True)

        # Stack to track the arrival times of car fleets.
        stack = []
        
        # Iterate over the sorted cars.
        for p, s in pair:  # Reverse sorted order ensures we process cars from closest to farthest from the target.
            # Calculate the time required for the current car to reach the target.
            stack.append((target - p) / s)
            # If the current car's arrival time is less than or equal to the fleet at the top of the stack,
            # it joins the same fleet (pop the stack).
            if len(stack) >= 2 and stack[-1] <= stack[-2]:
                stack.pop()
        
        # The number of elements in the stack is the number of car fleets.
        return len(stack)    
```
