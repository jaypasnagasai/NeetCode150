# 215. Kth Largest Element in an Array
Difficulty: Medium

## QUESTION

Given an unsorted array of integers `nums` and an integer `k`, return the `kth` largest element in the array.

By `kth` largest element, we mean the `kth` largest element in the sorted order, not the `kth` distinct element.

Follow-up: Can you solve it without sorting?

### EXAMPLE

```
Input: nums = [2,3,1,5,4], k = 2
Output: 4
```

```
Input: nums = [2,3,1,1,5,5,4], k = 3
Output: 4
```

## SOLUTION


### APPROACH: SORTING

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Sort the list in ascending order
        nums.sort()
        # Return the k-th largest element
        return nums[len(nums) - k]
```

### APPROACH: MIN-HEAP

```python
import heapq

class Solution:
    def findKthLargest(self, nums, k):
        # nlargest returns the k largest elements, sorted in descending order
        # The last element (index -1) in that list is the k-th largest element
        return heapq.nlargest(k, nums)[-1]
```

### APPROACH: QUICK SELECT

```python
class Solution:
    def partition(self, nums: List[int], left: int, right: int) -> int:
        # Calculate mid index
        mid = (left + right) >> 1
        # Swap mid element with left+1
        nums[mid], nums[left + 1] = nums[left + 1], nums[mid]
        
        # Rearrange the first three elements
        if nums[left] < nums[right]:
            nums[left], nums[right] = nums[right], nums[left]
        if nums[left + 1] < nums[right]:
            nums[left + 1], nums[right] = nums[right], nums[left + 1]
        if nums[left] < nums[left + 1]:
            nums[left], nums[left + 1] = nums[left + 1], nums[left]
        
        # Set pivot
        pivot = nums[left + 1]
        i = left + 1
        j = right
        
        # Partition the array based on pivot
        while True:
            while True:
                i += 1
                if not nums[i] > pivot:
                    break
            while True:
                j -= 1
                if not nums[j] < pivot:
                    break
            if i > j:
                break
            nums[i], nums[j] = nums[j], nums[i]
        
        # Final swap to place pivot at its correct position
        nums[left + 1], nums[j] = nums[j], nums[left + 1]
        return j
    
    def quickSelect(self, nums: List[int], k: int) -> int:
        # Initialize boundaries
        left = 0
        right = len(nums) - 1
        
        # Quickselect loop
        while True:
            if right <= left + 1:
                if right == left + 1 and nums[right] > nums[left]:
                    nums[left], nums[right] = nums[right], nums[left]
                return nums[k]
            
            # Partition the array
            j = self.partition(nums, left, right)
            
            # Narrow down the search space
            if j >= k:
                right = j - 1
            if j <= k:
                left = j + 1
    
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # K-th largest means k-1 index in 0-based quickselect
        return self.quickSelect(nums, k - 1)
```
