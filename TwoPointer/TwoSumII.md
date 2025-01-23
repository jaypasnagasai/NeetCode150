# 167. Two Sum II - Input Array Is Sorted
Difficulty: Medium

## QUESTION

Given a 1-indexed array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number. Let these two numbers be numbers[index1] and numbers[index2] where 1 <= index1 < index2 <= numbers.length.

Return the indices of the two numbers, index1 and index2, added by one as an integer array [index1, index2] of length 2.

The tests are generated such that there is exactly one solution. You may not use the same element twice.

Your solution must use only constant extra space.

### EXAMPLE

```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].
```

```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].
```

```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].
```

## SOLUTION

### APPROACH: REVERSE STRING

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Initialize an empty string to hold only alphanumeric characters
        newStr = ""

        # Iterate over each character in the input string
        for c in s:
            # Check if the character is alphanumeric (letters or numbers)
            if c.isalnum():
                # Add the lowercase version of the character to the new string
                newStr += c.lower()

        # Compare the filtered string with its reverse to determine if it's a palindrome
        return newStr == newStr[::-1]
```

### APPROACH: TWO POINTER

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        # Initialize two pointers: one at the start (l) and one at the end (r) of the string
        l, r = 0, len(s) - 1

        # Loop until the two pointers meet in the middle
        while l < r:
            # Move the left pointer forward if the character at `l` is not alphanumeric
            while l < r and not self.alphaNum(s[l]):
                l += 1
            # Move the right pointer backward if the character at `r` is not alphanumeric
            while r > l and not self.alphaNum(s[r]):
                r -= 1

            # Compare the characters at the left and right pointers (case insensitive)
            if s[l].lower() != s[r].lower():
                return False  # If they don't match, it's not a palindrome

            # Move both pointers closer to the center
            l, r = l + 1, r - 1

        # If the loop completes without mismatches, the string is a palindrome
        return True

    def alphaNum(self, c):
        # Check if the character is alphanumeric using ASCII values
        return (ord('A') <= ord(c) <= ord('Z') or  # Uppercase letters
                ord('a') <= ord(c) <= ord('z') or  # Lowercase letters
                ord('0') <= ord(c) <= ord('9'))    # Digits
```
