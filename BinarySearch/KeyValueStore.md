# 000. Time Based Key-Value Store
Difficulty: Medium

## QUESTION

Implement a time-based key-value data structure that supports:

- Storing multiple values for the same key at specified time stamps
- Retrieving the key's value at a specified timestamp

Implement the `TimeMap` class:

- `TimeMap()` Initializes the object.
- `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
- `String get(String key, int timestamp)` Returns the most recent value of `key` if `set` was previously called on it and the most recent timestamp for that key `prev_timestamp` is less than or equal to the given timestamp `(prev_timestamp <= timestamp)`. If there are no values, it returns `""`.

Note: For all calls to `set`, the timestamps are in strictly increasing order.

### EXAMPLE

```
Input:
["TimeMap", "set", ["alice", "happy", 1], "get", ["alice", 1], "get", ["alice", 2], "set", ["alice", "sad", 3], "get", ["alice", 3]]

Output:
[null, null, "happy", "happy", null, "sad"]

Explanation:
TimeMap timeMap = new TimeMap();
timeMap.set("alice", "happy", 1);  // store the key "alice" and value "happy" along with timestamp = 1.
timeMap.get("alice", 1);           // return "happy"
timeMap.get("alice", 2);           // return "happy", there is no value stored for timestamp 2, thus we return the value at timestamp 1.
timeMap.set("alice", "sad", 3);    // store the key "alice" and value "sad" along with timestamp = 3.
timeMap.get("alice", 3);           // return "sad"
```

## SOLUTION


### APPROACH: BRUTE FORCE

```python
class TimeMap:
    def __init__(self):
        # Dictionary to store key-value pairs with timestamps
        self.keyStore = {}

    def set(self, key: str, value: str, timestamp: int) -> None:
        # If the key is not present, initialize an empty dictionary for timestamps
        if key not in self.keyStore:
            self.keyStore[key] = {}

        # If the timestamp is not present, initialize an empty list for values
        if timestamp not in self.keyStore[key]:
            self.keyStore[key][timestamp] = []

        # Append the value to the list for the given timestamp
        self.keyStore[key][timestamp].append(value)

    def get(self, key: str, timestamp: int) -> str:
        # If the key does not exist, return an empty string
        if key not in self.keyStore:
            return ""

        seen = 0  # Variable to track the latest valid timestamp

        # Iterate through stored timestamps for the key
        for time in self.keyStore[key]:
            # Check if the timestamp is less than or equal to the requested timestamp
            if time <= timestamp:
                seen = max(seen, time)  # Keep track of the latest valid timestamp

        # Return the last stored value for the closest timestamp or an empty string if no valid timestamp exists
        return "" if seen == 0 else self.keyStore[key][seen][-1]
```

### APPROACH: BINARY SEARCH (SORTED MAP)

```python
from sortedcontainers import SortedDict
from collections import defaultdict

class TimeMap:
    def __init__(self):
        # Dictionary where each key maps to a SortedDict for efficient timestamp retrieval
        self.m = defaultdict(SortedDict)

    def set(self, key: str, value: str, timestamp: int) -> None:
        # Directly store the value at the given timestamp
        self.m[key][timestamp] = value

    def get(self, key: str, timestamp: int) -> str:
        # If the key doesn't exist, return an empty string
        if key not in self.m:
            return ""

        timestamps = self.m[key]  # Retrieve the SortedDict for the given key
        idx = timestamps.bisect_right(timestamp) - 1  # Find the closest timestamp ≤ target

        if idx >= 0:
            closest_time = timestamps.iloc[idx]  # Retrieve the closest timestamp
            return timestamps[closest_time]  # Return the corresponding value
        
        return ""  # Return empty string if no valid timestamp exists
```

### APPROACH: BINARY SEARCH (ARRAY)

```python
class TimeMap:
    def __init__(self):
        # Dictionary to store key-value pairs where each key maps to a list of [value, timestamp]
        self.keyStore = {}  # key: list of [value, timestamp]

    def set(self, key: str, value: str, timestamp: int) -> None:
        # If the key is not present, initialize an empty list
        if key not in self.keyStore:
            self.keyStore[key] = []
        # Append the (value, timestamp) pair to the list
        self.keyStore[key].append([value, timestamp])

    def get(self, key: str, timestamp: int) -> str:
        # Get the list of (value, timestamp) pairs for the key, or an empty list if key is not found
        res, values = "", self.keyStore.get(key, [])
        
        # Perform binary search to find the closest timestamp ≤ given timestamp
        l, r = 0, len(values) - 1
        while l <= r:
            m = (l + r) // 2
            if values[m][1] <= timestamp:  # Check if the timestamp is within range
                res = values[m][0]  # Update result with the most recent valid value
                l = m + 1  # Continue searching in the right half
            else:
                r = m - 1  # Search in the left half if the timestamp is too large
        
        return res  # Return the closest valid value or an empty string if none found
```
