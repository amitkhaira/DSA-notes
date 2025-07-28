# Recursion Techniques - Complete Interview Cheat Sheet

**Recursion**: A function that calls itself to solve smaller subproblems of the same type.

## Essential Components ⭐⭐⭐⭐⭐
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

### Binary Recursion ⭐⭐⭐⭐⭐ (Two calls per execution)
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

## 3. Backtracking Pattern ⭐⭐⭐⭐⭐
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

## 4. Divide & Conquer ⭐⭐⭐⭐⭐
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

## 5. Memoization (Top-Down DP) ⭐⭐⭐⭐⭐
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

---

# 📚 INTERVIEW QUESTIONS BY DIFFICULTY

## 🟢 Easy Level Questions

### 1. Maximum Depth of Binary Tree ⭐⭐⭐⭐⭐
**Problem**: Find the maximum depth of a binary tree.

```javascript
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```
**Time**: O(n), **Space**: O(h) where h is height

### 2. Factorial ⭐⭐⭐⭐
**Problem**: Calculate factorial of n.

```javascript
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```
**Time**: O(n), **Space**: O(n)

### 3. Fibonacci Number ⭐⭐⭐⭐⭐
**Problem**: Find nth Fibonacci number.

```javascript
function fib(n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}

// Optimized with memoization
function fibMemo(n, memo = {}) {
    if (n in memo) return memo[n];
    if (n <= 1) return n;
    return memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
}
```
**Time**: O(2^n) → O(n) with memo, **Space**: O(n)

### 4. Reverse String ⭐⭐⭐
**Problem**: Reverse a string using recursion.

```javascript
function reverseString(str) {
    if (str.length <= 1) return str;
    return reverseString(str.substring(1)) + str.charAt(0);
}
```
**Time**: O(n), **Space**: O(n)

### 5. Sum of Array ⭐⭐⭐
**Problem**: Find sum of all elements in array.

```javascript
function arraySum(arr, index = 0) {
    if (index >= arr.length) return 0;
    return arr[index] + arraySum(arr, index + 1);
}
```
**Time**: O(n), **Space**: O(n)

### 6. Power Function ⭐⭐⭐⭐
**Problem**: Calculate x^n using recursion.

```javascript
function power(x, n) {
    if (n === 0) return 1;
    if (n < 0) return 1 / power(x, -n);
    
    // Optimized: O(log n)
    if (n % 2 === 0) {
        const half = power(x, n / 2);
        return half * half;
    }
    return x * power(x, n - 1);
}
```
**Time**: O(log n), **Space**: O(log n)

### 7. Check Palindrome ⭐⭐⭐⭐
**Problem**: Check if string is palindrome.

```javascript
function isPalindrome(str, left = 0, right = str.length - 1) {
    if (left >= right) return true;
    if (str[left] !== str[right]) return false;
    return isPalindrome(str, left + 1, right - 1);
}
```
**Time**: O(n), **Space**: O(n)

## 🟡 Medium Level Questions

### 1. Binary Tree Paths ⭐⭐⭐⭐⭐
**Problem**: Find all root-to-leaf paths in binary tree.

```javascript
function binaryTreePaths(root) {
    const result = [];
    
    function dfs(node, path) {
        if (!node) return;
        
        path += node.val;
        
        if (!node.left && !node.right) {
            result.push(path);
            return;
        }
        
        path += '->';
        dfs(node.left, path);
        dfs(node.right, path);
    }
    
    dfs(root, '');
    return result;
}
```
**Time**: O(n), **Space**: O(n×h)

### 2. Generate Parentheses ⭐⭐⭐⭐⭐
**Problem**: Generate all valid combinations of n pairs of parentheses.

```javascript
function generateParenthesis(n) {
    const result = [];
    
    function backtrack(current, open, close) {
        if (current.length === 2 * n) {
            result.push(current);
            return;
        }
        
        if (open < n) {
            backtrack(current + '(', open + 1, close);
        }
        
        if (close < open) {
            backtrack(current + ')', open, close + 1);
        }
    }
    
    backtrack('', 0, 0);
    return result;
}
```
**Time**: O(4^n / √n), **Space**: O(4^n / √n)

### 3. Combination Sum ⭐⭐⭐⭐⭐
**Problem**: Find all unique combinations that sum to target.

```javascript
function combinationSum(candidates, target) {
    const result = [];
    
    function backtrack(start, path, remaining) {
        if (remaining === 0) {
            result.push([...path]);
            return;
        }
        
        for (let i = start; i < candidates.length; i++) {
            if (candidates[i] <= remaining) {
                path.push(candidates[i]);
                backtrack(i, path, remaining - candidates[i]);
                path.pop();
            }
        }
    }
    
    backtrack(0, [], target);
    return result;
}
```
**Time**: O(2^target), **Space**: O(target)

### 4. Word Search ⭐⭐⭐⭐⭐
**Problem**: Find if word exists in 2D board.

```javascript
function exist(board, word) {
    function dfs(i, j, index) {
        if (index === word.length) return true;
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return false;
        if (board[i][j] !== word[index]) return false;
        
        const temp = board[i][j];
        board[i][j] = '#'; // Mark as visited
        
        const found = dfs(i+1, j, index+1) || dfs(i-1, j, index+1) || 
                      dfs(i, j+1, index+1) || dfs(i, j-1, index+1);
        
        board[i][j] = temp; // Backtrack
        return found;
    }
    
    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[0].length; j++) {
            if (dfs(i, j, 0)) return true;
        }
    }
    return false;
}
```
**Time**: O(m×n×4^L), **Space**: O(L)

### 5. Decode Ways ⭐⭐⭐⭐
**Problem**: Count ways to decode a string of digits.

```javascript
function numDecodings(s) {
    const memo = {};
    
    function dp(index) {
        if (index in memo) return memo[index];
        if (index === s.length) return 1;
        if (s[index] === '0') return 0;
        
        let result = dp(index + 1);
        
        if (index + 1 < s.length) {
            const twoDigit = parseInt(s.substring(index, index + 2));
            if (twoDigit <= 26) {
                result += dp(index + 2);
            }
        }
        
        return memo[index] = result;
    }
    
    return dp(0);
}
```
**Time**: O(n), **Space**: O(n)

### 6. Letter Combinations of Phone Number ⭐⭐⭐⭐
**Problem**: Generate all letter combinations for phone number.

```javascript
function letterCombinations(digits) {
    if (!digits) return [];
    
    const mapping = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };
    
    const result = [];
    
    function backtrack(index, path) {
        if (index === digits.length) {
            result.push(path);
            return;
        }
        
        const letters = mapping[digits[index]];
        for (let letter of letters) {
            backtrack(index + 1, path + letter);
        }
    }
    
    backtrack(0, '');
    return result;
}
```
**Time**: O(4^n), **Space**: O(4^n)

## 🔴 Hard Level Questions

### 1. N-Queens ⭐⭐⭐⭐⭐
**Problem**: Place N queens on N×N chessboard so they don't attack each other.

```javascript
function solveNQueens(n) {
    const result = [];
    const board = Array(n).fill().map(() => Array(n).fill('.'));
    
    function isSafe(row, col) {
        // Check column
        for (let i = 0; i < row; i++) {
            if (board[i][col] === 'Q') return false;
        }
        
        // Check diagonal
        for (let i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] === 'Q') return false;
        }
        
        for (let i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] === 'Q') return false;
        }
        
        return true;
    }
    
    function backtrack(row) {
        if (row === n) {
            result.push(board.map(row => row.join('')));
            return;
        }
        
        for (let col = 0; col < n; col++) {
            if (isSafe(row, col)) {
                board[row][col] = 'Q';
                backtrack(row + 1);
                board[row][col] = '.';
            }
        }
    }
    
    backtrack(0);
    return result;
}
```
**Time**: O(n!), **Space**: O(n²)

### 2. Sudoku Solver ⭐⭐⭐⭐⭐
**Problem**: Solve 9×9 Sudoku puzzle.

```javascript
function solveSudoku(board) {
    function isValid(row, col, num) {
        // Check row
        for (let j = 0; j < 9; j++) {
            if (board[row][j] === num) return false;
        }
        
        // Check column
        for (let i = 0; i < 9; i++) {
            if (board[i][col] === num) return false;
        }
        
        // Check 3x3 box
        const boxRow = Math.floor(row / 3) * 3;
        const boxCol = Math.floor(col / 3) * 3;
        for (let i = boxRow; i < boxRow + 3; i++) {
            for (let j = boxCol; j < boxCol + 3; j++) {
                if (board[i][j] === num) return false;
            }
        }
        
        return true;
    }
    
    function backtrack() {
        for (let i = 0; i < 9; i++) {
            for (let j = 0; j < 9; j++) {
                if (board[i][j] === '.') {
                    for (let num = '1'; num <= '9'; num++) {
                        if (isValid(i, j, num)) {
                            board[i][j] = num;
                            if (backtrack()) return true;
                            board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
    
    backtrack();
}
```
**Time**: O(9^m), **Space**: O(m) where m is empty cells

### 3. Word Break II ⭐⭐⭐⭐⭐
**Problem**: Return all possible sentences from word break.

```javascript
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict);
    const memo = {};
    
    function dp(start) {
        if (start in memo) return memo[start];
        if (start === s.length) return [''];
        
        const result = [];
        for (let end = start + 1; end <= s.length; end++) {
            const word = s.substring(start, end);
            if (wordSet.has(word)) {
                const suffixes = dp(end);
                for (let suffix of suffixes) {
                    result.push(word + (suffix === '' ? '' : ' ' + suffix));
                }
            }
        }
        
        return memo[start] = result;
    }
    
    return dp(0);
}
```
**Time**: O(2^n), **Space**: O(2^n)

### 4. Expression Add Operators ⭐⭐⭐⭐⭐
**Problem**: Add operators (+, -, *) to make expression equal target.

```javascript
function addOperators(num, target) {
    const result = [];
    
    function backtrack(index, path, value, prev) {
        if (index === num.length) {
            if (value === target) {
                result.push(path);
            }
            return;
        }
        
        for (let i = index; i < num.length; i++) {
            const curr = num.substring(index, i + 1);
            const currNum = parseInt(curr);
            
            // Skip numbers with leading zeros
            if (curr.length > 1 && curr[0] === '0') break;
            
            if (index === 0) {
                backtrack(i + 1, curr, currNum, currNum);
            } else {
                // Addition
                backtrack(i + 1, path + '+' + curr, value + currNum, currNum);
                
                // Subtraction
                backtrack(i + 1, path + '-' + curr, value - currNum, -currNum);
                
                // Multiplication
                backtrack(i + 1, path + '*' + curr, 
                         value - prev + prev * currNum, prev * currNum);
            }
        }
    }
    
    backtrack(0, '', 0, 0);
    return result;
}
```
**Time**: O(4^n), **Space**: O(n)

### 5. Regular Expression Matching ⭐⭐⭐⭐⭐
**Problem**: Implement regex matching with '.' and '*'.

```javascript
function isMatch(s, p) {
    const memo = {};
    
    function dp(i, j) {
        const key = `${i},${j}`;
        if (key in memo) return memo[key];
        
        if (j === p.length) return i === s.length;
        
        const firstMatch = i < s.length && (s[i] === p[j] || p[j] === '.');
        
        if (j + 1 < p.length && p[j + 1] === '*') {
            const result = dp(i, j + 2) || (firstMatch && dp(i + 1, j));
            return memo[key] = result;
        } else {
            const result = firstMatch && dp(i + 1, j + 1);
            return memo[key] = result;
        }
    }
    
    return dp(0, 0);
}
```
**Time**: O(m×n), **Space**: O(m×n)

---

## Complexity Quick Reference
- **Linear Recursion**: O(n) time, O(n) space
- **Binary Recursion**: O(2^n) time, O(n) space  
- **With Memoization**: O(n) time, O(n) space
- **Tail Optimized**: O(n) time, O(1) space
- **Backtracking**: O(b^d) where b=branching factor, d=depth

## Problem-Solving Steps ⭐⭐⭐⭐⭐
1. **Base case**: When to stop
2. **Recursive case**: How to break down
3. **Progress**: Move toward base case
4. **Combine**: How to merge results
5. **Optimize**: Add memoization if needed

## Common Interview Tips ⭐⭐⭐⭐⭐
1. **Always identify base case first**
2. **Draw recursion tree for complex problems**
3. **Consider memoization for overlapping subproblems**
4. **Think about space complexity (call stack)**
5. **Practice converting recursion to iteration**
6. **Master backtracking template**
7. **Understand when to use divide & conquer**
