# 000. LRU Cache
Difficulty: Medium

## QUESTION

Implement the Least Recently Used (LRU) cache class `LRUCache`. The class should support the following operations

- `LRUCache(int capacity)` Initialize the LRU cache of size `capacity`.
- `int get(int key)` Return the `value` corresponding to the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the `value` of the `key` if the `key` exists. Otherwise, add the `key`-`value` pair to the cache. If the introduction of the new pair causes the cache to exceed its capacity, remove the least recently used key.

A key is considered used if a `get` or a `put` operation is called on it.

Ensure that `get` and `put` each run in O(1) average time complexity.

### EXAMPLE

```
Input:
["LRUCache", [2], "put", [1, 10],  "get", [1], "put", [2, 20], "put", [3, 30], "get", [2], "get", [1]]

Output:
[null, null, 10, null, null, 20, -1]

Explanation:
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 10);  // cache: {1=10}
lRUCache.get(1);      // return 10
lRUCache.put(2, 20);  // cache: {1=10, 2=20}
lRUCache.put(3, 30);  // cache: {2=20, 3=30}, key=1 was evicted
lRUCache.get(2);      // returns 20 
lRUCache.get(1);      // return -1 (not found)
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class LRUCache:
    """
    Implements an LRU (Least Recently Used) Cache using a list.
    This approach is inefficient for large data sets due to O(n) lookup time.

    :param capacity: Maximum number of key-value pairs the cache can hold.
    """

    def __init__(self, capacity: int):
        """
        Initializes the LRU cache with a given capacity.

        :param capacity: Maximum number of key-value pairs the cache can hold.
        """
        self.cache = []  # List to store cache items as [key, value]
        self.capacity = capacity  # Maximum capacity of the cache

    def get(self, key: int) -> int:
        """
        Retrieves the value associated with a given key in the cache.
        If the key exists, it is moved to the most recently used position.
        
        :param key: The key to search for in the cache.
        :return: The associated value if found, otherwise -1.
        """
        for i in range(len(self.cache)):  # O(n) lookup
            if self.cache[i][0] == key:  # If key is found
                tmp = self.cache.pop(i)  # Remove it from its current position
                self.cache.append(tmp)  # Move it to the most recently used position
                return tmp[1]  # Return the stored value
        
        return -1  # Key not found in the cache

    def put(self, key: int, value: int) -> None:
        """
        Adds a key-value pair to the cache. If the key already exists, updates its value
        and moves it to the most recently used position.
        If the cache reaches its capacity, it removes the least recently used (LRU) item.

        :param key: The key to insert or update in the cache.
        :param value: The value associated with the key.
        """
        for i in range(len(self.cache)):  # O(n) lookup
            if self.cache[i][0] == key:  # If key already exists
                tmp = self.cache.pop(i)  # Remove old entry
                tmp[1] = value  # Update value
                self.cache.append(tmp)  # Move it to the most recently used position
                return

        if self.capacity == len(self.cache):  # If cache is full
            self.cache.pop(0)  # Remove the least recently used (LRU) item
        
        self.cache.append([key, value])  # Add new entry
```

### APPROACH: DOUBLY LINKED LIST

```python
class Node:
    def __init__(self, key, val):
        """
        Doubly linked list node representing a cache entry.

        :param key: The key of the cache entry.
        :param val: The value associated with the key.
        """
        self.key, self.val = key, val  # Store key-value pair
        self.prev = self.next = None  # Pointers to previous and next nodes

class LRUCache:
    def __init__(self, capacity: int):
        """
        Initializes the LRU Cache with a given capacity.

        Uses:
        - A **dictionary (`cache`)** for O(1) lookups.
        - A **doubly linked list (DLL)** for O(1) insertions & deletions.

        :param capacity: The maximum number of key-value pairs the cache can hold.
        """
        self.cap = capacity  # Store cache capacity
        self.cache = {}  # Dictionary to map keys to their corresponding DLL nodes

        # Dummy head (left) and dummy tail (right) for easier DLL operations
        self.left, self.right = Node(0, 0), Node(0, 0)
        self.left.next, self.right.prev = self.right, self.left  # Connect dummy nodes

    def remove(self, node):
        """
        Removes a node from the doubly linked list.

        :param node: The node to be removed.
        """
        prev, nxt = node.prev, node.next  # Get previous and next nodes
        prev.next, nxt.prev = nxt, prev  # Bypass the node to remove it from DLL

    def insert(self, node):
        """
        Inserts a node at the right (most recently used position) of the DLL.

        :param node: The node to insert.
        """
        prev, nxt = self.right.prev, self.right  # Get last node and dummy tail
        prev.next = nxt.prev = node  # Link new node with neighbors
        node.next, node.prev = nxt, prev  # Set node pointers

    def get(self, key: int) -> int:
        """
        Retrieves a value from the cache.

        - If key exists:
          - Moves the key to the most recently used (right) position.
          - Returns the value.
        - Otherwise, returns -1.

        :param key: The key to search for in the cache.
        :return: The associated value if found, otherwise -1.
        """
        if key in self.cache:  # Key exists in cache
            self.remove(self.cache[key])  # Remove node from current position
            self.insert(self.cache[key])  # Move node to most recently used position
            return self.cache[key].val  # Return value
        
        return -1  # Key not found

    def put(self, key: int, value: int) -> None:
        """
        Adds a key-value pair to the cache.

        - If key already exists:
          - Update its value and move it to the most recently used position.
        - If key is new:
          - Insert it at the most recently used position.
          - If capacity is exceeded, remove the least recently used (leftmost) node.

        :param key: The key to insert or update.
        :param value: The value associated with the key.
        """
        if key in self.cache:  # If key already exists, remove old node
            self.remove(self.cache[key])

        # Create a new node and store it in the dictionary
        self.cache[key] = Node(key, value)
        self.insert(self.cache[key])  # Move new node to most recently used position

        if len(self.cache) > self.cap:  # If cache exceeds capacity
            lru = self.left.next  # Least Recently Used (LRU) node (leftmost)
            self.remove(lru)  # Remove LRU node from DLL
            del self.cache[lru.key]  # Remove from dictionary
```

### APPROACH: BUILT-IN DATA STRUCTURE

```python
from collections import OrderedDict  # Import OrderedDict to maintain insertion order

class LRUCache:
    """
    Implements an LRU (Least Recently Used) Cache using OrderedDict.
    
    This approach provides **O(1) time complexity** for `get()` and `put()` operations.
    
    :param capacity: Maximum number of key-value pairs the cache can hold.
    """

    def __init__(self, capacity: int):
        """
        Initializes the LRU Cache with a given capacity.

        Uses:
        - **OrderedDict** to maintain insertion order and allow efficient reordering.
        
        :param capacity: The maximum number of key-value pairs the cache can hold.
        """
        self.cache = OrderedDict()  # OrderedDict maintains order of key insertion
        self.cap = capacity  # Store cache capacity

    def get(self, key: int) -> int:
        """
        Retrieves the value associated with a given key in the cache.
        - If key exists:
          - Moves the key to the most recently used (rightmost) position.
          - Returns the value.
        - Otherwise, returns -1.

        :param key: The key to search for in the cache.
        :return: The associated value if found, otherwise -1.
        """
        if key not in self.cache:  # Key not in cache
            return -1
        
        self.cache.move_to_end(key)  # Move accessed key to the end (most recently used)
        return self.cache[key]  # Return value of the key

    def put(self, key: int, value: int) -> None:
        """
        Adds a key-value pair to the cache.
        - If key already exists:
          - Update its value.
          - Move it to the most recently used position.
        - If cache reaches its capacity:
          - Remove the least recently used (leftmost) entry.

        :param key: The key to insert or update.
        :param value: The value associated with the key.
        """
        if key in self.cache:  # If key exists, move it to the end (most recently used)
            self.cache.move_to_end(key)
        
        self.cache[key] = value  # Insert or update the key-value pair

        if len(self.cache) > self.cap:  # If cache exceeds capacity
            self.cache.popitem(last=False)  # Remove the least recently used (first) item
```
