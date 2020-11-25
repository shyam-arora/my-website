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
