# Recursion Techniques - Quick Notes

**Recursion**: A function that calls itself to solve smaller subproblems of the same type.

### Essential Components
1. **Base Case**: Condition that stops recursion
2. **Recursive Case**: Function calls itself with modified parameters
3. **Progress**: Each call moves closer to base case


## Basic Template

```javascript
function recursiveFunction(params) {
    // Base case
    if (baseCondition) {
        return baseValue;
    }
    
    // Recursive case
    return recursiveFunction(modifiedParams);
}
```

## 1. Basic Recursion Types

### Linear Recursion (One call per execution)
```javascript
// Factorial: n! = n × (n-1)!
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// Sum: sum(n) = n + sum(n-1)
const sum = n => n <= 0 ? 0 : n + sum(n-1);
```

### Binary Recursion (Two calls per execution)
```javascript
// Fibonacci: fib(n) = fib(n-1) + fib(n-2)
const fib = n => n <= 1 ? n : fib(n-1) + fib(n-2);

// Binary search
const binarySearch = (arr, target, left=0, right=arr.length-1) => {
    if (left > right) return -1;
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    return arr[mid] > target ? 
        binarySearch(arr, target, left, mid-1) : 
        binarySearch(arr, target, mid+1, right);
};
```

### Multiple Recursion (3+ calls per execution)
```javascript
// Tree traversal
const traverse = node => {
    if (!node) return;
    process(node);
    node.children.forEach(child => traverse(child));
};
```

## 2. Tail Recursion
Last operation is recursive call - can be optimized to iteration.

```javascript
// Tail recursive factorial
function factorialTail(n, acc = 1) {
    if (n <= 1) return acc;
    return factorialTail(n - 1, n * acc);
}

// Tail recursive sum
const sumTail = (arr, i=0, acc=0) => 
    i >= arr.length ? acc : sumTail(arr, i+1, acc+arr[i]);
```

## 3. Backtracking Pattern
Try → Recurse → Undo

```javascript
function backtrack(path, options) {
    if (isComplete(path)) { saveResult(path); return; }
    
    for (let option of options) {
        if (isValid(option, path)) {
            path.push(option);        // Try
            backtrack(path, newOptions); // Recurse
            path.pop();               // Undo
        }
    }
}
```

### Key Backtracking Problems
```javascript
// Permutations
const permute = nums => {
    const result = [];
    const backtrack = path => {
        if (path.length === nums.length) { result.push([...path]); return; }
        for (let num of nums) {
            if (!path.includes(num)) {
                path.push(num); backtrack(path); path.pop();
            }
        }
    };
    backtrack([]); return result;
};

// Subsets
const subsets = nums => {
    const result = [];
    const backtrack = (start, path) => {
        result.push([...path]);
        for (let i = start; i < nums.length; i++) {
            path.push(nums[i]); backtrack(i+1, path); path.pop();
        }
    };
    backtrack(0, []); return result;
};
```

## 4. Divide & Conquer
Split problem → Solve subproblems → Combine results

```javascript
// Merge Sort
const mergeSort = arr => {
    if (arr.length <= 1) return arr;
    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));
    return merge(left, right);
};

// Quick Sort
const quickSort = arr => {
    if (arr.length <= 1) return arr;
    const pivot = arr[0];
    const left = arr.slice(1).filter(x => x < pivot);
    const right = arr.slice(1).filter(x => x >= pivot);
    return [...quickSort(left), pivot, ...quickSort(right)];
};
```

## 5. Memoization (Top-Down DP)
Cache results to avoid recomputation.

```javascript
// Fibonacci with memo
const fibMemo = (n, memo={}) => {
    if (n in memo) return memo[n];
    if (n <= 1) return n;
    return memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
};

// Generic memoization pattern
function memoize(fn) {
    const cache = new Map();
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn.apply(this, args);
        cache.set(key, result);
        return result;
    };
}
```

## 6. Tree Recursion Patterns

### Binary Tree
```javascript
// Depth
const maxDepth = root => !root ? 0 : 1 + Math.max(maxDepth(root.left), maxDepth(root.right));

// Path Sum
const hasPathSum = (root, sum) => {
    if (!root) return false;
    if (!root.left && !root.right) return root.val === sum;
    return hasPathSum(root.left, sum-root.val) || hasPathSum(root.right, sum-root.val);
};

// Traversals  
const inorder = root => !root ? [] : [...inorder(root.left), root.val, ...inorder(root.right)];
const preorder = root => !root ? [] : [root.val, ...preorder(root.left), ...preorder(root.right)];
const postorder = root => !root ? [] : [...postorder(root.left), ...postorder(root.right), root.val];
```

## 7. Graph Recursion (DFS)
```javascript
const dfs = (graph, node, visited = new Set()) => {
    if (visited.has(node)) return;
    visited.add(node);
    process(node);
    graph[node].forEach(neighbor => dfs(graph, neighbor, visited));
};
```

## 8. String/Array Recursion

### String Processing
```javascript
// Reverse
const reverse = str => str.length <= 1 ? str : reverse(str.slice(1)) + str[0];

// Palindrome
const isPalindrome = (str, l=0, r=str.length-1) => 
    l >= r ? true : str[l] !== str[r] ? false : isPalindrome(str, l+1, r-1);
```

### Array Processing
```javascript
// Find element
const find = (arr, target, i=0) => 
    i >= arr.length ? -1 : arr[i] === target ? i : find(arr, target, i+1);

// Filter
const filterRecursive = (arr, predicate, i=0) => 
    i >= arr.length ? [] : 
    predicate(arr[i]) ? [arr[i], ...filterRecursive(arr, predicate, i+1)] : 
    filterRecursive(arr, predicate, i+1);
```

## 9. Advanced Patterns

### Mutual Recursion
```javascript
const isEven = n => n === 0 ? true : isOdd(n-1);
const isOdd = n => n === 0 ? false : isEven(n-1);
```

### Nested Recursion
```javascript
const ackermann = (m, n) => 
    m === 0 ? n + 1 :
    n === 0 ? ackermann(m-1, 1) :
    ackermann(m-1, ackermann(m, n-1));
```

## 10. Optimization Techniques

### Convert to Iteration
```javascript
// Recursive → Iterative using stack
const iterativeDFS = (root) => {
    if (!root) return;
    const stack = [root];
    while (stack.length) {
        const node = stack.pop();
        process(node);
        if (node.right) stack.push(node.right);
        if (node.left) stack.push(node.left);
    }
};
```

### Space Optimization
```javascript
// Use accumulator to reduce stack space
const sumAcc = (arr, i=0, acc=0) => 
    i >= arr.length ? acc : sumAcc(arr, i+1, acc+arr[i]);
```

## Complexity Quick Reference
- **Linear**: O(n) time, O(n) space
- **Binary**: O(2^n) time, O(n) space  
- **With Memo**: O(n) time, O(n) space
- **Tail Optimized**: O(n) time, O(1) space

## Problem-Solving Steps
1. **Base case**: When to stop
2. **Recursive case**: How to break down
3. **Progress**: Move toward base case
4. **Combine**: How to merge results
5. **Optimize**: Add memoization if needed
