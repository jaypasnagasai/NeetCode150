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
        newStr = ""

        for c in s:
            if c.isaln():
                newStr += c.islower()
        return newStr == newStr[::-1]
```
