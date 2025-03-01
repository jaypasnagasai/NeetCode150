# 134. Gas Station
Difficulty: Medium

## QUESTION

There are `n` gas stations along a circular route. You are given two integer arrays `gas` and `cost` where:

- `gas[i]` is the amount of gas at the `ith` station.
- `cost[i]` is the amount of gas needed to travel from the `ith` station to the `(i + 1)th` station. (The last station is connected to the first station)

You have a car that can store an unlimited amount of gas, but you begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index such that you can travel around the circuit once in the clockwise direction. If it's impossible, then return `-1`.

### EXAMPLE

```
Input: gas = [1,2,3,4], cost = [2,2,4,1]
Output: 3
```

```
Input: gas = [1,2,3], cost = [2,3,2]
Output: -1
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        n = len(gas)

        # Try each station as a starting point
        for i in range(n):
            tank = gas[i] - cost[i]  # Initial fuel balance after refueling at station i
            if tank < 0:
                continue  # If we can't even leave station i, skip it
            
            j = (i + 1) % n  # Move to the next station
            while j != i:
                tank += gas[j]  # Refuel at station j
                tank -= cost[j]  # Travel to the next station
                if tank < 0:
                    break  # If we run out of gas, stop checking this route
                
                j = (j + 1) % n  # Move to the next station
            
            if j == i:
                return i  # If we completed the loop, return the starting index
        
        return -1  # If no valid starting station is found, return -1
```

### APPROACH: TWO POINTERS

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        n = len(gas)
        start, end = n - 1, 0  # Start from the last station, end from the first
        tank = gas[start] - cost[start]  # Initialize the fuel balance

        while start > end:
            if tank < 0:
                # If we don't have enough fuel, move the start backward
                start -= 1
                tank += gas[start] - cost[start]
            else:
                # If we have enough fuel, move the end forward
                tank += gas[end] - cost[end]
                end += 1

        # If the tank is still non-negative, return the starting index
        return start if tank >= 0 else -1
```

### APPROACH: GREEDY

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        # If total gas is less than total cost, completing a circuit is impossible
        if sum(gas) < sum(cost):
            return -1

        total = 0  # Tracks the fuel balance
        res = 0  # Stores the starting station index

        # Iterate through each gas station
        for i in range(len(gas)):
            total += (gas[i] - cost[i])  # Update fuel balance
            
            # If fuel balance becomes negative, reset starting position
            if total < 0:
                total = 0  # Reset the fuel balance
                res = i + 1  # Move the starting index to the next station

        return res  # Return the valid starting station index
```
