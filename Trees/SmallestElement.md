# 230. Kth Smallest Element In A BST
Difficulty: Medium

## QUESTION

Given the `root` of a binary search tree, and an integer `k`, return the `kth` smallest value (1-indexed) in the tree.

A binary search tree satisfies the following constraints:

- The left subtree of every node contains only nodes with keys less than the node's key.
- The right subtree of every node contains only nodes with keys greater than the node's key.
- Both the left and right subtrees are also binary search trees.

### EXAMPLE

```
Input: root = [2,1,3], k = 1
Output: 1
```

```
Input: root = [4,3,5,2,null], k = 4
Output: 5
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
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        """
        Finds the k-th smallest element in a Binary Search Tree (BST).
        This approach uses depth-first search (DFS) to traverse all nodes,
        stores them in an array, sorts the array, and retrieves the k-th element.

        :param root: Root node of the BST
        :param k: The k-th smallest element to find (1-based index)
        :return: The value of the k-th smallest element in the BST
        """

        arr = []  # List to store the values of all nodes

        def dfs(node):
            """
            Performs Depth-First Search (DFS) to collect all node values.

            :param node: Current node being processed
            """
            if not node:  # Base case: If the node is None, return
                return
            
            arr.append(node.val)  # Store the current node's value
            dfs(node.left)  # Recursively traverse the left subtree
            dfs(node.right)  # Recursively traverse the right subtree
        
        dfs(root)  # Start DFS traversal from the root
        
        arr.sort()  # Sort the collected values to get elements in ascending order
        
        return arr[k - 1]  # Return the k-th smallest element (1-based index)
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
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        """
        Finds the k-th smallest element in a Binary Search Tree (BST).
        This approach uses an iterative in-order traversal with a stack.

        :param root: Root node of the BST
        :param k: The k-th smallest element to find (1-based index)
        :return: The value of the k-th smallest element in the BST
        """

        stack = []  # Stack for iterative in-order traversal
        curr = root  # Start from the root node

        while stack or curr:  # Continue traversal while there are nodes to process
            while curr:  # Traverse the leftmost path and push nodes onto the stack
                stack.append(curr)
                curr = curr.left

            # Pop the leftmost node (smallest value at current step)
            curr = stack.pop()
            k -= 1  # Decrement k, since we've found one more smallest element

            if k == 0:  # If k reaches 0, return the current node's value
                return curr.val

            # Move to the right subtree
            curr = curr.right
```

### APPROACH: MORRIS TRANSVERSAL

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
        """
        Finds the k-th smallest element in a Binary Search Tree (BST) using Morris Traversal.
        Morris Traversal allows in-order traversal with O(1) extra space.

        :param root: Root node of the BST
        :param k: The k-th smallest element to find (1-based index)
        :return: The value of the k-th smallest element in the BST
        """

        curr = root  # Start traversal from the root
        
        while curr:
            if not curr.left:  # If there is no left subtree, process current node
                k -= 1  # Decrement k since we found the next smallest element
                if k == 0:
                    return curr.val  # If k becomes 0, return the current node's value
                curr = curr.right  # Move to the right subtree
            else:
                # Find the in-order predecessor (rightmost node in the left subtree)
                pred = curr.left
                while pred.right and pred.right != curr:
                    pred = pred.right
                
                if not pred.right:  # If the predecessor has no right child, create a link to curr
                    pred.right = curr  # Establish a temporary link (thread)
                    curr = curr.left  # Move to the left subtree
                else:
                    # Revert the changes (restore original tree structure)
                    pred.right = None
                    k -= 1  # Decrement k since we are visiting the current node
                    if k == 0:
                        return curr.val  # If k becomes 0, return the current node's value
                    curr = curr.right  # Move to the right subtree

        return -1  # Return -1 if k is out of range (should never happen in valid input)
```
