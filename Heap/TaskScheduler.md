# 621. Task Scheduler
Difficulty: Medium

## QUESTION

You are given an array of CPU tasks `tasks`, where `tasks[i]` is an uppercase english character from `A` to `Z`. You are also given an integer `n`.

Each CPU cycle allows the completion of a single task, and tasks may be completed in any order.

The only constraint is that identical tasks must be separated by at least `n` CPU cycles, to cooldown the CPU.

Return the minimum number of CPU cycles required to complete all tasks.

### EXAMPLE

```
Input: tasks = ["X","X","Y","Y"], n = 2
Output: 5
```

```
Input: tasks = ["A","A","A","B","C"], n = 3
Output: 9
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Count the frequency of each task
        count = [0] * 26
        for task in tasks:
            count[ord(task) - ord('A')] += 1
        
        # Create an array of (frequency, task_index) for tasks with nonzero frequency
        arr = []
        for i in range(26):
            if count[i] > 0:
                arr.append([count[i], i])
        
        # Keep track of time units and a list of processed tasks in order
        time = 0
        processed = []
        
        # Process tasks until no tasks remain
        while arr:
            # Choose a task that can be scheduled next, ensuring cooldown constraints
            maxi = -1
            for i in range(len(arr)):
                # Check if the task wasn't processed in the cooldown window
                if all(processed[j] != arr[i][1] for j in range(max(0, time - n), time)):
                    # Pick the task with the maximum remaining frequency
                    if maxi == -1 or arr[maxi][0] < arr[i][0]:
                        maxi = i
            
            # Advance time
            time += 1
            cur = -1
            
            # If a valid task was found, decrement its frequency
            if maxi != -1:
                cur = arr[maxi][1]
                arr[maxi][0] -= 1
                # Remove the task if its frequency is depleted
                if arr[maxi][0] == 0:
                    arr.pop(maxi)
            
            # Record the processed task (or idle state if cur = -1)
            processed.append(cur)
        
        # The total time taken is the maximum number of scheduling slots used
        return time
```

### APPROACH: GREEDY

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Count the frequency of each task
        count = [0] * 26
        for task in tasks:
            count[ord(task) - ord('A')] += 1
        
        # Sort frequencies in ascending order
        count.sort()
        
        # Max frequency among tasks
        maxf = count[25]
        
        # Calculate initial idle time slots
        idle = (maxf - 1) * n
        
        # Reduce the idle time based on other tasks
        for i in range(24, -1, -1):
            idle -= min(maxf - 1, count[i])
        
        # Total time is tasks plus idle slots (idle cannot be negative)
        return max(0, idle) + len(tasks)
```

### APPROACH: MATH

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # Count how many times each task appears
        count = [0] * 26
        for task in tasks:
            count[ord(task) - ord('A')] += 1
        
        # Find the highest task frequency
        maxf = max(count)
        
        # Count how many tasks appear with this max frequency
        maxCount = 0
        for i in count:
            maxCount += 1 if i == maxf else 0

        # Calculate the total time using the formula
        time = (maxf - 1) * (n + 1) + maxCount
        
        # The result is the maximum between the total tasks length and the computed time
        return max(len(tasks), time)
```
