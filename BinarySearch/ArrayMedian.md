# 4. Median of Two Sorted Arrays
Difficulty: Hard

## QUESTION

You are given two integer arrays `nums1` and `nums2` of size `m` and `n` respectively, where each is sorted in ascending order. Return the median value among all elements of the two arrays.

Your solution must run in O(log(m+n)) time.

### EXAMPLE

```
Input: nums1 = [1,2], nums2 = [3]
Output: 2.0
```

```
Input: nums1 = [1,3], nums2 = [2,4]
Output: 2.5
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        # Get the lengths of both input arrays
        len1 = len(nums1)
        len2 = len(nums2)
        
        # Merge both sorted arrays into one
        merged = nums1 + nums2
        merged.sort()  # Sorting the merged array
        
        # Calculate the total length of the merged array
        totalLen = len(merged)
        
        # If the total length is even, return the average of the two middle elements
        if totalLen % 2 == 0:
            return (merged[totalLen // 2 - 1] + merged[totalLen // 2]) / 2.0
        else:
            # If the total length is odd, return the middle element
            return merged[totalLen // 2]
```

### APPROACH: TWO POINTER

```python
class Solution:
    def findMedianSortedArrays(self, nums1, nums2):
        # Get the lengths of both input arrays
        len1, len2 = len(nums1), len(nums2)
        
        # Initialize pointers for both arrays
        i = j = 0
        
        # Variables to store the two middle elements
        median1 = median2 = 0

        # Iterate until we reach the middle index of the combined sorted array
        for count in range((len1 + len2) // 2 + 1):
            median2 = median1  # Store the previous median value
            
            # Compare elements from both arrays and move the pointer in the smaller element's array
            if i < len1 and j < len2:
                if nums1[i] > nums2[j]:
                    median1 = nums2[j]
                    j += 1
                else:
                    median1 = nums1[i]
                    i += 1
            elif i < len1:  # If elements are remaining in nums1
                median1 = nums1[i]
                i += 1
            else:  # If elements are remaining in nums2
                median1 = nums2[j]
                j += 1

        # If the total length is odd, return the single middle element
        if (len1 + len2) % 2 == 1:
            return float(median1)
        else:  # If the total length is even, return the average of the two middle elements
            return (median1 + median2) / 2.0
```

### APPROACH: BINARY SEARCH

```python
from typing import List  # Importing List type hint from the typing module

class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        # Ensure the smaller array is A for efficient binary search
        A, B = nums1, nums2
        total = len(nums1) + len(nums2)
        half = total // 2

        if len(B) < len(A):  # Swap A and B if necessary to keep A as the smaller array
            A, B = B, A

        l, r = 0, len(A) - 1  # Binary search on the smaller array A
        while True:
            i = (l + r) // 2  # Partition index for A
            j = half - i - 2  # Partition index for B

            # Elements around the partition
            Aleft = A[i] if i >= 0 else float("-infinity")  # Left side of A
            Aright = A[i + 1] if (i + 1) < len(A) else float("infinity")  # Right side of A
            Bleft = B[j] if j >= 0 else float("-infinity")  # Left side of B
            Bright = B[j + 1] if (j + 1) < len(B) else float("infinity")  # Right side of B

            # Valid partition: all elements in left partition are ≤ elements in right partition
            if Aleft <= Bright and Bleft <= Aright:
                if total % 2:  # If total length is odd, return the median element
                    return min(Aright, Bright)
                # If total length is even, return the average of the two middle elements
                return (max(Aleft, Bleft) + min(Aright, Bright)) / 2
            elif Aleft > Bright:
                r = i - 1  # Move left in A
            else:
                l = i + 1  # Move right in A
```
