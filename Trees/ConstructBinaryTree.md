# 105. Construct Binary Tree from Preorder and Inorder Traversal
Difficulty: Medium

## QUESTION

You are given two integer arrays `preorder` and `inorder`.

- `preorder` is the preorder traversal of a binary tree
- `inorder` is the inorder traversal of the same tree
- Both arrays are of the same size and consist of unique values.

Rebuild the binary tree from the preorder and inorder traversals and return its root.

### EXAMPLE

```
Input: preorder = [1,2,3,4], inorder = [2,1,3,4]
Output: [1,2,3,null,null,null,4]
```

```
Input: preorder = [1], inorder = [1]
Output: [1]
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

from typing import List, Optional  # Import necessary type hints

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        """
        Constructs a binary tree from preorder and inorder traversal lists.

        :param preorder: List of node values in preorder traversal (Root -> Left -> Right)
        :param inorder: List of node values in inorder traversal (Left -> Root -> Right)
        :return: The root of the constructed binary tree.
        """
        
        # Base case: If either traversal list is empty, return None (no tree/subtree to construct)
        if not preorder or not inorder:
            return None

        # The first element of preorder traversal is always the root of the tree/subtree
        root = TreeNode(preorder[0])

        # Find the index of the root value in the inorder traversal
        # This index splits the inorder list into left and right subtrees
        mid = inorder.index(preorder[0])

        # Recursively construct the left subtree using:
        # - Elements in preorder from index 1 to mid+1 (corresponding left subtree elements)
        # - Elements in inorder from the beginning up to 'mid' (left subtree nodes)
        root.left = self.buildTree(preorder[1 : mid + 1], inorder[:mid])

        # Recursively construct the right subtree using:
        # - Remaining elements in preorder from index mid+1 onward (right subtree elements)
        # - Elements in inorder from index mid+1 onward (right subtree nodes)
        root.right = self.buildTree(preorder[mid + 1 :], inorder[mid + 1 :])

        # Return the constructed root node of the tree/subtree
        return root
```

### APPROACH: HASH MAP + DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the node's value
#         self.left = left  # Pointer to the left child node
#         self.right = right  # Pointer to the right child node

from typing import List, Optional  # Import necessary type hints

class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
        """
        Constructs a binary tree from preorder and inorder traversal lists.

        :param preorder: List of node values in preorder traversal (Root -> Left -> Right)
        :param inorder: List of node values in inorder traversal (Left -> Root -> Right)
        :return: The root of the constructed binary tree.
        """
        
        # Create a hashmap to store the index of each value in the inorder list.
        # This allows quick lookup to find the position of root nodes in the inorder list.
        indices = {val: idx for idx, val in enumerate(inorder)}

        # Initialize a variable to keep track of the current index in the preorder list
        self.pre_idx = 0

        def dfs(l, r):
            """
            Recursive function to build the tree.

            :param l: Left boundary index in inorder list
            :param r: Right boundary index in inorder list
            :return: Constructed TreeNode or None if invalid range
            """
            # Base case: If left boundary exceeds right boundary, return None (no node to construct)
            if l > r:
                return None

            # Get the root value from the preorder list at the current index
            root_val = preorder[self.pre_idx]

            # Move to the next index in preorder list
            self.pre_idx += 1

            # Create the root node with the retrieved value
            root = TreeNode(root_val)

            # Find the index of this root value in the inorder list
            mid = indices[root_val]

            # Recursively construct the left subtree using the inorder indices [l, mid-1]
            root.left = dfs(l, mid - 1)

            # Recursively construct the right subtree using the inorder indices [mid+1, r]
            root.right = dfs(mid + 1, r)

            # Return the constructed subtree
            return root

        # Start the recursive tree construction with the full inorder range
        return dfs(0, len(inorder) - 1)
```
