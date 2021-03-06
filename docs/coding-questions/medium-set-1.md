---
id: medium-set-1
title: Medium Set 1
---

## LRU Cache [(Leetcode)](https://leetcode.com/problems/lru-cache/)

Design a data structure that follows the constraints of a Least Recently Used (LRU) cache.

Implement the LRUCache class:

LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
int get(int key) Return the value of the key if the key exists, otherwise return -1.
void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
Follow up:
Could you do get and put in O(1) time complexity?

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

```javascript
function DLLNode(val, key, next, prev) {
  this.val = val;
  this.key = key;
  this.next = next;
  this.prev = prev;
}

function DLL() {
  this.head = new DLLNode(null);
  this.tail = new DLLNode(null);
  this.head.next = this.tail;
  this.tail.prev = this.head;
}

DLL.prototype.insertFront = function (node) {
  node.prev = this.head;
  node.next = this.head.next;
  this.head.next.prev = node;
  this.head.next = node;
};

DLL.prototype.removeNode = function (node) {
  const prev = node.prev;
  const next = node.next;
  prev.next = next;
  next.prev = prev;
};

DLL.prototype.removeEnd = function () {
  const tail = this.tail.prev;
  this.removeNode(tail);
  return tail.key;
};

DLL.prototype.moveAhead = function (node) {
  this.removeNode(node);
  this.insertFront(node);
};

/**
 * @param {number} capacity
 */

var LRUCache = function (capacity) {
  this.capacity = capacity;
  this.map = new Map();
  this.list = new DLL();
};

/**
 * @param {number} key
 * @return {number}
 */
LRUCache.prototype.get = function (key) {
  if (!this.map.has(key)) return -1;
  const node = this.map.get(key);
  this.list.moveAhead(node);
  return node.val;
};

/**
 * @param {number} key
 * @param {number} value
 * @return {void}
 */
LRUCache.prototype.put = function (key, value) {
  if (this.map.has(key)) {
    const node = this.map.get(key);
    node.val = value;
    this.list.moveAhead(node);
  } else {
    const node = new DLLNode(value, key);
    this.map.set(key, node);
    this.list.insertFront(node);
    if (this.map.size > this.capacity) {
      const tailKey = this.list.removeEnd();
      this.map.delete(tailKey);
    }
  }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * var obj = new LRUCache(capacity)
 * var param_1 = obj.get(key)
 * obj.put(key,value)
 */
```

**Reference**

1. LRU Cache (https://leetcode.com/problems/lru-cache/)

## Number of Islands [(Leetcode)](https://leetcode.com/problems/number-of-islands/)

Given an m x n 2d grid map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

```javascript
/**
 * @param {character[][]} grid
 * @return {number}
 */
var numIslands = function (grid) {
  if (!grid.length) return 0;
  let count = 0;

  const mRow = grid.length;
  const mCol = grid[0].length;

  const helper = (row, col) => {
    if (
      row < 0 ||
      col < 0 ||
      row >= mRow ||
      col >= mCol ||
      grid[row][col] === "0"
    )
      return;
    grid[row][col] = "0";
    helper(row + 1, col);
    helper(row, col + 1);
    helper(row - 1, col);
    helper(row, col - 1);
  };

  for (let row = 0; row < mRow; row++) {
    for (let col = 0; col < mCol; col++) {
      if (grid[row][col] === "1") {
        count++;
        helper(row, col);
      }
    }
  }
  return count;
};
```

**Reference**

1. Number of Islands (https://leetcode.com/problems/number-of-islands/)

## Longest Palindromic Substring [(Leetcode)](https://leetcode.com/problems/longest-palindromic-substring/)

Given a string s, return the longest palindromic substring in s.

**Example 1:**

```
Input: s = "babad"
Output: "bab"
Note: "aba" is also a valid answer.
```

**Example 2**

```
Input: s = "cbbd"
Output: "bb"
```

**Example 3**

```
Input: s = "a"
Output: "a"
```

**Example 4**

```
Input: s = "ac"
Output: "a"
```

### Brute Force

```javascript
/**
 * @param {string} s
 * @return {string}
 */

const isPalindrome = (str) => {
  const strLen = str.length;
  const mid = Math.trunc(strLen / 2);
  for (let i = 0; i < mid; i++) {
    if (str[i] != str[strLen - i - 1]) return false;
  }
  return true;
};

var longestPalindrome = function (s) {
  let longest = s[0];
  for (let i = 1; i < s.length; i++) {
    for (let j = 0; j < i; j++) {
      if (i + 1 - j > longest.length) {
        const slice = s.slice(j, i + 1);
        if (isPalindrome(slice)) {
          longest = slice;
        }
      }
    }
  }
  return longest;
};
```

### DP

```javascript
/**
 * @param {string} s
 * @return {string}
 */

var longestPalindrome = function (s) {
  const sLen = s.length;
  const dp = [];

  let result = "";

  for (let i = 0; i < sLen; i++) {
    dp.push(new Array(sLen));
  }

  for (let i = 0; i < sLen; i++) {
    dp[i][i] = true;
    result = s[i];
  }

  for (let i = 0; i < sLen - 1; i++) {
    if (s[i] === s[i + 1]) {
      dp[i][i + 1] = true;
      result = s.slice(i, i + 2);
    } else {
      dp[i][i + 1] = false;
    }
  }

  for (let i = 2; i < sLen; i++) {
    for (let j = 0; j < sLen - i; j++) {
      const end = j + i;
      if (s[j] === s[end] && dp[j + 1][end - 1] === true) {
        dp[j][end] = true;
        result = s.slice(j, end + 1);
      } else {
        dp[j][end] = false;
      }
    }
  }
  return result;
};
```

**Reference**

1. Longest Palindromic Substring (https://leetcode.com/problems/longest-palindromic-substring/)

## Merge Intervals [(Leetcode)](https://leetcode.com/problems/merge-intervals/solution/)

Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Example 1:**

```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```

**Example 2**

```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function (intervals) {
  intervals.sort((a, b) => a[0] - b[0]);
  const result = [intervals[0]];
  for (let i = 1; i < intervals.length; i++) {
    const lastResultIdx = result.length - 1;
    if (result[lastResultIdx][1] >= intervals[i][0]) {
      result[lastResultIdx][1] = Math.max(
        intervals[i][1],
        result[lastResultIdx][1]
      );
    } else {
      result.push(intervals[i]);
    }
  }
  return result;
};
```

**Reference**

1. Merge Intervals (https://leetcode.com/problems/merge-intervals/solution/)

## Add Two Numbers [(Leetcode)](https://leetcode.com/problems/add-two-numbers/)

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807
```

**Example 2**

```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3**

```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var addTwoNumbers = function (l1, l2) {
  let l3 = { next: null };
  let current = l3;
  let carry = 0;
  while (l1 || l2) {
    const sum = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + carry;
    current.next = new ListNode(sum % 10);
    if (sum > 9) {
      carry = 1;
    } else {
      carry = 0;
    }
    if (l1) l1 = l1.next;
    if (l2) l2 = l2.next;
    current = current.next;
  }
  if (carry) {
    current.next = new ListNode(1);
  }
  return l3.next;
};
```

**Reference**

1. Add Two Numbers (https://leetcode.com/problems/add-two-numbers/)

## Longest Substring Without Repeating Characters [(Leetcode)](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string s, find the length of the longest substring without repeating characters.

**Example 1:**

```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2**

```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3**

```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

### Brute force

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function (s) {
  const sLen = s.length;
  let maxLen = 0;
  const isUnique = (i, j) => {
    const set = new Set();
    for (let k = i; k <= j; k++) {
      if (set.has(s[k])) {
        return false;
      } else {
        set.add(s[k]);
      }
    }
    return true;
  };

  for (let i = 0; i < sLen; i++) {
    for (let j = 0; j < sLen; j++) {
      if (isUnique(i, j)) {
        maxLen = Math.max(maxLen, j - i + 1);
      }
    }
  }
  return maxLen;
};
```

**Reference**

1. Longest Substring Without Repeating Characters (https://leetcode.com/problems/longest-substring-without-repeating-characters/)
