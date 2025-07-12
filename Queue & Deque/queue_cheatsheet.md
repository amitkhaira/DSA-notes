# Complete Queue & Deque Cheat Sheet (JavaScript) - Interview Edition

## 1. Queue Fundamentals

### Definition & Characteristics
- **FIFO**: First In, First Out principle
- **Linear Data Structure**: Elements arranged in sequence
- **Two Pointers**: Front (head) and Rear (tail)
- **Main Operations**: Enqueue (insert), Dequeue (remove)

### Types of Queues
1. **Simple Queue**: Basic FIFO queue
2. **Circular Queue**: Last position connects to first
3. **Priority Queue**: Elements have priorities
4. **Double-ended Queue (Deque)**: Insert/delete from both ends

## 2. Queue Operations & Time Complexities

| Operation | Description | Time | Space |
|-----------|-------------|------|-------|
| enqueue() | Add element to rear | O(1) | O(1) |
| dequeue() | Remove element from front | O(1) | O(1) |
| front/peek() | Get front element | O(1) | O(1) |
| rear() | Get rear element | O(1) | O(1) |
| isEmpty() | Check if queue is empty | O(1) | O(1) |
| isFull() | Check if queue is full | O(1) | O(1) |
| size() | Get number of elements | O(1) | O(1) |

## 3. Queue Implementations

### 3.1 Array-based Queue (Simple but Inefficient)
```js
class SimpleQueue {
    constructor() {
        this.items = [];
    }
    
    enqueue(element) {
        this.items.push(element);
    }
    
    dequeue() {
        if (this.isEmpty()) return null;
        return this.items.shift(); // O(n) - shifts all elements
    }
    
    front() {
        return this.isEmpty() ? null : this.items[0];
    }
    
    rear() {
        return this.isEmpty() ? null : this.items[this.items.length - 1];
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
    
    display() {
        return this.items.toString();
    }
}
```

### 3.2 Efficient Queue (Object-based with Pointers)
```js
class Queue {
    constructor() {
        this.items = {};
        this.head = 0;
        this.tail = 0;
    }
    
    enqueue(element) {
        this.items[this.tail] = element;
        this.tail++;
    }
    
    dequeue() {
        if (this.isEmpty()) return null;
        const item = this.items[this.head];
        delete this.items[this.head];
        this.head++;
        return item;
    }
    
    front() {
        return this.isEmpty() ? null : this.items[this.head];
    }
    
    rear() {
        return this.isEmpty() ? null : this.items[this.tail - 1];
    }
    
    isEmpty() {
        return this.head === this.tail;
    }
    
    size() {
        return this.tail - this.head;
    }
    
    clear() {
        this.items = {};
        this.head = 0;
        this.tail = 0;
    }
    
    toArray() {
        const result = [];
        for (let i = this.head; i < this.tail; i++) {
            result.push(this.items[i]);
        }
        return result;
    }
}
```

### 3.3 Circular Queue
```js
class CircularQueue {
    constructor(capacity) {
        this.items = new Array(capacity);
        this.capacity = capacity;
        this.head = 0;
        this.tail = 0;
        this.count = 0;
    }
    
    enqueue(element) {
        if (this.isFull()) return false;
        this.items[this.tail] = element;
        this.tail = (this.tail + 1) % this.capacity;
        this.count++;
        return true;
    }
    
    dequeue() {
        if (this.isEmpty()) return null;
        const item = this.items[this.head];
        this.items[this.head] = undefined;
        this.head = (this.head + 1) % this.capacity;
        this.count--;
        return item;
    }
    
    front() {
        return this.isEmpty() ? null : this.items[this.head];
    }
    
    rear() {
        const index = this.tail === 0 ? this.capacity - 1 : this.tail - 1;
        return this.isEmpty() ? null : this.items[index];
    }
    
    isEmpty() {
        return this.count === 0;
    }
    
    isFull() {
        return this.count === this.capacity;
    }
    
    size() {
        return this.count;
    }
}
```

## 4. Deque (Double-ended Queue)

### 4.1 Complete Deque Implementation
```js
class Deque {
    constructor() {
        this.items = {};
        this.head = 0;
        this.tail = 0;
    }
    
    // Add to front
    addFront(element) {
        this.head--;
        this.items[this.head] = element;
    }
    
    // Add to rear
    addRear(element) {
        this.items[this.tail] = element;
        this.tail++;
    }
    
    // Remove from front
    removeFront() {
        if (this.isEmpty()) return null;
        const item = this.items[this.head];
        delete this.items[this.head];
        this.head++;
        return item;
    }
    
    // Remove from rear
    removeRear() {
        if (this.isEmpty()) return null;
        this.tail--;
        const item = this.items[this.tail];
        delete this.items[this.tail];
        return item;
    }
    
    // Peek operations
    peekFront() {
        return this.isEmpty() ? null : this.items[this.head];
    }
    
    peekRear() {
        return this.isEmpty() ? null : this.items[this.tail - 1];
    }
    
    isEmpty() {
        return this.head === this.tail;
    }
    
    size() {
        return this.tail - this.head;
    }
    
    clear() {
        this.items = {};
        this.head = 0;
        this.tail = 0;
    }
    
    toArray() {
        const result = [];
        for (let i = this.head; i < this.tail; i++) {
            result.push(this.items[i]);
        }
        return result;
    }
}
```

### 4.2 Deque Operations
| Operation | Time | Description |
|-----------|------|-------------|
| addFront() | O(1) | Add element to front |
| addRear() | O(1) | Add element to rear |
| removeFront() | O(1) | Remove from front |
| removeRear() | O(1) | Remove from rear |
| peekFront() | O(1) | View front element |
| peekRear() | O(1) | View rear element |

## 5. Priority Queue
```js
class PriorityQueue {
    constructor() {
        this.items = [];
    }
    
    enqueue(element, priority) {
        const queueElement = { element, priority };
        let added = false;
        
        for (let i = 0; i < this.items.length; i++) {
            if (queueElement.priority < this.items[i].priority) {
                this.items.splice(i, 0, queueElement);
                added = true;
                break;
            }
        }
        
        if (!added) {
            this.items.push(queueElement);
        }
    }
    
    dequeue() {
        return this.isEmpty() ? null : this.items.shift().element;
    }
    
    front() {
        return this.isEmpty() ? null : this.items[0].element;
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
}
```

---

# 📋 INTERVIEW QUESTIONS BY DIFFICULTY LEVEL

## 🟢 Easy Level Questions

### 1. Implement Queue using Array ⭐⭐⭐⭐⭐
**Problem**: Implement a queue using JavaScript array with basic operations.

**Solution**:
```js
class Queue {
    constructor() {
        this.items = [];
    }
    
    enqueue(element) {
        this.items.push(element);
    }
    
    dequeue() {
        return this.items.shift();
    }
    
    front() {
        return this.items[0];
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
}
```

### 2. Binary Tree Level Order Traversal ⭐⭐⭐⭐⭐
**Problem**: Given a binary tree, return the level order traversal of its nodes' values.

**Solution**:
```js
function levelOrder(root) {
    if (!root) return [];
    
    const queue = [root];
    const result = [];
    
    while (queue.length > 0) {
        const levelSize = queue.length;
        const level = [];
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift();
            level.push(node.val);
            
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        
        result.push(level);
    }
    
    return result;
}
```

### 3. Generate Binary Numbers ⭐⭐⭐⭐
**Problem**: Generate binary numbers from 1 to n using a queue.

**Solution**:
```js
function generateBinaryNumbers(n) {
    const queue = ["1"];
    const result = [];
    
    for (let i = 0; i < n; i++) {
        const binary = queue.shift();
        result.push(binary);
        
        queue.push(binary + "0");
        queue.push(binary + "1");
    }
    
    return result;
}
```

### 4. Implement Stack using Queue ⭐⭐⭐⭐
**Problem**: Implement a stack using only queue operations.

**Solution**:
```js
class StackUsingQueue {
    constructor() {
        this.queue = [];
    }
    
    push(element) {
        this.queue.push(element);
        
        // Rotate queue to maintain stack order
        for (let i = 0; i < this.queue.length - 1; i++) {
            this.queue.push(this.queue.shift());
        }
    }
    
    pop() {
        return this.queue.shift();
    }
    
    top() {
        return this.queue[0];
    }
    
    empty() {
        return this.queue.length === 0;
    }
}
```

### 5. Queue Reversal ⭐⭐⭐
**Problem**: Reverse a queue using only queue operations.

**Solution**:
```js
function reverseQueue(queue) {
    const stack = [];
    
    // Transfer all elements to stack
    while (!queue.isEmpty()) {
        stack.push(queue.dequeue());
    }
    
    // Transfer back to queue
    while (stack.length > 0) {
        queue.enqueue(stack.pop());
    }
    
    return queue;
}
```

## 🟡 Medium Level Questions

### 1. Sliding Window Maximum ⭐⭐⭐⭐⭐
**Problem**: Find maximum element in every sliding window of size k.

**Solution**:
```js
function maxSlidingWindow(nums, k) {
    const deque = [];
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        // Remove elements outside current window
        while (deque.length > 0 && deque[0] < i - k + 1) {
            deque.shift();
        }
        
        // Remove smaller elements from rear
        while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
            deque.pop();
        }
        
        deque.push(i);
        
        // Add maximum to result when window is complete
        if (i >= k - 1) {
            result.push(nums[deque[0]]);
        }
    }
    
    return result;
}
```

### 2. First Non-Repeating Character in Stream ⭐⭐⭐⭐⭐
**Problem**: Find first non-repeating character in a stream of characters.

**Solution**:
```js
function firstNonRepeating(stream) {
    const queue = [];
    const charCount = {};
    const result = [];
    
    for (let char of stream) {
        charCount[char] = (charCount[char] || 0) + 1;
        queue.push(char);
        
        // Remove characters that are no longer non-repeating
        while (queue.length > 0 && charCount[queue[0]] > 1) {
            queue.shift();
        }
        
        result.push(queue.length === 0 ? '#' : queue[0]);
    }
    
    return result;
}
```

### 3. Rotting Oranges ⭐⭐⭐⭐⭐
**Problem**: Find minimum time to rot all oranges using BFS.

**Solution**:
```js
function orangesRotting(grid) {
    const m = grid.length, n = grid[0].length;
    const queue = [];
    let freshCount = 0;
    
    // Find all rotten oranges and count fresh ones
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 2) {
                queue.push([i, j]);
            } else if (grid[i][j] === 1) {
                freshCount++;
            }
        }
    }
    
    if (freshCount === 0) return 0;
    
    const directions = [[0, 1], [0, -1], [1, 0], [-1, 0]];
    let minutes = 0;
    
    while (queue.length > 0) {
        const size = queue.length;
        
        for (let i = 0; i < size; i++) {
            const [row, col] = queue.shift();
            
            for (const [dr, dc] of directions) {
                const newRow = row + dr;
                const newCol = col + dc;
                
                if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n && 
                    grid[newRow][newCol] === 1) {
                    grid[newRow][newCol] = 2;
                    queue.push([newRow, newCol]);
                    freshCount--;
                }
            }
        }
        
        if (queue.length > 0) minutes++;
    }
    
    return freshCount === 0 ? minutes : -1;
}
```

### 4. Design Hit Counter ⭐⭐⭐⭐
**Problem**: Design a hit counter which counts hits in the past 5 minutes.

**Solution**:
```js
class HitCounter {
    constructor() {
        this.queue = [];
    }
    
    hit(timestamp) {
        this.queue.push(timestamp);
    }
    
    getHits(timestamp) {
        // Remove hits older than 5 minutes (300 seconds)
        while (this.queue.length > 0 && this.queue[0] <= timestamp - 300) {
            this.queue.shift();
        }
        
        return this.queue.length;
    }
}
```

### 5. Circular Array Loop ⭐⭐⭐⭐
**Problem**: Detect if there exists a loop in a circular array.

**Solution**:
```js
function circularArrayLoop(nums) {
    const n = nums.length;
    
    for (let i = 0; i < n; i++) {
        if (nums[i] === 0) continue;
        
        let slow = i, fast = i;
        const positive = nums[i] > 0;
        
        do {
            slow = nextIndex(nums, slow, positive);
            fast = nextIndex(nums, fast, positive);
            if (fast !== -1) {
                fast = nextIndex(nums, fast, positive);
            }
        } while (slow !== -1 && fast !== -1 && slow !== fast);
        
        if (slow !== -1 && slow === fast) {
            return true;
        }
    }
    
    return false;
}

function nextIndex(nums, index, positive) {
    const direction = nums[index] > 0;
    if (direction !== positive) return -1;
    
    const nextIdx = (index + nums[index]) % nums.length;
    if (nextIdx < 0) nextIdx += nums.length;
    
    return nextIdx === index ? -1 : nextIdx;
}
```

## 🔴 Hard Level Questions

### 1. Shortest Path in Binary Matrix ⭐⭐⭐⭐⭐
**Problem**: Find shortest path from top-left to bottom-right in binary matrix.

**Solution**:
```js
function shortestPathBinaryMatrix(grid) {
    const n = grid.length;
    if (grid[0][0] === 1 || grid[n-1][n-1] === 1) return -1;
    
    const queue = [[0, 0, 1]]; // [row, col, distance]
    const visited = Array(n).fill().map(() => Array(n).fill(false));
    visited[0][0] = true;
    
    const directions = [
        [-1, -1], [-1, 0], [-1, 1],
        [0, -1],           [0, 1],
        [1, -1],  [1, 0],  [1, 1]
    ];
    
    while (queue.length > 0) {
        const [row, col, dist] = queue.shift();
        
        if (row === n - 1 && col === n - 1) {
            return dist;
        }
        
        for (const [dr, dc] of directions) {
            const newRow = row + dr;
            const newCol = col + dc;
            
            if (newRow >= 0 && newRow < n && newCol >= 0 && newCol < n && 
                grid[newRow][newCol] === 0 && !visited[newRow][newCol]) {
                visited[newRow][newCol] = true;
                queue.push([newRow, newCol, dist + 1]);
            }
        }
    }
    
    return -1;
}
```

### 2. Sliding Window Median ⭐⭐⭐⭐⭐
**Problem**: Find median of all sliding windows of size k.

**Solution**:
```js
function medianSlidingWindow(nums, k) {
    const result = [];
    
    for (let i = 0; i <= nums.length - k; i++) {
        const window = nums.slice(i, i + k).sort((a, b) => a - b);
        const median = k % 2 === 1 ? 
            window[Math.floor(k / 2)] : 
            (window[k / 2 - 1] + window[k / 2]) / 2;
        result.push(median);
    }
    
    return result;
}
```

### 3. Shortest Subarray with Sum at Least K ⭐⭐⭐⭐⭐
**Problem**: Find shortest subarray with sum at least K using deque.

**Solution**:
```js
function shortestSubarray(nums, k) {
    const n = nums.length;
    const prefixSum = new Array(n + 1).fill(0);
    
    for (let i = 0; i < n; i++) {
        prefixSum[i + 1] = prefixSum[i] + nums[i];
    }
    
    const deque = [];
    let result = n + 1;
    
    for (let i = 0; i <= n; i++) {
        while (deque.length > 0 && prefixSum[i] - prefixSum[deque[0]] >= k) {
            result = Math.min(result, i - deque.shift());
        }
        
        while (deque.length > 0 && prefixSum[i] <= prefixSum[deque[deque.length - 1]]) {
            deque.pop();
        }
        
        deque.push(i);
    }
    
    return result === n + 1 ? -1 : result;
}
```

### 4. Constrained Subsequence Sum ⭐⭐⭐⭐⭐
**Problem**: Find maximum sum of non-empty subsequence with constraint.

**Solution**:
```js
function constrainedSubsetSum(nums, k) {
    const n = nums.length;
    const dp = new Array(n);
    const deque = [];
    
    for (let i = 0; i < n; i++) {
        // Remove elements outside window
        while (deque.length > 0 && deque[0] < i - k) {
            deque.shift();
        }
        
        dp[i] = nums[i] + (deque.length > 0 ? Math.max(0, dp[deque[0]]) : 0);
        
        // Maintain decreasing order in deque
        while (deque.length > 0 && dp[deque[deque.length - 1]] <= dp[i]) {
            deque.pop();
        }
        
        deque.push(i);
    }
    
    return Math.max(...dp);
}
```

### 5. Find Median from Data Stream ⭐⭐⭐⭐⭐
**Problem**: Design data structure to find median from stream efficiently.

**Solution**:
```js
class MedianFinder {
    constructor() {
        this.small = []; // max heap (simulated with array)
        this.large = []; // min heap (simulated with array)
    }
    
    addNum(num) {
        if (this.small.length === 0 || num <= this.small[0]) {
            this.small.push(num);
            this.small.sort((a, b) => b - a); // max heap
        } else {
            this.large.push(num);
            this.large.sort((a, b) => a - b); // min heap
        }
        
        // Balance heaps
        if (this.small.length > this.large.length + 1) {
            this.large.push(this.small.shift());
            this.large.sort((a, b) => a - b);
        } else if (this.large.length > this.small.length + 1) {
            this.small.push(this.large.shift());
            this.small.sort((a, b) => b - a);
        }
    }
    
    findMedian() {
        if (this.small.length === this.large.length) {
            return (this.small[0] + this.large[0]) / 2;
        }
        return this.small.length > this.large.length ? this.small[0] : this.large[0];
    }
}
```

---

## Applications & Use Cases

### Queue Applications
- **Process Scheduling**: CPU scheduling, job queues
- **Breadth-First Search**: Graph/tree traversal
- **Buffering**: I/O buffers, print queues
- **Handling Requests**: Web servers, API rate limiting
- **Level Order Processing**: Tree operations
- **Simulation**: Modeling real-world queues

### Deque Applications
- **Sliding Window Problems**: Maximum/minimum in window
- **Palindrome Checking**: Add/remove from both ends
- **Undo/Redo Operations**: Browser history
- **Work Stealing**: Parallel processing
- **Caching**: LRU cache implementation

### Priority Queue Applications
- **Dijkstra's Algorithm**: Shortest path
- **Huffman Coding**: Data compression
- **A* Search Algorithm**: Pathfinding
- **Task Scheduling**: Priority-based scheduling
- **Merge K Sorted Lists**: Efficient merging

## Tips & Best Practices

### Performance Tips
- Use object-based implementation for better performance
- Avoid array.shift() for large queues (O(n) complexity)
- Consider circular queue for fixed-size scenarios
- Use deque when you need operations at both ends

### Memory Management
- Clean up references when dequeuing
- Use circular queue to avoid memory leaks
- Consider capacity limits for long-running applications

### Error Handling
```js
// Always check for empty queue
if (queue.isEmpty()) {
    throw new Error("Queue is empty");
}

// Handle null/undefined gracefully
const element = queue.dequeue() || defaultValue;
```

## Complexity Summary

| Implementation | Enqueue | Dequeue | Space |
|----------------|---------|---------|-------|
| Array (simple) | O(1) | O(n) | O(n) |
| Object-based | O(1) | O(1) | O(n) |
| Circular Array | O(1) | O(1) | O(k) |
| Linked List | O(1) | O(1) | O(n) |

**Note**: k = capacity for circular queue, n = number of elements

## 🎯 Interview Success Tips

1. **Always clarify requirements** - Ask about constraints, edge cases
2. **Start with brute force** - Then optimize with queue/deque
3. **Explain time/space complexity** - Justify your choice of data structure
4. **Handle edge cases** - Empty queues, single elements, null inputs
5. **Code cleanly** - Use meaningful variable names and comments
6. **Test with examples** - Walk through your solution step by step
