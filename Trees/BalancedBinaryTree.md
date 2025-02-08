# 110. Balanced Binary Tree
Difficulty: Easy

## QUESTION

Given a binary tree, return `true` if it is height-balanced and `false` otherwise.

A height-balanced binary tree is defined as a binary tree in which the left and right subtrees of every node differ in height by no more than 1.

### EXAMPLE

```
Input: root = [1,2,3,null,null,4]
Output: true
```

```
Input: root = [1,2,3,null,null,4,null,5]
Output: false
```

```
Input: root = []
Output: true
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
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        """
        Determines if a binary tree is height-balanced.
        A binary tree is balanced if for every node, the height difference between
        the left and right subtrees is at most 1.
        
        :param root: Root node of the binary tree
        :return: True if the tree is balanced, False otherwise
        """
        
        # Base case: An empty tree is considered balanced
        if not root:
            return True
        
        # Calculate the height of the left and right subtrees
        left = self.height(root.left)
        right = self.height(root.right)

        # If the absolute difference between left and right subtree heights is greater than 1,
        # the tree is unbalanced
        if abs(left - right) > 1:
            return False

        # Recursively check if both the left and right subtrees are balanced
        return self.isBalanced(root.left) and self.isBalanced(root.right)

    def height(self, root: Optional[TreeNode]) -> int:
        """
        Computes the height of a given subtree.
        Height is defined as the number of edges on the longest path from the node to a leaf.
        
        :param root: Root node of the subtree
        :return: Height of the subtree
        """
        
        # Base case: An empty subtree has a height of 0
        if not root:
            return 0

        # Recursively calculate the height of the left and right subtrees,
        # and return the maximum height plus one (to include the current node)
        return 1 + max(self.height(root.left), self.height(root.right))
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
    def isBalanced(self, root):
        """
        Determines if a binary tree is height-balanced using an iterative approach.
        A binary tree is balanced if for every node, the height difference between
        the left and right subtrees is at most 1.
        
        :param root: Root node of the binary tree
        :return: True if the tree is balanced, False otherwise
        """

        stack = []  # Stack for iterative postorder traversal (left-right-root)
        node = root  # Current node being processed
        last = None  # Last visited node (used to track backtracking)
        depths = {}  # Dictionary to store subtree heights for each visited node

        while stack or node:
            if node:
                # Traverse left subtree first (push current node onto stack)
                stack.append(node)
                node = node.left
            else:
                # Peek at the node on top of the stack
                node = stack[-1]

                # If right subtree is not processed, process it first
                if not node.right or last == node.right:
                    stack.pop()  # Remove node from stack (postorder processing)

                    # Get the height of left and right subtrees, default to 0 if not present
                    left = depths.get(node.left, 0)
                    right = depths.get(node.right, 0)

                    # If height difference is greater than 1, the tree is unbalanced
                    if abs(left - right) > 1:
                        return False

                    # Store the height of the current node's subtree
                    depths[node] = 1 + max(left, right)

                    # Mark this node as the last processed node
                    last = node
                    node = None  # Prevent reprocessing of the same node
                else:
                    # Move to the right subtree
                    node = node.right

        # If all nodes satisfy the balance condition, return True
        return True
```
