# 98. Validate Binary Search Tree
Difficulty: Medium

## QUESTION

Given the `root` of a binary tree, return `true` if it is a valid binary search tree, otherwise return `false`.

A valid binary search tree satisfies the following constraints:

- The left subtree of every node contains only nodes with keys less than the node's key.
- The right subtree of every node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees are also binary search trees.

### EXAMPLE

```
Input: root = [2,1,3]
Output: true
```

```
Input: root = [1,2,3]
Output: false
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    # Static lambda functions to check BST constraints
    left_check = staticmethod(lambda val, limit: val < limit)  # Ensure left subtree values are less than the root
    right_check = staticmethod(lambda val, limit: val > limit)  # Ensure right subtree values are greater than the root

    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        """
        Determines if a given binary tree is a valid Binary Search Tree (BST).
        A valid BST satisfies the property:
        - The left subtree of a node contains only nodes with values less than the node’s value.
        - The right subtree of a node contains only nodes with values greater than the node’s value.
        - Both left and right subtrees must also be BSTs.

        :param root: Root node of the binary tree
        :return: True if the tree is a valid BST, False otherwise
        """

        if not root:  # An empty tree is a valid BST
            return True

        # Check if the left subtree follows the BST property with the current root as the limit
        if (not self.isValid(root.left, root.val, self.left_check) or
            not self.isValid(root.right, root.val, self.right_check)):
            return False  # If any subtree violates BST property, return False

        # Recursively check the entire tree
        return self.isValidBST(root.left) and self.isValidBST(root.right)

    def isValid(self, root: Optional[TreeNode], limit: int, check) -> bool:
        """
        Helper function to check if a subtree satisfies the BST constraint.

        :param root: Root node of the subtree
        :param limit: The value limit based on the parent node
        :param check: A function (lambda) to check if node values are within the limit
        :return: True if the subtree is valid, False otherwise
        """

        if not root:  # Base case: an empty subtree is always valid
            return True

        if not check(root.val, limit):  # If the node value does not satisfy the BST condition, return False
            return False

        # Recursively check left and right subtrees with the same limit condition
        return (self.isValid(root.left, limit, check) and
                self.isValid(root.right, limit, check))
```

### APPROACH: DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        """
        Determines if a given binary tree is a valid Binary Search Tree (BST).
        A valid BST satisfies the following properties:
        - The left subtree of a node contains only nodes with values less than the node’s value.
        - The right subtree of a node contains only nodes with values greater than the node’s value.
        - Both the left and right subtrees must also be BSTs.

        :param root: Root node of the binary tree
        :return: True if the tree is a valid BST, False otherwise
        """

        def valid(node, left, right):
            """
            Helper function to check if a subtree satisfies the BST property.

            :param node: Current node being checked
            :param left: Lower bound for the node's value
            :param right: Upper bound for the node's value
            :return: True if the subtree is a valid BST, False otherwise
            """

            if not node:  # Base case: An empty node (None) is always valid
                return True

            if not (left < node.val < right):  # Check if the node's value is within the allowed range
                return False

            # Recursively validate the left and right subtrees
            return valid(node.left, left, node.val) and valid(node.right, node.val, right)

        # Start the validation with the entire range (-∞, ∞)
        return valid(root, float("-inf"), float("inf"))
```

### APPROACH: BREADTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

from collections import deque  # Import deque for efficient queue-based BFS

class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        """
        Determines if a given binary tree is a valid Binary Search Tree (BST).
        A valid BST satisfies the following properties:
        - The left subtree of a node contains only nodes with values less than the node’s value.
        - The right subtree of a node contains only nodes with values greater than the node’s value.
        - Both left and right subtrees must also be BSTs.

        :param root: Root node of the binary tree
        :return: True if the tree is a valid BST, False otherwise
        """

        if not root:  # An empty tree is considered a valid BST
            return True

        # Initialize a queue for iterative BFS traversal with (node, min_limit, max_limit)
        q = deque([(root, float("-inf"), float("inf"))])

        while q:
            node, left, right = q.popleft()  # Dequeue a node along with its valid value range

            # If the node's value is outside the valid range, it's not a BST
            if not (left < node.val < right):
                return False

            # Enqueue the left child with an updated upper limit (root's value)
            if node.left:
                q.append((node.left, left, node.val))

            # Enqueue the right child with an updated lower limit (root's value)
            if node.right:
                q.append((node.right, node.val, right))

        return True  # If all nodes satisfy the BST property, return True
```
