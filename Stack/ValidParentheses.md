# 20. Valid Parentheses
Difficulty: Easy

## QUESTION

You are given a string `s` consisting of the following characters: `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`.

The input string `s` is valid if and only if:

Every open bracket is closed by the same type of close bracket.
Open brackets are closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

Return `true` if `s` is a valid string, and `false` otherwise.

### EXAMPLE

```
Input: s = "[]"
Output: true
```

```
Input: s = "([{}])"
Output: true
```

```
Input: s = "[(])"
Output: false
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'isValid' that checks if the input string 's' has valid parentheses.
    def isValid(self, s: str) -> bool:
        # Keep reducing the string by replacing valid pairs of parentheses until no more replacements can be made.
        while '()' in s or '{}' in s or '[]' in s:
            # Replace all occurrences of '()', '{}', and '[]' with an empty string.
            s = s.replace('()', '')
            s = s.replace('{}', '')
            s = s.replace('[]', '')
        
        # If the string is empty after all replacements, the parentheses were valid.
        return s == ''
```

### APPROACH: STACK

```python
class Solution:
    # Define a method 'isValid' that checks if the input string 's' has valid parentheses.
    def isValid(self, s: str) -> bool:
        # Initialize an empty stack to keep track of opening brackets.
        stack = []
        # Define a dictionary that maps closing brackets to their corresponding opening brackets.
        closeToOpen = { ")" : "(", "]" : "[", "}" : "{" }

        # Iterate through each character in the input string.
        for c in s:
            # If the character is a closing bracket:
            if c in closeToOpen:
                # Check if the stack is not empty and the top of the stack matches the corresponding opening bracket.
                if stack and stack[-1] == closeToOpen[c]:
                    # If so, pop the top of the stack as the pair is valid.
                    stack.pop()
                else:
                    # Otherwise, return False as the string is invalid.
                    return False
            else:
                # If the character is an opening bracket, push it onto the stack.
                stack.append(c)
        
        # If the stack is empty at the end, all brackets were valid and matched; otherwise, return False.
        return True if not stack else False
```
