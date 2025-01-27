# 155. Min Stack
Difficulty: Medium

## QUESTION

Design a stack class that supports the `push`, `pop`, `top`, and `getMin` operations.

- `MinStack()` initializes the stack object.
- `void push(int val)` pushes the element `val` onto the stack.
- `void pop()` removes the element on the top of the stack.
- `int top()` gets the top element of the stack.
- `int getMin()` retrieves the minimum element in the stack.

  Each function should run in O(1) time.

### EXAMPLE

```
Input: ["MinStack", "push", 1, "push", 2, "push", 0, "getMin", "pop", "top", "getMin"]

Output: [null,null,null,null,0,null,2,1]

Explanation:
MinStack minStack = new MinStack();
minStack.push(1);
minStack.push(2);
minStack.push(0);
minStack.getMin(); // return 0
minStack.pop();
minStack.top();    // return 2
minStack.getMin(); // return 1

```

## SOLUTION

### APPROACH: BRUTE FORCE

```python
class MinStack:
    # Constructor to initialize the MinStack object.
    def __init__(self):
        # Use a list to simulate the stack.
        self.stack = []

    # Method to push an element onto the stack.
    def push(self, val: int) -> None:
        # Add the given value to the top of the stack.
        self.stack.append(val)

    # Method to remove the top element from the stack.
    def pop(self) -> None:
        # Remove the last element from the stack.
        self.stack.pop()

    # Method to retrieve the top element of the stack.
    def top(self) -> int:
        # Return the last element in the stack without removing it.
        return self.stack[-1]

    # Method to retrieve the minimum element in the stack.
    def getMin(self) -> int:
        # Temporary list to store elements while searching for the minimum.
        tmp = []
        # Initialize 'mini' with the top element of the stack.
        mini = self.stack[-1]

        # Iterate through the stack to find the minimum value.
        while len(self.stack):
            # Update 'mini' with the smaller of the current minimum and the top element.
            mini = min(mini, self.stack[-1])
            # Move the top element from the stack to the temporary list.
            tmp.append(self.stack.pop())
        
        # Restore the stack by moving elements back from the temporary list.
        while len(tmp):
            self.stack.append(tmp.pop())
        
        # Return the minimum value found.
        return mini

```

### APPROACH: TWO STACKS

```python
class MinStack:
    # Constructor to initialize the MinStack object.
    def __init__(self):
        # Primary stack to store all values.
        self.stack = []
        # Auxiliary stack to keep track of the minimum value at each level of the stack.
        self.minStack = []

    # Method to push an element onto the stack.
    def push(self, val: int) -> None:
        # Push the value onto the main stack.
        self.stack.append(val)
        # Determine the new minimum by comparing the current value with the
        # top of the minStack (if it exists). Push the smaller value onto the minStack.
        val = min(val, self.minStack[-1] if self.minStack else val)
        self.minStack.append(val)

    # Method to remove the top element from the stack.
    def pop(self) -> None:
        # Pop the top element from both the main stack and the minStack.
        self.stack.pop()
        self.minStack.pop()

    # Method to retrieve the top element of the stack.
    def top(self) -> int:
        # Return the last element in the main stack without removing it.
        return self.stack[-1]

    # Method to retrieve the minimum element in the stack.
    def getMin(self) -> int:
        # Return the top element of the minStack, which stores the current minimum.
        return self.minStack[-1]
```


### APPROACH: ONE STACKS

```python
class MinStack:
    # Constructor to initialize the MinStack object.
    def __init__(self):
        # Use a variable to track the minimum value in the stack.
        self.min = float('inf')
        # Use a single stack to store differences relative to the current minimum.
        self.stack = []

    # Method to push an element onto the stack.
    def push(self, x: int) -> None:
        if not self.stack:
            # If the stack is empty, push 0 (difference) and update the minimum to 'x'.
            self.stack.append(0)
            self.min = x
        else:
            # Store the difference between 'x' and the current minimum.
            # If 'x' is smaller than the current minimum, update the minimum.
            self.stack.append(x - self.min)
            if x < self.min:
                self.min = x

    # Method to remove the top element from the stack.
    def pop(self) -> None:
        if not self.stack:
            return
        
        # Pop the top element (difference) from the stack.
        pop = self.stack.pop()
        
        # If the popped value is negative, it means the popped element was the minimum.
        # Update the minimum to restore the previous minimum.
        if pop < 0:
            self.min = self.min - pop

    # Method to retrieve the top element of the stack.
    def top(self) -> int:
        top = self.stack[-1]
        # If the top value is positive, it is the actual difference, so add it to 'min'.
        # If the top value is negative, it means the top element is the current minimum.
        if top > 0:
            return top + self.min
        else:
            return self.min

    # Method to retrieve the minimum element in the stack.
    def getMin(self) -> int:
        # Return the current minimum value.
        return self.min

```
