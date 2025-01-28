# 347. Top K Frequent Elements
Difficulty: Medium

## QUESTION

Given an integer array `nums` and an integer `k`, return the `k` most frequent elements within the array.

The test cases are generated such that the answer is always unique.

You may return the output in any order.

### EXAMPLE

```
Input: nums = [1,2,2,3,3,3], k = 2
Output: [2,3]
```

```
Input: nums = [7,7], k = 1
Output: [7]
```

## SOLUTION


### APPROACH: SORTING

```python
class Solution:
    # Define a method 'topKFrequent' to find the K most frequent elements in a list.
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Create a dictionary to count the frequency of each number in the list.
        count = {}
        for num in nums:
            # Increment the count of the current number or initialize it to 1 if not present.
            count[num] = 1 + count.get(num, 0)

        # Create a list to store pairs of (frequency, number).
        arr = []
        for num, cnt in count.items():
            # Append each pair of frequency and number to the list.
            arr.append([cnt, num])

        # Sort the array by frequency in ascending order.
        arr.sort()

        # Initialize a result list to store the top K frequent elements.
        res = []
        while len(res) < k:
            # Pop the element with the highest frequency (last element after sorting).
            res.append(arr.pop()[1])

        # Return the result list containing the top K frequent elements.
        return res
```

### APPROACH: HEAP

```python
class Solution:
    # Define a method 'topKFrequent' to find the K most frequent elements in a list.
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Create a dictionary to count the frequency of each number in the list.
        count = {}
        for num in nums:
            # Increment the count of the current number or initialize it to 1 if not present.
            count[num] = 1 + count.get(num, 0)

        # Initialize a min-heap to store the top K frequent elements.
        heap = []

        # Iterate through the keys of the frequency dictionary.
        for num in count.keys():
            # Push the frequency and number as a tuple onto the heap.
            heapq.heappush(heap, (count[num], num))
            # If the size of the heap exceeds K, pop the smallest frequency element.
            if len(heap) > k:
                heapq.heappop(heap)

        # Initialize a result list to store the top K frequent elements.
        res = []
        # Pop all elements from the heap and extract the numbers.
        for i in range(k):
            res.append(heapq.heappop(heap)[1])

        # Return the result list.
        return res
```

### APPROACH: BUCKET SORT

```python
class Solution:
    # Define a method 'topKFrequent' to find the K most frequent elements in a list.
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        # Create a dictionary to count the frequency of each number in the list.
        count = {}
        # Initialize a list of buckets, where the index represents frequency.
        freq = [[] for i in range(len(nums) + 1)]

        # Count the frequency of each number in the list.
        for num in nums:
            count[num] = 1 + count.get(num, 0)
        
        # Place numbers into buckets based on their frequency.
        for num, cnt in count.items():
            freq[cnt].append(num)

        # Initialize a result list to store the top K frequent elements.
        res = []

        # Traverse the buckets in reverse order to get the most frequent elements.
        for i in range(len(freq) - 1, 0, -1):  # Start from the highest frequency.
            for num in freq[i]:  # Iterate through all numbers in the current bucket.
                res.append(num)  # Add the number to the result list.
                if len(res) == k:  # If we have collected K elements, return the result.
                    return res
```

