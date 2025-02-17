# 355. Design Twitter
Difficulty: Medium

## QUESTION

Implement a simplified version of Twitter which allows users to post tweets, follow/unfollow each other, and view the `10` most recent tweets within their own news feed.

Users and tweets are uniquely identified by their IDs (integers).

Implement the following methods:

- `Twitter()` Initializes the twitter object.
- `void postTweet(int userId, int tweetId)` Publish a new tweet with ID `tweetId` by the user `userId`. You may assume that each `tweetId` is unique.
- `List<Integer> getNewsFeed(int userId)` Fetches at most the `10` most recent tweet IDs in the user's news feed. Each item must be posted by users who the user is following or by the user themself. Tweets IDs should be ordered from most recent to least recent.
- `void follow(int followerId, int followeeId)` The user with ID followerId follows the user with ID `followeeId`.
- `void unfollow(int followerId, int followeeId)` The user with ID followerId unfollows the user with ID `followeeId`.

### EXAMPLE

```
Input:
["Twitter", "postTweet", [1, 10], "postTweet", [2, 20], "getNewsFeed", [1], "getNewsFeed", [2], "follow", [1, 2], "getNewsFeed", [1], "getNewsFeed", [2], "unfollow", [1, 2], "getNewsFeed", [1]]

Output:
[null, null, null, [10], [20], null, [20, 10], [20], null, [10]]

Explanation:
Twitter twitter = new Twitter();
twitter.postTweet(1, 10); // User 1 posts a new tweet with id = 10.
twitter.postTweet(2, 20); // User 2 posts a new tweet with id = 20.
twitter.getNewsFeed(1);   // User 1's news feed should only contain their own tweets -> [10].
twitter.getNewsFeed(2);   // User 2's news feed should only contain their own tweets -> [20].
twitter.follow(1, 2);     // User 1 follows user 2.
twitter.getNewsFeed(1);   // User 1's news feed should contain both tweets from user 1 and user 2 -> [20, 10].
twitter.getNewsFeed(2);   // User 2's news feed should still only contain their own tweets -> [20].
twitter.unfollow(1, 2);   // User 1 follows user 2.
twitter.getNewsFeed(1);   // User 1's news feed should only contain their own tweets -> [10].
```

## SOLUTION


### APPROACH: SORTING

```python
from collections import defaultdict
from typing import List

class Twitter:

    def __init__(self):
        # Tracks the current timestamp
        self.time = 0
        # Maps a user to the set of users they follow
        self.followMap = defaultdict(set)
        # Maps a user to their list of tweets (timestamp, tweetId)
        self.tweetMap = defaultdict(list)

    def postTweet(self, userId: int, tweetId: int) -> None:
        # Append a new tweet with the current timestamp
        self.tweetMap[userId].append((self.time, tweetId))
        # Increment the timestamp
        self.time += 1

    def getNewsFeed(self, userId: int) -> List[int]:
        # Start with the user's own tweets
        feed = self.tweetMap[userId][:]
        # Add tweets from followed users
        for followeeId in self.followMap[userId]:
            feed.extend(self.tweetMap[followeeId])
        # Sort tweets by most recent timestamp
        feed.sort(key=lambda x: -x[0])
        # Return the 10 most recent tweetIds
        return [tweetId for _, tweetId in feed[:10]]

    def follow(self, followerId: int, followeeId: int) -> None:
        # A user cannot follow themselves
        if followerId != followeeId:
            self.followMap[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        # Remove followeeId if present
        self.followMap[followerId].discard(followeeId)
```

### APPROACH: HEAP

```python
import heapq
from collections import defaultdict
from typing import List

class Twitter:

    def __init__(self):
        # Global counter to track the order of tweets (larger count means older tweet)
        self.count = 0
        # Maps each user to a list of their recent tweets [count, tweetId]
        self.tweetMap = defaultdict(list)
        # Maps each user to the set of followees
        self.followMap = defaultdict(set)

    def postTweet(self, userId: int, tweetId: int) -> None:
        # Add a new tweet with the current global count
        self.tweetMap[userId].append([self.count, tweetId])
        # If more than 10 tweets, pop the oldest
        if len(self.tweetMap[userId]) > 10:
            self.tweetMap[userId].pop(0)
        # Decrement the global count to keep track of newer tweets
        self.count -= 1

    def getNewsFeed(self, userId: int) -> List[int]:
        # List to store the result of the 10 most recent tweets
        res = []
        # Min-heap for managing tweet order
        minHeap = []
        # Ensure the user is following themselves
        self.followMap[userId].add(userId)

        # If the user follows 10 or more users, use a max-heap first
        if len(self.followMap[userId]) >= 10:
            maxHeap = []
            # Collect tweets from followees
            for followeeId in self.followMap[userId]:
                if followeeId in self.tweetMap:
                    index = len(self.tweetMap[followeeId]) - 1
                    count, tweetId = self.tweetMap[followeeId][index]
                    heapq.heappush(maxHeap, [-count, tweetId, followeeId, index - 1])
                    if len(maxHeap) > 10:
                        heapq.heappop(maxHeap)
            # Transfer items from max-heap to min-heap
            while maxHeap:
                count, tweetId, followeeId, index = heapq.heappop(maxHeap)
                heapq.heappush(minHeap, [-count, tweetId, followeeId, index])
        else:
            # If fewer followees, directly push tweets into min-heap
            for followeeId in self.followMap[userId]:
                if followeeId in self.tweetMap:
                    index = len(self.tweetMap[followeeId]) - 1
                    count, tweetId = self.tweetMap[followeeId][index]
                    heapq.heappush(minHeap, [count, tweetId, followeeId, index - 1])

        # Extract up to 10 most recent tweets from the min-heap
        while minHeap and len(res) < 10:
            count, tweetId, followeeId, index = heapq.heappop(minHeap)
            res.append(tweetId)
            if index >= 0:
                count, tweetId = self.tweetMap[followeeId][index]
                heapq.heappush(minHeap, [count, tweetId, followeeId, index - 1])

        return res

    def follow(self, followerId: int, followeeId: int) -> None:
        # Add the followee to the follower's set
        self.followMap[followerId].add(followeeId)

    def unfollow(self, followerId: int, followeeId: int) -> None:
        # Remove the followee if present
        if followeeId in self.followMap[followerId]:
            self.followMap[followerId].remove(followeeId)
```

