# 1046. Last Stone Weight
Difficulty: Easy

## QUESTION

You are given an array of integers `stones` where `stones[i]` represents the weight of the `ith` stone.

We want to run a simulation on the stones as follows:

- At each step we choose the two heaviest stones, with weight `x` and `y` and smash them togethers
- If `x == y`, both stones are destroyed
- If `x < y`, the stone of weight `x` is destroyed, and the stone of weight `y` has new weight `y - x`.

Continue the simulation until there is no more than one stone remaining.

Return the weight of the last remaining stone or return `0` if none remain.

### EXAMPLE

```
Input: stones = [2,3,6,2,4]
Output: 1
```

```
Input: stones = [1,2]
Output: 1
```

## SOLUTION


### APPROACH: SORTING

```python
import heapq

class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        """
        Simulates the process of smashing stones together, returning the weight of the last remaining stone.

        :param stones: List of stone weights.
        :return: The weight of the last stone, or 0 if no stones remain.
        """
        # Convert stones to a max heap by negating values (Python has a min heap by default)
        stones = [-s for s in stones]
        heapq.heapify(stones)  # Transform list into a max heap

        while len(stones) > 1:
            # Extract the two heaviest stones (largest values in negative form)
            first = -heapq.heappop(stones)
            second = -heapq.heappop(stones)
            
            # If they are not equal, push the remaining weight back into the heap
            if first != second:
                heapq.heappush(stones, -(first - second))

        # Return the last stone weight or 0 if no stones remain
        return -stones[0] if stones else 0
```

### APPROACH: HEAP

```python
import heapq

class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        """
        Simulates the process of smashing stones together using a max heap.

        :param stones: List of stone weights.
        :return: The weight of the last remaining stone, or 0 if no stones remain.
        """
        # Convert stones into a max heap by negating values (since Python's heapq is a min-heap)
        stones = [-s for s in stones]
        heapq.heapify(stones)  # Transform list into a max heap

        while len(stones) > 1:
            first = heapq.heappop(stones)  # Extract the heaviest stone
            second = heapq.heappop(stones)  # Extract the second heaviest stone

            if second > first:  # If the stones have different weights
                heapq.heappush(stones, first - second)  # Push the remaining weight back into the heap

        stones.append(0)  # Ensure there's always at least one element to return
        return abs(stones[0])  # Return the absolute value of the last remaining stone
```

### APPROACH: BUCKET SORT

```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        """
        Simulates the process of smashing stones together using a counting sort (bucket approach).

        :param stones: List of stone weights.
        :return: The weight of the last remaining stone, or 0 if no stones remain.
        """
        maxStone = max(stones)  # Find the maximum stone weight
        bucket = [0] * (maxStone + 1)  # Create a frequency array (bucket)

        # Populate the bucket with stone frequencies
        for stone in stones:
            bucket[stone] += 1
        
        first = second = maxStone  # Track the two heaviest stones

        while first > 0:
            # If there are an even number of stones of weight `first`, skip them
            if bucket[first] % 2 == 0:
                first -= 1
                continue
            
            # Find the next heaviest stone
            j = min(first - 1, second)
            while j > 0 and bucket[j] == 0:
                j -= 1
            
            # If no second stone is found, return the remaining stone
            if j == 0:
                return first
            
            second = j  # Set the second heaviest stone
            bucket[first] -= 1  # Smash one `first` stone
            bucket[second] -= 1  # Smash one `second` stone
            bucket[first - second] += 1  # Store the new stone weight
            
            first = max(first - second, second)  # Update `first` to the heaviest remaining stone

        return first  # Return the last remaining stone's weight
```
