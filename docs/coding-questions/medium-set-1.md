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
