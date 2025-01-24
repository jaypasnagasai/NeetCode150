# 125 - Valid Palindrome
Difficulty: Easy

## QUESTION

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. **Alphanumeric** characters include letters and numbers.

Given a string `s`, return `true` if it is a palindrome, or `false` otherwise.

### EXAMPLE

```
Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.
```

```
Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.
```

```
Input: s = " "
Output: true
Explanation: s is an empty string "" after removing non-alphanumeric characters.
Since an empty string reads the same forward and backward, it is a palindrome.
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
