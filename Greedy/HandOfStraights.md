# 846. Hand of Straights
Difficulty: Medium

## QUESTION

You are given an integer array `hand` where `hand[i]` is the value written on the `ith` card and an integer `groupSize`.

You want to rearrange the cards into groups so that each group is of size `groupSize`, and card values are consecutively increasing by `1`.

Return `true` if it's possible to rearrange the cards in this way, otherwise, return `false`.

### EXAMPLE

```
Input: hand = [1,2,4,2,3,5,3,4], groupSize = 4
Output: true
```

```
Input: hand = [1,2,3,3,4,5,6,7], groupSize = 4
Output: false
```

## SOLUTION


### APPROACH: SORTING

```python
from collections import Counter

class Solution:
    def isNStraightHand(self, hand, groupSize):
        # If the total number of cards isn't a multiple of groupSize, return False
        if len(hand) % groupSize:
            return False
        
        # Count the frequency of each card
        count = Counter(hand)
        
        # Sort the hand to process cards in ascending order
        hand.sort()

        # Try forming groups
        for num in hand:
            if count[num]:  # If this card is still available
                for i in range(num, num + groupSize):
                    if not count[i]:  # If the required card is missing, return False
                        return False
                    count[i] -= 1  # Use one occurrence of this card
        
        return True  # Successfully formed all groups
```

### APPROACH: HEAP

```python
import heapq
from typing import List

class Solution:
    def isNStraightHand(self, hand: List[int], groupSize: int) -> bool:
        # If the total number of cards isn't a multiple of groupSize, return False
        if len(hand) % groupSize:
            return False

        # Count the frequency of each card
        count = {}
        for n in hand:
            count[n] = 1 + count.get(n, 0)

        # Min-heap to process the smallest available card first
        minH = list(count.keys())
        heapq.heapify(minH)

        # Process groups from the smallest available card
        while minH:
            first = minH[0]  # Get the smallest available card
            
            # Try to form a group of size `groupSize`
            for i in range(first, first + groupSize):
                if i not in count:  # If the required card is missing, return False
                    return False
                count[i] -= 1  # Use one occurrence of this card
                
                # If count[i] reaches zero, remove it from the heap
                if count[i] == 0:
                    if i != minH[0]:  # Ensure that it's the top of the heap
                        return False
                    heapq.heappop(minH)  # Remove from heap

        return True  # Successfully formed all groups
```

### APPROACH: HEAP MAP

```python
from collections import Counter
from typing import List

class Solution:
    def isNStraightHand(self, hand: List[int], groupSize: int) -> bool:
        # If the total number of cards isn't a multiple of groupSize, return False
        if len(hand) % groupSize != 0:
            return False

        # Count the frequency of each card
        count = Counter(hand)

        # Process each card in the sorted order
        for num in hand:
            start = num
            # Move to the smallest available card in the group
            while count[start - 1]:
                start -= 1

            # Process groups
            while start <= num:
                while count[start]:
                    # Try to form a group of size `groupSize`
                    for i in range(start, start + groupSize):
                        if not count[i]:  # If the required card is missing, return False
                            return False
                        count[i] -= 1  # Use one occurrence of this card
                start += 1

        return True  # Successfully formed all groups
```
