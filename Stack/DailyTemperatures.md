# 739. Daily Temperatures
Difficulty: Medium

## QUESTION

You are given an array of integers `temperatures` where `temperatures[i]` represents the daily temperatures on the `ith` day.

Return an array `result` where `result[i]` is the number of days after the `ith` day before a warmer temperature appears on a future day. If there is no day in the future where a warmer temperature will appear for the `ith` day, set `result[i]` to `0` instead.

### EXAMPLE

```
Input: temperatures = [30,38,30,36,35,40,28]
Output: [1,4,1,2,1,0,0]
```

```
Input: temperatures = [22,21,20]
Output: [0,0,0]
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'dailyTemperatures' that calculates the number of days until a warmer temperature.
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        # Get the number of days (length of the input list).
        n = len(temperatures)
        # Initialize an empty list to store the results.
        res = []

        # Iterate through each day in the temperature list.
        for i in range(n):
            # Initialize a counter to track the number of days until a warmer temperature.
            count = 1
            # Start looking for a warmer day from the next day.
            j = i + 1
            while j < n:
                # If a warmer temperature is found, stop the loop.
                if temperatures[j] > temperatures[i]:
                    break
                # Otherwise, move to the next day and increment the counter.
                j += 1
                count += 1
            
            # If no warmer day is found, set the count to 0.
            count = 0 if j == n else count
            # Append the result for the current day to the results list.
            res.append(count)
        
        # Return the list of results.
        return res
```

### APPROACH: STACK

```python
class Solution:
    # Define a method 'dailyTemperatures' that calculates the number of days until a warmer temperature.
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        # Initialize a result list with zeros, one for each day.
        res = [0] * len(temperatures)
        # Initialize a monotonic decreasing stack to store pairs of temperature and index.
        stack = []  # Each element is a tuple: (temperature, index).

        # Iterate through each temperature with its index.
        for i, t in enumerate(temperatures):
            # While the stack is not empty and the current temperature is greater than
            # the temperature at the top of the stack, process the stack.
            while stack and t > stack[-1][0]:
                # Pop the temperature and index from the top of the stack.
                stackT, stackInd = stack.pop()
                # Calculate the number of days until a warmer temperature.
                res[stackInd] = i - stackInd
            # Push the current temperature and index onto the stack.
            stack.append((t, i))
        
        # Return the result list containing the number of days until a warmer temperature.
        return res  
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    # Define a method 'dailyTemperatures' that calculates the number of days until a warmer temperature.
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        # Get the number of days (length of the input list).
        n = len(temperatures)
        # Initialize the result list with zeros, one for each day.
        res = [0] * n

        # Iterate through the temperatures in reverse, starting from the second-to-last day.
        for i in range(n - 2, -1, -1):
            # Start checking from the next day.
            j = i + 1
            # Continue until we find a warmer temperature or reach the end of the list.
            while j < n and temperatures[j] <= temperatures[i]:
                # If there are no warmer temperatures for day `j`, stop the search.
                if res[j] == 0:
                    j = n
                    break
                # Jump directly to the next day with a potential warmer temperature using the `res` array.
                j += res[j]
            
            # If a warmer temperature is found, calculate the difference in days.
            if j < n:
                res[i] = j - i
        
        # Return the result list containing the number of days until a warmer temperature.
        return res     
```
