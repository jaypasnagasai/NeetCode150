# 235. Lowest Common Ancestor of a Binary Search Tree
Difficulty: Medium

## QUESTION

Given a binary search tree (BST) where all node values are unique, and two nodes from the tree `p` and `q`, return the lowest common ancestor (LCA) of the two nodes.

The lowest common ancestor between two nodes `p` and `q` is the lowest node in a tree `T` such that both `p` and `q` as descendants. The ancestor is allowed to be a descendant of itself.

### EXAMPLE

```
Input: root = [5,3,8,1,4,7,9,null,2], p = 3, q = 8
Output: 5
```

```
Input: root = [5,3,8,1,4,7,9,null,2], p = 3, q = 4
Output: 3
```

## SOLUTION


### APPROACH: RECURSION

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        """
        Finds the Lowest Common Ancestor (LCA) of two nodes (p and q) in a Binary Search Tree (BST).
        The LCA is the deepest node that is an ancestor of both p and q.

        :param root: Root node of the BST
        :param p: First node
        :param q: Second node
        :return: The lowest common ancestor node
        """

        # If the root is None or either p or q is missing, return None (invalid case)
        if not root or not p or not q:
            return None

        # If both p and q values are smaller than root, LCA must be in the left subtree
        if max(p.val, q.val) < root.val:
            return self.lowestCommonAncestor(root.left, p, q)

        # If both p and q values are greater than root, LCA must be in the right subtree
        elif min(p.val, q.val) > root.val:
            return self.lowestCommonAncestor(root.right, p, q)

        # If p and q are on different sides or one is the root itself, root is the LCA
        else:
            return root
```

### APPROACH: ITERATION

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val  # Assigns the value of the node
#         self.left = left  # Points to the left child node
#         self.right = right  # Points to the right child node

class Solution:
    def lowestCommonAncestor(self, root: TreeNode, p: TreeNode, q: TreeNode) -> TreeNode:
        """
        Finds the Lowest Common Ancestor (LCA) of two nodes (p and q) in a Binary Search Tree (BST).
        The LCA is the deepest node that is an ancestor of both p and q.

        :param root: Root node of the BST
        :param p: First node
        :param q: Second node
        :return: The lowest common ancestor node
        """

        cur = root  # Start traversal from the root node

        while cur:  # Continue traversing until a valid LCA is found
            # If both p and q are greater than the current node, move to the right subtree
            if p.val > cur.val and q.val > cur.val:
                cur = cur.right

            # If both p and q are smaller than the current node, move to the left subtree
            elif p.val < cur.val and q.val < cur.val:
                cur = cur.left

            # If p and q are on different sides, or one of them is equal to cur, return cur (LCA found)
            else:
                return cur  # The LCA of p and q
```
