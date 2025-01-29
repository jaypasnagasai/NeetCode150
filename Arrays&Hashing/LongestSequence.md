# 000. Longest Consecutive Sequence
Difficulty: Medium

## QUESTION

Given an array of integers `nums`, return the length of the longest consecutive sequence of elements that can be formed.

A consecutive sequence is a sequence of elements in which each element is exactly `1` greater than the previous element. The elements do not have to be consecutive in the original array.

You must write an algorithm that runs in `O(n)` time.

### EXAMPLE

```
Input: nums = [2,20,4,10,3,4,5]
Output: 4
```

```
Input: nums = [0,3,2,5,4,6,1,1]
Output: 7
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'longestConsecutive' to find the longest consecutive sequence in an unsorted list.
    def longestConsecutive(self, nums: List[int]) -> int:
        # Initialize a variable to track the maximum sequence length.
        res = 0
        # Convert the input list into a set for O(1) lookups.
        store = set(nums)

        # Iterate through each number in the original list.
        for num in nums:
            # Initialize streak counter and current number tracker.
            streak, curr = 0, num

            # While the current number exists in the set, extend the sequence.
            while curr in store:
                streak += 1
                curr += 1  # Move to the next consecutive number.

            # Update the maximum sequence length found.
            res = max(res, streak)

        # Return the length of the longest consecutive sequence.
        return res
```

### APPROACH: SORTING

```python
class Solution:
    # Define a method 'longestConsecutive' to find the longest consecutive sequence in an unsorted list.
    def longestConsecutive(self, nums: List[int]) -> int:
        # Edge case: If the input list is empty, return 0.
        if not nums:
            return 0

        # Initialize the maximum streak length.
        res = 0
        # Sort the array to bring consecutive numbers next to each other.
        nums.sort()

        # Initialize variables to track the current number and streak count.
        curr, streak = nums[0], 0
        i = 0

        # Iterate through the sorted array.
        while i < len(nums):
            # If the current number is not the same as nums[i], reset streak.
            if curr != nums[i]:
                curr = nums[i]
                streak = 0

            # Skip duplicate numbers.
            while i < len(nums) and nums[i] == curr:
                i += 1

            # Extend the streak and move to the next expected number.
            streak += 1
            curr += 1

            # Update the longest streak found.
            res = max(res, streak)

        # Return the length of the longest consecutive sequence.
        return res
```

### APPROACH: HASH SET

```python
class Solution:
    # Define a method 'longestConsecutive' to find the longest consecutive sequence in an unsorted list.
    def longestConsecutive(self, nums: List[int]) -> int:
        # Convert the input list into a set for O(1) lookups.
        numSet = set(nums)
        # Initialize a variable to track the maximum sequence length.
        longest = 0

        # Iterate through each unique number in the set.
        for num in numSet:
            # Only start counting if 'num - 1' is not in the set (ensures we start from the beginning of a sequence).
            if (num - 1) not in numSet:
                length = 1  # Start with a sequence of length 1.
                
                # Extend the sequence by checking if consecutive numbers exist.
                while (num + length) in numSet:
                    length += 1

                # Update the longest sequence found.
                longest = max(length, longest)

        # Return the length of the longest consecutive sequence.
        return longest
```

### APPROACH: HASH MAP

```python
class Solution:
    # Define a method 'longestConsecutive' to find the longest consecutive sequence in an unsorted list.
    def longestConsecutive(self, nums: List[int]) -> int:
        # Use a defaultdict to track sequence lengths dynamically.
        mp = defaultdict(int)
        # Initialize a variable to track the maximum sequence length.
        res = 0

        # Iterate through each number in the input list.
        for num in nums:
            # If the number is not already present in the map, process it.
            if not mp[num]:
                # Compute the length of the sequence at num.
                mp[num] = mp[num - 1] + mp[num + 1] + 1

                # Update the boundary values of the sequence.
                mp[num - mp[num - 1]] = mp[num]  # Update left boundary.
                mp[num + mp[num + 1]] = mp[num]  # Update right boundary.

                # Update the maximum sequence length.
                res = max(res, mp[num])

        # Return the length of the longest consecutive sequence.
        return res
```
