---
id: easy-set-2
title: East Set 2
---

## Happy Number [(Leetcode)](https://leetcode.com/problems/happy-number/)

Write an algorithm to determine if a number n is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Return True if n is a happy number, and False if not

**Example 1:**

Input: 19

Output: true

Explanation:

1<sup>2</sup> + 9<sup>2</sup> = 82

8<sup>2</sup> + 2<sup>2</sup> = 68

6<sup>2</sup> + 8<sup>2</sup> = 100

1<sup>2</sup> + 0<sup>2</sup> + 0<sup>2</sup> = 1

```javascript
/**
 * @param {number} n
 * @return {boolean}
 */
var isHappy = function (n) {
  const getSum = (num) => {
    let sum = 0;
    let current = num;
    while (current > 0) {
      const remain = current % 10;
      sum = sum + remain * remain;
      current = Math.trunc(current / 10);
    }
    return sum;
  };

  let set = new Set();
  let result = n;
  set.add(result);
  while (true) {
    result = getSum(result);
    if (result === 1) return true;
    if (set.has(result)) return false;
    set.add(result);
  }
};
```

**Reference**

1. Happy Number (https://leetcode.com/problems/happy-number/)

## Minimum Moves to Equal Array Elements\* [(Leetcode)](https://leetcode.com/problems/minimum-moves-to-equal-array-elements/)

Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

**Example 1:**

```
Input:
[1,2,3]

Output:
3

Explanation:
Only three moves are needed (remember each move increments two elements):

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
```

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var minMoves = function (nums) {
  const min = Math.min(...nums);
  let count = 0;
  for (let i = 0; i < nums.length; i++) {
    count = count + nums[i] - min;
  }
  return count;
};
```

**Reference**

1. Minimum Moves to Equal Array Elements (https://leetcode.com/problems/minimum-moves-to-equal-array-elements/)

## First Unique Character in a String Elements [(Leetcode)](https://leetcode.com/problems/first-unique-character-in-a-string/)

Given a string, find the first non-repeating character in it and return its index. If it doesn't exist, return -1.

**Example 1:**

```
s = "leetcode"
return 0.

s = "loveleetcode"
return 2.
```

```javascript
/**
 * @param {string} s
 * @return {number}
 */
var firstUniqChar = function (s) {
  const occArray = [];
  const sLen = s.length;
  for (let i = 0; i < sLen; i++) {
    const idx = s.charCodeAt(i) - 48;
    occArray[idx] = (occArray[idx] || 0) + 1;
  }
  for (let i = 0; i < sLen; i++) {
    if (occArray[s.charCodeAt(i) - 48] === 1) {
      return i;
    }
  }
  return -1;
};
```

**Reference**

1. First Unique Character in a String (https://leetcode.com/problems/first-unique-character-in-a-string/)

## House Robber [(Leetcode)](https://leetcode.com/problems/house-robber/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```

**Example 2:**

```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

### Brute force

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function (nums) {
  let max = 0;
  if (!nums.length) return 0;
  if (nums.length <= 2) return Math.max(...nums);

  const helper = (sum, i) => {
    if (!nums[i]) return;
    const tSum = sum + nums[i];
    max = Math.max(max, tSum);
    helper(tSum, i + 2);
    helper(tSum, i + 3);
  };
  helper(0, 0);
  helper(0, 1);
  return max;
};
```

### DP

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function (nums) {
  if (!nums.length) return 0;
  if (nums.length <= 2) return Math.max(...nums);
  let dp = [nums[0], Math.max(nums[0], nums[1])];
  for (let i = 2; i < nums.length; i++) {
    dp[i] = Math.max(dp[i - 1], nums[i] + dp[i - 2]);
  }
  return dp[dp.length - 1];
};
```

### DP (optimized)

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var rob = function (nums) {
  let max = 0;
  if (!nums.length) return 0;
  if (nums.length <= 2) return Math.max(...nums);
  let prev = nums[0];
  let current = Math.max(prev, nums[1]);
  for (let i = 2; i < nums.length; i++) {
    const temp = Math.max(current, nums[i] + prev);
    prev = current;
    current = temp;
  }
  return current;
};
```

**Reference**

1. House Robber (https://leetcode.com/problems/house-robber/)

## Diameter of Binary Tree [(Leetcode)](https://leetcode.com/problems/diameter-of-binary-tree/solution/)

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

**Example 1:**
Given a binary tree

```
          1
         / \
        2   3
       / \
      4   5
```

Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var diameterOfBinaryTree = function (root) {
  let max = 0;
  const helper = (node) => {
    if (!node) return 0;
    const left = helper(node.left);
    const right = helper(node.right);
    max = Math.max(max, left + right);
    return 1 + Math.max(left, right);
  };
  helper(root);
  return max;
};
```

**Reference**

1. Diameter of Binary Tree (https://leetcode.com/problems/diameter-of-binary-tree/solution/)

## Missing Number [(Leetcode)](https://leetcode.com/problems/missing-number/)

Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

Follow up: Could you implement a solution using only O(1) extra space complexity and O(n) runtime complexity?

**Example 1:**

```
Input: nums = [3,0,1]
Output: 2
Explanation: n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.
```

**Example 2**

```
Input: nums = [9,6,4,2,3,5,7,0,1]
Output: 8
Explanation: n = 9 since there are 9 numbers, so all numbers are in the range [0,9]. 8 is the missing number in the range since it does not appear in nums.
```

**Example 3**

```
Input: nums = [0,1]
Output: 2
Explanation: n = 2 since there are 2 numbers, so all numbers are in the range [0,2]. 2 is the missing number in the range since it does not appear in nums.
```

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function (nums) {
  let sum = 0;
  const numsLen = nums.length;
  for (let i = 0; i < numsLen; i++) {
    sum = sum + nums[i];
  }
  const reqd = ((numsLen + 1) * numsLen) / 2;
  return reqd - sum;
};
```

**Reference**

1. Missing Number (https://leetcode.com/problems/missing-number/)

## Path Sum [(Leetcode)](https://leetcode.com/problems/path-sum/)

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

Note: A leaf is a node with no children.

**Example 1:**

Given the below binary tree and sum = 22,

```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {boolean}
 */
var hasPathSum = function (root, sum) {
  if (!root) return false;
  let pathExist = false;
  const helper = (node, cSum) => {
    if (pathExist || !node) return;
    if (!node.left && !node.right) {
      if (sum === cSum + node.val) {
        pathExist = true;
      }
      return;
    }
    helper(node.left, cSum + node.val);
    helper(node.right, cSum + node.val);
  };
  helper(root, 0);
  return pathExist;
};
```

**Reference**

1. Path Sum (https://leetcode.com/problems/path-sum/)

## Invert Binary Tree [(Leetcode)](https://leetcode.com/problems/invert-binary-tree/)

Invert a binary tree.

**Example 1:**

Input

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9

```

Output

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1

```

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var invertTree = function (root) {
  const helper = (node) => {
    if (!node) return;
    const temp = node.left;
    node.left = node.right;
    node.right = temp;
    helper(node.left);
    helper(node.right);
  };
  helper(root);
  return root;
};
```

**Reference**

1. Invert Binary Tree (https://leetcode.com/problems/invert-binary-tree/)

## Palindrome Number [(Leetcode)](https://leetcode.com/problems/palindrome-number/)

Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

Follow up: Could you solve it without converting the integer to a string?

**Example 1:**

```
Input: x = 121
Output: true
```

**Example 2:**
Input: x = -121

```
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```

**Example 3:**

```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
```

**Example 4:**

```
Input: x = -101
Output: false
```

```javascript
/**
 * @param {number} x
 * @return {boolean}
 */
var isPalindrome = function (x) {
  if (x < 0 || (x % 10 === 0 && x !== 0)) return false;
  if (x < 10) return true;
  let rev = 0;
  while (x > rev) {
    rev = rev * 10 + (x % 10);
    x = Math.trunc(x / 10);
  }
  return x === rev || x === Math.trunc(rev / 10);
};
```

**Reference**

1. Palindrome Number (https://leetcode.com/problems/palindrome-number/)

## Single Number [(Leetcode)](https://leetcode.com/problems/single-number/)

Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

Follow up: Could you implement a solution with a linear runtime complexity and without using extra memory?

**Example 1:**

```
Input: nums = [2,2,1]
Output: 1
```

**Example 2:**

```
Input: nums = [4,1,2,1,2]
Output: 4
```

**Example 3:**

```
Input: nums = [1]
Output: 1
```

```javascript
/**
 * @param {number[]} nums
 * @return {number}
 */
var singleNumber = function (nums) {
  let result = 0;
  for (let i = 0; i < nums.length; i++) {
    result = result ^ nums[i];
  }
  return result;
};
```

**Reference**

1. Single Number (https://leetcode.com/problems/single-number/)
