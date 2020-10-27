---
id: polyfills
title: Polyfills
---

## bfs-dfs.js

```javascript
class Node {
  constructor(value = null, parent = null, children = []) {
    this.value = value;
    this.children = children;
  }
}

Node.prototype.traverseBFS = function () {
  const traverse = [];
  const queue = [this];
  while (queue.length !== 0) {
    const item = queue.shift();
    traverse.push(item.value);
    if (Array.isArray(item.children)) {
      item.children.forEach((_item) => queue.push(_item));
    }
  }
  return traverse;
};

Node.prototype.traverseDFS = function () {
  let traverse = [];
  traverse.push(this.value);
  if (Array.isArray(this.children)) {
    this.children.forEach((_item) => {
      traverse = [...traverse, ..._item.traverseDFS()];
    });
  }
  return traverse;
};

class Tree {
  constructor(value) {
    this.root = new Node(value);
  }
}

Tree.prototype.traverseBFS = function () {
  return this.root.traverseBFS();
};

Tree.prototype.traverseDFS = function () {
  return this.root.traverseDFS();
};

const tree1 = new Tree("root node");

tree1.root.children.length = 3;
tree1.root.children[0] = new Node("L1-0");
tree1.root.children[1] = new Node("L1-1");
tree1.root.children[2] = new Node("L1-2");

tree1.root.children[0].children.length = 3;
tree1.root.children[0].children[0] = new Node("L2-0-0");
tree1.root.children[0].children[1] = new Node("L2-0-1");
tree1.root.children[0].children[2] = new Node("L2-0-2");

tree1.root.children[1].children.length = 3;
tree1.root.children[1].children[0] = new Node("L2-1-0");
tree1.root.children[1].children[1] = new Node("L2-1-1");
tree1.root.children[1].children[2] = new Node("L2-1-2");

tree1.root.children[2].children.length = 3;
tree1.root.children[2].children[0] = new Node("L2-2-0");
tree1.root.children[2].children[1] = new Node("L2-2-1");
tree1.root.children[2].children[2] = new Node("L2-2-2");

console.log(tree1.traverseBFS());

console.log(tree1.traverseDFS());
```

## bind.js

```javascript
const bind = (fn, context, ...boundedArgs) => (...newArrgs) =>
  fn.apply(context, [...boundedArgs, newArrgs]);

const test = {
  name: "shyam",
  sayHello: function () {
    console.log("Hi", this.name);
  },
};

const boundedHello = test.sayHello.bind({ name: "arora" });
const customBind = bind(test.sayHello, { name: "new context" });

test.sayHello();
boundedHello();
customBind();
```

## bst.js

```javascript
class BinaryTreeNode {
  constructor(value = null, left = null, right = null, parent = null) {
    this.left = left;
    this.right = right;
    this.value = value;
    this.parent = parent;
  }
}

BinaryTreeNode.prototype.setValue = function (value) {
  this.value = value;
};

BinaryTreeNode.prototype.setLeft = function (node) {
  if (this.left) this.left = null;
  this.left = node;
  if (this.left) this.left.parent = this;
};

BinaryTreeNode.prototype.setRight = function (node) {
  if (this.right) this.right = null;
  this.right = node;
  if (this.right) this.right.parent = this;
};

BinaryTreeNode.prototype.removeChild = function (node) {
  if (this.left === node) {
    this.left = null;
    return true;
  }
  if (this.right === node) {
    this.right = null;
    return true;
  }
  return false;
};

BinaryTreeNode.prototype.traverseInorder = function () {
  let traverse = [];
  if (this.left) traverse = [...traverse, ...this.left.traverseInorder()];
  traverse.push(this.value);
  if (this.right) traverse = [...traverse, ...this.right.traverseInorder()];
  return traverse;
};

BinaryTreeNode.prototype.traversePostorder = function () {
  let traverse = [];
  if (this.left) traverse = [...traverse, ...this.left.traversePostorder()];
  if (this.right) traverse = [...traverse, ...this.right.traversePostorder()];
  traverse.push(this.value);
  return traverse;
};

BinaryTreeNode.prototype.traversePreorder = function () {
  let traverse = [];
  traverse.push(this.value);
  if (this.left) traverse = [...traverse, ...this.left.traversePreorder()];
  if (this.right) traverse = [...traverse, ...this.right.traversePreorder()];
  return traverse;
};

BinaryTreeNode.prototype.traverseLevelorder = function () {
  const traversed = [];
  const queue = [this];
  while (queue.length !== 0) {
    const current = queue.shift();
    if (current.left) {
      queue.push(current.left);
    }
    if (current.right) {
      queue.push(current.right);
    }
    traversed.push(current.value);
  }
  return traversed;
};

class BinarySearchTreeNode extends BinaryTreeNode {
  constructor(value) {
    super(value);
  }
}

BinarySearchTreeNode.prototype.insert = function (value) {
  if (!this.value) {
    this.value = value;
    return this;
  }

  if (value <= this.value) {
    if (this.left) return this.left.insert(value);
    const newNode = new BinarySearchTreeNode(value);
    this.setLeft(newNode);
    return newNode;
  }

  if (value > this.value) {
    if (this.right) return this.right.insert(value);
    const newNode = new BinarySearchTreeNode(value);
    this.setRight(newNode);
    return newNode;
  }
  return this;
};

BinarySearchTreeNode.prototype.find = function (value) {
  if (this.value === value) {
    return this;
  }
  if (value < this.value && this.left) {
    return this.left.find(value);
  }
  if (value > this.value && this.right) {
    return this.right.find(value);
  }
  return null;
};

BinarySearchTreeNode.prototype.remove = function (value) {
  const nodeToRemove = this.find(value);
  if (!nodeToRemove) {
    throw new Error("Node not found");
  }
  const { parent = null, left, right } = nodeToRemove;
  if (!left && !right) {
    if (parent) parent.removeChild(nodeToRemove);
    nodeToRemove.setValue(null);
  } else if (left && right) {
    const inorderSuccessor = right.findMin();
    const inorderSuccessorValue = inorderSuccessor.value;
    if (inorderSuccessor === right) {
      nodeToRemove.setValue(inorderSuccessorValue);
      nodeToRemove.setRight(inorderSuccessor.right);
    } else {
      this.remove(inorderSuccessorValue);
      nodeToRemove.setValue(inorderSuccessorValue);
    }
    inorderSuccessor.parent = null;
  } else {
    let childNode = left || right;
    if (parent) {
      childNode.parent = parent;
      if (parent.left === nodeToRemove) {
        parent.left = childNode;
      } else if (parent.right === nodeToRemove) {
        parent.right = childNode;
      }
    } else {
      this.setLeft(childNode.left);
      this.setRight(childNode.right);
      this.setValue(childNode.value);
      childNode = null;
    }
  }
};

BinarySearchTreeNode.prototype.findMin = function () {
  if (!this.left) {
    return this;
  }
  return this.left.findMin();
};

const getCircularReplacer = () => {
  const seen = new WeakSet();
  return (key, value) => {
    if (typeof value === "object" && value !== null) {
      if (seen.has(value)) {
        return;
      }
      seen.add(value);
    }
    return value;
  };
};

class BinarySearchTree {
  constructor() {
    this.root = new BinarySearchTreeNode();
  }
}

BinarySearchTree.prototype.insert = function (value) {
  this.root.insert(value);
};

BinarySearchTree.prototype.find = function (value) {
  return this.root.find(value);
};

BinarySearchTree.prototype.remove = function (value) {
  return this.root.remove(value);
};

BinarySearchTree.prototype.traversePreorder = function () {
  return this.root.traversePreorder();
};

BinarySearchTree.prototype.traverseInorder = function () {
  return this.root.traverseInorder();
};

BinarySearchTree.prototype.traversePostorder = function () {
  return this.root.traversePostorder();
};

BinarySearchTree.prototype.traverseLevelorder = function () {
  return this.root.traverseLevelorder();
};

const bst = new BinarySearchTree();

bst.insert(20);
bst.insert(16);
bst.insert(12);
bst.insert(22);
bst.insert(14);
bst.insert(21);
bst.insert(23);
bst.insert(11);
bst.insert(13);

const minimum = bst.root.findMin();
console.log("Minimum value => ", minimum && minimum.value);
console.log(JSON.stringify(bst, getCircularReplacer(), 4));

const preorder = bst.traversePreorder();
console.log("Preorder => ", preorder);
const postorder = bst.traversePostorder();
console.log("Postorder => ", postorder);
const inorder = bst.traverseInorder();
console.log("Inorder", inorder);
const levetorder = bst.traverseLevelorder();
console.log("TraverseLevelorder", levetorder);

const node = bst.find(14);
console.log(node ? "Node Exist" : "Node doesn't exist");

// bst.remove(20)
// console.log(JSON.stringify(bst, getCircularReplacer(),4))
```

## compose.js

```javascript
const multiplyBy5 = (x) => x * 5;
const add = (a, b) => a + b;

const compose = (...fns) =>
  fns.reduce((acc, curr) => (...args) => acc(curr(...args)));

const result = compose(multiplyBy5, add);

console.log(result(10, 10));
```

## composeRight.js

```javascript
const multiplyBy5 = (x, y) => x * y;
const add = (a) => a + 9;

const compose = (...fns) =>
  fns.reduce((acc, curr) => (...args) => curr(acc(...args)));
const compose2 = (...fns) =>
  fns.reduceRight((acc, curr) => (...args) => acc(curr(...args)));

const result = compose(multiplyBy5, add);

const result2 = compose2(multiplyBy5, add);

console.log(result(10, 10));
console.log(result2(10, 10));
```

## curry.js

```javascript
const curry = (fn, ...args) =>
  fn.length === args.length ? fn(...args) : curry.bind(null, fn, ...args);

const add = (a, b, c, d, e, f, g, h, i) => a + b + c + d + e + f + g + h + i;

const curried = curry(add, 2, 3);

console.log(curried(1, 2, 2)(4, 4, 3, 2)(4, 4));

//const res = add1.bind({ temp: 1 }, 1, 2, 2).bind({ temp: 2 }, 4, 4, 3, 2).bind({ temp: 3 }, 4, 4).apply({ temp: 4 });
```

## debounce.js

````javascript
const debounce = (fn, delay) => {
	let timeoutId;
	return function(...args){
		clearInterval(timeoutId)
		timeoutId = setTimeout(()=>fn.apply(this,args),delay)
	}
}```


## deepClone.js

```javascript
const deepClone = (inputObj) => {
  if (typeof inputObj !== "object") return inputObj;
  const copy = Array.isArray(inputObj) ? [] : {};
  Object.keys(inputObj).forEach((key) => {
    copy[key] = deepClone(inputObj[key]);
  });
  return copy;
};

const temp = [
  {
    a: {
      b: {
        c: [1, 2],
        fn: (name) => console.log("I am executed by", name),
      },
    },
    d: {
      e: {
        f: [3, 4],
      },
    },
  },
];

const temp2 = deepClone(temp);
temp2[0].a.b.fn = null;
temp[0].a.b.fn("temp");

console.log(JSON.stringify(temp2, null, 2));
console.log(JSON.stringify(temp, null, 2));
````

## equals.js

```javascript
const equals = (a, b) => {
  if (a === b) return true;
  if (typeof a !== "object" && typeof b !== "object") return a === b;
  let keys = Object.keys(a);
  if (keys.length !== Object.keys(b).length) return false;
  return keys.every((k) => equals(a[k], b[k]));
};

const result = equals(
  { a: [2, { e: 3 }], b: [4], c: "foo" },
  { a: [2, { e: 3 }], b: [4], c: "foo" }
); // true

console.log(result);
```

## filter.js

```javascript
Array.prototype.filter = function (fn) {
  const filteredArray = [];
  for (let i = 0; i < this.length; i++) {
    const value = fn(this[i], i);
    if (value) filteredArray.push(this[i]);
  }
  return filteredArray;
};

console.log([1, 2, 3, 4, 5].filter((item) => item % 2));
```

## graph.js

```javascript

```

## linkedList.js

```javascript
function SinglyLinkedListNode(value, next = null) {
  this.value = value;
  this.next = next;
}

function SinglyLinkedList() {
  this.head = null;
  this.tail = null;
  this.size = 0;
}

SinglyLinkedList.prototype.size = function () {
  return this.size + 1;
};

SinglyLinkedList.prototype.insert = function (value) {
  this.size++;
  const newNode = new SinglyLinkedListNode(value);
  if (!this.head) {
    this.head = newNode;
    this.tail = newNode;
    return;
  }
  this.tail.next = newNode;
  this.tail = newNode;
};

SinglyLinkedList.prototype.insertAtFront = function (value) {
  if (!this.head) {
    this.insert(value);
    return;
  }
  this.size++;
  const newNode = new SinglyLinkedListNode(value);
  newNode.next = this.head;
  this.head = newNode;
};

SinglyLinkedList.prototype.findIndex = function (value) {
  let currentNode = this.head;
  let i = 0;
  while (currentNode) {
    if (currentNode.value === value) {
      console.log(`Node exist at ${i} index`);
      return i;
    }
    currentNode = currentNode.next;
    i++;
  }
  console.log("Node doesn't exist");
  return -1;
};

SinglyLinkedList.prototype.traverse = function () {
  let listString = "";
  let currentNode = this.head;
  while (currentNode) {
    listString = `${listString} ${currentNode.value} =>`;
    currentNode = currentNode.next;
  }
  console.log(`[${this.size}]: ${listString}`);
};

SinglyLinkedList.prototype.insertAt = function (index, value) {
  if (index > this.size)
    throw new Error(`${this.constructor.name} Error: IndexOverflow`);
  if (this.size === index) {
    this.insert(value);
    return;
  }
  if (index === 0) {
    this.insertAtFront(value);
    return;
  }
  let currentNode = this.head;
  for (let i = 0; i < index - 1; i++) {
    currentNode = currentNode.next;
  }
  const newNode = new SinglyLinkedListNode(value, currentNode.next);
  currentNode.next = newNode;
  this.size++;
};

SinglyLinkedList.prototype.reveseList = function () {
  let lastNode = null;
  let currentNode = this.head;
  let nextNode = null;
  while (currentNode) {
    nextNode = currentNode.next;
    currentNode.next = lastNode;
    lastNode = currentNode;
    currentNode = nextNode;
  }
  this.tail = this.head;
  this.head = lastNode;
};

SinglyLinkedList.prototype.getReversedCopy = function () {
  const copy = new SinglyLinkedList();
  let currentNode = this.head;
  while (currentNode) {
    copy.insertAtFront(currentNode.value);
    currentNode = currentNode.next;
  }
  return copy;
};

// const ll1 = new SinglyLinkedList();

// ll1.insert(12);
// ll1.insert(24);
// ll1.insert(36);
// ll1.insertAtFront(48);
// ll1.findIndex(24);
// ll1.traverse();
// ll1.insertAt(1, 60);

function DLLNode(val) {
  this.val = val;
  this.prev = null;
  this.next = null;
}

function DoublyLinkedList() {
  this.front = null;
  this.end = null;
}

DoublyLinkedList.prototype.addFront = function (val) {
  const newNode = new DLLNode(val);
  if (!this.front) {
    this.front = this.end = newNode;
  } else {
    newNode.next = this.front;
    this.front.prev = newNode;
    this.front = newNode;
  }
};

DoublyLinkedList.prototype.addEnd = function (val) {
  const newNode = new DLLNode(val);
  if (!this.end) {
    this.front = this.end = newNode;
  } else {
    this.end.next = newNode;
    newNode.prev = this.end;
    this.end = newNode;
  }
};

DoublyLinkedList.prototype.delete = function (val) {
  if (!this.front) return false;
  if (this.front.val === val && this.end.val === val) {
    this.front = null;
    this.end = null;
  } else if (this.front.val === val) {
    this.front = this.front.next;
    this.front.prev = null;
  } else if (this.end.val === val) {
    this.end = this.end.prev;
    this.end.next = null;
  }
  let current = this.front;
  while (current && current.next) {
    if (current.next.val === val) {
      current.next.next.prev = current;
      current.next = current.next.next;
      return true;
    } else {
      current = current.next;
    }
  }
  return false;
};

DoublyLinkedList.prototype.forwardTraverse = function () {
  let current = this.front;
  const result = [];
  while (current) {
    result.push(current.val);
    current = current.next;
  }
  return result;
};

DoublyLinkedList.prototype.reverseTraverse = function () {
  let current = this.end;
  const result = [];
  while (current) {
    result.push(current.val);
    current = current.prev;
  }
  return result;
};

const dll = new DoublyLinkedList();
dll.addEnd(11);
dll.addEnd(13);
dll.addEnd(15);
dll.addEnd(17);
dll.addEnd(19);
dll.addFront(2);
dll.addFront(3);
dll.addFront(5);
dll.addFront(7);
dll.addFront(9);
dll.addEnd(21);
dll.addEnd(23);

console.log(dll.forwardTraverse());
console.log([...dll.reverseTraverse()].reverse());

console.log(dll.forwardTraverse());
console.log([...dll.reverseTraverse()].reverse());
```

## map.js

```javascript
Array.prototype.map = function (fn) {
  let transformedArray = Array.from({ length: this.length });
  for (let i = 0; i < this.length; i++) {
    transformedArray[i] = fn(this[i], i);
  }
  return transformedArray;
};

console.log([1, 2].map((item) => item + 2));
```

## memoize.js

```javascript
const memoize = (fn) => {
  const store = new Map();
  return function (val) {
    if (!store.has(val)) {
      const result = fn.call(this, val);
      store.set(val, result);
    }
    return store.get(val);
  };
};

const add = (input) => input + 10;

const memoizeAdd = memoize(add);

console.log(memoizeAdd(1));
console.log(memoizeAdd(2));
console.log(memoizeAdd(3));
console.log(memoizeAdd(1));
console.log(memoizeAdd(2));
console.log(memoizeAdd(3));
```

## pathOr.js

```javascript
const testInput = { a: { b: { "c.d": [{ a: 12 }] } } };

const pathOr = (input, path = [], or) =>
  path.reduce((acc, current) => acc[current] || or, input);

const result = pathOr(testInput, ["a", "b", "c.d", [0], "a"], "hi");

console.log(result);
```

## pubsub.js

```javascript
const PubSub = (() => {
  const store = {};
  return {
    publish: (eventName, data = {}) => {
      if (store[eventName]) {
        store[eventName].forEach((fn) => {
          fn(data);
        });
      }
    },
    subscribe: (eventName, callback) => {
      if (!store[eventName]) {
        store[eventName] = [];
      }
      const index = store[eventName].push(callback) - 1;
      return {
        remove() {
          delete store[eventName][index];
        },
      };
    },
  };
})();

PubSub.subscribe("eventId", (data) => {
  console.log("Sb1", data);
});
PubSub.subscribe("eventId", (data) => {
  console.log("Sb2", data);
});
PubSub.subscribe("eventId", (data) => {
  console.log("Sb3", data);
});
const ref4 = PubSub.subscribe("eventId", (data) => {
  console.log("Sb4", data);
});

ref4.remove();

PubSub.publish("eventId", "hello");
```

## reduce.js

```javascript
Array.prototype.reduce = function (fn, initial) {
  if (!this.length) return;
  let last = initial;
  if (initial) {
    for (let i = 0; i < this.length; i++) {
      last = fn(last, this[i]);
    }
  } else {
    last = this[0];
    for (let i = 1; i < this.length; i++) {
      last = fn(last, this[i]);
    }
  }
  return last;
};

const result = [1, 2, 3, 4].reduce((prev, current) => prev + current);

console.log(result);
```

## singleton.js

```javascript
const Singleton = (() => {
  let instance;
  const createInstance = () => new Object("I am the instance");

  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    },
  };
})();

const result = Singleton.getInstance();
const result2 = Singleton.getInstance();
console.log(result === result2);
```

## test.js

```javascript

```

## throttle.js

```javascript
const throttle = (fn, delay) => {
  let firedAt;
  return function (...args) {
    const currentTime = Date.now();
    if (!firedAt || delay < currentTime - firedAt) {
      firedAt = currentTime;
      fn.apply(this, args);
    }
  };
};
```
