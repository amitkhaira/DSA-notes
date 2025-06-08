# Complete Queue & Deque Cheat Sheet (JavaScript)

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

## 6. Common Coding Patterns & Problems

### 6.1 Breadth-First Search (BFS)
```js
function bfs(graph, start) {
    const queue = new Queue();
    const visited = new Set();
    const result = [];
    
    queue.enqueue(start);
    visited.add(start);
    
    while (!queue.isEmpty()) {
        const node = queue.dequeue();
        result.push(node);
        
        for (let neighbor of graph[node] || []) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.enqueue(neighbor);
            }
        }
    }
    
    return result;
}
```

### 6.2 Level Order Tree Traversal
```js
function levelOrder(root) {
    if (!root) return [];
    
    const queue = new Queue();
    const result = [];
    
    queue.enqueue(root);
    
    while (!queue.isEmpty()) {
        const levelSize = queue.size();
        const level = [];
        
        for (let i = 0; i < levelSize; i++) {
            const node = queue.dequeue();
            level.push(node.val);
            
            if (node.left) queue.enqueue(node.left);
            if (node.right) queue.enqueue(node.right);
        }
        
        result.push(level);
    }
    
    return result;
}
```

### 6.3 Sliding Window Maximum (Deque)
```js
function maxSlidingWindow(nums, k) {
    const deque = new Deque();
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        // Remove elements outside current window
        while (!deque.isEmpty() && deque.peekFront() < i - k + 1) {
            deque.removeFront();
        }
        
        // Remove smaller elements from rear
        while (!deque.isEmpty() && nums[deque.peekRear()] < nums[i]) {
            deque.removeRear();
        }
        
        deque.addRear(i);
        
        // Add maximum to result when window is complete
        if (i >= k - 1) {
            result.push(nums[deque.peekFront()]);
        }
    }
    
    return result;
}
```

### 6.4 Generate Binary Numbers
```js
function generateBinaryNumbers(n) {
    const queue = new Queue();
    const result = [];
    
    queue.enqueue("1");
    
    for (let i = 0; i < n; i++) {
        const binary = queue.dequeue();
        result.push(binary);
        
        queue.enqueue(binary + "0");
        queue.enqueue(binary + "1");
    }
    
    return result;
}
```

### 6.5 First Non-Repeating Character in Stream
```js
function firstNonRepeating(stream) {
    const queue = new Queue();
    const charCount = {};
    const result = [];
    
    for (let char of stream) {
        charCount[char] = (charCount[char] || 0) + 1;
        queue.enqueue(char);
        
        // Remove characters that are no longer non-repeating
        while (!queue.isEmpty() && charCount[queue.front()] > 1) {
            queue.dequeue();
        }
        
        result.push(queue.isEmpty() ? '#' : queue.front());
    }
    
    return result;
}
```

### 6.6 Implement Stack using Queues
```js
class StackUsingQueues {
    constructor() {
        this.q1 = new Queue();
        this.q2 = new Queue();
    }
    
    push(element) {
        this.q2.enqueue(element);
        
        while (!this.q1.isEmpty()) {
            this.q2.enqueue(this.q1.dequeue());
        }
        
        [this.q1, this.q2] = [this.q2, this.q1];
    }
    
    pop() {
        return this.q1.dequeue();
    }
    
    top() {
        return this.q1.front();
    }
    
    empty() {
        return this.q1.isEmpty();
    }
}
```

## 7. JavaScript Built-in Methods

### Array as Queue (Less Efficient)
```js
const queue = [];

// Enqueue
queue.push(element);

// Dequeue
const element = queue.shift(); // O(n) operation

// Front
const front = queue[0];

// Size
const size = queue.length;

// Empty check
const isEmpty = queue.length === 0;
```

### Using Array as Deque
```js
const deque = [];

// Add to front/rear
deque.unshift(element); // Add to front
deque.push(element);    // Add to rear

// Remove from front/rear
const front = deque.shift(); // Remove from front
const rear = deque.pop();    // Remove from rear
```

## 8. Applications & Use Cases

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

## 9. Common Interview Questions

1. **Implement queue using stacks**
2. **Implement stack using queues**
3. **Design circular queue**
4. **Find first non-repeating character**
5. **Sliding window maximum**
6. **Level order traversal**
7. **Generate binary numbers**
8. **Implement LRU cache using deque**
9. **Shortest path in binary matrix**
10. **Design hit counter**

## 10. Tips & Best Practices

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

## 11. Complexity Summary

| Implementation | Enqueue | Dequeue | Space |
|----------------|---------|---------|-------|
| Array (simple) | O(1) | O(n) | O(n) |
| Object-based | O(1) | O(1) | O(n) |
| Circular Array | O(1) | O(1) | O(k) |
| Linked List | O(1) | O(1) | O(n) |

**Note**: k = capacity for circular queue, n = number of elements
