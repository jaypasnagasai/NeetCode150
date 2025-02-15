# 17. Letter Combinations of a Phone Number
Difficulty: Medium

## QUESTION

You are given a string `digits` made up of digits from `2` through `9` inclusive.

Each digit (not including 1) is mapped to a set of characters as shown below:

A digit could represent any one of the characters it maps to.

Return all possible letter combinations that `digits` could represent. You may return the answer in any order

### EXAMPLE

```
Input: digits = "34"
Output: ["dg","dh","di","eg","eh","ei","fg","fh","fi"]
```

```
Input: digits = ""
Output: []
```

## SOLUTION


### APPROACH: BACKTRACKING

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        """
        Generates all possible letter combinations for a given phone number string.

        :param digits: A string of digits from 2-9.
        :return: A list of possible letter combinations.
        """
        res = []  # List to store the final letter combinations
        
        # Mapping of digits to their corresponding letters on a phone keypad
        digitToChar = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",  # Corrected mapping for '7'
            "8": "tuv",
            "9": "wxyz",
        }

        def backtrack(i, curStr):
            """
            Recursive backtracking function to generate letter combinations.

            :param i: Current index in the digits string.
            :param curStr: The current combination being built.
            """
            if len(curStr) == len(digits):  # Base case: when the combination length matches input length
                res.append(curStr)  # Add the completed combination to results
                return
            
            # Iterate over possible characters mapped to the current digit
            for c in digitToChar[digits[i]]:
                backtrack(i + 1, curStr + c)  # Recur for the next digit

        if digits:  # Ensure input is not empty
            backtrack(0, "")

        return res  # Return the list of generated letter combinations

```

### APPROACH: ITERATION

```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        """
        Generates all possible letter combinations for a given phone number string using an iterative approach.

        :param digits: A string of digits from 2-9.
        :return: A list of possible letter combinations.
        """
        if not digits:  # Return an empty list if input is empty
            return []

        res = [""]  # Initialize result list with an empty string
        
        # Mapping of digits to their corresponding letters on a phone keypad
        digitToChar = {
            "2": "abc",
            "3": "def",
            "4": "ghi",
            "5": "jkl",
            "6": "mno",
            "7": "pqrs",  # Corrected mapping for '7'
            "8": "tuv",
            "9": "wxyz",
        }

        # Iteratively build combinations
        for digit in digits:
            tmp = []  # Temporary list to store new combinations
            for curStr in res:  # Expand each existing combination
                for c in digitToChar[digit]:  # Append new characters
                    tmp.append(curStr + c)
            res = tmp  # Update the result list

        return res  # Return the list of generated letter combinations
```
