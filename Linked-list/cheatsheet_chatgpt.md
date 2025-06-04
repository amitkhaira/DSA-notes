Key logic and patterns for **Singly, Doubly, and Circular Linked Lists**, using **JavaScript** code examples.

---

```markdown
# 🧠 Linked List Coding Cheat Sheet (JS Edition)

## 📌 Node Definitions

### Singly Linked List Node
```js
class Node {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}
```

### Doubly Linked List Node
```js
class DNode {
  constructor(data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }
}
```

### Circular Linked List Node (Singly)
```js
class CNode {
  constructor(data) {
    this.data = data;
    this.next = null;
  }
}
```

---

## 🔧 Core Operations

### Insert at Head (Singly)
```js
function insertHead(head, data) {
  const node = new Node(data);
  node.next = head;
  return node;
}
```

### Insert at Tail (Singly)
```js
function insertTail(head, data) {
  const node = new Node(data);
  if (!head) return node;
  let curr = head;
  while (curr.next) curr = curr.next;
  curr.next = node;
  return head;
}
```

### Delete from Head (Singly)
```js
function deleteHead(head) {
  return head ? head.next : null;
}
```

### Delete from Tail (Singly)
```js
function deleteTail(head) {
  if (!head || !head.next) return null;
  let curr = head;
  while (curr.next.next) curr = curr.next;
  curr.next = null;
  return head;
}
```

---

## 🔁 Common Patterns

### Reverse a Linked List (Iterative)
```js
function reverse(head) {
  let prev = null;
  let curr = head;
  while (curr) {
    let next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }
  return prev;
}
```

### Detect Cycle (Floyd’s Algorithm)
```js
function hasCycle(head) {
  let slow = head, fast = head;
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow === fast) return true;
  }
  return false;
}
```

### Find Middle Node
```js
function findMiddle(head) {
  let slow = head, fast = head;
  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }
  return slow;
}
```

### Merge Two Sorted Lists
```js
function merge(l1, l2) {
  let dummy = new Node(0), tail = dummy;
  while (l1 && l2) {
    if (l1.data < l2.data) {
      tail.next = l1;
      l1 = l1.next;
    } else {
      tail.next = l2;
      l2 = l2.next;
    }
    tail = tail.next;
  }
  tail.next = l1 || l2;
  return dummy.next;
}
```

---

## 🔄 Doubly Linked List Logic

### Insert at Head
```js
function insertDHead(head, data) {
  const node = new DNode(data);
  node.next = head;
  if (head) head.prev = node;
  return node;
}
```

### Delete from Tail
```js
function deleteDTail(head) {
  if (!head || !head.next) return null;
  let curr = head;
  while (curr.next) curr = curr.next;
  if (curr.prev) curr.prev.next = null;
  return head;
}
```

---

## 🔁 Circular Linked List Logic

### Create Circular List
```js
function makeCircular(head) {
  if (!head) return null;
  let curr = head;
  while (curr.next) curr = curr.next;
  curr.next = head;
  return head;
}
```

### Traverse Circular List (safe loop)
```js
function traverseCircular(head) {
  if (!head) return;
  let curr = head;
  do {
    console.log(curr.data);
    curr = curr.next;
  } while (curr && curr !== head);
}
```

---

## 🧠 Tips

- Always check for `null` before accessing `.next` or `.prev`
- Use **dummy node** to simplify head operations
- For cycle detection, remember **fast and slow pointers**
- Doubly lists need **extra attention to both links**
- In circular lists, use **do-while** to avoid infinite loops

---
```
