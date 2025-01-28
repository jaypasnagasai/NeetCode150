# 271. Encode And Decode String
Difficulty: Medium

## QUESTION

Design an algorithm to encode a list of strings to a single string. The encoded string is then decoded back to the original list of strings.

Please implement `encode` and `decode`.

### EXAMPLE

```
Input: ["neet","code","love","you"]
Output:["neet","code","love","you"]
```

```
Input: ["we","say",":","yes"]
Output: ["we","say",":","yes"]
```

## SOLUTION


### APPROACH: ENCODING AND DECODING

```python
class Solution:
    # Method to encode a list of strings into a single string.
    def encode(self, strs: List[str]) -> str:
        # If the input list is empty, return an empty string.
        if not strs:
            return ""

        # Create a list to store the lengths of each string.
        sizes = []
        # Initialize the result string.
        res = ""

        # Iterate through each string in the input list.
        for s in strs:
            # Store the length of each string.
            sizes.append(len(s))
        
        # Append the sizes (comma-separated) to the result string, followed by a delimiter `#`.
        for sz in sizes:
            res += str(sz)
            res += ','
        res += '#'  # Add the delimiter to separate sizes from the actual strings.

        # Append each string from the input list to the result string.
        for s in strs:
            res += s

        # Return the encoded string.
        return res

    # Method to decode the encoded string back into a list of strings.
    def decode(self, s: str) -> List[str]:
        # If the input string is empty, return an empty list.
        if not s:
            return []

        # Initialize a list to store the sizes of the original strings.
        sizes = []
        # Initialize a list to store the decoded strings.
        res = []
        # Pointer to traverse the input string.
        i = 0

        # Extract sizes from the encoded string until the `#` delimiter.
        while s[i] != '#':
            cur = ""
            # Collect digits of each size until a comma is encountered.
            while s[i] != ',':
                cur += s[i]
                i += 1
            # Convert the collected size to an integer and add it to the sizes list.
            sizes.append(int(cur))
            i += 1  # Move past the comma.
        i += 1  # Move past the `#`.

        # Extract the strings from the encoded data using the sizes.
        for sz in sizes:
            # Slice the substring of the given size and append it to the result list.
            res.append(s[i:i + sz])
            i += sz  # Move the pointer past the current string.

        # Return the decoded list of strings.
        return res
```


### APPROACH: ENCODING AND DECODING [OPTIMAL]

```python
class Solution:
    # Method to encode a list of strings into a single string.
    def encode(self, strs: List[str]) -> str:
        # Initialize an empty string to store the encoded result.
        res = ""
        # Iterate through each string in the input list.
        for s in strs:
            # Append the length of the string, followed by a delimiter (`#`), and then the string itself.
            res += str(len(s)) + "#" + s
        # Return the encoded string.
        return res

    # Method to decode the encoded string back into a list of strings.
    def decode(self, s: str) -> List[str]:
        # Initialize a list to store the decoded strings.
        res = []
        # Pointer to traverse the encoded string.
        i = 0

        # Traverse the encoded string until all strings are decoded.
        while i < len(s):
            # Start at the current position to find the next delimiter (`#`).
            j = i
            while s[j] != '#':
                j += 1
            # Extract the length of the next string (from the start index to `j`).
            length = int(s[i:j])
            # Move the pointer to the start of the actual string (right after `#`).
            i = j + 1
            # Extract the substring of the given length.
            j = i + length
            res.append(s[i:j])
            # Move the pointer past the extracted string.
            i = j
        
        # Return the decoded list of strings.
        return res
```

