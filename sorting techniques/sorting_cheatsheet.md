# Complete Sorting Techniques Study Notes

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

### Bubble Sort
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

### Selection Sort
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

### Insertion Sort
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

### Merge Sort
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

### Quick Sort
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

### Heap Sort
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

### Counting Sort
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

### Radix Sort
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

### Bucket Sort
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

### Hybrid Sorting (Introsort)
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

### 3-Way Quick Sort (Dutch National Flag)
```javascript
function threeWayQuickSort(arr, low, high) {
    if (low >= high) return;
    
    const [lt, gt] = partition3Way(arr, low, high);
    threeWayQuickSort(arr, low, lt - 1);
    threeWayQuickSort(arr, gt + 1, high);
}
```
**Use**: Many duplicate elements

## 6. Sorting Applications

### Custom Comparators
```javascript
// Sort objects by multiple criteria
const students = [{name: "John", grade: 85}, {name: "Jane", grade: 92}];
students.sort((a, b) => {
    if (a.grade !== b.grade) return b.grade - a.grade; // Grade desc
    return a.name.localeCompare(b.name); // Name asc
});
```

### Sorting Variations
- **Partial sorting**: Only sort k smallest/largest elements
- **External sorting**: Sort data larger than memory using disk
- **Parallel sorting**: Utilize multiple processors
- **Stable sorting**: Maintain relative order of equal elements

## 7. Time & Space Complexity Summary

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | ✓ | ✓ |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | ✗ | ✓ |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | ✓ | ✓ |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | ✓ | ✗ |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | ✗ | ✓ |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | ✗ | ✓ |
| Counting | O(n + k) | O(n + k) | O(n + k) | O(k) | ✓ | ✗ |
| Radix | O(d(n + k)) | O(d(n + k)) | O(d(n + k)) | O(n + k) | ✓ | ✗ |
| Bucket | O(n + k) | O(n + k) | O(n²) | O(n) | ✓ | ✗ |

## 8. Decision Tree & Lower Bounds

### Comparison-Based Sorting Lower Bound
- Any comparison-based sorting algorithm requires **Ω(n log n)** comparisons in worst case
- Decision tree has at least **n!** leaves (all permutations)
- Tree height ≥ **log₂(n!) = Ω(n log n)**

### When to Use Which Algorithm

**Small arrays (n < 50)**: Insertion Sort
**General purpose**: Quick Sort or Merge Sort
**Stability required**: Merge Sort
**Memory constrained**: Heap Sort
**Known range**: Counting Sort
**String/Integer keys**: Radix Sort
**Uniform distribution**: Bucket Sort

## 9. Optimization Techniques

### Quick Sort Optimizations
- **Median-of-3 pivot**: Choose median of first, middle, last
- **Cutoff to insertion sort**: Switch to insertion for small subarrays
- **3-way partitioning**: Handle duplicates efficiently

### Memory Optimizations
- **Tail recursion**: Optimize recursive calls
- **Iterative implementations**: Avoid stack overflow
- **In-place merging**: Reduce space complexity

## 10. Interview Tips & Common Questions

### Implementation Questions
- "Sort an array of 0s, 1s, and 2s" → Dutch National Flag
- "Sort colors/balls" → Counting Sort or 3-way partition
- "Merge k sorted arrays" → Min heap approach
- "Sort large file" → External merge sort

### Analysis Questions
- "Why is Quick Sort faster than Merge Sort in practice?" → Cache locality, in-place
- "When would you use Heap Sort over Quick Sort?" → Guaranteed O(n log n), real-time systems
- "Explain stability with example" → Sorting students by grade then name

### Edge Cases to Consider
- Empty array or single element
- Already sorted (best case for some algorithms)
- Reverse sorted (worst case for some algorithms)
- All elements identical
- Very large or very small datasets

## 11. Built-in Sort Functions

### JavaScript Array.sort()
```javascript
// Uses Timsort (hybrid stable sort)
arr.sort(); // Lexicographic by default
arr.sort((a, b) => a - b); // Numeric ascending
arr.sort((a, b) => b - a); // Numeric descending
```

### Performance Characteristics
- **Timsort**: O(n) best, O(n log n) worst, stable
- **Optimized for real-world data**: Handles partially sorted data well
- **Adaptive**: Identifies and exploits existing order in data
