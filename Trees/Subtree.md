# 572. Subtree of Another Tree 
Difficulty: Easy

## QUESTION

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

### EXAMPLE

```
Input: root = [1,2,3,4,5], subRoot = [2,4,5]
Output: true
```

```
Input: root = [1,2,3,4,5,null,null,6], subRoot = [2,4,5]
Output: false
```

## SOLUTION


### APPROACH: DEPTH FIRST SEARCH

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        """
        Determines if subRoot is a subtree of root.
        A subtree must have the same structure and node values as part of the main tree.
        
        :param root: Root node of the main tree
        :param subRoot: Root node of the subtree
        :return: True if subRoot is a subtree of root, False otherwise
        """

        # If subRoot is None, it's always a valid subtree (empty tree is a subtree of any tree)
        if not subRoot:
            return True

        # If the main tree root is None but subRoot isn't, subRoot can't be a subtree
        if not root:
            return False

        # If root and subRoot are identical, return True
        if self.sameTree(root, subRoot):
            return True
        
        # Recursively check if subRoot is a subtree of either left or right subtree of root
        return (self.isSubtree(root.left, subRoot) or 
                self.isSubtree(root.right, subRoot))

    def sameTree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        """
        Checks if two trees are identical in structure and node values.

        :param root: Root node of the first tree
        :param subRoot: Root node of the second tree
        :return: True if both trees are identical, False otherwise
        """

        # If both trees are None, they are identical
        if not root and not subRoot:
            return True

        # If both nodes are not None and their values match, check left and right subtrees
        if root and subRoot and root.val == subRoot.val:
            return (self.sameTree(root.left, subRoot.left) and 
                    self.sameTree(root.right, subRoot.right))
        
        # If one node is None or values do not match, trees are not identical
        return False
```

### APPROACH: SERIALIZATION AND PATTERN MAKING

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution: 
    def serialize(self, root: Optional[TreeNode]) -> str:
        """
        Serializes a binary tree into a string representation using a preorder traversal.
        Each node is prefixed with '$' to handle unique node values, and null values are represented as '$#'.
        
        :param root: Root node of the binary tree
        :return: Serialized string representation of the tree
        """
        if root == None:
            return "$#"  # Represents a null node
        
        # Construct serialized string using preorder traversal (Root-Left-Right)
        return ("$" + str(root.val) + 
                self.serialize(root.left) + 
                self.serialize(root.right))  

    def z_function(self, s: str) -> list:
        """
        Computes the Z-function of a string, which finds occurrences of a substring efficiently.
        The Z-array is defined such that z[i] represents the length of the longest substring 
        starting from s[i] that matches a prefix of s.

        :param s: Input string
        :return: Z-array, where each index i contains the length of the substring that matches the prefix
        """
        z = [0] * len(s)  # Initialize Z-array with zeros
        l, r, n = 0, 0, len(s)  # Left and right bounds of the Z-box
        
        for i in range(1, n):
            if i <= r:
                z[i] = min(r - i + 1, z[i - l])  # Use previously computed Z-values

            # Extend Z-box as long as characters match
            while i + z[i] < n and s[z[i]] == s[i + z[i]]:
                z[i] += 1

            # Update the Z-box if necessary
            if i + z[i] - 1 > r:
                l, r = i, i + z[i] - 1

        return z

    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        """
        Determines if subRoot is a subtree of root using a string-matching approach with Z-function.
        The function serializes both trees into strings and checks if the serialized subRoot appears
        as a substring in the serialized root using the Z-function.

        :param root: Root node of the main tree
        :param subRoot: Root node of the subtree
        :return: True if subRoot is a subtree of root, False otherwise
        """
        # Serialize both trees into string representations
        serialized_root = self.serialize(root)
        serialized_subRoot = self.serialize(subRoot)

        # Combine serialized subtree and tree with a separator to avoid false matches
        combined = serialized_subRoot + "|" + serialized_root
        
        # Compute the Z-function for the combined string
        z_values = self.z_function(combined)
        sub_len = len(serialized_subRoot)  # Length of the serialized subtree string
        
        # Check for a match of subRoot in root using Z-values
        for i in range(sub_len + 1, len(combined)):  # Start checking after the separator
            if z_values[i] == sub_len:  # If a match of length sub_len is found
                return True
        
        return False  # No match found
```
