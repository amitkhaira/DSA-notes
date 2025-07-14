# Heap Data Structure - Interview Cheatsheet (JavaScript)

## 📚 What is a Heap?
A heap is a specialized tree-based data structure that satisfies the heap property:
- **Max Heap**: Parent node ≥ all child nodes
- **Min Heap**: Parent node ≤ all child nodes

## 🔧 Key Properties
- **Complete Binary Tree**: All levels filled except possibly the last level
- **Heap Property**: Maintains order between parent and children
- **Array Representation**: Can be efficiently stored in an array
- **Time Complexity**: Insert/Delete O(log n), Find Min/Max O(1)

## 🧠 Important Heap Concepts

### 1. Array Representation
For node at index `i`:
```javascript
// Parent index
const parent = (i) => Math.floor((i - 1) / 2);

// Left child index
const leftChild = (i) => 2 * i + 1;

// Right child index
const rightChild = (i) => 2 * i + 2;
```

### 2. Heapify Operations
```javascript
// Heapify Up (for insertion)
function heapifyUp(heap, index) {
    while (index > 0) {
        const parentIndex = Math.floor((index - 1) / 2);
        if (heap[index] <= heap[parentIndex]) break;
        
        [heap[index], heap[parentIndex]] = [heap[parentIndex], heap[index]];
        index = parentIndex;
    }
}

// Heapify Down (for deletion)
function heapifyDown(heap, index) {
    const length = heap.length;
    
    while (true) {
        let largest = index;
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        
        if (left < length && heap[left] > heap[largest]) {
            largest = left;
        }
        
        if (right < length && heap[right] > heap[largest]) {
            largest = right;
        }
        
        if (largest === index) break;
        
        [heap[index], heap[largest]] = [heap[largest], heap[index]];
        index = largest;
    }
}
```

### 3. Custom Heap Implementation
```javascript
class MaxHeap {
    constructor() {
        this.heap = [];
    }
    
    insert(value) {
        this.heap.push(value);
        this.heapifyUp(this.heap.length - 1);
    }
    
    extractMax() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();
        
        const max = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.heapifyDown(0);
        return max;
    }
    
    peek() {
        return this.heap[0] || null;
    }
    
    size() {
        return this.heap.length;
    }
    
    heapifyUp(index) {
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.heap[index] <= this.heap[parentIndex]) break;
            
            [this.heap[index], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[index]];
            index = parentIndex;
        }
    }
    
    heapifyDown(index) {
        while (true) {
            let largest = index;
            const left = 2 * index + 1;
            const right = 2 * index + 2;
            
            if (left < this.heap.length && this.heap[left] > this.heap[largest]) {
                largest = left;
            }
            
            if (right < this.heap.length && this.heap[right] > this.heap[largest]) {
                largest = right;
            }
            
            if (largest === index) break;
            
            [this.heap[index], this.heap[largest]] = [this.heap[largest], this.heap[index]];
            index = largest;
        }
    }
}
```

### 4. Priority Queue Implementation
```javascript
class PriorityQueue {
    constructor(compareFn = (a, b) => a - b) {
        this.heap = [];
        this.compare = compareFn;
    }
    
    enqueue(item) {
        this.heap.push(item);
        this.heapifyUp(this.heap.length - 1);
    }
    
    dequeue() {
        if (this.heap.length === 0) return null;
        if (this.heap.length === 1) return this.heap.pop();
        
        const root = this.heap[0];
        this.heap[0] = this.heap.pop();
        this.heapifyDown(0);
        return root;
    }
    
    peek() {
        return this.heap[0] || null;
    }
    
    size() {
        return this.heap.length;
    }
    
    heapifyUp(index) {
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            if (this.compare(this.heap[index], this.heap[parentIndex]) >= 0) break;
            
            [this.heap[index], this.heap[parentIndex]] = [this.heap[parentIndex], this.heap[index]];
            index = parentIndex;
        }
    }
    
    heapifyDown(index) {
        while (true) {
            let smallest = index;
            const left = 2 * index + 1;
            const right = 2 * index + 2;
            
            if (left < this.heap.length && this.compare(this.heap[left], this.heap[smallest]) < 0) {
                smallest = left;
            }
            
            if (right < this.heap.length && this.compare(this.heap[right], this.heap[smallest]) < 0) {
                smallest = right;
            }
            
            if (smallest === index) break;
            
            [this.heap[index], this.heap[smallest]] = [this.heap[smallest], this.heap[index]];
            index = smallest;
        }
    }
}
```

### 5. Build Heap from Array (O(n) time)
```javascript
function buildMaxHeap(arr) {
    const n = arr.length;
    
    // Start from last non-leaf node
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapifyDown(arr, i, n);
    }
    
    return arr;
}

function heapifyDown(arr, index, heapSize) {
    while (true) {
        let largest = index;
        const left = 2 * index + 1;
        const right = 2 * index + 2;
        
        if (left < heapSize && arr[left] > arr[largest]) {
            largest = left;
        }
        
        if (right < heapSize && arr[right] > arr[largest]) {
            largest = right;
        }
        
        if (largest === index) break;
        
        [arr[index], arr[largest]] = [arr[largest], arr[index]];
        index = largest;
    }
}
```

---

## 🟢 Easy Level Questions

### 1. Kth Largest Element in Array ⭐⭐⭐⭐⭐
**Problem**: Find the kth largest element in an unsorted array.

**Approach**: Use min heap of size k
```javascript
function findKthLargest(nums, k) {
    const minHeap = new PriorityQueue((a, b) => a - b);
    
    for (const num of nums) {
        minHeap.enqueue(num);
        if (minHeap.size() > k) {
            minHeap.dequeue();
        }
    }
    
    return minHeap.peek();
}
```
**Time**: O(n log k) | **Space**: O(k)

### 2. Last Stone Weight ⭐⭐⭐⭐
**Problem**: Stones are smashed together, heavier stone remains with reduced weight.

**Approach**: Use max heap
```javascript
function lastStoneWeight(stones) {
    const maxHeap = new PriorityQueue((a, b) => b - a);
    
    for (const stone of stones) {
        maxHeap.enqueue(stone);
    }
    
    while (maxHeap.size() > 1) {
        const first = maxHeap.dequeue();
        const second = maxHeap.dequeue();
        
        if (first !== second) {
            maxHeap.enqueue(first - second);
        }
    }
    
    return maxHeap.size() === 0 ? 0 : maxHeap.peek();
}
```

### 3. Merge k Sorted Lists ⭐⭐⭐⭐⭐
**Problem**: Merge k sorted linked lists into one sorted list.

**Approach**: Use min heap with list heads
```javascript
function mergeKLists(lists) {
    const minHeap = new PriorityQueue((a, b) => a.val - b.val);
    
    // Add all non-null heads to heap
    for (const head of lists) {
        if (head) {
            minHeap.enqueue(head);
        }
    }
    
    const dummy = new ListNode(0);
    let current = dummy;
    
    while (minHeap.size() > 0) {
        const node = minHeap.dequeue();
        current.next = node;
        current = current.next;
        
        if (node.next) {
            minHeap.enqueue(node.next);
        }
    }
    
    return dummy.next;
}
```

### 4. Top K Frequent Elements ⭐⭐⭐⭐⭐
**Problem**: Find k most frequent elements in array.

**Approach**: Use min heap with frequency
```javascript
function topKFrequent(nums, k) {
    const freqMap = new Map();
    
    // Count frequencies
    for (const num of nums) {
        freqMap.set(num, (freqMap.get(num) || 0) + 1);
    }
    
    // Use min heap to keep track of k most frequent
    const minHeap = new PriorityQueue((a, b) => a[1] - b[1]);
    
    for (const [num, freq] of freqMap) {
        minHeap.enqueue([num, freq]);
        if (minHeap.size() > k) {
            minHeap.dequeue();
        }
    }
    
    return minHeap.heap.map(item => item[0]);
}
```

### 5. Kth Smallest Element in Sorted Matrix ⭐⭐⭐⭐
**Problem**: Find kth smallest element in row and column sorted matrix.

**Approach**: Use min heap
```javascript
function kthSmallest(matrix, k) {
    const n = matrix.length;
    const minHeap = new PriorityQueue((a, b) => a[0] - b[0]);
    const visited = new Set();
    
    minHeap.enqueue([matrix[0][0], 0, 0]);
    visited.add('0,0');
    
    for (let i = 0; i < k - 1; i++) {
        const [val, row, col] = minHeap.dequeue();
        
        // Add right neighbor
        if (col + 1 < n && !visited.has(`${row},${col + 1}`)) {
            minHeap.enqueue([matrix[row][col + 1], row, col + 1]);
            visited.add(`${row},${col + 1}`);
        }
        
        // Add bottom neighbor
        if (row + 1 < n && !visited.has(`${row + 1},${col}`)) {
            minHeap.enqueue([matrix[row + 1][col], row + 1, col]);
            visited.add(`${row + 1},${col}`);
        }
    }
    
    return minHeap.peek()[0];
}
```

### 6. Ugly Number II ⭐⭐⭐⭐
**Problem**: Find the nth ugly number (divisible only by 2, 3, 5).

**Approach**: Use min heap to generate ugly numbers
```javascript
function nthUglyNumber(n) {
    const minHeap = new PriorityQueue((a, b) => a - b);
    const seen = new Set();
    const factors = [2, 3, 5];
    
    minHeap.enqueue(1);
    seen.add(1);
    
    let ugly = 1;
    
    for (let i = 0; i < n; i++) {
        ugly = minHeap.dequeue();
        
        for (const factor of factors) {
            const next = ugly * factor;
            if (!seen.has(next)) {
                minHeap.enqueue(next);
                seen.add(next);
            }
        }
    }
    
    return ugly;
}
```

### 7. Minimum Cost to Connect Sticks ⭐⭐⭐⭐
**Problem**: Connect sticks with minimum cost (cost = sum of lengths).

**Approach**: Use min heap - always connect two smallest sticks
```javascript
function connectSticks(sticks) {
    const minHeap = new PriorityQueue((a, b) => a - b);
    
    for (const stick of sticks) {
        minHeap.enqueue(stick);
    }
    
    let totalCost = 0;
    
    while (minHeap.size() > 1) {
        const first = minHeap.dequeue();
        const second = minHeap.dequeue();
        const cost = first + second;
        
        totalCost += cost;
        minHeap.enqueue(cost);
    }
    
    return totalCost;
}
```

---

## 🟡 Medium Level Questions

### 1. Find Median from Data Stream ⭐⭐⭐⭐⭐
**Problem**: Design data structure to find median of stream of integers.

**Approach**: Use two heaps (max heap for smaller half, min heap for larger half)
```javascript
class MedianFinder {
    constructor() {
        this.maxHeap = new PriorityQueue((a, b) => b - a); // smaller half
        this.minHeap = new PriorityQueue((a, b) => a - b); // larger half
    }
    
    addNum(num) {
        // Add to max heap first
        this.maxHeap.enqueue(num);
        
        // Balance: ensure max heap root <= min heap root
        if (this.minHeap.size() > 0 && this.maxHeap.peek() > this.minHeap.peek()) {
            this.minHeap.enqueue(this.maxHeap.dequeue());
        }
        
        // Balance sizes
        if (this.maxHeap.size() > this.minHeap.size() + 1) {
            this.minHeap.enqueue(this.maxHeap.dequeue());
        } else if (this.minHeap.size() > this.maxHeap.size() + 1) {
            this.maxHeap.enqueue(this.minHeap.dequeue());
        }
    }
    
    findMedian() {
        if (this.maxHeap.size() === this.minHeap.size()) {
            return (this.maxHeap.peek() + this.minHeap.peek()) / 2;
        } else if (this.maxHeap.size() > this.minHeap.size()) {
            return this.maxHeap.peek();
        } else {
            return this.minHeap.peek();
        }
    }
}
```

### 2. Task Scheduler ⭐⭐⭐⭐
**Problem**: Schedule tasks with cooling time between same tasks.

**Approach**: Use max heap for task frequencies
```javascript
function leastInterval(tasks, n) {
    const freqMap = new Map();
    
    for (const task of tasks) {
        freqMap.set(task, (freqMap.get(task) || 0) + 1);
    }
    
    const maxHeap = new PriorityQueue((a, b) => b - a);
    
    for (const freq of freqMap.values()) {
        maxHeap.enqueue(freq);
    }
    
    let time = 0;
    const queue = []; // [frequency, available_time]
    
    while (maxHeap.size() > 0 || queue.length > 0) {
        time++;
        
        if (maxHeap.size() > 0) {
            const freq = maxHeap.dequeue() - 1;
            if (freq > 0) {
                queue.push([freq, time + n]);
            }
        }
        
        if (queue.length > 0 && queue[0][1] === time) {
            const [freq] = queue.shift();
            maxHeap.enqueue(freq);
        }
    }
    
    return time;
}
```

### 3. Reorganize String ⭐⭐⭐⭐
**Problem**: Rearrange string so no two adjacent characters are same.

**Approach**: Use max heap by character frequency
```javascript
function reorganizeString(s) {
    const freqMap = new Map();
    
    for (const char of s) {
        freqMap.set(char, (freqMap.get(char) || 0) + 1);
    }
    
    const maxHeap = new PriorityQueue((a, b) => b[1] - a[1]);
    
    for (const [char, freq] of freqMap) {
        maxHeap.enqueue([char, freq]);
    }
    
    let result = '';
    let prev = null;
    
    while (maxHeap.size() > 0) {
        const [char, freq] = maxHeap.dequeue();
        result += char;
        
        if (prev && prev[1] > 0) {
            maxHeap.enqueue(prev);
        }
        
        prev = [char, freq - 1];
    }
    
    return result.length === s.length ? result : '';
}
```

### 4. Meeting Rooms II ⭐⭐⭐⭐⭐
**Problem**: Find minimum number of meeting rooms required.

**Approach**: Use min heap for end times
```javascript
function minMeetingRooms(intervals) {
    if (intervals.length === 0) return 0;
    
    intervals.sort((a, b) => a[0] - b[0]);
    
    const minHeap = new PriorityQueue((a, b) => a - b);
    
    for (const [start, end] of intervals) {
        if (minHeap.size() > 0 && minHeap.peek() <= start) {
            minHeap.dequeue();
        }
        minHeap.enqueue(end);
    }
    
    return minHeap.size();
}
```

### 5. Sliding Window Maximum ⭐⭐⭐⭐⭐
**Problem**: Find maximum in each sliding window of size k.

**Approach**: Use max heap with indices
```javascript
function maxSlidingWindow(nums, k) {
    const maxHeap = new PriorityQueue((a, b) => {
        if (a[0] === b[0]) return b[1] - a[1]; // If values equal, prioritize later index
        return b[0] - a[0]; // Max heap by value
    });
    
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        maxHeap.enqueue([nums[i], i]);
        
        if (i >= k - 1) {
            // Remove elements outside window
            while (maxHeap.size() > 0 && maxHeap.peek()[1] <= i - k) {
                maxHeap.dequeue();
            }
            
            result.push(maxHeap.peek()[0]);
        }
    }
    
    return result;
}
```

### 6. Furthest Building You Can Reach ⭐⭐⭐⭐
**Problem**: Use ladders and bricks optimally to reach furthest building.

**Approach**: Use min heap to track ladder usage
```javascript
function furthestBuilding(heights, bricks, ladders) {
    const minHeap = new PriorityQueue((a, b) => a - b);
    
    for (let i = 0; i < heights.length - 1; i++) {
        const diff = heights[i + 1] - heights[i];
        
        if (diff > 0) {
            minHeap.enqueue(diff);
            
            if (minHeap.size() > ladders) {
                const minDiff = minHeap.dequeue();
                bricks -= minDiff;
                
                if (bricks < 0) {
                    return i;
                }
            }
        }
    }
    
    return heights.length - 1;
}
```

### 7. K Closest Points to Origin ⭐⭐⭐⭐⭐
**Problem**: Find k closest points to origin (0,0).

**Approach**: Use max heap of size k
```javascript
function kClosest(points, k) {
    const maxHeap = new PriorityQueue((a, b) => b[0] - a[0]);
    
    for (const point of points) {
        const distance = point[0] * point[0] + point[1] * point[1];
        maxHeap.enqueue([distance, point]);
        
        if (maxHeap.size() > k) {
            maxHeap.dequeue();
        }
    }
    
    return maxHeap.heap.map(item => item[1]);
}
```

### 8. Minimum Number of Refueling Stops ⭐⭐⭐⭐⭐
**Problem**: Find minimum refueling stops to reach target.

**Approach**: Use max heap for fuel amounts
```javascript
function minRefuelStops(target, startFuel, stations) {
    const maxHeap = new PriorityQueue((a, b) => b - a);
    
    let fuel = startFuel;
    let stops = 0;
    let i = 0;
    
    while (fuel < target) {
        // Add all reachable stations to heap
        while (i < stations.length && stations[i][0] <= fuel) {
            maxHeap.enqueue(stations[i][1]);
            i++;
        }
        
        if (maxHeap.size() === 0) {
            return -1; // Cannot reach target
        }
        
        fuel += maxHeap.dequeue();
        stops++;
    }
    
    return stops;
}
```

---

## 🔴 Hard Level Questions

### 1. Merge k Sorted Arrays ⭐⭐⭐⭐⭐
**Problem**: Merge k sorted arrays into one sorted array.

**Approach**: Use min heap with array indices
```javascript
function mergeKSortedArrays(arrays) {
    const minHeap = new PriorityQueue((a, b) => a[0] - b[0]);
    const result = [];
    
    // Initialize heap with first element of each array
    for (let i = 0; i < arrays.length; i++) {
        if (arrays[i].length > 0) {
            minHeap.enqueue([arrays[i][0], i, 0]);
        }
    }
    
    while (minHeap.size() > 0) {
        const [val, arrayIndex, elementIndex] = minHeap.dequeue();
        result.push(val);
        
        if (elementIndex + 1 < arrays[arrayIndex].length) {
            const nextVal = arrays[arrayIndex][elementIndex + 1];
            minHeap.enqueue([nextVal, arrayIndex, elementIndex + 1]);
        }
    }
    
    return result;
}
```

### 2. Smallest Range Covering Elements from K Lists ⭐⭐⭐⭐⭐
**Problem**: Find smallest range that contains at least one element from each list.

**Approach**: Use min heap with tracking max element
```javascript
function smallestRange(nums) {
    const minHeap = new PriorityQueue((a, b) => a[0] - b[0]);
    let max = -Infinity;
    
    // Initialize heap with first element of each list
    for (let i = 0; i < nums.length; i++) {
        minHeap.enqueue([nums[i][0], i, 0]);
        max = Math.max(max, nums[i][0]);
    }
    
    let rangeStart = 0;
    let rangeEnd = Infinity;
    
    while (minHeap.size() === nums.length) {
        const [min, listIndex, elementIndex] = minHeap.dequeue();
        
        if (max - min < rangeEnd - rangeStart) {
            rangeStart = min;
            rangeEnd = max;
        }
        
        if (elementIndex + 1 < nums[listIndex].length) {
            const nextVal = nums[listIndex][elementIndex + 1];
            minHeap.enqueue([nextVal, listIndex, elementIndex + 1]);
            max = Math.max(max, nextVal);
        }
    }
    
    return [rangeStart, rangeEnd];
}
```

### 3. IPO (Maximum Capital) ⭐⭐⭐⭐⭐
**Problem**: Select up to k projects to maximize capital.

**Approach**: Use two heaps (min heap for capital, max heap for profit)
```javascript
function findMaximizedCapital(k, w, profits, capital) {
    const availableProjects = new PriorityQueue((a, b) => a[0] - b[0]); // Min heap by capital
    const affordableProjects = new PriorityQueue((a, b) => b - a); // Max heap by profit
    
    for (let i = 0; i < profits.length; i++) {
        availableProjects.enqueue([capital[i], profits[i]]);
    }
    
    for (let i = 0; i < k; i++) {
        // Move all affordable projects to max heap
        while (availableProjects.size() > 0 && availableProjects.peek()[0] <= w) {
            const [cap, prof] = availableProjects.dequeue();
            affordableProjects.enqueue(prof);
        }
        
        if (affordableProjects.size() === 0) {
            break;
        }
        
        w += affordableProjects.dequeue();
    }
    
    return w;
}
```

### 4. Trapping Rain Water II ⭐⭐⭐⭐⭐
**Problem**: Calculate trapped rainwater in 2D elevation map.

**Approach**: Use min heap for boundary cells
```javascript
function trapRainWater(heightMap) {
    if (!heightMap || heightMap.length === 0 || heightMap[0].length === 0) {
        return 0;
    }
    
    const m = heightMap.length;
    const n = heightMap[0].length;
    const visited = Array(m).fill().map(() => Array(n).fill(false));
    const minHeap = new PriorityQueue((a, b) => a[0] - b[0]);
    
    // Add boundary cells to heap
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (i === 0 || i === m - 1 || j === 0 || j === n - 1) {
                minHeap.enqueue([heightMap[i][j], i, j]);
                visited[i][j] = true;
            }
        }
    }
    
    let water = 0;
    const directions = [[0, 1], [1, 0], [0, -1], [-1, 0]];
    
    while (minHeap.size() > 0) {
        const [height, x, y] = minHeap.dequeue();
        
        for (const [dx, dy] of directions) {
            const nx = x + dx;
            const ny = y + dy;
            
            if (nx >= 0 && nx < m && ny >= 0 && ny < n && !visited[nx][ny]) {
                water += Math.max(0, height - heightMap[nx][ny]);
                minHeap.enqueue([Math.max(height, heightMap[nx][ny]), nx, ny]);
                visited[nx][ny] = true;
            }
        }
    }
    
    return water;
}
```

### 5. Skyline Problem ⭐⭐⭐⭐⭐
**Problem**: Find skyline silhouette of buildings.

**Approach**: Use max heap with sweep line algorithm
```javascript
function getSkyline(buildings) {
    const events = [];
    
    for (const [left, right, height] of buildings) {
        events.push([left, -height, 's']); // Start event
        events.push([right, height, 'e']); // End event
    }
    
    events.sort((a, b) => {
        if (a[0] !== b[0]) return a[0] - b[0];
        if (a[2] !== b[2]) return a[2] === 's' ? -1 : 1;
        return a[2] === 's' ? a[1] - b[1] : b[1] - a[1];
    });
    
    const result = [];
    const maxHeap = new PriorityQueue((a, b) => b - a);
    maxHeap.enqueue(0); // Ground level
    
    for (const [x, h, type] of events) {
        if (type === 's') {
            maxHeap.enqueue(-h);
        } else {
            // Remove height from heap
            const tempHeap = new PriorityQueue((a, b) => b - a);
            let removed = false;
            
            while (maxHeap.size() > 0) {
                const top = maxHeap.dequeue();
                if (!removed && top === h) {
                    removed = true;
                } else {
                    tempHeap.enqueue(top);
                }
            }
            
            maxHeap.heap = tempHeap.heap;
        }
        
        const maxHeight = maxHeap.peek();
        
        if (result.length === 0 || result[result.length - 1][1] !== maxHeight) {
            result.push([x, maxHeight]);
        }
    }
    
    return result;
}
```

### 6. Sliding Window Median ⭐⭐⭐⭐⭐
**Problem**: Find median in each sliding window of size k.

**Approach**: Use two heaps with window management
```javascript
function medianSlidingWindow(nums, k) {
    const result = [];
    const maxHeap = new PriorityQueue((a, b) => b - a); // Smaller half
    const minHeap = new PriorityQueue((a, b) => a - b); // Larger half
    
    const balance = () => {
        if (maxHeap.size() > minHeap.size() + 1) {
            minHeap.enqueue(maxHeap.dequeue());
        } else if (minHeap.size() > maxHeap.size() + 1) {
            maxHeap.enqueue(minHeap.dequeue());
        }
    };
    
    const getMedian = () => {
        if (k % 2 === 1) {
            return maxHeap.size() > minHeap.size() ? maxHeap.peek() : minHeap.peek();
        } else {
            return (maxHeap.peek() + minHeap.peek()) / 2;
        }
    };
    
    const remove = (val) => {
        if (val <= maxHeap.peek()) {
            removeFromHeap(maxHeap, val);
        } else {
            removeFromHeap(minHeap, val);
        }
    };
    
    const removeFromHeap = (heap, val) => {
        const tempHeap = new PriorityQueue(heap.compare);
        while (heap.size() > 0) {
            const top = heap.dequeue();
            if (top !== val) {
                tempHeap.enqueue(top);
            } else {
                break;
            }
        }
        while (tempHeap.size() > 0) {
            heap.enqueue(tempHeap.dequeue());
        }
    };
    
    for (let i = 0; i < nums.length; i++) {
        // Add number
        if (maxHeap.size() === 0 || nums[i] <= maxHeap.peek()) {
            maxHeap.enqueue(nums[i]);
        } else {
            minHeap.enqueue(nums[i]);
        }
        
        balance();
        
        // Remove number from window
        if (i >= k) {
            remove(nums[i - k]);
            balance();
        }
        
        // Add median to result
        if (i >= k - 1) {
            result.push(getMedian());
        }
    }
    
    return result;
}
```

### 7. Super Ugly Number ⭐⭐⭐⭐⭐
**Problem**: Find nth super ugly number (divisible only by given prime factors).

**Approach**: Use min heap with multiple prime factors
```javascript
function nthSuperUglyNumber(n, primes) {
    const minHeap = new PriorityQueue((a, b) => a - b);
    const seen = new Set();
    
    minHeap.enqueue(1);
    seen.add(1);
    
    let ugly = 1;
    
    for (let i = 0; i < n; i++) {
        ugly = minHeap.dequeue();
        
        for (const prime of primes) {
            const next = ugly * prime;
            if (!seen.has(next)) {
                minHeap.enqueue(next);
                seen.add(next);
            }
        }
    }
    
    return ugly;
}
```

### 8. Employee Free Time ⭐⭐⭐⭐⭐
**Problem**: Find common free time for all employees.

**Approach**: Use min heap to merge intervals
```javascript
function employeeFreeTime(schedule) {
    const minHeap = new PriorityQueue((a, b) => a.start - b.start);
    
    // Add all intervals to heap
    for (const employee of schedule) {
        for (const interval of employee) {
            minHeap.enqueue(interval);
        }
    }
    
    const result = [];
    let prev = minHeap.dequeue();
    
    while (minHeap.size() > 0) {
        const curr = minHeap.dequeue();
        
        if (prev.end < curr.start) {
            // Found free time
            result.push(new Interval(prev.end, curr.start));
            prev = curr;
        } else {
            // Merge overlapping intervals
            prev.end = Math.max(prev.end, curr.end);
        }
    }
    
    return result;
}
```

---

## 🎯 Advanced Heap Concepts

### 1. Heap Sort Algorithm
```javascript
function heapSort(arr) {
    const n = arr.length;
    
    // Build max heap
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
    // Extract elements from heap one by one
    for (let i = n - 1; i > 0; i--) {
        [arr[0], arr[i]] = [arr[i], arr[0]];
        heapify(arr, i, 0);
    }
    
    return arr;
}

function heapify(arr, n, i) {
    let largest = i;
    const left = 2 * i + 1;
    const right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    if (largest !== i) {
        [arr[i], arr[largest]] = [arr[largest], arr[i]];
        heapify(arr, n, largest);
    }
}
```

### 2. Indexed Priority Queue
```javascript
class IndexedPriorityQueue {
    constructor(maxSize, compareFn = (a, b) => a - b) {
        this.maxSize = maxSize;
        this.heap = [];
        this.indexMap = new Map(); // key -> heap index
        this.heapMap = new Map(); // heap index -> key
        this.values = new Map(); // key -> value
        this.compare = compareFn;
    }
    
    insert(key, value) {
        if (this.contains(key)) {
            this.update(key, value);
            return;
        }
        
        this.values.set(key, value);
        const heapIndex = this.heap.length;
        this.heap.push(key);
        this.indexMap.set(key, heapIndex);
        this.heapMap.set(heapIndex, key);
        
        this.swimUp(heapIndex);
    }
    
    update(key, value) {
        if (!this.contains(key)) return;
        
        const oldValue = this.values.get(key);
        this.values.set(key, value);
        const heapIndex = this.indexMap.get(key);
        
        if (this.compare(value, oldValue) < 0) {
            this.swimUp(heapIndex);
        } else {
            this.sinkDown(heapIndex);
        }
    }
    
    contains(key) {
        return this.indexMap.has(key);
    }
    
    peek() {
        return this.heap.length > 0 ? this.heap[0] : null;
    }
    
    poll() {
        if (this.heap.length === 0) return null;
        
        const minKey = this.heap[0];
        this.swap(0, this.heap.length - 1);
        
        this.heap.pop();
        this.indexMap.delete(minKey);
        this.heapMap.delete(this.heap.length);
        this.values.delete(minKey);
        
        if (this.heap.length > 0) {
            this.sinkDown(0);
        }
        
        return minKey;
    }
    
    swimUp(index) {
        while (index > 0) {
            const parentIndex = Math.floor((index - 1) / 2);
            const key = this.heap[index];
            const parentKey = this.heap[parentIndex];
            
            if (this.compare(this.values.get(key), this.values.get(parentKey)) >= 0) {
                break;
            }
            
            this.swap(index, parentIndex);
            index = parentIndex;
        }
    }
    
    sinkDown(index) {
        while (true) {
            let smallest = index;
            const left = 2 * index + 1;
            const right = 2 * index + 2;
            
            if (left < this.heap.length) {
                const leftKey = this.heap[left];
                const smallestKey = this.heap[smallest];
                if (this.compare(this.values.get(leftKey), this.values.get(smallestKey)) < 0) {
                    smallest = left;
                }
            }
            
            if (right < this.heap.length) {
                const rightKey = this.heap[right];
                const smallestKey = this.heap[smallest];
                if (this.compare(this.values.get(rightKey), this.values.get(smallestKey)) < 0) {
                    smallest = right;
                }
            }
            
            if (smallest === index) break;
            
            this.swap(index, smallest);
            index = smallest;
        }
    }
    
    swap(i, j) {
        const keyI = this.heap[i];
        const keyJ = this.heap[j];
        
        [this.heap[i], this.heap[j]] = [this.heap[j], this.heap[i]];
        
        this.indexMap.set(keyI, j);
        this.indexMap.set(keyJ, i);
        this.heapMap.set(i, keyJ);
        this.heapMap.set(j, keyI);
    }
}
```

### 3. Binomial Heap (Advanced)
```javascript
class BinomialNode {
    constructor(key) {
        this.key = key;
        this.degree = 0;
        this.parent = null;
        this.child = null;
        this.sibling = null;
    }
}

class BinomialHeap {
    constructor() {
        this.head = null;
        this.size = 0;
    }
    
    insert(key) {
        const node = new BinomialNode(key);
        const tempHeap = new BinomialHeap();
        tempHeap.head = node;
        tempHeap.size = 1;
        
        this.union(tempHeap);
        this.size++;
        
        return node;
    }
    
    minimum() {
        if (!this.head) return null;
        
        let min = this.head;
        let current = this.head.sibling;
        
        while (current) {
            if (current.key < min.key) {
                min = current;
            }
            current = current.sibling;
        }
        
        return min;
    }
    
    extractMin() {
        if (!this.head) return null;
        
        const min = this.minimum();
        this.removeNode(min);
        
        // Create new heap from children
        const newHeap = new BinomialHeap();
        let child = min.child;
        
        while (child) {
            const next = child.sibling;
            child.sibling = null;
            child.parent = null;
            
            if (!newHeap.head) {
                newHeap.head = child;
            } else {
                child.sibling = newHeap.head;
                newHeap.head = child;
            }
            
            child = next;
        }
        
        this.union(newHeap);
        this.size--;
        
        return min.key;
    }
    
    union(otherHeap) {
        this.head = this.merge(this.head, otherHeap.head);
        
        if (!this.head) return;
        
        let prev = null;
        let current = this.head;
        let next = current.sibling;
        
        while (next) {
            if (current.degree !== next.degree || 
                (next.sibling && next.sibling.degree === current.degree)) {
                prev = current;
                current = next;
            } else {
                if (current.key <= next.key) {
                    current.sibling = next.sibling;
                    this.link(next, current);
                } else {
                    if (!prev) {
                        this.head = next;
                    } else {
                        prev.sibling = next;
                    }
                    this.link(current, next);
                    current = next;
                }
            }
            next = current.sibling;
        }
    }
    
    merge(h1, h2) {
        if (!h1) return h2;
        if (!h2) return h1;
        
        let head = null;
        let tail = null;
        
        while (h1 && h2) {
            let node;
            if (h1.degree <= h2.degree) {
                node = h1;
                h1 = h1.sibling;
            } else {
                node = h2;
                h2 = h2.sibling;
            }
            
            if (!head) {
                head = tail = node;
            } else {
                tail.sibling = node;
                tail = node;
            }
        }
        
        if (h1) tail.sibling = h1;
        if (h2) tail.sibling = h2;
        
        return head;
    }
    
    link(child, parent) {
        child.parent = parent;
        child.sibling = parent.child;
        parent.child = child;
        parent.degree++;
    }
    
    removeNode(node) {
        if (node === this.head) {
            this.head = node.sibling;
            return;
        }
        
        let current = this.head;
        while (current.sibling !== node) {
            current = current.sibling;
        }
        current.sibling = node.sibling;
    }
}
```

---

## 🎯 Key Interview Tips

### Time Complexity Patterns
- **Build Heap**: O(n)
- **Insert/Delete**: O(log n)
- **Find Min/Max**: O(1)
- **Heap Sort**: O(n log n)
- **Merge K Lists**: O(n log k)

### Space Complexity Considerations
- **Heap Storage**: O(n)
- **K-sized Heap**: O(k)
- **Two Heaps**: O(n)

### Common Interview Patterns
1. **K-th Element Problems**: Use heap of size k
2. **Median Finding**: Use two heaps (max for smaller, min for larger)
3. **Merge Multiple Sources**: Use min heap with indices
4. **Sliding Window**: Use heap with element removal
5. **Scheduling/Priority**: Use heap for task management
6. **Graph Algorithms**: Dijkstra's, Prim's use heaps

### JavaScript Specific Tips
```javascript
// Creating comparison functions
const minHeap = new PriorityQueue((a, b) => a - b);
const maxHeap = new PriorityQueue((a, b) => b - a);

// For objects
const taskHeap = new PriorityQueue((a, b) => a.priority - b.priority);

// For arrays/tuples
const coordinateHeap = new PriorityQueue((a, b) => a[0] - b[0]);

// Multiple criteria
const complexHeap = new PriorityQueue((a, b) => {
    if (a.priority !== b.priority) return a.priority - b.priority;
    return a.timestamp - b.timestamp;
});
```

### Edge Cases to Consider
1. **Empty heap operations**
2. **Single element heap**
3. **Duplicate elements**
4. **Integer overflow** (for large numbers)
5. **Negative numbers**
6. **Memory constraints** (for large datasets)

### Companies That Ask Heap Questions
- **Google**: Median problems, K-th element, merge operations
- **Amazon**: Task scheduling, priority systems, sliding window
- **Microsoft**: Meeting rooms, interval problems, top-k queries
- **Facebook/Meta**: Stream processing, real-time data, ranking
- **Apple**: System design with priority queues, resource management
- **Netflix**: Content recommendation, priority streaming
- **Uber**: Dynamic pricing, driver matching, route optimization

---

## 🚀 Practice Strategy

### Level 1: Master Basic Operations
1. Implement min/max heap from scratch
2. Understand heapify up/down operations
3. Practice with simple k-th element problems

### Level 2: Learn Common Patterns
1. Two-heap technique for median
2. Heap with indices for sliding window
3. Multiple heap merge operations

### Level 3: Advanced Applications
1. Custom comparators for complex objects
2. Heap-based graph algorithms
3. System design with priority queues

### Level 4: Optimization Techniques
1. Lazy deletion in heaps
2. Indexed priority queues
3. Binomial/Fibonacci heaps

### Mock Interview Questions
1. "Design a system to find trending topics"
2. "Implement a task scheduler with priorities"
3. "Find median of a stream of integers"
4. "Merge k sorted files"
5. "Design a leaderboard system"

Remember: Practice implementing heaps from scratch before using library implementations in interviews!
