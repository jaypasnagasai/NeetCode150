# 543. Diameter of Binary Tree
Difficulty: Easy

## QUESTION

The diameter of a binary tree is defined as the length of the longest path between any two nodes within the tree. The path does not necessarily have to pass through the root.

The length of a path between two nodes in a binary tree is the number of edges between the nodes.

Given the root of a binary tree `root`, return the diameter of the tree.

### EXAMPLE

```
Input: root = [1,null,2,3,4,5]
Output: 3
```

```
Input: root = [1,2,3]
Output: 2
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the value of the node
#         self.left = left  # Pointer to the left child
#         self.right = right  # Pointer to the right child

from typing import Optional  # Import Optional for type hinting

class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        """
        Computes the diameter of a binary tree.
        
        The **diameter** of a binary tree is defined as the **longest path between any two nodes**, 
        which may or may not pass through the root.

        This implementation **recursively** calculates the diameter by:
        - Computing the height of the left and right subtrees.
        - Finding the longest path that goes through the root (`leftHeight + rightHeight`).
        - Recursively computing the diameter for the left and right subtrees.
        - Returning the **maximum of the above**.

        :param root: The root of the binary tree.
        :return: The diameter of the tree (longest path length).
        """

        if not root:
            return 0  # If the tree is empty, diameter is 0

        # Compute the height of left and right subtrees
        leftHeight = self.maxHeight(root.left)
        rightHeight = self.maxHeight(root.right)

        # Diameter passing through the root
        diameter = leftHeight + rightHeight  

        # Recursively compute the diameter for left and right subtrees
        sub = max(self.diameterOfBinaryTree(root.left),
                  self.diameterOfBinaryTree(root.right))

        return max(diameter, sub)  # Return the maximum diameter found

    def maxHeight(self, root: Optional[TreeNode]) -> int:
        """
        Computes the height of a binary tree.

        :param root: The root node of the subtree.
        :return: The height of the subtree rooted at `root`.
        """

        if not root:
            return 0  # Base case: Empty subtree has height 0

        return 1 + max(self.maxHeight(root.left), self.maxHeight(root.right))
```

### APPROACH: DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the value of the node
#         self.left = left  # Pointer to the left child
#         self.right = right  # Pointer to the right child

from typing import Optional  # Import Optional for type hinting

class Solution:
    def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
        """
        Computes the diameter of a binary tree using an **iterative depth-first search (DFS) approach**.
        
        The **diameter** of a binary tree is defined as the **longest path between any two nodes**, 
        which may or may not pass through the root.

        This implementation avoids recursion by **using a stack for DFS traversal** and
        **a dictionary (mp) to store computed heights and diameters**.

        :param root: The root of the binary tree.
        :return: The diameter of the tree (longest path length).
        """

        if not root:
            return 0  # Edge case: If the tree is empty, diameter is 0

        stack = [root]  # Initialize stack for DFS traversal
        mp = {None: (0, 0)}  # Dictionary to store (height, diameter) for each node

        while stack:
            node = stack[-1]  # Peek at the top of the stack

            # If left child exists and hasn't been processed, push it to stack
            if node.left and node.left not in mp:
                stack.append(node.left)
            # If right child exists and hasn't been processed, push it to stack
            elif node.right and node.right not in mp:
                stack.append(node.right)
            else:
                # Process node (Postorder: left -> right -> node)
                node = stack.pop()

                # Retrieve stored values (height, diameter) for left and right subtrees
                leftHeight, leftDiameter = mp[node.left]
                rightHeight, rightDiameter = mp[node.right]

                # Compute current node height and diameter
                mp[node] = (1 + max(leftHeight, rightHeight),  # Height of current node
                            max(leftHeight + rightHeight, leftDiameter, rightDiameter))  # Diameter at this node

        return mp[root][1]  # Return the diameter of the entire tree
```
