# Complete Linked Lists Master Cheat Sheet - JavaScript

## 1. Fundamentals & Theory

### What are Linked Lists?
- **Definition**: Dynamic data structure where elements (nodes) are stored in sequence
- **Memory**: Non-contiguous memory allocation
- **Access**: Sequential access only (no random access like arrays)
- **Size**: Dynamic size (grows/shrinks during runtime)

### Why Use Linked Lists?
- **Advantages**:
  - Dynamic size
  - Efficient insertion/deletion at beginning (O(1))
  - Memory efficient (allocates as needed)
  - No memory waste (unlike arrays with fixed size)
- **Disadvantages**:
  - No random access (O(n) to access element)
  - Extra memory for storing pointers
  - Not cache-friendly
  - No backward traversal (in singly linked lists)

### Types Comparison Table
| Feature | Singly | Doubly | Circular |
|---------|--------|--------|----------|
| Memory per node | 1 pointer | 2 pointers | 1 pointer |
| Backward traversal | ❌ | ✅ | ❌ |
| Insert at head | O(1) | O(1) | O(1) |
| Insert at tail | O(n) | O(1)* | O(n) |
| Delete from middle | O(n) | O(1)** | O(n) |
| Space complexity | O(n) | O(2n) | O(n) |

*With tail pointer, **If node reference is given

## 2. Node Structures & Initialization

### Singly Linked List
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

class SinglyLinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }
}
```

### Doubly Linked List
```javascript
class DoublyListNode {
    constructor(val = 0, prev = null, next = null) {
        this.val = val;
        this.prev = prev;
        this.next = next;
    }
}

class DoublyLinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
}
```

### Circular Linked List
```javascript
class CircularListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

class CircularLinkedList {
    constructor() {
        this.head = null;
        this.tail = null; // Optional: for O(1) tail operations
        this.size = 0;
    }
}
```

## 3. Core Patterns & Techniques

### Two Pointer Techniques
```javascript
// 1. Fast & Slow (Floyd's Cycle Detection)
function twoPointerSlow(head) {
    let slow = head, fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        // Use for: cycle detection, middle finding
    }
    return slow;
}

// 2. Distance-based two pointers
function twoPointerDistance(head, k) {
    let first = head, second = head;
    // Move first k steps ahead
    for (let i = 0; i < k && first; i++) {
        first = first.next;
    }
    // Move both until first reaches end
    while (first) {
        first = first.next;
        second = second.next;
    }
    
    second.next = second.next.next;
    return dummy.next;
}

// 2. Reorder List (L0→L1→…→Ln-1→Ln to L0→Ln→L1→Ln-1→L2→Ln-2→…)
function reorderList(head) {
    if (!head || !head.next) return;
    
    // Find middle
    let slow = head, fast = head;
    while (fast.next && fast.next.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse second half
    let secondHalf = reverseList(slow.next);
    slow.next = null;
    
    // Merge alternately
    let first = head, second = secondHalf;
    while (second) {
        let temp1 = first.next;
        let temp2 = second.next;
        first.next = second;
        second.next = temp1;
        first = temp1;
        second = temp2;
    }
}

// 3. Copy List with Random Pointer
function copyRandomList(head) {
    if (!head) return null;
    
    let map = new Map();
    let current = head;
    
    // First pass: create all nodes
    while (current) {
        map.set(current, new ListNode(current.val));
        current = current.next;
    }
    
    // Second pass: set next and random pointers
    current = head;
    while (current) {
        if (current.next) {
            map.get(current).next = map.get(current.next);
        }
        if (current.random) {
            map.get(current).random = map.get(current.random);
        }
        current = current.next;
    }
    
    return map.get(head);
}

// 4. Intersection of Two Linked Lists
function getIntersectionNode(headA, headB) {
    let a = headA, b = headB;
    while (a !== b) {
        a = a ? a.next : headB;
        b = b ? b.next : headA;
    }
    return a;
}

// 5. Linked List Cycle II (Find cycle start)
function detectCycle(head) {
    let slow = head, fast = head;
    
    // Detect cycle
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) break;
    }
    
    if (!fast || !fast.next) return null;
    
    // Find cycle start
    slow = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }
    
    return slow;
}
```

### Hard Level
```javascript
// 1. Reverse Nodes in k-Group
function reverseKGroup(head, k) {
    let count = 0;
    let current = head;
    
    // Count k nodes
    while (current && count < k) {
        current = current.next;
        count++;
    }
    
    if (count === k) {
        current = reverseKGroup(current, k);
        
        // Reverse current k nodes
        while (count-- > 0) {
            let next = head.next;
            head.next = current;
            current = head;
            head = next;
        }
        head = current;
    }
    
    return head;
}

// 2. Merge k Sorted Lists
function mergeKLists(lists) {
    if (!lists || lists.length === 0) return null;
    
    while (lists.length > 1) {
        let mergedLists = [];
        for (let i = 0; i < lists.length; i += 2) {
            let l1 = lists[i];
            let l2 = i + 1 < lists.length ? lists[i + 1] : null;
            mergedLists.push(mergeTwoSortedLists(l1, l2));
        }
        lists = mergedLists;
    }
    
    return lists[0];
}

// 3. Sort List (Merge Sort)
function sortList(head) {
    if (!head || !head.next) return head;
    
    // Split list
    let mid = getMid(head);
    let right = mid.next;
    mid.next = null;
    
    // Sort both halves
    let left = sortList(head);
    right = sortList(right);
    
    // Merge sorted halves
    return mergeTwoSortedLists(left, right);
}

function getMid(head) {
    let slow = head, fast = head, prev = null;
    while (fast && fast.next) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    return prev;
}

// 4. LRU Cache (using doubly linked list + hash map)
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
        
        // Create dummy head and tail
        this.head = new DoublyListNode(-1);
        this.tail = new DoublyListNode(-1);
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }
    
    addToHead(node) {
        node.prev = this.head;
        node.next = this.head.next;
        this.head.next.prev = node;
        this.head.next = node;
    }
    
    removeNode(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    moveToHead(node) {
        this.removeNode(node);
        this.addToHead(node);
    }
    
    removeTail() {
        let last = this.tail.prev;
        this.removeNode(last);
        return last;
    }
    
    get(key) {
        let node = this.cache.get(key);
        if (node) {
            this.moveToHead(node);
            return node.val;
        }
        return -1;
    }
    
    put(key, value) {
        let node = this.cache.get(key);
        if (node) {
            node.val = value;
            this.moveToHead(node);
        } else {
            let newNode = new DoublyListNode(value);
            newNode.key = key;
            
            if (this.cache.size >= this.capacity) {
                let tail = this.removeTail();
                this.cache.delete(tail.key);
            }
            
            this.addToHead(newNode);
            this.cache.set(key, newNode);
        }
    }
}
```

## 10. Edge Cases & Common Pitfalls

### Critical Edge Cases Always Test
```javascript
// Edge case handler template
function handleEdgeCases(head) {
    // 1. Empty list
    if (!head) return null;
    
    // 2. Single node
    if (!head.next) {
        // Handle single node case
        return head;
    }
    
    // 3. Two nodes
    if (!head.next.next) {
        // Handle two node case
        return head;
    }
    
    // Normal case logic here
}

// Common edge cases checklist:
const edgeCases = [
    "null/empty list",
    "single node",
    "two nodes",
    "all nodes same value",
    "already sorted/reversed",
    "cycle present",
    "very large list",
    "negative values",
    "duplicate values"
];
```

### Memory Management & Common Mistakes
```javascript
// ❌ Common Mistakes
function commonMistakes() {
    // 1. Not handling null pointers
    // if (head.next) // Wrong! head could be null
    
    // 2. Losing reference to head
    // head = head.next; // Lost original head!
    
    // 3. Infinite loops in circular lists
    // while (current.next) // Will loop forever in circular list
    
    // 4. Not updating size counter
    // Forgetting to increment/decrement size
    
    // 5. Memory leaks in deletion
    // Not setting deleted node pointers to null
}

// ✅ Best Practices
function bestPractices() {
    // 1. Always check for null
    if (!head || !head.next) return head;
    
    // 2. Use dummy nodes for head changes
    let dummy = new ListNode(0);
    dummy.next = head;
    
    // 3. Track previous node for deletions
    let prev = dummy, current = head;
    
    // 4. Clean up deleted nodes
    let nodeToDelete = current;
    prev.next = current.next;
    nodeToDelete.next = null; // Clean reference
    
    // 5. Use two-pointer technique for optimization
    let slow = head, fast = head;
}
```

## 11. Testing & Debugging Utilities

### Debug Helper Functions
```javascript
// Print list with cycle detection
function printList(head, maxNodes = 20) {
    let result = [];
    let current = head;
    let visited = new Set();
    let count = 0;
    
    while (current && count < maxNodes) {
        if (visited.has(current)) {
            result.push(`-> CYCLE at ${current.val}`);
            break;
        }
        
        visited.add(current);
        result.push(current.val);
        current = current.next;
        count++;
    }
    
    if (count >= maxNodes && current) {
        result.push('... (truncated)');
    }
    
    console.log(result.join(' -> '));
    return result;
}

// Create list from array
function createListFromArray(arr) {
    if (!arr || arr.length === 0) return null;
    
    let head = new ListNode(arr[0]);
    let current = head;
    
    for (let i = 1; i < arr.length; i++) {
        current.next = new ListNode(arr[i]);
        current = current.next;
    }
    
    return head;
}

// Convert list to array
function listToArray(head) {
    let result = [];
    let current = head;
    let visited = new Set();
    
    while (current && !visited.has(current)) {
        visited.add(current);
        result.push(current.val);
        current = current.next;
    }
    
    return result;
}

// Create cycle for testing
function createCycle(head, cyclePos) {
    if (!head || cyclePos < 0) return head;
    
    let current = head;
    let cycleNode = null;
    let tail = null;
    let index = 0;
    
    while (current) {
        if (index === cyclePos) {
            cycleNode = current;
        }
        if (!current.next) {
            tail = current;
        }
        current = current.next;
        index++;
    }
    
    if (tail && cycleNode) {
        tail.next = cycleNode;
    }
    
    return head;
}

// Validate list integrity
function validateList(head) {
    let issues = [];
    let current = head;
    let visited = new Set();
    let count = 0;
    
    while (current && count < 10000) {
        if (visited.has(current)) {
            issues.push(`Cycle detected at node ${current.val}`);
            break;
        }
        
        visited.add(current);
        
        // Check for invalid references
        if (current.next === current) {
            issues.push(`Self-reference at node ${current.val}`);
        }
        
        current = current.next;
        count++;
    }
    
    if (count >= 10000) {
        issues.push('Possible infinite loop or very large list');
    }
    
    return {
        isValid: issues.length === 0,
        issues: issues,
        nodeCount: visited.size
    };
}
```

### Performance Testing
```javascript
// Benchmark linked list operations
function benchmarkOperations() {
    const sizes = [1000, 10000, 100000];
    
    sizes.forEach(size => {
        console.log(`\n--- Testing with ${size} nodes ---`);
        
        // Create test list
        let arr = Array.from({length: size}, (_, i) => i);
        let head = createListFromArray(arr);
        
        // Test traversal
        console.time(`Traversal ${size}`);
        let count = 0;
        let current = head;
        while (current) {
            count++;
            current = current.next;
        }
        console.timeEnd(`Traversal ${size}`);
        
        // Test search
        console.time(`Search ${size}`);
        let searchValue = Math.floor(size / 2);
        let found = false;
        current = head;
        while (current) {
            if (current.val === searchValue) {
                found = true;
                break;
            }
            current = current.next;
        }
        console.timeEnd(`Search ${size}`);
        
        // Test reversal
        console.time(`Reverse ${size}`);
        head = reverseList(head);
        console.timeEnd(`Reverse ${size}`);
    });
}
```

## 12. Interview Tips & Strategies

### Problem-Solving Approach
1. **Clarify Requirements**
   - Ask about edge cases (empty list, single node)
   - Confirm input/output format
   - Check for cycles, duplicates, constraints

2. **Choose Right Data Structure**
   - Singly: Simple operations, memory efficient
   - Doubly: Need backward traversal, frequent deletions
   - Circular: Round-robin, Josephus problems

3. **Common Patterns Recognition**
   - Two pointers → Middle, cycle, nth from end
   - Dummy node → Head modifications
   - Stack → Reverse, parentheses matching
   - Hash map → Fast lookups, cycle detection

4. **Optimization Strategy**
   - Single pass vs multiple passes
   - Space-time tradeoffs
   - In-place vs extra space

### Code Template Structure
```javascript
function solveLinkedListProblem(head) {
    // 1. Handle edge cases
    if (!head) return null;
    if (!head.next) return head; // Single node
    
    // 2. Initialize variables
    let dummy = new ListNode(0);
    dummy.next = head;
    let slow = head, fast = head;
    let prev = dummy;
    
    // 3. Main algorithm
    while (/* condition */) {
        // Core logic
    }
    
    // 4. Clean up and return
    return dummy.next;
}
```

### Time Complexity Quick Reference
- **O(1)**: Insert/delete at known position with reference
- **O(n)**: Traversal, search, most single-pass algorithms
- **O(n log n)**: Merge sort, divide & conquer approaches
- **O(n²)**: Nested loops, bubble sort, naive approaches

### Space Complexity Patterns
- **O(1)**: Two pointers, iterative approaches
- **O(n)**: Hash maps, recursion stack, copying structures
- **O(log n)**: Divide & conquer recursion

This comprehensive cheat sheet covers every aspect of linked lists you'll encounter in coding interviews and real-world applications. Practice these patterns and you'll master linked list problems!
    }
    return second; // kth from end
}

// 3. Meeting point technique
function findMeetingPoint(head1, head2) {
    let a = head1, b = head2;
    while (a !== b) {
        a = a ? a.next : head2;
        b = b ? b.next : head1;
    }
    return a; // intersection point
}
```

### Dummy Node Patterns
```javascript
// 1. Simple dummy for head changes
function withDummy(head) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let current = dummy;
    // ... operations that might change head
    return dummy.next;
}

// 2. Multiple dummies for partitioning
function partition(head, x) {
    let smallDummy = new ListNode(0);
    let largeDummy = new ListNode(0);
    let small = smallDummy, large = largeDummy;
    
    while (head) {
        if (head.val < x) {
            small.next = head;
            small = small.next;
        } else {
            large.next = head;
            large = large.next;
        }
        head = head.next;
    }
    
    large.next = null;
    small.next = largeDummy.next;
    return smallDummy.next;
}
```

### Reversal Patterns
```javascript
// 1. Iterative reversal
function reverseIterative(head) {
    let prev = null, current = head;
    while (current) {
        let next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    return prev;
}

// 2. Recursive reversal
function reverseRecursive(head) {
    if (!head || !head.next) return head;
    
    let newHead = reverseRecursive(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}

// 3. Reverse in groups
function reverseKGroup(head, k) {
    let count = 0, current = head;
    while (current && count < k) {
        current = current.next;
        count++;
    }
    
    if (count === k) {
        current = reverseKGroup(current, k);
        while (count-- > 0) {
            let next = head.next;
            head.next = current;
            current = head;
            head = next;
        }
        head = current;
    }
    return head;
}
```

## 4. Singly Linked List - Complete Operations

### Basic Operations
```javascript
class SinglyLinkedList {
    constructor() {
        this.head = null;
        this.size = 0;
    }

    // Insert at beginning - O(1)
    insertAtHead(val) {
        let newNode = new ListNode(val);
        newNode.next = this.head;
        this.head = newNode;
        this.size++;
        return this;
    }

    // Insert at end - O(n)
    insertAtTail(val) {
        let newNode = new ListNode(val);
        if (!this.head) {
            this.head = newNode;
        } else {
            let current = this.head;
            while (current.next) {
                current = current.next;
            }
            current.next = newNode;
        }
        this.size++;
        return this;
    }

    // Insert at position - O(n)
    insertAt(index, val) {
        if (index < 0 || index > this.size) return false;
        if (index === 0) return this.insertAtHead(val);
        if (index === this.size) return this.insertAtTail(val);

        let newNode = new ListNode(val);
        let current = this.head;
        for (let i = 0; i < index - 1; i++) {
            current = current.next;
        }
        newNode.next = current.next;
        current.next = newNode;
        this.size++;
        return this;
    }

    // Delete by value - O(n)
    deleteByValue(val) {
        if (!this.head) return false;
        
        if (this.head.val === val) {
            this.head = this.head.next;
            this.size--;
            return true;
        }
        
        let current = this.head;
        while (current.next && current.next.val !== val) {
            current = current.next;
        }
        
        if (current.next) {
            current.next = current.next.next;
            this.size--;
            return true;
        }
        return false;
    }

    // Delete at position - O(n)
    deleteAt(index) {
        if (index < 0 || index >= this.size) return false;
        
        if (index === 0) {
            this.head = this.head.next;
            this.size--;
            return true;
        }
        
        let current = this.head;
        for (let i = 0; i < index - 1; i++) {
            current = current.next;
        }
        current.next = current.next.next;
        this.size--;
        return true;
    }

    // Search - O(n)
    search(val) {
        let current = this.head;
        let index = 0;
        while (current) {
            if (current.val === val) return index;
            current = current.next;
            index++;
        }
        return -1;
    }

    // Get element at index - O(n)
    get(index) {
        if (index < 0 || index >= this.size) return null;
        let current = this.head;
        for (let i = 0; i < index; i++) {
            current = current.next;
        }
        return current.val;
    }

    // Utility methods
    isEmpty() { return this.size === 0; }
    getSize() { return this.size; }
    clear() { this.head = null; this.size = 0; }
    
    toArray() {
        let result = [];
        let current = this.head;
        while (current) {
            result.push(current.val);
            current = current.next;
        }
        return result;
    }
}
```

### Advanced Singly Linked List Operations
```javascript
// Remove duplicates from sorted list
function removeDuplicatesSorted(head) {
    let current = head;
    while (current && current.next) {
        if (current.val === current.next.val) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }
    return head;
}

// Remove duplicates from unsorted list
function removeDuplicatesUnsorted(head) {
    if (!head) return head;
    let seen = new Set();
    let prev = null, current = head;
    
    while (current) {
        if (seen.has(current.val)) {
            prev.next = current.next;
        } else {
            seen.add(current.val);
            prev = current;
        }
        current = current.next;
    }
    return head;
}

// Find nth node from end
function findNthFromEnd(head, n) {
    let first = head, second = head;
    for (let i = 0; i < n; i++) {
        if (!first) return null;
        first = first.next;
    }
    while (first) {
        first = first.next;
        second = second.next;
    }
    return second;
}

// Rotate list by k positions
function rotateRight(head, k) {
    if (!head || !head.next || k === 0) return head;
    
    // Find length and make circular
    let length = 1;
    let tail = head;
    while (tail.next) {
        tail = tail.next;
        length++;
    }
    tail.next = head;
    
    // Find new tail and head
    k = k % length;
    let stepsToNewTail = length - k;
    let newTail = head;
    for (let i = 1; i < stepsToNewTail; i++) {
        newTail = newTail.next;
    }
    
    let newHead = newTail.next;
    newTail.next = null;
    return newHead;
}
```

## 5. Doubly Linked List - Complete Operations

```javascript
class DoublyLinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    // Insert at beginning - O(1)
    insertAtHead(val) {
        let newNode = new DoublyListNode(val);
        if (!this.head) {
            this.head = this.tail = newNode;
        } else {
            newNode.next = this.head;
            this.head.prev = newNode;
            this.head = newNode;
        }
        this.size++;
        return this;
    }

    // Insert at end - O(1)
    insertAtTail(val) {
        let newNode = new DoublyListNode(val);
        if (!this.tail) {
            this.head = this.tail = newNode;
        } else {
            this.tail.next = newNode;
            newNode.prev = this.tail;
            this.tail = newNode;
        }
        this.size++;
        return this;
    }

    // Insert at position - O(n)
    insertAt(index, val) {
        if (index < 0 || index > this.size) return false;
        if (index === 0) return this.insertAtHead(val);
        if (index === this.size) return this.insertAtTail(val);

        let newNode = new DoublyListNode(val);
        let current;
        
        // Optimize: start from head or tail based on index
        if (index <= this.size / 2) {
            current = this.head;
            for (let i = 0; i < index; i++) {
                current = current.next;
            }
        } else {
            current = this.tail;
            for (let i = this.size - 1; i > index; i--) {
                current = current.prev;
            }
        }
        
        newNode.next = current;
        newNode.prev = current.prev;
        current.prev.next = newNode;
        current.prev = newNode;
        this.size++;
        return this;
    }

    // Delete node (given node reference) - O(1)
    deleteNode(node) {
        if (!node) return false;
        
        if (node.prev) {
            node.prev.next = node.next;
        } else {
            this.head = node.next;
        }
        
        if (node.next) {
            node.next.prev = node.prev;
        } else {
            this.tail = node.prev;
        }
        
        this.size--;
        return true;
    }

    // Delete by value - O(n)
    deleteByValue(val) {
        let current = this.head;
        while (current) {
            if (current.val === val) {
                this.deleteNode(current);
                return true;
            }
            current = current.next;
        }
        return false;
    }

    // Reverse doubly linked list - O(n)
    reverse() {
        let current = this.head;
        let temp = null;
        
        while (current) {
            temp = current.prev;
            current.prev = current.next;
            current.next = temp;
            current = current.prev;
        }
        
        if (temp) {
            this.tail = this.head;
            this.head = temp.prev;
        }
        return this;
    }

    // Forward traversal
    forwardTraversal() {
        let result = [];
        let current = this.head;
        while (current) {
            result.push(current.val);
            current = current.next;
        }
        return result;
    }

    // Backward traversal
    backwardTraversal() {
        let result = [];
        let current = this.tail;
        while (current) {
            result.push(current.val);
            current = current.prev;
        }
        return result;
    }
}
```

### Advanced Doubly Linked List Operations
```javascript
// LRU Cache implementation using doubly linked list
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
        this.head = new DoublyListNode(-1);
        this.tail = new DoublyListNode(-1);
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }

    addToHead(node) {
        node.prev = this.head;
        node.next = this.head.next;
        this.head.next.prev = node;
        this.head.next = node;
    }

    removeNode(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    moveToHead(node) {
        this.removeNode(node);
        this.addToHead(node);
    }

    removeTail() {
        let last = this.tail.prev;
        this.removeNode(last);
        return last;
    }

    get(key) {
        let node = this.cache.get(key);
        if (node) {
            this.moveToHead(node);
            return node.val;
        }
        return -1;
    }

    put(key, value) {
        let node = this.cache.get(key);
        if (node) {
            node.val = value;
            this.moveToHead(node);
        } else {
            let newNode = new DoublyListNode(value);
            newNode.key = key;
            
            if (this.cache.size >= this.capacity) {
                let tail = this.removeTail();
                this.cache.delete(tail.key);
            }
            
            this.addToHead(newNode);
            this.cache.set(key, newNode);
        }
    }
}
```

## 6. Circular Linked List - Complete Operations

```javascript
class CircularLinkedList {
    constructor() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    // Insert at beginning - O(1)
    insertAtHead(val) {
        let newNode = new CircularListNode(val);
        if (!this.head) {
            this.head = this.tail = newNode;
            newNode.next = newNode;
        } else {
            newNode.next = this.head;
            this.tail.next = newNode;
            this.head = newNode;
        }
        this.size++;
        return this;
    }

    // Insert at end - O(1) with tail pointer
    insertAtTail(val) {
        let newNode = new CircularListNode(val);
        if (!this.head) {
            this.head = this.tail = newNode;
            newNode.next = newNode;
        } else {
            newNode.next = this.head;
            this.tail.next = newNode;
            this.tail = newNode;
        }
        this.size++;
        return this;
    }

    // Delete by value - O(n)
    deleteByValue(val) {
        if (!this.head) return false;
        
        // Single node case
        if (this.head === this.tail && this.head.val === val) {
            this.head = this.tail = null;
            this.size--;
            return true;
        }
        
        // Head deletion
        if (this.head.val === val) {
            this.tail.next = this.head.next;
            this.head = this.head.next;
            this.size--;
            return true;
        }
        
        // Search and delete
        let current = this.head;
        while (current.next !== this.head) {
            if (current.next.val === val) {
                if (current.next === this.tail) {
                    this.tail = current;
                }
                current.next = current.next.next;
                this.size--;
                return true;
            }
            current = current.next;
        }
        return false;
    }

    // Check if list is circular
    isCircular() {
        if (!this.head) return false;
        let current = this.head;
        do {
            current = current.next;
        } while (current && current !== this.head);
        return current === this.head;
    }

    // Convert to regular linked list
    breakCircle() {
        if (this.tail) {
            this.tail.next = null;
        }
        return this.head;
    }

    // Josephus problem solution
    josephus(k) {
        if (!this.head) return null;
        
        let current = this.head;
        while (this.size > 1) {
            // Move k-1 steps
            for (let i = 1; i < k; i++) {
                current = current.next;
            }
            
            // Delete current node
            let nodeToDelete = current;
            let prev = current;
            while (prev.next !== current) {
                prev = prev.next;
            }
            
            if (current === this.head) {
                this.head = current.next;
            }
            if (current === this.tail) {
                this.tail = prev;
            }
            
            prev.next = current.next;
            current = current.next;
            this.size--;
        }
        
        return this.head;
    }

    // Split circular list into two halves
    splitCircular() {
        if (!this.head || this.size < 2) return [this.head, null];
        
        let slow = this.head, fast = this.head;
        let prev = null;
        
        // Find middle using Floyd's algorithm
        do {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        } while (fast !== this.head && fast.next !== this.head);
        
        // Split the list
        let head2 = slow;
        prev.next = this.head;
        
        // Make second half circular
        let current = head2;
        while (current.next !== this.head) {
            current = current.next;
        }
        current.next = head2;
        
        return [this.head, head2];
    }
}
```

## 7. Advanced Algorithms & Problem Patterns

### Cycle Detection & Analysis
```javascript
// Floyd's Cycle Detection with meeting point
function detectCycleAdvanced(head) {
    let slow = head, fast = head;
    
    // Phase 1: Detect cycle
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) break;
    }
    
    if (!fast || !fast.next) return null; // No cycle
    
    // Phase 2: Find cycle start
    slow = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }
    
    // Phase 3: Find cycle length
    let cycleLength = 1;
    fast = fast.next;
    while (fast !== slow) {
        fast = fast.next;
        cycleLength++;
    }
    
    return {
        hasCycle: true,
        cycleStart: slow,
        cycleLength: cycleLength
    };
}

// Remove cycle from linked list
function removeCycle(head) {
    let cycleInfo = detectCycleAdvanced(head);
    if (!cycleInfo.hasCycle) return head;
    
    let current = cycleInfo.cycleStart;
    while (current.next !== cycleInfo.cycleStart) {
        current = current.next;
    }
    current.next = null;
    return head;
}
```

### Sorting Algorithms
```javascript
// Merge Sort for Linked List - O(n log n)
function mergeSort(head) {
    if (!head || !head.next) return head;
    
    // Split list into two halves
    let middle = getMiddle(head);
    let secondHalf = middle.next;
    middle.next = null;
    
    // Recursively sort both halves
    let left = mergeSort(head);
    let right = mergeSort(secondHalf);
    
    // Merge sorted halves
    return merge(left, right);
}

function getMiddle(head) {
    let slow = head, fast = head, prev = null;
    while (fast && fast.next) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    return prev;
}

function merge(l1, l2) {
    let dummy = new ListNode(0);
    let current = dummy;
    
    while (l1 && l2) {
        if (l1.val <= l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    current.next = l1 || l2;
    return dummy.next;
}

// Quick Sort for Linked List - O(n log n) average
function quickSort(head, tail = null) {
    if (head !== tail && head && head.next !== tail) {
        let pivot = partition(head, tail);
        quickSort(head, pivot);
        quickSort(pivot.next, tail);
    }
    return head;
}

function partition(head, tail) {
    let pivot = head;
    let current = head.next;
    let pivotPos = head;
    
    while (current !== tail) {
        if (current.val < pivot.val) {
            pivotPos = pivotPos.next;
            [pivotPos.val, current.val] = [current.val, pivotPos.val];
        }
        current = current.next;
    }
    
    [pivot.val, pivotPos.val] = [pivotPos.val, pivot.val];
    return pivotPos;
}
```

### Complex Manipulations
```javascript
// Add two numbers represented as linked lists
function addTwoNumbers(l1, l2) {
    let dummy = new ListNode(0);
    let current = dummy;
    let carry = 0;
    
    while (l1 || l2 || carry) {
        let sum = carry;
        if (l1) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2) {
            sum += l2.val;
            l2 = l2.next;
        }
        
        carry = Math.floor(sum / 10);
        current.next = new ListNode(sum % 10);
        current = current.next;
    }
    
    return dummy.next;
}

// Clone linked list with random pointers
function cloneWithRandomPointer(head) {
    if (!head) return null;
    
    // Step 1: Create copy nodes
    let current = head;
    while (current) {
        let copy = new ListNode(current.val);
        copy.next = current.next;
        current.next = copy;
        current = copy.next;
    }
    
    // Step 2: Set random pointers
    current = head;
    while (current) {
        if (current.random) {
            current.next.random = current.random.next;
        }
        current = current.next.next;
    }
    
    // Step 3: Separate original and copy
    let dummy = new ListNode(0);
    let copyCurrent = dummy;
    current = head;
    
    while (current) {
        copyCurrent.next = current.next;
        current.next = current.next.next;
        current = current.next;
        copyCurrent = copyCurrent.next;
    }
    
    return dummy.next;
}

// Flatten multilevel doubly linked list
function flatten(head) {
    if (!head) return head;
    let stack = [];
    let current = head;
    
    while (current) {
        if (current.child) {
            if (current.next) {
                stack.push(current.next);
            }
            current.next = current.child;
            current.child.prev = current;
            current.child = null;
        }
        
        if (!current.next && stack.length > 0) {
            let next = stack.pop();
            current.next = next;
            next.prev = current;
        }
        
        current = current.next;
    }
    
    return head;
}
```

## 8. Time & Space Complexity Analysis

### Operation Complexities
| Operation | Singly LL | Doubly LL | Circular LL |
|-----------|-----------|-----------|-------------|
| Insert Head | O(1) | O(1) | O(1) |
| Insert Tail | O(n) / O(1)* | O(1) | O(1)* |
| Insert Middle | O(n) | O(n) | O(n) |
| Delete Head | O(1) | O(1) | O(1) |
| Delete Tail | O(n) | O(1) | O(n) / O(1)* |
| Delete Middle | O(n) | O(1)** | O(n) |
| Search | O(n) | O(n) | O(n) |
| Access by Index | O(n) | O(n) | O(n) |
| Reverse | O(n) | O(n) | O(n) |

*With tail pointer, **With node reference

### Space Complexities
- **Storage**: O(n) for singly/circular, O(2n) for doubly
- **Most algorithms**: O(1) auxiliary space
- **Recursive solutions**: O(n) call stack space
- **Sorting**: O(log n) for merge sort, O(1) for quick sort average

## 9. Common Interview Problems & Solutions

### Easy Level Problems

#### 1. Reverse Linked List (LeetCode 206)
```javascript
// Iterative approach - O(n) time, O(1) space
function reverseList(head) {
    let prev = null, current = head;
    while (current) {
        let next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    return prev;
}

// Recursive approach - O(n) time, O(n) space
function reverseListRecursive(head) {
    if (!head || !head.next) return head;
    
    let newHead = reverseListRecursive(head.next);
    head.next.next = head;
    head.next = null;
    return newHead;
}
```

#### 2. Middle of Linked List (LeetCode 876)
```javascript
function findMiddle(head) {
    let slow = head, fast = head;
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}

// If you need the node before middle for even length
function findMiddlePrev(head) {
    let slow = head, fast = head, prev = null;
    while (fast && fast.next) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    return prev;
}
```

#### 3. Merge Two Sorted Lists (LeetCode 21)
```javascript
function mergeTwoLists(l1, l2) {
    let dummy = new ListNode(0);
    let current = dummy;
    
    while (l1 && l2) {
        if (l1.val <= l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    current.next = l1 || l2;
    return dummy.next;
}

// Recursive approach
function mergeTwoListsRecursive(l1, l2) {
    if (!l1) return l2;
    if (!l2) return l1;
    
    if (l1.val <= l2.val) {
        l1.next = mergeTwoListsRecursive(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoListsRecursive(l1, l2.next);
        return l2;
    }
}
```

#### 4. Linked List Cycle (LeetCode 141)
```javascript
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

#### 5. Delete Node in Linked List (LeetCode 237)
```javascript
// Given only access to the node to be deleted
function deleteNode(node) {
    node.val = node.next.val;
    node.next = node.next.next;
}
```

#### 6. Remove Duplicates from Sorted List (LeetCode 83)
```javascript
function deleteDuplicates(head) {
    let current = head;
    
    while (current && current.next) {
        if (current.val === current.next.val) {
            current.next = current.next.next;
        } else {
            current = current.next;
        }
    }
    
    return head;
}
```

#### 7. Palindrome Linked List (LeetCode 234)
```javascript
function isPalindrome(head) {
    if (!head || !head.next) return true;
    
    // Find middle
    let slow = head, fast = head;
    while (fast.next && fast.next.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse second half
    let secondHalf = reverseList(slow.next);
    slow.next = null;
    
    // Compare
    let p1 = head, p2 = secondHalf;
    let result = true;
    
    while (p2) {
        if (p1.val !== p2.val) {
            result = false;
            break;
        }
        p1 = p1.next;
        p2 = p2.next;
    }
    
    // Restore original list (optional)
    slow.next = reverseList(secondHalf);
    
    return result;
}
```

### Medium Level Problems

#### 1. Remove Nth Node From End (LeetCode 19)
```javascript
function removeNthFromEnd(head, n) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let first = dummy, second = dummy;
    
    // Move first n+1 steps ahead
    for (let i = 0; i <= n; i++) {
        first = first.next;
    }
    
    // Move both until first reaches end
    while (first) {
        first = first.next;
        second = second.next;
    }
    
    second.next = second.next.next;
    return dummy.next;
}
```

#### 2. Add Two Numbers (LeetCode 2)
```javascript
function addTwoNumbers(l1, l2) {
    let dummy = new ListNode(0);
    let current = dummy;
    let carry = 0;
    
    while (l1 || l2 || carry) {
        let sum = carry;
        if (l1) {
            sum += l1.val;
            l1 = l1.next;
        }
        if (l2) {
            sum += l2.val;
            l2 = l2.next;
        }
        
        carry = Math.floor(sum / 10);
        current.next = new ListNode(sum % 10);
        current = current.next;
    }
    
    return dummy.next;
}
```

#### 3. Linked List Cycle II (LeetCode 142)
```javascript
function detectCycle(head) {
    let slow = head, fast = head;
    
    // Phase 1: Detect cycle
    while (fast && fast.next) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow === fast) break;
    }
    
    if (!fast || !fast.next) return null;
    
    // Phase 2: Find cycle start
    slow = head;
    while (slow !== fast) {
        slow = slow.next;
        fast = fast.next;
    }
    
    return slow;
}
```

#### 4. Reorder List (LeetCode 143)
```javascript
function reorderList(head) {
    if (!head || !head.next) return;
    
    // Find middle
    let slow = head, fast = head;
    while (fast.next && fast.next.next) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    // Reverse second half
    let secondHalf = reverseList(slow.next);
    slow.next = null;
    
    // Merge alternately
    let first = head, second = secondHalf;
    while (second) {
        let temp1 = first.next;
        let temp2 = second.next;
        first.next = second;
        second.next = temp1;
        first = temp1;
        second = temp2;
    }
}
```

#### 5. Swap Nodes in Pairs (LeetCode 24)
```javascript
function swapPairs(head) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let prev = dummy;
    
    while (prev.next && prev.next.next) {
        let first = prev.next;
        let second = prev.next.next;
        
        // Swap
        prev.next = second;
        first.next = second.next;
        second.next = first;
        
        prev = first;
    }
    
    return dummy.next;
}
```

#### 6. Odd Even Linked List (LeetCode 328)
```javascript
function oddEvenList(head) {
    if (!head || !head.next) return head;
    
    let odd = head;
    let even = head.next;
    let evenHead = even;
    
    while (even && even.next) {
        odd.next = even.next;
        odd = odd.next;
        even.next = odd.next;
        even = even.next;
    }
    
    odd.next = evenHead;
    return head;
}
```

#### 7. Partition List (LeetCode 86)
```javascript
function partition(head, x) {
    let smallHead = new ListNode(0);
    let largeHead = new ListNode(0);
    let small = smallHead, large = largeHead;
    
    while (head) {
        if (head.val < x) {
            small.next = head;
            small = small.next;
        } else {
            large.next = head;
            large = large.next;
        }
        head = head.next;
    }
    
    large.next = null;
    small.next = largeHead.next;
    return smallHead.next;
}
```

#### 8. Remove Duplicates from Sorted List II (LeetCode 82)
```javascript
function deleteDuplicates(head) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let prev = dummy;
    
    while (head) {
        if (head.next && head.val === head.next.val) {
            // Skip all nodes with same value
            while (head.next && head.val === head.next.val) {
                head = head.next;
            }
            prev.next = head.next;
        } else {
            prev = prev.next;
        }
        head = head.next;
    }
    
    return dummy.next;
}
```

#### 9. Rotate List (LeetCode 61)
```javascript
function rotateRight(head, k) {
    if (!head || !head.next || k === 0) return head;
    
    // Find length and make circular
    let length = 1;
    let tail = head;
    while (tail.next) {
        tail = tail.next;
        length++;
    }
    tail.next = head;
    
    // Find new tail
    k = k % length;
    let stepsToNewTail = length - k;
    let newTail = head;
    for (let i = 1; i < stepsToNewTail; i++) {
        newTail = newTail.next;
    }
    
    let newHead = newTail.next;
    newTail.next = null;
    
    return newHead;
}
```

#### 10. Copy List with Random Pointer (LeetCode 138)
```javascript
function copyRandomList(head) {
    if (!head) return null;
    
    let map = new Map();
    let current = head;
    
    // First pass: create nodes
    while (current) {
        map.set(current, new ListNode(current.val));
        current = current.next;
    }
    
    // Second pass: set pointers
    current = head;
    while (current) {
        if (current.next) {
            map.get(current).next = map.get(current.next);
        }
        if (current.random) {
            map.get(current).random = map.get(current.random);
        }
        current = current.next;
    }
    
    return map.get(head);
}

// Space optimized O(1) approach
function copyRandomListOptimized(head) {
    if (!head) return null;
    
    // Step 1: Create copy nodes
    let current = head;
    while (current) {
        let copy = new ListNode(current.val);
        copy.next = current.next;
        current.next = copy;
        current = copy.next;
    }
    
    // Step 2: Set random pointers
    current = head;
    while (current) {
        if (current.random) {
            current.next.random = current.random.next;
        }
        current = current.next.next;
    }
    
    // Step 3: Separate lists
    let dummy = new ListNode(0);
    let copyCurrent = dummy;
    current = head;
    
    while (current) {
        copyCurrent.next = current.next;
        current.next = current.next.next;
        current = current.next;
        copyCurrent = copyCurrent.next;
    }
    
    return dummy.next;
}
```

### Hard Level Problems

#### 1. Reverse Nodes in k-Group (LeetCode 25)
```javascript
function reverseKGroup(head, k) {
    // Check if we have k nodes
    let count = 0;
    let current = head;
    while (current && count < k) {
        current = current.next;
        count++;
    }
    
    if (count === k) {
        // Reverse next k nodes
        current = reverseKGroup(current, k);
        
        // Reverse current k nodes
        while (count-- > 0) {
            let next = head.next;
            head.next = current;
            current = head;
            head = next;
        }
        head = current;
    }
    
    return head;
}

// Iterative approach
function reverseKGroupIterative(head, k) {
    let dummy = new ListNode(0);
    dummy.next = head;
    let prevGroupEnd = dummy;
    
    while (true) {
        let kthNode = getKthNode(prevGroupEnd, k);
        if (!kthNode) break;
        
        let nextGroupStart = kthNode.next;
        
        // Reverse group
        let prev = nextGroupStart;
        let current = prevGroupEnd.next;
        
        while (current !== nextGroupStart) {
            let next = current.next;
            current.next = prev;
            prev = current;
            current = next;
        }
        
        let temp = prevGroupEnd.next;
        prevGroupEnd.next = kthNode;
        prevGroupEnd = temp;
    }
    
    return dummy.next;
}

function getKthNode(start, k) {
    while (start && k > 0) {
        start = start.next;
        k--;
    }
    return start;
}
```

#### 2. Merge k Sorted Lists (LeetCode 23)
```javascript
// Divide and conquer approach - O(n log k)
function mergeKLists(lists) {
    if (!lists || lists.length === 0) return null;
    
    while (lists.length > 1) {
        let mergedLists = [];
        
        for (let i = 0; i < lists.length; i += 2) {
            let l1 = lists[i];
            let l2 = i + 1 < lists.length ? lists[i + 1] : null;
            mergedLists.push(mergeTwoLists(l1, l2));
        }
        
        lists = mergedLists;
    }
    
    return lists[0];
}

// Priority queue approach using heap
function mergeKListsHeap(lists) {
    if (!lists || lists.length === 0) return null;
    
    // MinHeap implementation
    class MinHeap {
        constructor() {
            this.heap = [];
        }
        
        insert(node) {
            this.heap.push(node);
            this.bubbleUp(this.heap.length - 1);
        }
        
        extractMin() {
            if (this.heap.length === 0) return null;
            if (this.heap.length === 1) return this.heap.pop();
            
            let min = this.heap[0];
            this.heap[0] = this.heap.pop();
            this.bubbleDown(0);
            return min;
        }
        
        bubbleUp(index) {
            while (index > 0) {
                let parentIndex = Math.floor((index - 1) / 2);
                if (this.heap[parentIndex].val <= this.heap[index].val) break;
                
                [this.heap[parentIndex], this.heap[index]] = 
                [this.heap[index], this.heap[parentIndex]];
                index = parentIndex;
            }
        }
        
        bubbleDown(index) {
            while (true) {
                let minIndex = index;
                let leftChild = 2 * index + 1;
                let rightChild = 2 * index + 2;
                
                if (leftChild < this.heap.length && 
                    this.heap[leftChild].val < this.heap[minIndex].val) {
                    minIndex = leftChild;
                }
                
                if (rightChild < this.heap.length && 
                    this.heap[rightChild].val < this.heap[minIndex].val) {
                    minIndex = rightChild;
                }
                
                if (minIndex === index) break;
                
                [this.heap[index], this.heap[minIndex]] = 
                [this.heap[minIndex], this.heap[index]];
                index = minIndex;
            }
        }
        
        isEmpty() {
            return this.heap.length === 0;
        }
    }
    
    let heap = new MinHeap();
    
    // Add first node of each list to heap
    for (let list of lists) {
        if (list) heap.insert(list);
    }
    
    let dummy = new ListNode(0);
    let current = dummy;
    
    while (!heap.isEmpty()) {
        let node = heap.extractMin();
        current.next = node;
        current = current.next;
        
        if (node.next) {
            heap.insert(node.next);
        }
    }
    
    return dummy.next;
}
```

#### 3. Sort List (LeetCode 148)
```javascript
// Merge sort - O(n log n) time, O(log n) space
function sortList(head) {
    if (!head || !head.next) return head;
    
    // Split list into two halves
    let mid = getMid(head);
    let right = mid.next;
    mid.next = null;
    
    // Recursively sort both halves
    let left = sortList(head);
    right = sortList(right);
    
    // Merge sorted halves
    return mergeTwoLists(left, right);
}

function getMid(head) {
    let slow = head, fast = head, prev = null;
    while (fast && fast.next) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    return prev;
}

// Bottom-up merge sort - O(n log n) time, O(1) space
function sortListBottomUp(head) {
    if (!head || !head.next) return head;
    
    // Get length
    let length = 0;
    let current = head;
    while (current) {
        length++;
        current = current.next;
    }
    
    let dummy = new ListNode(0);
    dummy.next = head;
    
    for (let size = 1; size < length; size *= 2) {
        let prev = dummy;
        let curr = dummy.next;
        
        while (curr) {
            let left = curr;
            let right = split(left, size);
            curr = split(right, size);
            
            prev = merge(prev, left, right);
        }
    }
    
    return dummy.next;
}

function split(head, size) {
    while (--size && head) {
        head = head.next;
    }
    
    if (!head) return null;
    
    let next = head.next;
    head.next = null;
    return next;
}

function merge(prev, l1, l2) {
    let curr = prev;
    
    while (l1 && l2) {
        if (l1.val <= l2.val) {
            curr.next = l1;
            l1 = l1.next;
        } else {
            curr.next = l2;
            l2 = l2.next;
        }
        curr = curr.next;
    }
    
    curr.next = l1 || l2;
    
    while (curr.next) {
        curr = curr.next;
    }
    
    return curr;
}
```

#### 4. LRU Cache (LeetCode 146)
```javascript
class LRUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.cache = new Map();
        
        // Create dummy head and tail nodes
        this.head = new DoublyListNode(-1, -1);
        this.tail = new DoublyListNode(-1, -1);
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }
    
    addNode(node) {
        node.prev = this.head;
        node.next = this.head.next;
        this.head.next.prev = node;
        this.head.next = node;
    }
    
    removeNode(node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    
    moveToHead(node) {
        this.removeNode(node);
        this.addNode(node);
    }
    
    popTail() {
        let last = this.tail.prev;
        this.removeNode(last);
        return last;
    }
    
    get(key) {
        let node = this.cache.get(key);
        if (node) {
            this.moveToHead(node);
            return node.val;
        }
        return -1;
    }
    
    put(key, value) {
        let node = this.cache.get(key);
        
        if (node) {
            node.val = value;
            this.moveToHead(node);
        } else {
            let newNode = new DoublyListNode(key, value);
            
            if (this.cache.size >= this.capacity) {
                let tail = this.popTail();
                this.cache.delete(tail.key);
            }
            
            this.addNode(newNode);
            this.cache.set(key, newNode);
        }
    }
}

class DoublyListNode {
    constructor(key, val) {
        this.key = key;
        this.val = val;
        this.prev = null;
        this.next = null;
    }
}
```

#### 5. LFU Cache (LeetCode 460)
```javascript
class LFUCache {
    constructor(capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.minFreq = 0;
        this.keyToNode = new Map();
        this.freqToList = new Map();
    }
    
    get(key) {
        let node = this.keyToNode.get(key);
        if (!node) return -1;
        
        this.updateFreq(node);
        return node.val;
    }
    
    put(key, value) {
        if (this.capacity === 0) return;
        
        let node = this.keyToNode.get(key);
        if (node) {
            node.val = value;
            this.updateFreq(node);
        } else {
            if (this.size >= this.capacity) {
                this.removeMinFreqNode();
            }
            
            let newNode = new LFUNode(key, value);
            this.keyToNode.set(key, newNode);
            this.addToFreqList(newNode);
            this.minFreq = 1;
            this.size++;
        }
    }
    
    updateFreq(node) {
        let oldFreq = node.freq;
        let newFreq = oldFreq + 1;
        
        this.removeFromFreqList(node);
        node.freq = newFreq;
        this.addToFreqList(node);
        
        if (this.minFreq === oldFreq && 
            (!this.freqToList.has(oldFreq) || 
             this.freqToList.get(oldFreq).isEmpty())) {
            this.minFreq++;
        }
    }
    
    removeMinFreqNode() {
        let list = this.freqToList.get(this.minFreq);
        let nodeToRemove = list.removeLast();
        this.keyToNode.delete(nodeToRemove.key);
        this.size--;
    }
    
    addToFreqList(node) {
        if (!this.freqToList.has(node.freq)) {
            this.freqToList.set(node.freq, new DoublyLinkedList());
        }
        this.freqToList.get(node.freq).addFirst(node);
    }
    
    removeFromFreqList(node) {
        this.freqToList.get(node.freq).remove(node);
    }
}

class LFUNode {
    constructor(key, val) {
        this.key = key;
        this.val = val;
        this.freq = 1;
        this.prev = null;
        this.next = null;
    }
}
```

#### 6. All O(1) Data Structure (LeetCode 432)
```javascript
class AllOne {
    constructor() {
        this.keyToNode = new Map();
        this.head = new BucketNode(0);
        this.tail = new BucketNode(0);
        this.head.next = this.tail;
        this.tail.prev = this.head;
    }
    
    inc(key) {
        if (this.keyToNode.has(key)) {
            this.changeKey(key, 1);
        } else {
            if (this.head.next.count !== 1) {
                this.addBucketAfter(new BucketNode(1), this.head);
            }
            this.head.next.keys.add(key);
            this.keyToNode.set(key, this.head.next);
        }
    }
    
    dec(key) {
        if (this.keyToNode.has(key)) {
            this.changeKey(key, -1);
        }
    }
    
    getMaxKey() {
        return this.tail.prev === this.head ? "" : 
               this.tail.prev.keys.values().next().value;
    }
    
    getMinKey() {
        return this.head.next === this.tail ? "" : 
               this.head.next.keys.values().next().value;
    }
    
    changeKey(key, delta) {
        let bucket = this.keyToNode.get(key);
        let newCount = bucket.count + delta;
        
        bucket.keys.delete(key);
        
        if (newCount === 0) {
            this.keyToNode.delete(key);
        } else {
            let newBucket;
            if (delta === 1) {
                newBucket = bucket.next.count === newCount ? 
                           bucket.next : 
                           this.addBucketAfter(new BucketNode(newCount), bucket);
            } else {
                newBucket = bucket.prev.count === newCount ? 
                           bucket.prev : 
                           this.addBucketAfter(new BucketNode(newCount), bucket.prev);
            }
            
            newBucket.keys.add(key);
            this.keyToNode.set(key, newBucket);
        }
        
        if (bucket.keys.size === 0) {
            this.removeBucket(bucket);
        }
    }
    
    addBucketAfter(newBucket, prevBucket) {
        newBucket.prev = prevBucket;
        newBucket.next = prevBucket.next;
        prevBucket.next.prev = newBucket;
        prevBucket.next = newBucket;
        return newBucket;
    }
    
    removeBucket(bucket) {
        bucket.prev.next = bucket.next;
        bucket.next.prev = bucket.prev;
    }
}

class BucketNode {
    constructor(count) {
        this.count = count;
        this.keys = new Set();
        this.prev = null;
        this.next = null;
    }
}
```

### Problem Pattern Summary

#### Two Pointer Patterns
- **Fast & Slow**: Cycle detection, middle finding
- **Distance-based**: Nth from end, intersection
- **Meeting point**: List intersection

#### Dummy Node Patterns
- **Head changes**: Deletion, insertion, reversal
- **Partitioning**: Split lists based on conditions
- **Merging**: Combine multiple lists

#### Reversal Patterns
- **Complete reversal**: Reverse entire list
- **Partial reversal**: Reverse in groups
- **Conditional reversal**: Reverse based on conditions

#### Sorting Patterns
- **Merge sort**: Divide and conquer
- **Quick sort**: Partition-based
- **Heap sort**: Priority queue based
