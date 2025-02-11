# 124. Binary Tree Maximum Path Sum
Difficulty: Hard

## QUESTION

Given the `root` of a non-empty binary tree, return the maximum path sum of any non-empty path.

A path in a binary tree is a sequence of nodes where each pair of adjacent nodes has an edge connecting them. A node can not appear in the sequence more than once. The path does not necessarily need to include the root.

The path sum of a path is the sum of the node's values in the path.

### EXAMPLE

```
Input: root = [1,2,3]
Output: 6
```

```
Input: root = [-15,10,20,null,null,15,5,-5]
Output: 40
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the node's value
#         self.left = left  # Pointer to the left child node
#         self.right = right  # Pointer to the right child node

from typing import Optional  # Import necessary type hint

class Solution:
    def maxPathSum(self, root: Optional[TreeNode]) -> int:
        """
        Finds the maximum path sum in a binary tree.
        
        :param root: The root node of the binary tree
        :return: The maximum path sum
        """

        # Initialize the result variable with the root node's value.
        # This ensures that even if the tree consists of only one node (or all negative values),
        # we at least consider the root node itself as the maximum path.
        res = [root.val]

        def dfs(root):
            """
            Helper function to calculate the maximum path sum for each subtree.
            
            :param root: The current node being processed
            :return: The maximum sum of a path that ends at this node and extends upwards
            """
            # Base case: If we reach a null node, return 0 (a non-existing path contributes nothing)
            if not root:
                return 0

            # Recursively compute the maximum path sum for the left and right subtrees
            leftMax = dfs(root.left)
            rightMax = dfs(root.right)

            # If the left or right subtree path sum is negative, we ignore it (take max with 0)
            leftMax = max(leftMax, 0)
            rightMax = max(rightMax, 0)

            # Calculate the maximum path sum that includes the current node as the root
            # This considers the case where the path goes through both left and right children
            res[0] = max(res[0], root.val + leftMax + rightMax)

            # Return the maximum sum of a path that includes the current node and extends upward
            # We can only return one side's maximum path (either left or right) plus the node value,
            # since a valid path cannot branch at the current node when returning to its parent.
            return root.val + max(leftMax, rightMax)

        # Start the DFS traversal from the root node
        dfs(root)

        # Return the maximum path sum found
        return res[0]
```
