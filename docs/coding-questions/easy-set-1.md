---
id: easy-set-1
title: East Set 1
---

## Two Sum [(Leetcode)](https://leetcode.com/problems/two-sum/)

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Output: Because nums[0] + nums[1] == 9, we return [0, 1].
```

```javascript
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
  const numsLen = nums.length;
  const valueIndexMap = new Map();
  for (let i = 0; i < numsLen; i++) {
    const required = target - nums[i];
    if (valueIndexMap.has(required)) {
      return [i, valueIndexMap.get(required)];
    } else {
      valueIndexMap.set(nums[i], i);
    }
  }
};
```

**Reference**

1. Two Sum (https://leetcode.com/problems/two-sum/)
2. Map (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

## Reorder Data in Log Files [(Leetcode)](https://leetcode.com/problems/reorder-data-in-log-files/)

You have an array of logs. Each log is a space delimited string of words.

For each log, the first word in each log is an alphanumeric identifier. Then, either:

Each word after the identifier will consist only of lowercase letters, or;
Each word after the identifier will consist only of digits.
We will call these two varieties of logs letter-logs and digit-logs. It is guaranteed that each log has at least one word after its identifier.

Reorder the logs so that all of the letter-logs come before any digit-log. The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties. The digit-logs should be put in their original order.

Return the final order of the logs.

**Example 1:**

```
Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
```

```javascript
/**
 * @param {string[]} logs
 * @return {string[]}
 */
var reorderLogFiles = function (logs) {
  const digitArray = [];
  const letterArray = [];
  const body = (str) => str.slice(str.indexOf(" ") + 1);
  const logsLen = logs.length;
  for (let i = 0; i < logsLen; i++) {
    if (body(logs[i]).charCodeAt(0) < 97) {
      digitArray.push(logs[i]);
    } else {
      letterArray.push(logs[i]);
    }
  }
  letterArray.sort((a, b) => {
    const result = body(a).localeCompare(body(b));
    if (result) return result;
    return a.localeCompare(b);
  });
  return [...letterArray, ...digitArray];
};
```

**Reference**

1. Reorder Data in Log Files (https://leetcode.com/problems/reorder-data-in-log-files/)
2. LocaleCompare (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare)
3. Sort (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

## Maximum Subarray [(Leetcode)](https://leetcode.com/problems/maximum-subarray/)

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example 1:**

```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6
```

**Example 2:**

```
Input: nums = [-1]
Output: -1
```

### Brute force (Time limit exceed)

```javascript
/**
 * @param {string[]} logs
 * @return {string[]}
 */
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
  let numsLen = nums.length;
  let max = Math.min(...nums); // Can't assume 0 because result can be negative value
  for (let i = 0; i < numsLen; i++) {
    for (let j = 0; j < numsLen; j++) {
      let sum = nums[i];
      for (let k = i + 1; k <= j; k++) {
        sum = sum + nums[k];
      }
      max = Math.max(sum, max);
    }
  }
  return max;
};
```

### Greedy

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
  let numsLen = nums.length;
  let gMax = nums[0];
  let lMax = nums[0];
  for (let i = 1; i < numsLen; i++) {
    lMax = Math.max(nums[i], lMax + nums[i]);
    gMax = Math.max(gMax, lMax);
  }
  return gMax;
};
```

### Dynamic Programming (Kadane Algorithm)

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function (nums) {
  let numsLen = nums.length;
  let gMax = nums[0];
  for (let i = 1; i < numsLen; i++) {
    nums[i] = Math.max(nums[i], nums[i] + nums[i - 1]);
    gMax = Math.max(gMax, nums[i]);
  }
  return gMax;
};
```

**Reference**

1. Maximum Subarray (https://leetcode.com/problems/maximum-subarray/)

## Merge Two Sorted Lists [(Leetcode)](https://leetcode.com/problems/merge-two-sorted-lists/)

Merge two sorted linked lists and return it as a new sorted list. The new list should be made by splicing together the nodes of the first two lists.

**Example 1**

```
Input: l1 = [1,2,4], l2 = [1,3,4]
Output: [1,1,2,3,4,4]
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
var mergeTwoLists = function (l1, l2) {
  let l3Head = new ListNode();
  let l3c = l3Head;
  while (l1 && l2) {
    if (l1.val <= l2.val) {
      l3c.next = l1;
      l1 = l1.next;
    } else {
      l3c.next = l2;
      l2 = l2.next;
    }
    l3c = l3c.next;
  }
  l3c.next = l1 || l2;
  return l3Head.next;
};
```

**Reference**

1. Merge Two Sorted Lists (https://leetcode.com/problems/merge-two-sorted-lists/)
