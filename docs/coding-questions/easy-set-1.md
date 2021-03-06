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

## Valid Parentheses [(Leetcode)](https://leetcode.com/problems/valid-parentheses/)

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

**Example 1**

```
Input: s = "()[]{}"
Output: true
```

**Example 2**

```
Input: s = "([)]"
Output: false
```

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function (s) {
  const map = new Map([
    [")", "("],
    ["]", "["],
    ["}", "{"],
  ]);
  const sLen = s.length;
  const stack = [];
  for (let i = 0; i < sLen; i++) {
    if (map.has(s[i])) {
      const temp = map.get(s[i]);
      if (stack[stack.length - 1] === temp) {
        stack.pop();
      } else {
        return false;
      }
    } else {
      stack.push(s[i]);
    }
  }
  return !stack.length;
};
```

**Reference**

1. Valid Parentheses (https://leetcode.com/problems/valid-parentheses/)

## Best Time to Buy and Sell Stock [(Leetcode)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

**Example 1**

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

```javascript
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function (prices) {
  const pricesLen = prices.length;
  let maxProfit = 0;
  if (pricesLen === 0 || pricesLen === 1) {
    return maxProfit;
  }
  let minBuy = prices[0];
  for (let i = 0; i < pricesLen; i++) {
    maxProfit = Math.max(maxProfit, prices[i] - minBuy);
    minBuy = Math.min(minBuy, prices[i]);
  }
  return maxProfit;
};
```

**Reference**

1. Best Time to Buy and Sell Stock (https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

## Verifying an Alien Dictionary [(Leetcode)](https://leetcode.com/problems/verifying-an-alien-dictionary/)

In an alien language, surprisingly they also use english lowercase letters, but possibly in a different order. The order of the alphabet is some permutation of lowercase letters.

Given a sequence of words written in the alien language, and the order of the alphabet, return true if and only if the given words are sorted lexicographicaly in this alien language.

**Example 1**

```
Input: words = ["word","world","row"], order = "worldabcefghijkmnpqstuvxyz"
Output: false
Explanation: As 'd' comes after 'l' in this language, then words[0] > words[1], hence the sequence is unsorted.
```

### With sort

```javascript
/**
 * @param {string[]} words
 * @param {string} order
 * @return {boolean}
 */
var isAlienSorted = function (words, order) {
  const map = new Map();
  const copy = [...words];
  for (let i = 0; i < order.length; i++) {
    map.set(order[i], i);
  }
  const comparator = (a, b) => {
    const maxLen = Math.max(a.length, b.length);
    for (let i = 0; i < maxLen; i++) {
      if (!a[i] && b[i]) return -1;
      if (!b[i] && a[i]) return 1;
      if (map.get(a[i]) < map.get(b[i])) return -1;
      if (map.get(a[i]) > map.get(b[i])) return 1;
    }
    return 0;
  };
  copy.sort(comparator);
  return words.every((item, i) => item === copy[i]);
};
```

### Without sort

```javascript
/**
 * @param {string[]} words
 * @param {string} order
 * @return {boolean}
 */
var isAlienSorted = function (words, order) {
  const map = new Map();

  for (let i = 0; i < order.length; i++) {
    map.set(order[i], i);
  }
  const comparator = (a, b) => {
    const maxLen = Math.max(a.length, b.length);
    for (let i = 0; i < maxLen; i++) {
      if (!a[i] && b[i]) return -1;
      if (!b[i] && a[i]) return 1;
      if (map.get(a[i]) < map.get(b[i])) return -1;
      if (map.get(a[i]) > map.get(b[i])) return 1;
    }
    return 0;
  };

  for (let i = 1; i < words.length; i++) {
    const result = comparator(words[i - 1], words[i]);
    if (result === 1) {
      return false;
    }
  }

  return true;
};
```

**Reference**

1. Verifying an Alien Dictionary (https://leetcode.com/problems/verifying-an-alien-dictionary/)

## Reverse Linked List [(Leetcode)](https://leetcode.com/problems/reverse-linked-list/)

Reverse a singly linked list.

**Example 1**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

### Iterative

```javascript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  let current = head;
  let prev = null;
  while (current) {
    const next = current.next;
    current.next = prev;
    prev = current;
    current = next;
  }
  return prev;
};
```

**Reference**

1. Reverse Linked List (https://leetcode.com/problems/reverse-linked-list/)

## Add Strings [(Leetcode)](https://leetcode.com/problems/add-strings/)

Given two non-negative integers num1 and num2 represented as string, return the sum of num1 and num2.

Note:

The length of both num1 and num2 is < 5100.
Both num1 and num2 contains only digits 0-9.
Both num1 and num2 does not contain any leading zero.
You must not use any built-in BigInteger library or convert the inputs to integer directly.

```javascript
/**
 * @param {string} num1
 * @param {string} num2
 * @return {string}
 */
var addStrings = function (num1, num2) {
  if (num1.length > num2.length) {
    const diff = num1.length - num2.length;
    for (let i = 0; i < diff; i++) {
      num2 = "0" + num2;
    }
  } else if (num1.length < num2.length) {
    const diff = num2.length - num1.length;
    for (let i = 0; i < diff; i++) {
      num1 = "0" + num1;
    }
  }
  let ptr = num1.length;
  let result = "";
  let carry = 0;
  while (ptr) {
    let i = ptr - 1;
    const sum = num1.charCodeAt(i) - 48 + num2.charCodeAt(i) - 48 + carry;
    result = `${sum % 10}${result}`;
    if (sum > 9) {
      carry = 1;
    } else {
      carry = 0;
    }
    ptr--;
  }
  if (carry) {
    result = `1${result}`;
  }
  return result;
};
```

**Reference**

1. Add Strings (https://leetcode.com/problems/add-strings/)

## Reverse Integer [(Leetcode)](https://leetcode.com/problems/reverse-integer/)

Given a 32-bit signed integer, reverse digits of an integer.

Note:
Assume we are dealing with an environment that could only store integers within the 32-bit signed integer range: [−2<sup>31</sup>, 2<sup>31</sup> − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

**Example: 1**

```
Input: x = 123
Output: 321
```

```javascript
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function (x) {
  const CONSTRAINT = Math.pow(2, 31);

  let num = x;
  let mux = 1;
  num = Math.trunc(num / 10);
  while (num) {
    mux = mux * 10;
    num = Math.trunc(num / 10);
  }
  num = x;
  let result = 0;
  while (num) {
    result = result + mux * (num % 10);
    mux = mux / 10;
    num = Math.trunc(num / 10);
  }
  if (result < CONSTRAINT - 1 && result > -1 * CONSTRAINT) {
    return result;
  } else {
    return 0;
  }
};
```

**Reference**

1. Reverse Integer (https://leetcode.com/problems/reverse-integer/)
2. Math.trunc (https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc)

## Merge Sorted Array [(Leetcode)](https://leetcode.com/problems/merge-sorted-array/)

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

Note:

The number of elements initialized in nums1 and nums2 are m and n respectively.
You may assume that nums1 has enough space (size that is equal to m + n) to hold additional elements from nums2.

**Example: 1**

```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

```javascript
/**
 * @param {number[]} nums1
 * @param {number} m
 * @param {number[]} nums2
 * @param {number} n
 * @return {void} Do not return anything, modify nums1 in-place instead.
 */
var merge = function (nums1, m, nums2, n) {
  let ptr1 = m - 1;
  let ptr2 = n - 1;
  let endPtr = m + n - 1;
  while (ptr2 !== -1) {
    if (nums1[ptr1] > nums2[ptr2]) {
      nums1[endPtr] = nums1[ptr1];
      ptr1--;
    } else {
      nums1[endPtr] = nums2[ptr2];
      ptr2--;
    }
    endPtr--;
  }
};
```

**Reference**

1. Merge Sorted Array (https://leetcode.com/problems/merge-sorted-array/)

## Palindrome Linked List [(Leetcode)](https://leetcode.com/problems/palindrome-linked-list/)

Given a singly linked list, determine if it is a palindrome.

**Example: 1**

```
Input: 1->2->2->1
Output: true
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
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function (head) {
  let slow = head;
  let fast = head;
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }
  let current = slow;
  let prev = null;
  while (current) {
    const next = current.next;
    current.next = prev;
    prev = current;
    current = next;
  }
  let h1 = head;
  let h2 = prev;
  while (h1 && h2) {
    if (h1.val === h2.val) {
      h1 = h1.next;
      h2 = h2.next;
    } else {
      return false;
    }
  }
  return true;
};
```

**Reference**

1. Palindrome Linked List (https://leetcode.com/problems/palindrome-linked-list/)

## Min Stack [(Leetcode)](https://leetcode.com/problems/min-stack/)

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.

**Example: 1**

```
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function () {
  this.minStack = [Number.MAX_SAFE_INTEGER];
  this.stack = [];
};

/**
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function (x) {
  this.stack.push(x);
  if (x <= this.minStack[this.minStack.length - 1]) {
    this.minStack.push(x);
  }
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
  const popedValue = this.stack.pop();
  if (this.minStack[this.minStack.length - 1] === popedValue) {
    this.minStack.pop();
  }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
  return this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function () {
  return this.minStack[this.minStack.length - 1];
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```

**Reference**

1.  Min Stack (https://leetcode.com/problems/min-stack/)

## Second Highest Salary [(Leetcode)](https://leetcode.com/problems/second-highest-salary/)

Write a SQL query to get the second highest salary from the Employee table.

```
+----+--------+
| Id | Salary |
+----+--------+
| 1 | 100 |
| 2 | 200 |
| 3 | 300 |
+----+--------+
```

For example, given the above Employee table, the query should return 200 as the second highest salary. If there is no second highest salary, then the query should return null.

```
+---------------------+
| SecondHighestSalary |
+---------------------+
| 200 |
+---------------------+
```

```sql
SELECT (SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET 1) AS SecondHighestSalary
```

**Reference**

1.  Second Highest Salary (https://leetcode.com/problems/second-highest-salary/)

## Move Zeroes [(Leetcode)](https://leetcode.com/problems/move-zeroes/)

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
Note:
```

You must do this in-place without making a copy of the array.
Minimize the total number of operations.

```javascript
/**
 * @param {number[]} nums
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var moveZeroes = function (nums) {
  let thres = nums.indexOf(0);
  for (let i = 0; i < nums.length; i++) {
    const item = nums[i];
    if (item != 0) {
      const nextZeroIndex = nums.indexOf(0, thres);
      if (nextZeroIndex == -1) return;
      if (nextZeroIndex <= i) {
        nums[nextZeroIndex] = item;
        nums[i] = 0;
        thres = nextZeroIndex + 1;
      }
    }
  }
};
```

**Reference**

1.  Move Zeroes (https://leetcode.com/problems/move-zeroes/)

## Valid Palindrome II [(Leetcode)](https://leetcode.com/problems/valid-palindrome-ii/)

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

**Example:**

Given a non-empty string s, you may delete at most one character. Judge whether you can make it a palindrome.

Example 1:

```
Input: "aba"
Output: True
```

Example 2:

```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

Note:
The string will only contain lowercase characters a-z. The maximum length of the string is 50000.

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
var validPalindrome = function (s) {
  const isPalindrome = (str) => {
    const strLen = str.length;
    const mid = Math.trunc(strLen / 2);
    for (let i = 0; i < mid; i++) {
      if (str.charAt(i) != str.charAt(strLen - i - 1)) {
        return i;
      }
    }
    return true;
  };

  const try1 = isPalindrome(s);
  if (try1 === true) return true;

  const try2 = isPalindrome(s.slice(try1 + 1, s.length - try1));
  if (try2 === true) return true;

  const try3 = isPalindrome(s.slice(try1, s.length - try1 - 1));
  if (try3 === true) return true;

  return false;
};
```

**Reference**

1.  Valid Palindrome II (https://leetcode.com/problems/valid-palindrome-ii/)

## Add Binary [(Leetcode)](https://leetcode.com/problems/add-binary/)

**Example 1**:

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2**:

```
Input: a = "1010", b = "1011"
Output: "10101"
```

```javascript
/**
 * @param {string} a
 * @param {string} b
 * @return {string}
 */
var addBinary = function (a, b) {
  let result = "";
  let maxLen = Math.max(a.length, b.length);
  if (a.length < maxLen) {
    a = a.padStart(maxLen, "0");
  } else if (b.length < maxLen) {
    b = b.padStart(maxLen, "0");
  }
  let endPtr = maxLen - 1;
  let carry = 0;
  while (endPtr != -1) {
    const v1 = a.charAt(endPtr) ? a.charCodeAt(endPtr) - 48 : 0;
    const v2 = b.charAt(endPtr) ? b.charCodeAt(endPtr) - 48 : 0;
    const sum = v1 + v2 + carry;
    if (sum === 0 || sum === 1) {
      carry = 0;
      result = `${sum}${result}`;
    } else if (sum === 2) {
      carry = 1;
      result = `0${result}`;
    } else if (sum === 3) {
      carry = 1;
      result = `1${result}`;
    }
    endPtr--;
  }
  if (carry) {
    result = `1${result}`;
  }
  return result;
};
```

**Reference**

1.  Add Binary (https://leetcode.com/problems/add-binary/)
