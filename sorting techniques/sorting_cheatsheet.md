# Complete Sorting Interview Guide 🎯

## 1. Sorting Fundamentals

### What is Sorting?
Arranging data in a specific order (ascending/descending) to enable efficient searching and data processing.

### Key Properties
- **Stable**: Maintains relative order of equal elements
- **In-place**: Uses O(1) extra space
- **Adaptive**: Performs better on partially sorted data
- **Online**: Can sort data as it arrives

### Sorting Categories
1. **Comparison-based**: Compare elements directly
2. **Non-comparison**: Use element properties (counting, radix)
3. **Internal**: Data fits in memory
4. **External**: Data too large for memory

## 2. Basic Sorting Algorithms

### Bubble Sort ⭐⭐
```javascript
// O(n²) time, O(1) space, Stable, In-place
function bubbleSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
            }
        }
    }
}
```
**Use**: Educational purposes, very small datasets

### Selection Sort ⭐⭐
```javascript
// O(n²) time, O(1) space, Unstable, In-place
function selectionSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        let minIdx = i;
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[j] < arr[minIdx]) minIdx = j;
        }
        [arr[i], arr[minIdx]] = [arr[minIdx], arr[i]];
    }
}
```
**Use**: When memory writes are costly

### Insertion Sort ⭐⭐⭐
```javascript
// O(n²) time, O(1) space, Stable, In-place, Adaptive
function insertionSort(arr) {
    for (let i = 1; i < arr.length; i++) {
        let key = arr[i], j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```
**Use**: Small arrays, nearly sorted data, online algorithms

## 3. Efficient Sorting Algorithms

### Merge Sort ⭐⭐⭐⭐
```javascript
// O(n log n) time, O(n) space, Stable, Not in-place
function mergeSort(arr) {
    if (arr.length <= 1) return arr;
    const mid = Math.floor(arr.length / 2);
    return merge(mergeSort(arr.slice(0, mid)), mergeSort(arr.slice(mid)));
}

function merge(left, right) {
    let result = [], i = 0, j = 0;
    while (i < left.length && j < right.length) {
        result.push(left[i] <= right[j] ? left[i++] : right[j++]);
    }
    return result.concat(left.slice(i), right.slice(j));
}
```
**Use**: When stability is required, external sorting, linked lists

### Quick Sort ⭐⭐⭐⭐⭐
```javascript
// O(n log n) avg, O(n²) worst, O(log n) space, Unstable, In-place
function quickSort(arr, low = 0, high = arr.length - 1) {
    if (low < high) {
        const pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

function partition(arr, low, high) {
    const pivot = arr[high];
    let i = low - 1;
    for (let j = low; j < high; j++) {
        if (arr[j] < pivot) {
            [arr[++i], arr[j]] = [arr[j], arr[i]];
        }
    }
    [arr[i + 1], arr[high]] = [arr[high], arr[i + 1]];
    return i + 1;
}
```
**Use**: General purpose, cache-efficient, when average case matters

### Heap Sort ⭐⭐⭐⭐
```javascript
// O(n log n) time, O(1) space, Unstable, In-place
function heapSort(arr) {
    buildMaxHeap(arr);
    for (let i = arr.length - 1; i > 0; i--) {
        [arr[0], arr[i]] = [arr[i], arr[0]];
        heapify(arr, i, 0);
    }
}

function buildMaxHeap(arr) {
    for (let i = Math.floor(arr.length / 2) - 1; i >= 0; i--) {
        heapify(arr, arr.length, i);
    }
}
```
**Use**: When guaranteed O(n log n) and O(1) space needed

## 4. Specialized Sorting Algorithms

### Counting Sort ⭐⭐⭐
```javascript
// O(n + k) time, O(k) space, Stable, Not in-place
function countingSort(arr) {
    const max = Math.max(...arr);
    const count = new Array(max + 1).fill(0);
    
    // Count occurrences
    arr.forEach(num => count[num]++);
    
    // Reconstruct array
    let idx = 0;
    count.forEach((freq, num) => {
        while (freq-- > 0) arr[idx++] = num;
    });
}
```
**Use**: Small range of integers, when k = O(n)

### Radix Sort ⭐⭐⭐⭐
```javascript
// O(d * n) time, O(n + k) space, Stable, Not in-place
function radixSort(arr) {
    const max = Math.max(...arr);
    for (let exp = 1; Math.floor(max / exp) > 0; exp *= 10) {
        countingSortByDigit(arr, exp);
    }
}
```
**Use**: Large integers, strings, when d is small

### Bucket Sort ⭐⭐⭐
```javascript
// O(n + k) avg, O(n²) worst, O(n) space, Stable
function bucketSort(arr, bucketSize = 5) {
    const min = Math.min(...arr);
    const max = Math.max(...arr);
    const bucketCount = Math.floor((max - min) / bucketSize) + 1;
    const buckets = Array(bucketCount).fill().map(() => []);
    
    // Distribute elements
    arr.forEach(num => {
        buckets[Math.floor((num - min) / bucketSize)].push(num);
    });
    
    // Sort and concatenate
    return buckets.reduce((sorted, bucket) => {
        return sorted.concat(bucket.sort((a, b) => a - b));
    }, []);
}
```
**Use**: Uniformly distributed data, floating point numbers

## 5. Advanced Sorting Concepts

### Hybrid Sorting (Introsort) ⭐⭐⭐⭐⭐
```javascript
function introsort(arr, depthLimit = Math.floor(Math.log2(arr.length)) * 2) {
    if (arr.length <= 16) {
        return insertionSort(arr);
    } else if (depthLimit === 0) {
        return heapSort(arr);
    } else {
        return quickSort(arr, 0, arr.length - 1, depthLimit - 1);
    }
}
```

### 3-Way Quick Sort (Dutch National Flag) ⭐⭐⭐⭐
```javascript
function threeWayQuickSort(arr, low, high) {
    if (low >= high) return;
    
    const [lt, gt] = partition3Way(arr, low, high);
    threeWayQuickSort(arr, low, lt - 1);
    threeWayQuickSort(arr, gt + 1, high);
}
```
**Use**: Many duplicate elements

## 6. Time & Space Complexity Summary

| Algorithm | Best | Average | Worst | Space | Stable | In-place | Stars |
|-----------|------|---------|-------|-------|--------|----------|-------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | ✓ | ✓ | ⭐⭐ |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | ✗ | ✓ | ⭐⭐ |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | ✓ | ✓ | ⭐⭐⭐ |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | ✓ | ✗ | ⭐⭐⭐⭐ |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | ✗ | ✓ | ⭐⭐⭐⭐⭐ |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | ✗ | ✓ | ⭐⭐⭐⭐ |
| Counting | O(n + k) | O(n + k) | O(n + k) | O(k) | ✓ | ✗ | ⭐⭐⭐ |
| Radix | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | ✓ | ✗ | ⭐⭐⭐⭐ |
| Bucket | O(n + k) | O(n + k) | O(n²) | O(n) | ✓ | ✗ | ⭐⭐⭐ |

---

# 📚 Interview Questions by Difficulty Level

## Easy Level Questions 💚

### 1. Sort Array of 0s, 1s, and 2s (Dutch National Flag) ⭐⭐⭐⭐⭐
**Problem**: Sort an array containing only 0s, 1s, and 2s in O(n) time.

**Solution**:
```javascript
function sortColors(nums) {
    let low = 0, mid = 0, high = nums.length - 1;
    
    while (mid <= high) {
        if (nums[mid] === 0) {
            [nums[low], nums[mid]] = [nums[mid], nums[low]];
            low++;
            mid++;
        } else if (nums[mid] === 1) {
            mid++;
        } else {
            [nums[mid], nums[high]] = [nums[high], nums[mid]];
            high--;
        }
    }
}
```

### 2. Check if Array is Sorted ⭐⭐⭐
**Problem**: Check if an array is sorted in ascending order.

**Solution**:
```javascript
function isSorted(arr) {
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) return false;
    }
    return true;
}
```

### 3. Sort String Characters ⭐⭐⭐
**Problem**: Sort characters in a string alphabetically.

**Solution**:
```javascript
function sortString(str) {
    return str.split('').sort().join('');
}
```

### 4. Find Kth Largest Element ⭐⭐⭐⭐
**Problem**: Find the kth largest element in an unsorted array.

**Solution**:
```javascript
function findKthLargest(nums, k) {
    // Using quickselect
    function quickSelect(left, right, k) {
        if (left === right) return nums[left];
        
        const pivotIndex = partition(left, right);
        if (k === pivotIndex) return nums[k];
        else if (k < pivotIndex) return quickSelect(left, pivotIndex - 1, k);
        else return quickSelect(pivotIndex + 1, right, k);
    }
    
    return quickSelect(0, nums.length - 1, nums.length - k);
}
```

### 5. Merge Two Sorted Arrays ⭐⭐⭐⭐
**Problem**: Merge two sorted arrays into one sorted array.

**Solution**:
```javascript
function mergeSortedArrays(arr1, arr2) {
    let result = [], i = 0, j = 0;
    
    while (i < arr1.length && j < arr2.length) {
        if (arr1[i] <= arr2[j]) {
            result.push(arr1[i++]);
        } else {
            result.push(arr2[j++]);
        }
    }
    
    return result.concat(arr1.slice(i), arr2.slice(j));
}
```

## Medium Level Questions 🟡

### 1. Sort Array by Frequency ⭐⭐⭐⭐
**Problem**: Sort array elements by their frequency. If frequencies are same, sort by value.

**Solution**:
```javascript
function sortByFrequency(arr) {
    const freqMap = new Map();
    arr.forEach(num => freqMap.set(num, (freqMap.get(num) || 0) + 1));
    
    return arr.sort((a, b) => {
        const freqA = freqMap.get(a);
        const freqB = freqMap.get(b);
        if (freqA !== freqB) return freqB - freqA;
        return a - b;
    });
}
```

### 2. Custom Sort String ⭐⭐⭐⭐
**Problem**: Sort string based on custom order defined by another string.

**Solution**:
```javascript
function customSortString(order, str) {
    const orderMap = new Map();
    for (let i = 0; i < order.length; i++) {
        orderMap.set(order[i], i);
    }
    
    return str.split('').sort((a, b) => {
        const posA = orderMap.get(a) ?? order.length;
        const posB = orderMap.get(b) ?? order.length;
        return posA - posB;
    }).join('');
}
```

### 3. Sort Matrix Diagonally ⭐⭐⭐⭐
**Problem**: Sort each diagonal of a matrix independently.

**Solution**:
```javascript
function diagonalSort(mat) {
    const m = mat.length, n = mat[0].length;
    
    function sortDiagonal(row, col) {
        const diagonal = [];
        let r = row, c = col;
        
        while (r < m && c < n) {
            diagonal.push(mat[r][c]);
            r++;
            c++;
        }
        
        diagonal.sort((a, b) => a - b);
        
        r = row;
        c = col;
        let idx = 0;
        while (r < m && c < n) {
            mat[r][c] = diagonal[idx++];
            r++;
            c++;
        }
    }
    
    // Sort all diagonals
    for (let i = 0; i < m; i++) sortDiagonal(i, 0);
    for (let j = 1; j < n; j++) sortDiagonal(0, j);
    
    return mat;
}
```

### 4. Pancake Sorting ⭐⭐⭐⭐
**Problem**: Sort array using only flip operations (reverse subarray from 0 to k).

**Solution**:
```javascript
function pancakeSort(arr) {
    const result = [];
    
    function flip(k) {
        arr.slice(0, k + 1).reverse().forEach((val, i) => {
            arr[i] = val;
        });
    }
    
    for (let size = arr.length; size > 1; size--) {
        const maxIdx = arr.indexOf(Math.max(...arr.slice(0, size)));
        
        if (maxIdx !== size - 1) {
            if (maxIdx !== 0) {
                flip(maxIdx);
                result.push(maxIdx + 1);
            }
            flip(size - 1);
            result.push(size);
        }
    }
    
    return result;
}
```

### 5. Sort Linked List ⭐⭐⭐⭐⭐
**Problem**: Sort a linked list in O(n log n) time and O(1) space.

**Solution**:
```javascript
function sortList(head) {
    if (!head || !head.next) return head;
    
    // Find middle and split
    let slow = head, fast = head, prev = null;
    while (fast && fast.next) {
        prev = slow;
        slow = slow.next;
        fast = fast.next.next;
    }
    prev.next = null;
    
    // Recursively sort both halves
    const left = sortList(head);
    const right = sortList(slow);
    
    // Merge sorted halves
    return mergeLists(left, right);
}

function mergeLists(l1, l2) {
    const dummy = new ListNode(0);
    let curr = dummy;
    
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
    return dummy.next;
}
```

## Hard Level Questions 🔴

### 1. Merge K Sorted Arrays ⭐⭐⭐⭐⭐
**Problem**: Merge k sorted arrays into one sorted array.

**Solution**:
```javascript
function mergeKSortedArrays(arrays) {
    if (!arrays || arrays.length === 0) return [];
    
    while (arrays.length > 1) {
        const mergedArrays = [];
        
        for (let i = 0; i < arrays.length; i += 2) {
            const arr1 = arrays[i];
            const arr2 = arrays[i + 1] || [];
            mergedArrays.push(mergeTwoSorted(arr1, arr2));
        }
        
        arrays = mergedArrays;
    }
    
    return arrays[0];
}

function mergeTwoSorted(arr1, arr2) {
    const result = [];
    let i = 0, j = 0;
    
    while (i < arr1.length && j < arr2.length) {
        if (arr1[i] <= arr2[j]) {
            result.push(arr1[i++]);
        } else {
            result.push(arr2[j++]);
        }
    }
    
    return result.concat(arr1.slice(i), arr2.slice(j));
}
```

### 2. Count Inversions ⭐⭐⭐⭐⭐
**Problem**: Count number of inversions in an array (pairs where i < j but arr[i] > arr[j]).

**Solution**:
```javascript
function countInversions(arr) {
    let count = 0;
    
    function mergeAndCount(left, right) {
        const result = [];
        let i = 0, j = 0;
        
        while (i < left.length && j < right.length) {
            if (left[i] <= right[j]) {
                result.push(left[i++]);
            } else {
                result.push(right[j++]);
                count += left.length - i; // All remaining elements in left form inversions
            }
        }
        
        return result.concat(left.slice(i), right.slice(j));
    }
    
    function mergeSortAndCount(arr) {
        if (arr.length <= 1) return arr;
        
        const mid = Math.floor(arr.length / 2);
        const left = mergeSortAndCount(arr.slice(0, mid));
        const right = mergeSortAndCount(arr.slice(mid));
        
        return mergeAndCount(left, right);
    }
    
    mergeSortAndCount(arr);
    return count;
}
```

### 3. External Sorting ⭐⭐⭐⭐⭐
**Problem**: Sort a large file that doesn't fit in memory.

**Solution Approach**:
```javascript
// Conceptual implementation for external sorting
function externalSort(inputFile, outputFile, memoryLimit) {
    const tempFiles = [];
    
    // Phase 1: Create sorted runs
    while (hasMoreData(inputFile)) {
        const chunk = readChunk(inputFile, memoryLimit);
        chunk.sort((a, b) => a - b);
        const tempFile = writeToTempFile(chunk);
        tempFiles.push(tempFile);
    }
    
    // Phase 2: Merge sorted runs
    return mergeFiles(tempFiles, outputFile);
}

function mergeFiles(files, outputFile) {
    const minHeap = new MinHeap();
    const fileReaders = files.map(file => new FileReader(file));
    
    // Initialize heap with first element from each file
    fileReaders.forEach((reader, idx) => {
        const value = reader.readNext();
        if (value !== null) {
            minHeap.push({ value, fileIdx: idx });
        }
    });
    
    const writer = new FileWriter(outputFile);
    
    while (!minHeap.isEmpty()) {
        const { value, fileIdx } = minHeap.pop();
        writer.write(value);
        
        const nextValue = fileReaders[fileIdx].readNext();
        if (nextValue !== null) {
            minHeap.push({ value: nextValue, fileIdx });
        }
    }
    
    writer.close();
    return outputFile;
}
```

### 4. Wiggle Sort II ⭐⭐⭐⭐⭐
**Problem**: Rearrange array such that nums[0] < nums[1] > nums[2] < nums[3]...

**Solution**:
```javascript
function wiggleSort(nums) {
    const n = nums.length;
    const temp = [...nums].sort((a, b) => a - b);
    
    const mid = Math.floor((n - 1) / 2);
    let left = mid, right = n - 1;
    
    for (let i = 0; i < n; i++) {
        if (i % 2 === 0) {
            nums[i] = temp[left--];
        } else {
            nums[i] = temp[right--];
        }
    }
}
```

### 5. Minimum Number of Swaps to Sort ⭐⭐⭐⭐⭐
**Problem**: Find minimum number of swaps needed to sort an array.

**Solution**:
```javascript
function minSwapsToSort(arr) {
    const n = arr.length;
    const arrWithIdx = arr.map((val, idx) => ({ val, idx }));
    
    arrWithIdx.sort((a, b) => a.val - b.val);
    
    const visited = new Array(n).fill(false);
    let swaps = 0;
    
    for (let i = 0; i < n; i++) {
        if (visited[i] || arrWithIdx[i].idx === i) continue;
        
        let cycleSize = 0;
        let j = i;
        
        while (!visited[j]) {
            visited[j] = true;
            j = arrWithIdx[j].idx;
            cycleSize++;
        }
        
        if (cycleSize > 1) {
            swaps += cycleSize - 1;
        }
    }
    
    return swaps;
}
```

---

# 🎯 Key Interview Insights

## Most Asked Questions in FAANG
1. **Sort Colors (Dutch National Flag)** - Google, Facebook
2. **Merge K Sorted Lists** - Amazon, Microsoft
3. **Kth Largest Element** - Apple, Netflix
4. **Sort by Frequency** - LinkedIn, Uber
5. **Custom Sort String** - Google, Facebook

## Common Follow-up Questions
- "What if the array is already sorted?"
- "How would you optimize for space?"
- "Can you make it stable?"
- "What about duplicate elements?"
- "How would you handle very large datasets?"

## Red Flags to Avoid
- Not asking about data constraints
- Not considering edge cases (empty array, single element)
- Not explaining time/space complexity
- Not mentioning stability when relevant
- Using built-in sort without explaining the algorithm

## Pro Tips for Success
1. **Always ask about constraints** (array size, element range, stability requirements)
2. **Start with brute force** then optimize
3. **Explain your approach** before coding
4. **Test with examples** including edge cases
5. **Discuss trade-offs** between different algorithms
6. **Know when to use each algorithm** based on requirements

---

# 🚀 Advanced Topics for Senior Roles

## Parallel Sorting Algorithms
- **Merge Sort Parallelization**: Divide work among threads
- **Quick Sort Parallelization**: Partition in parallel
- **Bitonic Sort**: Network-based parallel sorting

## Cache-Aware Sorting
- **Block-based algorithms**: Minimize cache misses
- **Tiled algorithms**: Work with cache-sized blocks
- **Memory hierarchy optimization**: L1/L2/L3 cache considerations

## Distributed Sorting
- **MapReduce sorting**: Hadoop/Spark implementations
- **Distributed merge sort**: Across multiple machines
- **Consistent hashing**: For distributed data

Remember: The key to acing sorting interviews is understanding the trade-offs and being able to choose the right algorithm for the specific constraints!
