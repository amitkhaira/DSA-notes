cheat sheet for Linked List logic, focusing on JavaScript for coding examples.

```markdown
# Linked List Logic Cheat Sheet (JS)

## 1. Node Structures

```javascript
// Singly Linked List Node
class ListNode {
    constructor(value, next = null) {
        this.value = value;
        this.next = next;
    }
}

// Doubly Linked List Node
class DoublyListNode {
    constructor(value, next = null, prev = null) {
        this.value = value;
        this.next = next;
        this.prev = prev;
    }
}
```

## 2. Core Concepts & Pointers

*   **`head`**: Points to the first node. `null` if list is empty.
*   **`tail`**: (Optional but useful) Points to the last node. `null` if list is empty.
*   **`current`**: Iterator pointer, used for traversal.
*   **`prev` / `previous`**: Pointer often used to keep track of the node before `current`.
*   **`next`**: Pointer within a node, points to the subsequent node.
*   **`null` termination**: In singly and doubly (non-circular), the last node's `next` is `null`. In doubly, the first node's `prev` is `null`.

## 3. Common Operations & Logic

---

### A. Singly Linked List (SLL)

**Key:** Manipulate `next` pointers.

*   **Traversal**:
    *   `let current = head;`
    *   `while (current !== null) { /* process current.value */; current = current.next; }`
    *   **Edge Case**: Empty list (`head === null`).

*   **Insertion**:
    *   **At Head**:
        1.  `newNode.next = head;`
        2.  `head = newNode;`
        3.  (If tracking `tail` and list was empty: `tail = newNode;`)
    *   **At Tail**:
        1.  If no `tail` pointer: Traverse to find last node (`lastNode`). `lastNode.next = newNode;`
        2.  If `tail` pointer: `tail.next = newNode; tail = newNode;`
        3.  **Edge Case**: Empty list (becomes insert at head).
    *   **In Middle (after `prevNode`)**:
        1.  `newNode.next = prevNode.next;`
        2.  `prevNode.next = newNode;`
        3.  **Edge Case**: `prevNode` is last node (becomes insert at tail).

*   **Deletion**:
    *   **At Head**:
        1.  `head = head.next;`
        2.  (If tracking `tail` and list becomes empty: `tail = null;`)
        3.  **Edge Case**: List has 1 node (head becomes `null`).
    *   **At Tail**:
        1.  Traverse to find `secondToLastNode`.
        2.  `secondToLastNode.next = null;`
        3.  (If tracking `tail`: `tail = secondToLastNode;`)
        4.  **Edge Case**: List has 1 node (becomes delete at head).
    *   **In Middle (node `toDelete`, `prevNode` points to `toDelete`)**:
        1.  `prevNode.next = toDelete.next;` (or `prevNode.next = prevNode.next.next;`)
        2.  **Edge Case**: `toDelete` is head (becomes delete at head), `toDelete` is tail (becomes delete at tail).

*   **Search (value)**: Traverse, compare `current.value`. Return `current` or `true` if found.

*   **Reverse**:
    *   `let prev = null; let current = head; let nextNode = null;`
    *   `while (current !== null) {`
        1.  `nextNode = current.next;`
        2.  `current.next = prev;`
        3.  `prev = current;`
        4.  `current = nextNode;`
    *   `}`
    *   `head = prev;` (Update tail if tracked)

---

### B. Doubly Linked List (DLL)

**Key:** Manipulate `next` AND `prev` pointers carefully. Order matters!

*   **Traversal**: Same as SLL (can also go backwards using `prev`).

*   **Insertion**:
    *   **At Head**:
        1.  `newNode.next = head;`
        2.  If `head !== null`, `head.prev = newNode;`
        3.  `head = newNode;`
        4.  (If tracking `tail` and list was empty: `tail = newNode;`)
    *   **At Tail**:
        1.  `newNode.prev = tail;`
        2.  If `tail !== null`, `tail.next = newNode;`
        3.  `tail = newNode;`
        4.  (If tracking `head` and list was empty: `head = newNode;`)
    *   **In Middle (after `prevNode`, so inserting between `prevNode` and `nextNode = prevNode.next`)**:
        1.  `newNode.next = nextNode;`
        2.  `newNode.prev = prevNode;`
        3.  `prevNode.next = newNode;`
        4.  If `nextNode !== null`, `nextNode.prev = newNode;`

*   **Deletion**:
    *   **At Head**:
        1.  `head = head.next;`
        2.  If `head !== null`, `head.prev = null;`
        3.  (If tracking `tail` and list becomes empty: `tail = null;`)
    *   **At Tail**:
        1.  `tail = tail.prev;`
        2.  If `tail !== null`, `tail.next = null;`
        3.  (If tracking `head` and list becomes empty: `head = null;`)
    *   **In Middle (node `toDelete`)**:
        1.  `let prevNode = toDelete.prev;`
        2.  `let nextNode = toDelete.next;`
        3.  If `prevNode !== null`, `prevNode.next = nextNode;`
        4.  If `nextNode !== null`, `nextNode.prev = prevNode;`
        5.  (If `toDelete` was `head`, update `head`. If `toDelete` was `tail`, update `tail`.)

---

### C. Circular Linked List (CLL)

**Key:** Last node's `next` points to `head`. For Circular DLL, `head`'s `prev` also points to `tail`.

*   **Traversal**:
    *   **Important**: Need a different stop condition.
    *   `if (!head) return;`
    *   `let current = head;`
    *   `do { /* process current.value */; current = current.next; } while (current !== head);`
    *   **Edge Case**: Empty list.

*   **Insertion**:
    *   **At Head (new node becomes new head)**:
        1.  Find `tail` (traverse or if tracked).
        2.  `newNode.next = head;`
        3.  `head = newNode;`
        4.  `tail.next = head;` (Crucial for circularity)
        5.  (For CDLL: `head.prev = tail;` `newNode.next.prev = newNode;` or old `head.prev = newNode`)
        6.  **Edge Case**: Empty list: `newNode.next = newNode; head = newNode; tail = newNode;` (For CDLL: `newNode.prev = newNode;`)
    *   **At Tail (new node becomes new tail)**:
        1.  `newNode.next = head;`
        2.  `tail.next = newNode;`
        3.  `tail = newNode;`
        4.  (For CDLL: `newNode.prev = oldTail;` `head.prev = newNode;`)

*   **Deletion**:
    *   **At Head**:
        1.  Find `tail`.
        2.  `tail.next = head.next;`
        3.  `head = head.next;`
        4.  (For CDLL: `head.prev = tail;`)
        5.  **Edge Case**: Single node: `head = null; tail = null;`
    *   **Any Node (given `prevNode` and `nodeToDelete`)**:
        1.  `prevNode.next = nodeToDelete.next;`
        2.  If `nodeToDelete === head`, update `head`.
        3.  If `nodeToDelete === tail`, update `tail` (and `tail.next` to new `head`).
        4.  (For CDLL: `nodeToDelete.next.prev = prevNode;`)

## 4. Common Patterns & Techniques

*   **Two Pointers (Fast & Slow)**:
    *   **Find Middle**: `slow` moves 1 step, `fast` moves 2 steps. When `fast` reaches end, `slow` is at middle.
        ```javascript
        // let slow = head, fast = head;
        // while (fast !== null && fast.next !== null) {
        //     slow = slow.next;
        //     fast = fast.next.next;
        // }
        // return slow; // slow is middle
        ```
    *   **Detect Cycle**: If `slow` and `fast` meet, there's a cycle.
        ```javascript
        // let slow = head, fast = head;
        // while (fast !== null && fast.next !== null) {
        //     slow = slow.next;
        //     fast = fast.next.next;
        //     if (slow === fast) return true; // Cycle detected
        // }
        // return false;
        ```
    *   **Find Nth Node from End**:
        1.  Move `p1` N steps forward.
        2.  Move `p1` and `p2` (starts at `head`) together until `p1` reaches end. `p2` is Nth from end.

*   **Dummy Head / Sentinel Node**:
    *   `let dummy = new ListNode(0); dummy.next = head;`
    *   Simplifies edge cases for insertion/deletion at the head, as the head is never truly "null" from the perspective of the algorithm (it's `dummy.next`).
    *   `prev` pointer can start at `dummy`. Return `dummy.next` as the new head.

## 5. General Tips & Gotchas

*   **Null Pointer Checks**: Always check `current`, `current.next`, `current.next.next` etc., before accessing their properties.
*   **Empty List**: Handle `head === null` as an edge case for most operations.
*   **Single Node List**: Another common edge case.
*   **Order of Pointer Updates**: CRITICAL. Draw diagrams if unsure. Generally, establish new links *before* breaking old ones.
    *   Insertion: `newNode.next = target; prev.next = newNode;`
    *   Deletion: `prev.next = target.next;`
*   **Modifying `head` / `tail`**: Ensure these are updated correctly after insertions/deletions at ends.
*   **Off-by-One Errors**: Common in loops or when finding Nth elements.
*   **Memory Leaks (Manual Memory Management)**: Not an issue with JS garbage collection, but conceptually, ensure deleted nodes are no longer referenced.
*   **Circular List Traversal**: Ensure your loop terminates correctly (`current !== head` after initial step) and doesn't run infinitely on an empty or single-node list.

## 6. Time & Space Complexity

| Operation         | SLL Avg   | SLL Worst | DLL Avg   | DLL Worst | Notes                                     |
| :---------------- | :-------- | :-------- | :-------- | :-------- | :---------------------------------------- |
| **Access (by idx)** | O(n)      | O(n)      | O(n)      | O(n)      | Must traverse                             |
| **Search (value)**  | O(n)      | O(n)      | O(n)      | O(n)      | Must traverse                             |
| **Insertion (Head)**| O(1)      | O(1)      | O(1)      | O(1)      |                                           |
| **Insertion (Tail)**| O(n)/O(1) | O(n)/O(1) | O(1)      | O(1)      | O(n) if no tail ptr, O(1) if tail ptr   |
| **Insertion (Mid)** | O(n)      | O(n)      | O(n)      | O(n)      | To find position, then O(1) for insert  |
| **Deletion (Head)** | O(1)      | O(1)      | O(1)      | O(1)      |                                           |
| **Deletion (Tail)** | O(n)      | O(n)      | O(1)      | O(1)      | SLL needs to find prev, DLL has tail.prev |
| **Deletion (Mid)**  | O(n)      | O(n)      | O(n)      | O(n)      | To find position, then O(1) for delete  |
| **Space (Overall)** | O(n)      | O(n)      | O(n)      | O(n)      | For storing n elements                    |

*Circular Lists generally have similar complexities to their SLL/DLL counterparts for equivalent operations, but traversal stop conditions are key.*
```
