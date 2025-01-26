# 000. Evaluate Reverse Polish Notation
Difficulty: Medium

## QUESTION

You are given an array of strings tokens that represents a valid arithmetic expression in [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).

Return the integer that represents the evaluation of the expression.

- The operands may be integers or the results of other operations.
- The operators include '+', '-', '*', and '/'.
- Assume that division between integers always truncates toward zero.

### EXAMPLE

```
Input: tokens = ["1","2","+","3","*","4","-"]
Output: 5
Explanation: ((1 + 2) * 3) - 4 = 5
```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class Solution:
    # Define a method 'evalRPN' that evaluates a list of tokens in Reverse Polish Notation (RPN).
    def evalRPN(self, tokens: List[str]) -> int:
        # Loop until there is only one token left in the list (the final result).
        while len(tokens) > 1:
            # Iterate through the tokens to find the first operator.
            for i in range(len(tokens)):
                # Check if the current token is an operator.
                if tokens[i] in "+-*/":
                    # Retrieve the two operands preceding the operator.
                    a = int(tokens[i-2])
                    b = int(tokens[i-1])
                    
                    # Perform the operation based on the operator.
                    if tokens[i] == '+':
                        result = a + b
                    elif tokens[i] == '-':
                        result = a - b
                    elif tokens[i] == '*':
                        result = a * b
                    elif tokens[i] == '/':
                        # Use integer division to truncate towards zero.
                        result = int(a / b)
                    
                    # Replace the operator and its two operands with the result in the token list.
                    tokens = tokens[:i-2] + [str(result)] + tokens[i+1:]
                    # Break the loop after processing one operation.
                    break
        
        # After all operations, the final result is the only remaining token.
        return int(tokens[0])

```

### APPROACH: DOUBLY LINKED LIST

```python
# Class representing a node in a doubly linked list.
class DoublyLinkedList:
    def __init__(self, val, next=None, prev=None):
        self.val = val    # Value of the node.
        self.next = next  # Pointer to the next node.
        self.prev = prev  # Pointer to the previous node.

class Solution:
    # Method to evaluate an RPN expression using a doubly linked list.
    def evalRPN(self, tokens: List[str]) -> int:
        # Create the head of the doubly linked list with the first token.
        head = DoublyLinkedList(tokens[0])
        curr = head

        # Build the doubly linked list by iterating over the tokens.
        for i in range(1, len(tokens)):
            curr.next = DoublyLinkedList(tokens[i], prev=curr)
            curr = curr.next

        # Traverse and evaluate the expression in the doubly linked list.
        while head is not None:
            # Check if the current node contains an operator.
            if head.val in "+-*/":
                # Retrieve the two operands from the previous nodes.
                l = int(head.prev.prev.val)  # Left operand.
                r = int(head.prev.val)       # Right operand.

                # Perform the operation based on the operator.
                if head.val == '+':
                    res = l + r
                elif head.val == '-':
                    res = l - r
                elif head.val == '*':
                    res = l * r
                else:
                    # Integer division truncating towards zero.
                    res = int(l / r)

                # Replace the operator node's value with the result.
                head.val = str(res)
                # Update pointers to remove the two operand nodes.
                head.prev = head.prev.prev.prev
                if head.prev is not None:
                    head.prev.next = head

            # Store the final result before moving to the next node.
            ans = int(head.val)
            # Move to the next node in the list.
            head = head.next

        # Return the final result after the entire list is processed.
        return ans
```


### APPROACH: RECURSION

```python
class Solution:
    # Define a method 'evalRPN' that evaluates an RPN expression using recursion.
    def evalRPN(self, tokens: List[str]) -> int:
        # Define a helper function for depth-first evaluation.
        def dfs():
            # Pop the last token from the tokens list (traverse in reverse order).
            token = tokens.pop()
            # If the token is not an operator, it is an operand, so return its integer value.
            if token not in "+-*/":
                return int(token)
            
            # If the token is an operator, recursively evaluate the right and left operands.
            # Note: The right operand is evaluated first because of the stack-like behavior of RPN.
            right = dfs()
            left = dfs()
            
            # Perform the operation based on the token (operator).
            if token == '+':
                return left + right
            elif token == '-':
                return left - right
            elif token == '*':
                return left * right
            elif token == '/':
                # Use integer division truncating towards zero.
                return int(left / right)
        
        # Call the helper function and return the final result.
        return dfs()
```

### APPROACH: STACK

```python
class Solution:
    # Define a method 'evalRPN' that evaluates a list of tokens in Reverse Polish Notation.
    def evalRPN(self, tokens: List[str]) -> int:
        # Initialize an empty stack to store operands and intermediate results.
        stack = []

        # Iterate through each token in the input list.
        for c in tokens:
            # If the token is an operator, pop the required number of operands from the stack
            # and perform the corresponding operation.
            if c == "+":
                # Pop the last two operands, add them, and push the result back onto the stack.
                stack.append(stack.pop() + stack.pop())
            elif c == "-":
                # Pop the last two operands, subtract the first popped from the second, and push the result.
                a, b = stack.pop(), stack.pop()
                stack.append(b - a)
            elif c == "*":
                # Pop the last two operands, multiply them, and push the result back onto the stack.
                stack.append(stack.pop() * stack.pop())
            elif c == "/":
                # Pop the last two operands, perform integer division (truncating towards zero),
                # and push the result back onto the stack.
                a, b = stack.pop(), stack.pop()
                stack.append(int(float(b) / a))
            else:
                # If the token is an operand, convert it to an integer and push it onto the stack.
                stack.append(int(c))

        # After all tokens are processed, the result will be the only element left in the stack.
        return stack[0]
```
