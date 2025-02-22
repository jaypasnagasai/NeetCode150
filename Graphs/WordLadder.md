# 127. Word Ladder
Difficulty: Hard

## QUESTION

You are given two words, `beginWord` and `endWord`, and also a list of words `wordList`. All of the given words are of the same length, consisting of lowercase English letters, and are all distinct.

Your goal is to transform `beginWord` into `endWord` by following the rules:

- You may transform `beginWord` to any word within `wordList`, provided that at exactly one position the words have a different character, and the rest of the positions have the same characters.
- You may repeat the previous step with the new word that you obtain, and you may do this as many times as needed.

Return the minimum number of words within the transformation sequence needed to obtain the `endWord`, or `0` if no such sequence exists.

### EXAMPLE

```
Input: beginWord = "cat", endWord = "sag", wordList = ["bat","bag","sag","dag","dot"]
Output: 4
```

```
Input: beginWord = "cat", endWord = "sag", wordList = ["bat","bag","sat","dag","dot"]
Output: 0
```

## SOLUTION


### APPROACH: BREADTH FIRST SEARCH

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        # If endWord is not in wordList or beginWord == endWord, no valid transformation
        if endWord not in wordList or beginWord == endWord:
            return 0

        # Length of each word (all words are of same length)
        m = len(wordList[0])

        # Convert wordList to a set for O(1) lookups
        wordSet = set(wordList)

        # Initialize two queues for bidirectional BFS
        qb, qe = deque([beginWord]), deque([endWord])

        # Dictionaries to track the number of steps from both directions
        fromBegin, fromEnd = {beginWord: 1}, {endWord: 1}

        # Perform bidirectional BFS
        while qb and qe:
            # Always expand the smaller queue for efficiency
            if len(qb) > len(qe):
                qb, qe = qe, qb
                fromBegin, fromEnd = fromEnd, fromBegin

            # Process current level
            for _ in range(len(qb)):
                word = qb.popleft()
                steps = fromBegin[word]

                # Try changing each character in the current word
                for i in range(m):
                    for c in range(97, 123):  # ASCII codes for 'a' to 'z'
                        if chr(c) == word[i]:
                            continue  # Skip if the character is the same

                        # Form new word by changing one character
                        nei = word[:i] + chr(c) + word[i + 1:]

                        # Skip if the new word is not in the word list
                        if nei not in wordSet:
                            continue

                        # If the new word is found from the opposite direction, return result
                        if nei in fromEnd:
                            return steps + fromEnd[nei]

                        # If the new word hasn't been visited yet, add it to the queue
                        if nei not in fromBegin:
                            fromBegin[nei] = steps + 1
                            qb.append(nei)

        # If no valid transformation sequence exists
        return 0
```
