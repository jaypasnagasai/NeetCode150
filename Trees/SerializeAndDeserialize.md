# 297. Serialize and Deserialize Binary Tree
Difficulty: Hard

## QUESTION

Implement an algorithm to serialize and deserialize a binary tree.

Serialization is the process of converting an in-memory structure into a sequence of bits so that it can be stored or sent across a network to be reconstructed later in another computer environment.

You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure. There is no additional restriction on how your serialization/deserialization algorithm should work.

Note: The input/output format in the examples is the same as how NeetCode serializes a binary tree. You do not necessarily need to follow this format.

### EXAMPLE

```
Input: root = [1,2,3,null,null,4,5]
Output: [1,2,3,null,null,4,5]
```

```
Input: root = []
Output: []
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

from typing import Optional  # Importing type hint for optional TreeNode

class Codec:
    
    # Encodes a tree to a single string.
    def serialize(self, root: Optional[TreeNode]) -> str:
        """
        Serializes a binary tree into a string using pre-order traversal.
        
        :param root: The root node of the binary tree
        :return: A string representation of the binary tree
        """
        res = []  # List to store the serialized tree data

        def dfs(node):
            """
            Helper function to perform depth-first traversal for serialization.
            Appends "N" for null nodes to maintain tree structure.
            
            :param node: Current node being processed
            """
            if not node:
                res.append("N")  # Mark null nodes with "N"
                return
            res.append(str(node.val))  # Append node value as string
            dfs(node.left)  # Recursively serialize the left subtree
            dfs(node.right)  # Recursively serialize the right subtree

        dfs(root)  # Start DFS traversal from the root
        return ",".join(res)  # Convert list to a comma-separated string
        
    # Decodes your encoded data to tree.
    def deserialize(self, data: str) -> Optional[TreeNode]:
        """
        Deserializes a string back into a binary tree.
        
        :param data: Serialized string of the binary tree
        :return: The reconstructed binary tree root node
        """
        vals = data.split(",")  # Split the serialized string into a list
        self.i = 0  # Pointer to track position in the list during reconstruction

        def dfs():
            """
            Helper function to reconstruct the tree using depth-first traversal.
            
            :return: The root node of the reconstructed subtree
            """
            if vals[self.i] == "N":  # If "N", return None (indicating a null node)
                self.i += 1  # Move to the next value
                return None
            
            # Create a new TreeNode with the current value
            node = TreeNode(int(vals[self.i]))
            self.i += 1  # Move to the next value
            
            # Recursively reconstruct the left and right subtrees
            node.left = dfs()
            node.right = dfs()
            
            return node  # Return the constructed node

        return dfs()  # Start DFS traversal to reconstruct the tree
```

### APPROACH: BREADTH FIRST SEARCH

```python
from collections import deque  # Import deque for efficient queue operations
from typing import Optional  # Import type hint for optional TreeNode

# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Stores the node's value
#         self.left = left  # Pointer to the left child node
#         self.right = right  # Pointer to the right child node

class Codec:
    
    # Encodes a tree to a single string.
    def serialize(self, root: Optional[TreeNode]) -> str:
        """
        Serializes a binary tree into a string using level-order traversal (BFS).
        
        :param root: The root node of the binary tree
        :return: A string representation of the binary tree
        """
        if not root:
            return "N"  # Return "N" to represent an empty tree
        
        res = []  # List to store serialized values
        queue = deque([root])  # Queue for BFS traversal
        
        while queue:
            node = queue.popleft()  # Process the front node in the queue
            
            if not node:
                res.append("N")  # Append "N" for null nodes
            else:
                res.append(str(node.val))  # Store the node's value
                queue.append(node.left)  # Add left child to queue (even if None)
                queue.append(node.right)  # Add right child to queue (even if None)

        return ",".join(res)  # Convert list to a comma-separated string
        
    # Decodes your encoded data to tree.
    def deserialize(self, data: str) -> Optional[TreeNode]:
        """
        Deserializes a string back into a binary tree using level-order traversal (BFS).
        
        :param data: Serialized string of the binary tree
        :return: The reconstructed binary tree root node
        """
        vals = data.split(",")  # Split the serialized string into a list of values
        
        if vals[0] == "N":
            return None  # Return None if the first value is "N" (empty tree)
        
        root = TreeNode(int(vals[0]))  # Create the root node from the first value
        queue = deque([root])  # Queue to reconstruct the tree level by level
        index = 1  # Pointer to track position in the list
        
        while queue:
            node = queue.popleft()  # Process the front node in the queue
            
            # Construct the left child
            if vals[index] != "N":  # If not "N", create a left child node
                node.left = TreeNode(int(vals[index]))
                queue.append(node.left)  # Add left child to queue for further processing
            index += 1  # Move to the next value
            
            # Construct the right child
            if vals[index] != "N":  # If not "N", create a right child node
                node.right = TreeNode(int(vals[index]))
                queue.append(node.right)  # Add right child to queue for further processing
            index += 1  # Move to the next value
        
        return root  # Return the reconstructed tree root
```
