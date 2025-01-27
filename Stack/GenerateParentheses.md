# 22. Generate Parentheses
Difficulty: Medium

## QUESTION

You are given an integer `n`. Return all well-formed parentheses strings that you can generate with `n` pairs of parentheses.

### EXAMPLE

```
Input: n = 1
Output: ["()"]
```

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'generateParenthesis' that generates all valid combinations of 'n' pairs of parentheses.
    def generateParenthesis(self, n: int) -> List[str]:
        # Initialize a list to store the resulting valid combinations.
        res = []

        # Helper function to check if a string of parentheses is valid.
        def valid(s: str) -> bool:
            # Counter to track the balance of parentheses.
            open = 0
            # Iterate through each character in the string.
            for c in s:
                # Increment the counter for an opening parenthesis '('.
                open += 1 if c == '(' else -1
                # If the counter goes negative, the string is invalid.
                if open < 0:
                    return False
            # Return True if the counter is zero (balanced), otherwise False.
            return not open

        # Depth-First Search (DFS) helper function to generate combinations.
        def dfs(s: str):
            # Base case: If the string has 2*n characters, check if it's valid.
            if n * 2 == len(s):
                if valid(s):
                    res.append(s)  # Add the valid string to the results.
                return
            
            # Recursive calls to add either '(' or ')' to the string.
            dfs(s + '(')
            dfs(s + ')')
        
        # Start the DFS with an empty string.
        dfs("")
        # Return the list of valid combinations.
        return res
```

### APPROACH: BACKTRACKING

```python
class Solution:
    # Define a method 'generateParenthesis' that generates all valid combinations of 'n' pairs of parentheses.
    def generateParenthesis(self, n: int) -> List[str]:
        # Initialize a stack to build the current combination and a list to store results.
        stack = []
        res = []

        # Helper function for backtracking, taking counts of open and closed parentheses.
        def backtrack(openN, closedN):
            # Base case: If both open and closed counts reach 'n', a valid combination is formed.
            if openN == closedN == n:
                # Add the valid combination to the result list.
                res.append("".join(stack))
                return

            # Add an open parenthesis '(' if the open count is less than 'n'.
            if openN < n:
                stack.append("(")
                # Recur with an incremented open count.
                backtrack(openN + 1, closedN)
                # Backtrack by removing the last added '('.
                stack.pop()
            
            # Add a close parenthesis ')' if the closed count is less than the open count.
            if closedN < openN:
                stack.append(")")
                # Recur with an incremented closed count.
                backtrack(openN, closedN + 1)
                # Backtrack by removing the last added ')'.
                stack.pop()

        # Start backtracking with zero open and closed parentheses.
        backtrack(0, 0)
        # Return the list of valid combinations.
        return res
```

### APPROACH: DYNAMIC PROGRAMMING

```python
class Solution:
    # Define a method 'generateParenthesis' to generate all valid combinations of 'n' pairs of parentheses.
    def generateParenthesis(self, n):
        # Initialize a list to store results for 0 to n pairs of parentheses.
        # Each element 'res[k]' will store all valid combinations for 'k' pairs.
        res = [[] for _ in range(n + 1)]
        # Base case: For 0 pairs of parentheses, the only valid combination is an empty string.
        res[0] = [""]

        # Build the results for each number of pairs from 1 to n.
        for k in range(n + 1):
            # For each 'k', iterate through all possible ways to split the pairs.
            for i in range(k):
                # 'i' pairs of parentheses go inside the current parentheses.
                for left in res[i]:
                    # 'k - i - 1' pairs go outside the current parentheses.
                    for right in res[k - i - 1]:
                        # Combine the results of 'i' and 'k - i - 1' pairs with a new pair.
                        res[k].append("(" + left + ")" + right)

        # Return the results for 'n' pairs of parentheses.
        return res[-1]
```
