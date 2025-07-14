# Complete Searching Algorithms DSA Interview Guide 🔍

## 1. Searching Fundamentals

### What is Searching?
Finding the location of a specific element or determining if an element exists in a data structure.

### Search Types
- **Linear Search**: Sequential checking
- **Binary Search**: Divide and conquer on sorted data
- **Hash-based Search**: Direct access using hash functions
- **Tree Search**: Traversing tree structures
- **Graph Search**: Exploring graph nodes

### Key Properties
- **Time Complexity**: How fast the search is
- **Space Complexity**: Extra memory required
- **Preconditions**: Data structure requirements (sorted, etc.)
- **Stability**: Consistent behavior with duplicates

---

## 2. Basic Searching Algorithms

### Linear Search ⭐⭐
```javascript
// O(n) time, O(1) space
function linearSearch(arr, target) {
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] === target) return i;
    }
    return -1;
}
```
**Use**: Unsorted arrays, small datasets, when simplicity matters

### Binary Search ⭐⭐⭐⭐⭐
```javascript
// O(log n) time, O(1) space - Iterative
function binarySearch(arr, target) {
    let left = 0, right = arr.length - 1;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        
        if (arr[mid] === target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return -1;
}

// O(log n) time, O(log n) space - Recursive
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
    if (left > right) return -1;
    
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) return mid;
    else if (arr[mid] < target) return binarySearchRecursive(arr, target, mid + 1, right);
    else return binarySearchRecursive(arr, target, left, mid - 1);
}
```
**Use**: Sorted arrays, large datasets, when O(log n) is needed

### Jump Search ⭐⭐⭐
```javascript
// O(√n) time, O(1) space
function jumpSearch(arr, target) {
    const n = arr.length;
    const step = Math.floor(Math.sqrt(n));
    let prev = 0;
    
    // Jump until we find a block where target might be
    while (arr[Math.min(step, n) - 1] < target) {
        prev = step;
        step += Math.floor(Math.sqrt(n));
        if (prev >= n) return -1;
    }
    
    // Linear search in the identified block
    while (arr[prev] < target) {
        prev++;
        if (prev === Math.min(step, n)) return -1;
    }
    
    return arr[prev] === target ? prev : -1;
}
```
**Use**: Sorted arrays where binary search is overkill

### Interpolation Search ⭐⭐⭐⭐
```javascript
// O(log log n) avg, O(n) worst, O(1) space
function interpolationSearch(arr, target) {
    let left = 0, right = arr.length - 1;
    
    while (left <= right && target >= arr[left] && target <= arr[right]) {
        if (left === right) {
            return arr[left] === target ? left : -1;
        }
        
        // Calculate position using interpolation formula
        const pos = left + Math.floor(
            ((target - arr[left]) / (arr[right] - arr[left])) * (right - left)
        );
        
        if (arr[pos] === target) return pos;
        else if (arr[pos] < target) left = pos + 1;
        else right = pos - 1;
    }
    
    return -1;
}
```
**Use**: Uniformly distributed sorted arrays

### Exponential Search ⭐⭐⭐
```javascript
// O(log n) time, O(1) space
function exponentialSearch(arr, target) {
    if (arr[0] === target) return 0;
    
    // Find range for binary search
    let bound = 1;
    while (bound < arr.length && arr[bound] < target) {
        bound *= 2;
    }
    
    // Binary search in the found range
    return binarySearch(arr.slice(bound / 2, Math.min(bound, arr.length)), target);
}
```
**Use**: Unbounded/infinite arrays, when size is unknown

---

## 3. Tree Search Algorithms

### Binary Search Tree Search ⭐⭐⭐⭐
```javascript
class TreeNode {
    constructor(val, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}

function searchBST(root, target) {
    if (!root) return null;
    
    if (root.val === target) return root;
    else if (target < root.val) return searchBST(root.left, target);
    else return searchBST(root.right, target);
}

// Iterative version
function searchBSTIterative(root, target) {
    while (root && root.val !== target) {
        root = target < root.val ? root.left : root.right;
    }
    return root;
}
```

### Depth-First Search (DFS) ⭐⭐⭐⭐
```javascript
// Preorder traversal
function dfsPreorder(root, target) {
    if (!root) return false;
    
    if (root.val === target) return true;
    
    return dfsPreorder(root.left, target) || dfsPreorder(root.right, target);
}

// Iterative DFS
function dfsIterative(root, target) {
    if (!root) return false;
    
    const stack = [root];
    
    while (stack.length > 0) {
        const node = stack.pop();
        
        if (node.val === target) return true;
        
        if (node.right) stack.push(node.right);
        if (node.left) stack.push(node.left);
    }
    
    return false;
}
```

### Breadth-First Search (BFS) ⭐⭐⭐⭐
```javascript
function bfs(root, target) {
    if (!root) return false;
    
    const queue = [root];
    
    while (queue.length > 0) {
        const node = queue.shift();
        
        if (node.val === target) return true;
        
        if (node.left) queue.push(node.left);
        if (node.right) queue.push(node.right);
    }
    
    return false;
}
```

---

## 4. Graph Search Algorithms

### Graph DFS ⭐⭐⭐⭐⭐
```javascript
function graphDFS(graph, start, target) {
    const visited = new Set();
    
    function dfs(node) {
        if (node === target) return true;
        if (visited.has(node)) return false;
        
        visited.add(node);
        
        for (const neighbor of graph[node] || []) {
            if (dfs(neighbor)) return true;
        }
        
        return false;
    }
    
    return dfs(start);
}
```

### Graph BFS ⭐⭐⭐⭐⭐
```javascript
function graphBFS(graph, start, target) {
    const visited = new Set();
    const queue = [start];
    visited.add(start);
    
    while (queue.length > 0) {
        const node = queue.shift();
        
        if (node === target) return true;
        
        for (const neighbor of graph[node] || []) {
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.push(neighbor);
            }
        }
    }
    
    return false;
}
```

### Bidirectional Search ⭐⭐⭐⭐⭐
```javascript
function bidirectionalSearch(graph, start, target) {
    if (start === target) return true;
    
    const visitedFromStart = new Set([start]);
    const visitedFromTarget = new Set([target]);
    const queueFromStart = [start];
    const queueFromTarget = [target];
    
    while (queueFromStart.length > 0 && queueFromTarget.length > 0) {
        // Expand from start
        if (expandLevel(graph, queueFromStart, visitedFromStart, visitedFromTarget)) {
            return true;
        }
        
        // Expand from target
        if (expandLevel(graph, queueFromTarget, visitedFromTarget, visitedFromStart)) {
            return true;
        }
    }
    
    return false;
}

function expandLevel(graph, queue, visited, otherVisited) {
    const levelSize = queue.length;
    
    for (let i = 0; i < levelSize; i++) {
        const node = queue.shift();
        
        for (const neighbor of graph[node] || []) {
            if (otherVisited.has(neighbor)) return true;
            
            if (!visited.has(neighbor)) {
                visited.add(neighbor);
                queue.push(neighbor);
            }
        }
    }
    
    return false;
}
```

---

## 5. Advanced Search Techniques

### Ternary Search ⭐⭐⭐⭐
```javascript
// For unimodal functions (single peak)
function ternarySearch(func, left, right, epsilon = 1e-9) {
    while (right - left > epsilon) {
        const m1 = left + (right - left) / 3;
        const m2 = right - (right - left) / 3;
        
        if (func(m1) < func(m2)) {
            left = m1;
        } else {
            right = m2;
        }
    }
    
    return (left + right) / 2;
}
```

### Hash Table Search ⭐⭐⭐
```javascript
class HashTable {
    constructor(size = 53) {
        this.keyMap = new Array(size);
    }
    
    _hash(key) {
        let total = 0;
        const WEIRD_PRIME = 31;
        for (let i = 0; i < Math.min(key.length, 100); i++) {
            const char = key[i];
            const value = char.charCodeAt(0) - 96;
            total = (total * WEIRD_PRIME + value) % this.keyMap.length;
        }
        return total;
    }
    
    set(key, value) {
        const index = this._hash(key);
        if (!this.keyMap[index]) {
            this.keyMap[index] = [];
        }
        this.keyMap[index].push([key, value]);
    }
    
    get(key) {
        const index = this._hash(key);
        if (this.keyMap[index]) {
            for (const [k, v] of this.keyMap[index]) {
                if (k === key) return v;
            }
        }
        return undefined;
    }
}
```

---

## 6. Time & Space Complexity Summary

| Algorithm | Best | Average | Worst | Space | Use Case |
|-----------|------|---------|-------|-------|----------|
| Linear Search | O(1) | O(n) | O(n) | O(1) | Unsorted data |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) | Sorted arrays |
| Jump Search | O(1) | O(√n) | O(√n) | O(1) | Sorted arrays |
| Interpolation | O(1) | O(log log n) | O(n) | O(1) | Uniform distribution |
| Exponential | O(1) | O(log n) | O(log n) | O(1) | Unbounded arrays |
| Hash Search | O(1) | O(1) | O(n) | O(n) | Key-value pairs |
| BST Search | O(log n) | O(log n) | O(n) | O(1) | Dynamic sorted data |
| DFS | O(1) | O(V + E) | O(V + E) | O(V) | Graph traversal |
| BFS | O(1) | O(V + E) | O(V + E) | O(V) | Shortest path |

---

# 📚 Interview Questions by Difficulty Level

## Easy Level Questions 💚

### 1. First and Last Position of Element ⭐⭐⭐⭐⭐
**Problem**: Find the first and last position of a target in a sorted array.

**Solution**:
```javascript
function searchRange(nums, target) {
    function findFirst(nums, target) {
        let left = 0, right = nums.length - 1;
        let result = -1;
        
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            if (nums[mid] === target) {
                result = mid;
                right = mid - 1; // Continue searching left
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
    
    function findLast(nums, target) {
        let left = 0, right = nums.length - 1;
        let result = -1;
        
        while (left <= right) {
            const mid = Math.floor((left + right) / 2);
            if (nums[mid] === target) {
                result = mid;
                left = mid + 1; // Continue searching right
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
    
    return [findFirst(nums, target), findLast(nums, target)];
}
```

### 2. Square Root using Binary Search ⭐⭐⭐⭐
**Problem**: Find the square root of a number using binary search.

**Solution**:
```javascript
function mySqrt(x) {
    if (x === 0) return 0;
    
    let left = 1, right = x;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        const square = mid * mid;
        
        if (square === x) return mid;
        else if (square < x) left = mid + 1;
        else right = mid - 1;
    }
    
    return right; // Return floor value
}
```

### 3. Search Insert Position ⭐⭐⭐
**Problem**: Find the position where target should be inserted in sorted array.

**Solution**:
```javascript
function searchInsert(nums, target) {
    let left = 0, right = nums.length - 1;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return left;
}
```

### 4. Peak Element ⭐⭐⭐⭐
**Problem**: Find a peak element in an array (element greater than its neighbors).

**Solution**:
```javascript
function findPeakElement(nums) {
    let left = 0, right = nums.length - 1;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (nums[mid] > nums[mid + 1]) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}
```

### 5. Binary Search in Rotated Array ⭐⭐⭐⭐
**Problem**: Search in a sorted rotated array.

**Solution**:
```javascript
function search(nums, target) {
    let left = 0, right = nums.length - 1;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        
        if (nums[mid] === target) return mid;
        
        // Check which half is sorted
        if (nums[left] <= nums[mid]) {
            // Left half is sorted
            if (nums[left] <= target && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {
            // Right half is sorted
            if (nums[mid] < target && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

## Medium Level Questions 🟡

### 1. Find Minimum in Rotated Sorted Array ⭐⭐⭐⭐
**Problem**: Find the minimum element in a rotated sorted array.

**Solution**:
```javascript
function findMin(nums) {
    let left = 0, right = nums.length - 1;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return nums[left];
}
```

### 2. Search in 2D Matrix ⭐⭐⭐⭐
**Problem**: Search for a target in a 2D matrix where each row and column is sorted.

**Solution**:
```javascript
function searchMatrix(matrix, target) {
    if (!matrix || matrix.length === 0) return false;
    
    let row = 0, col = matrix[0].length - 1;
    
    while (row < matrix.length && col >= 0) {
        if (matrix[row][col] === target) return true;
        else if (matrix[row][col] > target) col--;
        else row++;
    }
    
    return false;
}
```

### 3. Find K Closest Elements ⭐⭐⭐⭐
**Problem**: Find k closest elements to a target in a sorted array.

**Solution**:
```javascript
function findClosestElements(arr, k, x) {
    let left = 0, right = arr.length - k;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (x - arr[mid] > arr[mid + k] - x) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return arr.slice(left, left + k);
}
```

### 4. Capacity To Ship Packages Within D Days ⭐⭐⭐⭐⭐
**Problem**: Find minimum capacity to ship packages within D days.

**Solution**:
```javascript
function shipWithinDays(weights, D) {
    let left = Math.max(...weights);
    let right = weights.reduce((sum, w) => sum + w, 0);
    
    function canShip(capacity) {
        let days = 1, currentWeight = 0;
        
        for (const weight of weights) {
            if (currentWeight + weight > capacity) {
                days++;
                currentWeight = weight;
            } else {
                currentWeight += weight;
            }
        }
        
        return days <= D;
    }
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (canShip(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}
```

### 5. Koko Eating Bananas ⭐⭐⭐⭐
**Problem**: Find minimum eating speed to finish all bananas in H hours.

**Solution**:
```javascript
function minEatingSpeed(piles, H) {
    let left = 1, right = Math.max(...piles);
    
    function canFinish(speed) {
        let hours = 0;
        for (const pile of piles) {
            hours += Math.ceil(pile / speed);
        }
        return hours <= H;
    }
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (canFinish(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}
```

## Hard Level Questions 🔴

### 1. Median of Two Sorted Arrays ⭐⭐⭐⭐⭐
**Problem**: Find median of two sorted arrays in O(log(min(m,n))) time.

**Solution**:
```javascript
function findMedianSortedArrays(nums1, nums2) {
    if (nums1.length > nums2.length) {
        [nums1, nums2] = [nums2, nums1];
    }
    
    const m = nums1.length, n = nums2.length;
    let left = 0, right = m;
    
    while (left <= right) {
        const partitionX = Math.floor((left + right) / 2);
        const partitionY = Math.floor((m + n + 1) / 2) - partitionX;
        
        const maxLeftX = partitionX === 0 ? -Infinity : nums1[partitionX - 1];
        const minRightX = partitionX === m ? Infinity : nums1[partitionX];
        
        const maxLeftY = partitionY === 0 ? -Infinity : nums2[partitionY - 1];
        const minRightY = partitionY === n ? Infinity : nums2[partitionY];
        
        if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
            if ((m + n) % 2 === 0) {
                return (Math.max(maxLeftX, maxLeftY) + Math.min(minRightX, minRightY)) / 2;
            } else {
                return Math.max(maxLeftX, maxLeftY);
            }
        } else if (maxLeftX > minRightY) {
            right = partitionX - 1;
        } else {
            left = partitionX + 1;
        }
    }
}
```

### 2. Split Array Largest Sum ⭐⭐⭐⭐⭐
**Problem**: Split array into m subarrays to minimize the largest sum.

**Solution**:
```javascript
function splitArray(nums, m) {
    let left = Math.max(...nums);
    let right = nums.reduce((sum, num) => sum + num, 0);
    
    function canSplit(maxSum) {
        let subarrays = 1, currentSum = 0;
        
        for (const num of nums) {
            if (currentSum + num > maxSum) {
                subarrays++;
                currentSum = num;
            } else {
                currentSum += num;
            }
        }
        
        return subarrays <= m;
    }
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (canSplit(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}
```

### 3. Smallest Rectangle Enclosing Black Pixels ⭐⭐⭐⭐⭐
**Problem**: Find smallest rectangle that encloses all black pixels.

**Solution**:
```javascript
function minArea(image, x, y) {
    if (!image || image.length === 0) return 0;
    
    const m = image.length, n = image[0].length;
    
    // Find boundaries using binary search
    const left = searchColumns(image, 0, y, true);
    const right = searchColumns(image, y + 1, n, false);
    const top = searchRows(image, 0, x, left, right, true);
    const bottom = searchRows(image, x + 1, m, left, right, false);
    
    return (right - left) * (bottom - top);
}

function searchColumns(image, start, end, searchLeft) {
    while (start < end) {
        const mid = Math.floor((start + end) / 2);
        const hasBlack = image.some(row => row[mid] === '1');
        
        if (hasBlack === searchLeft) {
            end = mid;
        } else {
            start = mid + 1;
        }
    }
    
    return start;
}

function searchRows(image, start, end, left, right, searchTop) {
    while (start < end) {
        const mid = Math.floor((start + end) / 2);
        const hasBlack = image[mid].slice(left, right).includes('1');
        
        if (hasBlack === searchTop) {
            end = mid;
        } else {
            start = mid + 1;
        }
    }
    
    return start;
}
```

### 4. Count of Smaller Numbers After Self ⭐⭐⭐⭐⭐
**Problem**: Count smaller numbers after each element using binary search.

**Solution**:
```javascript
function countSmaller(nums) {
    const result = new Array(nums.length).fill(0);
    const sorted = [];
    
    for (let i = nums.length - 1; i >= 0; i--) {
        const index = binarySearchInsert(sorted, nums[i]);
        result[i] = index;
        sorted.splice(index, 0, nums[i]);
    }
    
    return result;
}

function binarySearchInsert(arr, target) {
    let left = 0, right = arr.length;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}
```

### 5. Aggressive Cows Problem ⭐⭐⭐⭐⭐
**Problem**: Place cows in stalls such that minimum distance is maximized.

**Solution**:
```javascript
function aggressiveCows(stalls, cows) {
    stalls.sort((a, b) => a - b);
    
    let left = 1;
    let right = stalls[stalls.length - 1] - stalls[0];
    let result = 0;
    
    function canPlaceCows(minDistance) {
        let count = 1;
        let lastPosition = stalls[0];
        
        for (let i = 1; i < stalls.length; i++) {
            if (stalls[i] - lastPosition >= minDistance) {
                count++;
                lastPosition = stalls[i];
                if (count === cows) return true;
            }
        }
        
        return false;
    }
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        
        if (canPlaceCows(mid)) {
            result = mid;
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

---

# 🎯 Key Interview Insights

## Most Asked Questions in FAANG
1. **Binary Search Variations** - All companies
2. **Search in Rotated Array** - Google, Amazon, Microsoft
3. **Find Peak Element** - Facebook, Apple
4. **Search in 2D Matrix** - LinkedIn, Uber
5. **Median of Two Sorted Arrays** - Google, Amazon (Hard)

## Common Patterns
1. **Template-based binary search** - Use consistent left/right logic
2. **Search space reduction** - Eliminate half the space each iteration
3. **Condition-based search** - Use helper functions to check conditions
4. **Boundary searching** - Finding first/last occurrence
5. **Answer searching** - Binary search on answer space

## Binary Search Templates

### Template 1: Exact Match
```javascript
function binarySearch(arr, target) {
    let left = 0, right = arr.length - 1;
    
    while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return -1;
}
```

### Template 2: Find Boundary
```javascript
function findBoundary(arr, target) {
    let left = 0, right = arr.length - 1;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (condition(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}
```

### Template 3: Search Answer Space
```javascript
function searchAnswerSpace(condition, left, right) {
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (condition(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

## Pro Tips for Success
1. **Identify search space** - What are you searching in?
2. **Define invariants** - What properties remain true?
3. **Handle edge cases** - Empty arrays, single elements
4. **Choose correct template** - Match problem type to template
5. **Test boundary conditions** - Off-by-one errors are common

## Red Flags to Avoid
- Infinite loops in binary search
- Integer overflow in mid calculation
- Wrong boundary conditions
- Not handling edge cases
- Using wrong template for problem type

---

# 🚀 Advanced Search Topics

## Parallel Search Algorithms
- **Parallel Binary Search**: Multiple threads searching different ranges
- **Distributed Search**: Search across multiple machines
- **GPU-accelerated Search**: Using parallel processing units

## Search in Specialized Data Structures
- **Trie Search**: Prefix-based searching
- **Segment Tree Search**: Range query problems
- **Suffix Array Search**: String pattern matching
- **B-Tree Search**: Database indexing

## Approximate Search
- **Fuzzy Search**: Finding similar strings
- **Nearest Neighbor Search**: Finding closest points
- **Probabilistic Search**: Bloom filters, sketches
