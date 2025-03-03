# 678. Valid Parenthesis String
Difficulty: Medium

## QUESTION

You are given a string `s` which contains only three types of characters: `'('`, `')'` and `'*'`.

Return `true` if `s` is valid, otherwise return `false`.

A string is valid if it follows all of the following rules:

- Every left parenthesis `'('` must have a corresponding right parenthesis `')'`.
- Every right parenthesis `')'` must have a corresponding left parenthesis `'('`.
- Left parenthesis `'('` must go before the corresponding right parenthesis `')'`.
- A `'*'` could be treated as a right parenthesis `')'` character or a left parenthesis `'('` character, or as an empty string `""`.

### EXAMPLE

```
Input: s = "((**)"
Output: true
```

```
Input: s = "(((*)"
Output: false
```

## SOLUTION


### APPROACH: RECURSION

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        
        def dfs(i, open):
            if open < 0:  # If open parentheses count goes negative, return False
                return False
            if i == len(s):  # If reached the end of the string, check if open count is 0
                return open == 0
            
            if s[i] == '(':  # If current character is '(', increment open count
                return dfs(i + 1, open + 1)
            elif s[i] == ')':  # If current character is ')', decrement open count
                return dfs(i + 1, open - 1)
            else:  # If current character is '*', consider all three possibilities
                return (dfs(i + 1, open) or  # Treat '*' as empty (ignore it)
                        dfs(i + 1, open + 1) or  # Treat '*' as '('
                        dfs(i + 1, open - 1))  # Treat '*' as ')'
        
        return dfs(0, 0)  # Start DFS from index 0 with open parentheses count as 0
```

### APPROACH: STACK

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        left = []  # Stack to store indices of '('
        star = []  # Stack to store indices of '*'
        
        # First pass: process '(' and '*' while checking ')'
        for i, ch in enumerate(s):
            if ch == '(':
                left.append(i)  # Store index of '('
            elif ch == '*':
                star.append(i)  # Store index of '*'
            else:  # ch == ')'
                if not left and not star:  # No matching '(' or '*' available
                    return False
                if left:
                    left.pop()  # Match ')' with '('
                else:
                    star.pop()  # Match ')' with '*'
        
        # Second pass: match remaining '(' with available '*'
        while left and star:
            if left.pop() > star.pop():  # '(' must appear before '*' for valid matching
                return False
        
        return not left  # Return True if all '(' are matched
```

### APPROACH: GREEDY

```python
class Solution:
    def checkValidString(self, s: str) -> bool:
        # Variables to track the possible range of open parentheses
        leftMin, leftMax = 0, 0

        # Iterate through the string
        for c in s:
            if c == "(":
                # Increase both min and max open counts for '('
                leftMin, leftMax = leftMin + 1, leftMax + 1
            elif c == ")":
                # Decrease both min and max open counts for ')'
                leftMin, leftMax = leftMin - 1, leftMax - 1
            else:  # '*' can act as '(', ')' or ''
                leftMin, leftMax = leftMin - 1, leftMax + 1

            # If at any point max open count goes negative, it's invalid
            if leftMax < 0:
                return False

            # Ensure min open count is not negative (reset to 0)
            if leftMin < 0:
                leftMin = 0

        # If min open count is 0, it means a valid sequence was formed
        return leftMin == 0
```
