Okay, here's a merged and enhanced cheat sheet for Linked Lists, combining the strengths of both notes:

```markdown
# Linked List Ultimate Cheat Sheet (JavaScript)

This cheat sheet provides a comprehensive overview of Linked List logic, data structures, common operations, patterns, and complexities, with JavaScript examples.

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

// Note: Circular Linked List nodes are structurally similar to Singly or Doubly
// list nodes, but their `next` (and `prev` for Doubly Circular) pointers
// are set to create a loop (e.g., tail.next points to head).
```

## 2. Core Concepts & Pointers

*   **`head`**: Points to the first node. `null` if the list is empty.
*   **`tail`**: (Optional but often useful) Points to the last node. `null` if the list is empty.
*   **`current` / `curr`**: Iterator pointer, used for traversal.
*   **`prev` / `previous`**: Pointer often used to keep track of the node before `current`.
*   **`next`**: Pointer within a node, points to the subsequent node.
*   **`null` termination**:
    *   In Singly Linked Lists (SLL) and non-circular Doubly Linked Lists (DLL), the last node's `next` is `null`.
    *   In DLLs, the first node's `prev` is `null`.

## 3. Common Operations

---

### A. Singly Linked List (SLL)

**Key:** Manipulate `next` pointers.

*   **Traversal**:
    *   Logic: Start from `head`, move `current = current.next` until `current` is `null`.
    *   Edge Case: Empty list (`head === null`).
    ```javascript
    function traverseSLL(head) {
        let current = head;
        while (current !== null) {
            console.log(current.value); // Process current.value
            current = current.next;
        }
    }
    ```

*   **Insertion**:
    *   **At Head**:
        1.  Create `newNode`.
        2.  `newNode.next = head;`
        3.  `head = newNode;`
        4.  (If tracking `tail` and list was empty: `tail = newNode;`)
        ```javascript
        function insertAtHeadSLL(head, value) {
            const newNode = new ListNode(value);
            newNode.next = head;
            return newNode; // New head
        }
        ```
    *   **At Tail**:
        1.  Create `newNode`.
        2.  If list is empty: `head = newNode;` (and `tail = newNode;` if tracked).
        3.  Else if no `tail` pointer: Traverse to find last node (`lastNode`). `lastNode.next = newNode;`
        4.  Else if `tail` pointer: `tail.next = newNode; tail = newNode;`
        ```javascript
        function insertAtTailSLL(head, value) {
            const newNode = new ListNode(value);
            if (!head) {
                return newNode; // New head
            }
            let current = head;
            while (current.next !== null) {
                current = current.next;
            }
            current.next = newNode;
            return head;
        }
        ```
    *   **In Middle (after `prevNode`)**:
        1.  Create `newNode`.
        2.  `newNode.next = prevNode.next;`
        3.  `prevNode.next = newNode;`
        4.  Edge Case: `prevNode` is last node (becomes insert at tail).

*   **Deletion**:
    *   **At Head**:
        1.  `head = head.next;`
        2.  (If tracking `tail` and list becomes empty: `tail = null;`)
        3.  Edge Case: List has 1 node (head becomes `null`).
        ```javascript
        function deleteAtHeadSLL(head) {
            if (!head) return null;
            return head.next; // New head
        }
        ```
    *   **At Tail**:
        1.  If list empty or has 1 node: `head = null;` (and `tail = null;`).
        2.  Else: Traverse to find `secondToLastNode`.
        3.  `secondToLastNode.next = null;`
        4.  (If tracking `tail`: `tail = secondToLastNode;`)
        ```javascript
        function deleteAtTailSLL(head) {
            if (!head || !head.next) { // Empty or single node list
                return null;
            }
            let current = head;
            while (current.next.next !== null) {
                current = current.next;
            }
            current.next = null;
            return head;
        }
        ```
    *   **In Middle (node `toDelete`, `prevNode` points to `toDelete`)**:
        1.  `prevNode.next = toDelete.next;` (or `prevNode.next = prevNode.next.next;`)
        2.  Edge Cases: `toDelete` is head (delete at head), `toDelete` is tail (delete at tail).

*   **Search (value)**: Traverse, compare `current.value`. Return `current` node or `true` if found, else `null` or `false`.

*   **Reverse**:
    *   Logic: Use three pointers: `prev = null`, `current = head`, `nextNode = null`.
    *   In a loop:
        1.  `nextNode = current.next;` (Store next node)
        2.  `current.next = prev;` (Reverse current node's pointer)
        3.  `prev = current;` (Move `prev` one step forward)
        4.  `current = nextNode;` (Move `current` one step forward)
    *   After loop: `head = prev;` (Update `tail` if tracked: original head becomes new tail).
    ```javascript
    function reverseSLL(head) {
        let prev = null;
        let current = head;
        let nextNode = null;
        while (current !== null) {
            nextNode = current.next;
            current.next = prev;
            prev = current;
            current = nextNode;
        }
        return prev; // New head
    }
    ```

---

### B. Doubly Linked List (DLL)

**Key:** Manipulate `next` AND `prev` pointers carefully. Order of updates matters!

*   **Traversal**: Same as SLL (can also go backwards using `prev`).
    ```javascript
    function traverseDLL(head) { // Forward
        let current = head;
        while (current !== null) {
            console.log(current.value);
            current = current.next;
        }
    }
    function traverseDLLBackward(tail) { // Backward (if tail is known)
        let current = tail;
        while (current !== null) {
            console.log(current.value);
            current = current.prev;
        }
    }
    ```

*   **Insertion**:
    *   **At Head**:
        1.  Create `newNode`.
        2.  `newNode.next = head;`
        3.  If `head !== null`, `head.prev = newNode;`
        4.  `head = newNode;`
        5.  (If tracking `tail` and list was empty: `tail = newNode;`)
        ```javascript
        function insertAtHeadDLL(head, value) {
            const newNode = new DoublyListNode(value);
            newNode.next = head;
            if (head !== null) {
                head.prev = newNode;
            }
            return newNode; // New head
        }
        ```
    *   **At Tail**:
        1.  Create `newNode`.
        2.  If list empty: `head = newNode; tail = newNode;`
        3.  Else (if `tail` pointer is tracked):
            *   `newNode.prev = tail;`
            *   `tail.next = newNode;`
            *   `tail = newNode;`
        4.  Else (no `tail` pointer): Traverse to find last node. Then link `newNode`.
        ```javascript
        // Assuming DLL class has head and tail properties
        function insertAtTailDLL(list, value) { // list is an object { head, tail }
            const newNode = new DoublyListNode(value);
            if (!list.head) {
                list.head = newNode;
                list.tail = newNode;
                return;
            }
            list.tail.next = newNode;
            newNode.prev = list.tail;
            list.tail = newNode;
        }
        ```
    *   **In Middle (insert `newNode` after `prevNode`)**:
        Let `nextNode = prevNode.next;`
        1.  `newNode.next = nextNode;`
        2.  `newNode.prev = prevNode;`
        3.  `prevNode.next = newNode;`
        4.  If `nextNode !== null`, `nextNode.prev = newNode;`

*   **Deletion**:
    *   **At Head**:
        1.  If `!head` return.
        2.  `head = head.next;`
        3.  If `head !== null`, `head.prev = null;`
        4.  (If tracking `tail` and list becomes empty: `tail = null;`)
        ```javascript
        function deleteAtHeadDLL(head) {
            if (!head) return null;
            const newHead = head.next;
            if (newHead) {
                newHead.prev = null;
            }
            return newHead;
        }
        ```
    *   **At Tail**:
        1.  If `!tail` return.
        2.  `tail = tail.prev;`
        3.  If `tail !== null`, `tail.next = null;`
        4.  (If tracking `head` and list becomes empty: `head = null;`)
        ```javascript
        // Assuming DLL class has head and tail properties
        function deleteAtTailDLL(list) { // list is an object { head, tail }
            if (!list.tail) return;
            if (list.head === list.tail) { // Single node
                list.head = null;
                list.tail = null;
                return;
            }
            const newTail = list.tail.prev;
            newTail.next = null;
            list.tail = newTail;
        }
        ```
    *   **In Middle (node `toDelete`)**:
        1.  `let prevNode = toDelete.prev;`
        2.  `let nextNode = toDelete.next;`
        3.  If `prevNode !== null`, `prevNode.next = nextNode;` else update `head`.
        4.  If `nextNode !== null`, `nextNode.prev = prevNode;` else update `tail`.
        5.  (Crucial: If `toDelete` was `head`, new `head` is `nextNode`. If `toDelete` was `tail`, new `tail` is `prevNode`.)

---

### C. Circular Linked List (CLL)

**Key:** Last node's `next` points to `head`. For Circular DLL (CDLL), `head`'s `prev` also points to `tail`. Traversal requires a careful stop condition.

*   **Creating a Circular List (from SLL head)**:
    ```javascript
    function makeCircularSLL(head) {
        if (!head) return null;
        let current = head;
        while (current.next !== null) {
            current = current.next;
        }
        current.next = head; // Last node points to head
        return head;
    }
    ```

*   **Traversal**:
    *   **Important**: Need a stop condition other than `current === null`. Use `do...while` to process the head node at least once.
    *   `if (!head) return;`
    *   `let current = head;`
    *   `do { /* process current.value */; current = current.next; } while (current !== head);`
    *   Edge Case: Empty list.
    ```javascript
    function traverseCLL(head) {
        if (!head) return;
        let current = head;
        do {
            console.log(current.value);
            current = current.next;
        } while (current !== null && current !== head); // current !== null for safety if list becomes non-circular by mistake
    }
    ```

*   **Insertion**:
    *   **At Head (new node becomes new head)**:
        1.  Find `tail` (traverse or if tracked). If list empty, new node points to itself.
        2.  `newNode.next = head;`
        3.  `head = newNode;`
        4.  `tail.next = head;` (Crucial for circularity)
        5.  (For CDLL: `head.prev = tail; newNode.next.prev = newNode;` or old `head.prev = newNode`)
        6.  Edge Case (Empty List): `newNode.next = newNode; head = newNode; tail = newNode;` (For CDLL: `newNode.prev = newNode;`)
    *   **At Tail (new node becomes new tail)**:
        1.  Find `oldTail` (traverse or if tracked).
        2.  `newNode.next = head;`
        3.  `oldTail.next = newNode;`
        4.  `tail = newNode;` (Update tail pointer)
        5.  (For CDLL: `newNode.prev = oldTail; head.prev = newNode;`)

*   **Deletion**:
    *   **At Head**:
        1.  Find `tail`.
        2.  If single node: `head = null; tail = null;`
        3.  Else: `tail.next = head.next; head = head.next;`
        4.  (For CDLL: `head.prev = tail;`)
    *   **Any Node (given `prevNode` and `nodeToDelete`)**:
        1.  `prevNode.next = nodeToDelete.next;`
        2.  If `nodeToDelete === head`, update `head = nodeToDelete.next;`.
        3.  If `nodeToDelete === tail`, update `tail = prevNode;` (and `tail.next` to new `head`).
        4.  (For CDLL: `nodeToDelete.next.prev = prevNode;`)

## 4. Common Patterns & Techniques

*   **Two Pointers (Fast & Slow)**:
    *   **Find Middle Node**: `slow` moves 1 step, `fast` moves 2 steps. When `fast` (or `fast.next`) reaches end, `slow` is at (or just before) the middle.
        ```javascript
        function findMiddleNode(head) {
            let slow = head, fast = head;
            while (fast !== null && fast.next !== null) {
                slow = slow.next;
                fast = fast.next.next;
            }
            return slow; // slow is the middle (or first middle if even length)
        }
        ```
    *   **Detect Cycle (Floyd's Tortoise and Hare Algorithm)**: If `slow` and `fast` meet, there's a cycle.
        ```javascript
        function hasCycle(head) {
            let slow = head, fast = head;
            while (fast !== null && fast.next !== null) {
                slow = slow.next;
                fast = fast.next.next;
                if (slow === fast) return true; // Cycle detected
            }
            return false; // No cycle
        }
        ```
    *   **Find Nth Node from End**:
        1.  Initialize two pointers, `p1` and `p2`, to `head`.
        2.  Move `p1` N steps forward.
        3.  If `p1` is `null` (N is larger than list size), handle error or return `null`.
        4.  Move `p1` and `p2` together one step at a time until `p1` reaches the last node (`p1.next === null`). `p2` will be at the Nth node from the end.

*   **Dummy Head / Sentinel Node**:
    *   `let dummy = new ListNode(0); dummy.next = head;`
    *   Simplifies edge cases for insertion/deletion at the head, as operations can treat `dummy` as the node *before* the actual head.
    *   `prev` pointer can start at `dummy`.
    *   The actual list starts at `dummy.next`. Return `dummy.next` as the new head of the list.

*   **Merge Two Sorted Lists**:
    *   Use a `dummy` node to build the new list. A `tail` pointer tracks the end of the merged list.
    ```javascript
    function mergeTwoSortedLists(l1, l2) {
        const dummy = new ListNode(0);
        let tail = dummy;

        while (l1 !== null && l2 !== null) {
            if (l1.value < l2.value) {
                tail.next = l1;
                l1 = l1.next;
            } else {
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }

        // Append remaining nodes from l1 or l2
        if (l1 !== null) {
            tail.next = l1;
        } else if (l2 !== null) {
            tail.next = l2;
        }
        return dummy.next; // Head of the merged list
    }
    ```

## 5. General Tips & Gotchas

*   **Null Pointer Checks**: CRITICAL. Always check `current`, `current.next`, `current.prev`, `current.next.next` etc., before accessing their properties to avoid "Cannot read property '...' of null" errors.
*   **Empty List**: Handle `head === null` as an edge case for most operations.
*   **Single Node List**: Another common edge case to test (`head.next === null`).
*   **Order of Pointer Updates**: Absolutely critical, especially for DLLs and complex SLL operations like deletion or insertion in the middle. Draw diagrams if unsure. Generally, establish new links *before* breaking old ones.
    *   Insertion: `newNode.next = targetNode; prevNode.next = newNode;`
    *   Deletion: `prevNode.next = nodeToDelete.next;`
*   **Modifying `head` / `tail`**: Ensure these global pointers are updated correctly after insertions/deletions at the ends or when the list structure fundamentally changes (e.g., reversal). Functions modifying the list structure should usually return the new `head`.
*   **Off-by-One Errors**: Common in loops, finding Nth elements, or conditions like `while (current.next !== null)` vs `while (current !== null)`.
*   **Memory Leaks (in languages without garbage collection)**: Not an issue with JavaScript's garbage collection, but conceptually, ensure deleted nodes are no longer referenced to allow GC to reclaim memory.
*   **Circular List Traversal**: Ensure your loop terminates correctly (e.g., `current !== head` after an initial step if using `do...while`) and handles empty or single-node lists gracefully to avoid infinite loops.
*   **Drawing is Your Friend**: When tackling complex linked list problems, draw the nodes and pointers on paper. Manually step through your pointer manipulations.

## 6. Time & Space Complexity

| Operation              | SLL Avg   | SLL Worst | DLL Avg   | DLL Worst | Notes                                                         |
| :--------------------- | :-------- | :-------- | :-------- | :-------- | :------------------------------------------------------------ |
| **Access (by index)**  | O(n)      | O(n)      | O(n)      | O(n)      | Must traverse from head (or tail for DLL if index is > n/2) |
| **Search (by value)**  | O(n)      | O(n)      | O(n)      | O(n)      | Must traverse                                                 |
| **Insertion (Head)**   | O(1)      | O(1)      | O(1)      | O(1)      | Direct pointer manipulation                                   |
| **Insertion (Tail)**   | O(n)/O(1) | O(n)/O(1) | O(1)      | O(1)      | O(n) for SLL without tail ptr; O(1) if tail ptr is maintained |
| **Insertion (Middle)** | O(n)      | O(n)      | O(n)      | O(n)      | O(n) to find position, then O(1) for actual pointer updates   |
| **Deletion (Head)**    | O(1)      | O(1)      | O(1)      | O(1)      | Direct pointer manipulation                                   |
| **Deletion (Tail)**    | O(n)      | O(n)      | O(1)      | O(1)      | SLL needs O(n) to find prev to tail; DLL has `tail.prev` (O(1)) |
| **Deletion (Middle)**  | O(n)      | O(n)      | O(n)      | O(n)      | O(n) to find node, then O(1) for actual pointer updates       |
| **Space (Overall)**    | O(n)      | O(n)      | O(n)      | O(n)      | For storing n elements                                        |

*Circular Lists generally have similar complexities to their SLL/DLL counterparts for equivalent operations. Traversal logic and edge cases for circularity are the main differences.*
```
