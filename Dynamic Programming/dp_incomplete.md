# Complete Dynamic Programming Interview Cheat Sheet

## Table of Contents
1. [Core Concepts](#core-concepts)
2. [DP Identification](#dp-identification)
3. [Implementation Approaches](#implementation-approaches)
4. [DP Patterns & Categories](#dp-patterns--categories)
5. [Easy Level Questions](#easy-level-questions)
6. [Medium Level Questions](#medium-level-questions)
7. [Hard Level Questions](#hard-level-questions)
8. [Advanced DP Techniques](#advanced-dp-techniques)
9. [Optimization Strategies](#optimization-strategies)
10. [Interview Strategy](#interview-strategy)
11. [Quick Reference](#quick-reference)

## Core Concepts

### What is Dynamic Programming?
- **Algorithmic paradigm** that solves complex problems by breaking them down into simpler subproblems
- **Stores results** of subproblems to avoid redundant calculations
- **Optimization technique** for problems with overlapping subproblems and optimal substructure

### Two Essential Properties

#### 1. Optimal Substructure
- Optimal solution to problem contains optimal solutions to subproblems
- **Example**: Shortest path from A to C through B = Shortest(A→B) + Shortest(B→C)

#### 2. Overlapping Subproblems
- Same subproblems are solved multiple times in naive recursive approach
- **Example**: Fibonacci - fib(n) calls fib(n-1) and fib(n-2), both call fib(n-2)

### DP vs Other Paradigms
- **DP vs Divide & Conquer**: DP has overlapping subproblems, D&C has independent subproblems
- **DP vs Greedy**: DP considers all possibilities, Greedy makes locally optimal choices
- **DP vs Backtracking**: DP stores solutions, Backtracking explores all paths

## DP Identification

### When to Use DP?
1. **Optimization problems** (min/max)
2. **Counting problems** (number of ways)
3. **Decision problems** (possible/impossible)
4. **Game theory problems**
5. **String/sequence problems**

### Red Flags for DP
- Keywords: "optimal", "maximum", "minimum", "longest", "shortest", "count ways"
- Multiple ways to solve the same subproblem
- Recursive solution with exponential time complexity
- Making series of choices/decisions

## Implementation Approaches

### 1. Memoization (Top-Down)
```javascript
function solveMemo(params, memo = new Map()) {
    if (baseCondition) return baseResult;
    
    const key = JSON.stringify(params);
    if (memo.has(key)) return memo.get(key);
    
    let result = /* recursive calls */;
    
    memo.set(key, result);
    return result;
}
```

### 2. Tabulation (Bottom-Up)
```javascript
function solveTab(n) {
    const dp = new Array(n + 1);
    dp[0] = base0;
    dp[1] = base1;
    
    for (let i = 2; i <= n; i++) {
        dp[i] = /* compute using previous values */;
    }
    
    return dp[n];
}
```

### 3. Space Optimization
```javascript
function fibOptimized(n) {
    if (n <= 1) return n;
    
    let prev2 = 0, prev1 = 1;
    for (let i = 2; i <= n; i++) {
        const curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## DP Patterns & Categories

### 1. Linear DP (1D)
**Pattern**: `dp[i]` depends on `dp[i-1]`, `dp[i-2]`, etc.

```javascript
// Template
function linearDP(arr) {
    const dp = new Array(arr.length);
    dp[0] = /* base case */;
    
    for (let i = 1; i < arr.length; i++) {
        dp[i] = /* function of dp[i-1], dp[i-2], ... */;
    }
    return dp[arr.length - 1];
}
```

**Examples**: Fibonacci, Climbing Stairs, House Robber, Maximum Subarray

### 2. Grid DP (2D)
**Pattern**: `dp[i][j]` depends on `dp[i-1][j]`, `dp[i][j-1]`, `dp[i-1][j-1]`

```javascript
// Template
function gridDP(grid) {
    const m = grid.length, n = grid[0].length;
    const dp = Array(m).fill().map(() => Array(n).fill(0));
    
    // Initialize first row and column
    for (let i = 0; i < m; i++) dp[i][0] = /* base case */;
    for (let j = 0; j < n; j++) dp[0][j] = /* base case */;
    
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = /* function of dp[i-1][j], dp[i][j-1], etc. */;
        }
    }
    return dp[m-1][n-1];
}
```

**Examples**: Unique Paths, Minimum Path Sum, Longest Common Subsequence

### 3. Knapsack DP
#### 0/1 Knapsack (Bounded)
```javascript
function knapsack01(weights, values, capacity) {
    const n = weights.length;
    const dp = Array(n+1).fill().map(() => Array(capacity+1).fill(0));
    
    for (let i = 1; i <= n; i++) { // Current item is at index i-1 in values/weights
        const currentWeight = weights[i - 1];
        const currentValue = values[i - 1];
        for (let w = 0; w <= capacity; w++) { // Current capacity
            if (currentWeight <= w) {
                // Option 1: Include the current item (i-1)
                // Option 2: Exclude the current item (i-1)
                dp[i][w] = Math.max(
                    dp[i - 1][w],                            // Excluded
                    currentValue + dp[i - 1][w - currentWeight] // Included
                );
            } else {
                // Cannot include the current item as its weight > w
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    return dp[n][capacity];
}
```

#### Unbounded Knapsack
```javascript
function knapsackUnbounded(weights, values, capacity) {
    const dp = new Array(capacity + 1).fill(0);
    
    for (let w = 1; w <= capacity; w++) {
        for (let i = 0; i < weights.length; i++) {
            if (weights[i] <= w) {
                dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
            }
        }
    }
    return dp[capacity];
}
```

### 4. Interval DP
**Pattern**: `dp[i][j]` represents optimal solution for interval `[i, j]`

```javascript
// Template
function intervalDP(arr) {
    const n = arr.length;
    const dp = Array(n).fill().map(() => Array(n).fill(0));
    
    // Length 1 intervals
    for (let i = 0; i < n; i++) {
        dp[i][i] = /* base case */;
    }
    
    // Fill for increasing lengths
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            dp[i][j] = Infinity; // or -Infinity for max
            
            for (let k = i; k < j; k++) {
                dp[i][j] = Math.min(dp[i][j], 
                    dp[i][k] + dp[k+1][j] + /* cost of merging */);
            }
        }
    }
    return dp[0][n-1];
}
```

**Examples**: Matrix Chain Multiplication, Burst Balloons, Palindrome Partitioning

### 5. Tree DP
**Pattern**: DP on tree structures

```javascript
// Template for tree DP
function treeDP(root) {
    if (!root) return [0, 0]; // [include, exclude]
    
    const left = treeDP(root.left);
    const right = treeDP(root.right);
    
    const include = root.val + left[1] + right[1];
    const exclude = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
    
    return [include, exclude];
}
```

**Examples**: House Robber III, Binary Tree Maximum Path Sum

### 6. State Machine DP
**Pattern**: Multiple states at each step

```javascript
// Template for state machine DP
function stateMachineDP(arr) {
    let state1 = /* initial value */;
    let state2 = /* initial value */;
    
    for (let i = 0; i < arr.length; i++) {
        const newState1 = /* transition from state2 */;
        const newState2 = /* transition from state1 */;
        
        state1 = newState1;
        state2 = newState2;
    }
    
    return Math.max(state1, state2);
}
```

**Examples**: Best Time to Buy/Sell Stock, Paint House

### 7. Digit DP
**Pattern**: DP on digits of numbers

```javascript
// Template for digit DP
function digitDP(num) {
    const digits = num.toString().split('').map(Number);
    const memo = new Map();
    
    function solve(pos, tight, started) {
        if (pos === digits.length) return started ? 1 : 0;
        
        const key = `${pos}-${tight}-${started}`;
        if (memo.has(key)) return memo.get(key);
        
        const limit = tight ? digits[pos] : 9;
        let result = 0;
        
        for (let digit = 0; digit <= limit; digit++) {
            const newTight = tight && (digit === limit);
            const newStarted = started || (digit > 0);
            result += solve(pos + 1, newTight, newStarted);
        }
        
        memo.set(key, result);
        return result;
    }
    
    return solve(0, true, false);
}
```

### 8. Bitmask DP
**Pattern**: DP with bitmask to represent states

```javascript
// Template for bitmask DP
function bitmaskDP(n) {
    const dp = new Array(1 << n).fill(0);
    dp[0] = 1; // Base case
    
    for (let mask = 0; mask < (1 << n); mask++) {
        if (dp[mask] === 0) continue;
        
        for (let i = 0; i < n; i++) {
            if (!(mask & (1 << i))) { // If bit i is not set
                const newMask = mask | (1 << i);
                dp[newMask] += dp[mask];
            }
        }
    }
    
    return dp[(1 << n) - 1];
}
```

**Examples**: Traveling Salesman Problem, Assignment Problem

---

# Easy Level Questions

## 1. Fibonacci Numbers ⭐⭐⭐⭐⭐
**Problem**: Find the nth Fibonacci number.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
// Recursive with Memoization - O(n) time, O(n) space
function fibonacci(n, memo = {}) {
    if (n <= 1) return n;
    if (n in memo) return memo[n];
    
    memo[n] = fibonacci(n-1, memo) + fibonacci(n-2, memo);
    return memo[n];
}

// Iterative - O(n) time, O(1) space
function fibonacciOptimized(n) {
    if (n <= 1) return n;
    let a = 0, b = 1;
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}
```

## 2. Climbing Stairs ⭐⭐⭐⭐⭐
**Problem**: You can climb 1 or 2 steps at a time. How many ways to reach nth step?
**Companies**: Amazon, Microsoft, Google, Apple

```javascript
// Bottom-up DP - O(n) time, O(1) space
function climbStairs(n) {
    if (n <= 2) return n;
    let prev2 = 1, prev1 = 2;
    for (let i = 3; i <= n; i++) {
        [prev2, prev1] = [prev1, prev1 + prev2];
    }
    return prev1;
}

// Variation: Different step sizes
function climbStairsVariation(n, steps) {
    const dp = new Array(n + 1).fill(0);
    dp[0] = 1;
    
    for (let i = 1; i <= n; i++) {
        for (const step of steps) {
            if (i >= step) {
                dp[i] += dp[i - step];
            }
        }
    }
    return dp[n];
}
```

## 3. Min Cost Climbing Stairs ⭐⭐⭐⭐
**Problem**: Pay cost[i] to climb from step i. Find minimum cost to reach top.
**Companies**: Amazon, Microsoft, Google

```javascript
function minCostClimbingStairs(cost) {
    const n = cost.length;
    let prev2 = 0, prev1 = 0;
    
    for (let i = 2; i <= n; i++) {
        const curr = Math.min(prev1 + cost[i-1], prev2 + cost[i-2]);
        prev2 = prev1;
        prev1 = curr;
    }
    
    return prev1;
}
```

## 4. House Robber ⭐⭐⭐⭐⭐
**Problem**: Rob houses without robbing adjacent ones. Maximize money.
**Companies**: Google, Amazon, Microsoft, Facebook

```javascript
function rob(nums) {
    let prev2 = 0, prev1 = 0;
    for (const num of nums) {
        [prev2, prev1] = [prev1, Math.max(prev1, prev2 + num)];
    }
    return prev1;
}

// With actual houses to rob
function robWithPath(nums) {
    const n = nums.length;
    const dp = new Array(n);
    const path = new Array(n).fill(false);
    
    dp[0] = nums[0];
    path[0] = true;
    if (n > 1) {
        dp[1] = Math.max(nums[0], nums[1]);
        path[1] = nums[1] > nums[0];
    }
    
    for (let i = 2; i < n; i++) {
        if (dp[i-1] > dp[i-2] + nums[i]) {
            dp[i] = dp[i-1];
            path[i] = false;
        } else {
            dp[i] = dp[i-2] + nums[i];
            path[i] = true;
        }
    }
    
    return { maxMoney: dp[n-1], houses: path };
}
```

## 5. Maximum Subarray (Kadane's Algorithm) ⭐⭐⭐⭐⭐
**Problem**: Find contiguous subarray with maximum sum.
**Companies**: Google, Microsoft, Amazon, Facebook, Netflix

```javascript
function maxSubArray(nums) {
    let maxSoFar = nums[0], maxEndingHere = nums[0];
    for (let i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}

// Return actual subarray
function maxSubArrayWithIndices(nums) {
    let maxSoFar = nums[0];
    let maxEndingHere = nums[0];
    let start = 0, end = 0, tempStart = 0;
    
    for (let i = 1; i < nums.length; i++) {
        if (maxEndingHere < 0) {
            maxEndingHere = nums[i];
            tempStart = i;
        } else {
            maxEndingHere += nums[i];
        }
        
        if (maxEndingHere > maxSoFar) {
            maxSoFar = maxEndingHere;
            start = tempStart;
            end = i;
        }
    }
    
    return {
        sum: maxSoFar,
        subarray: nums.slice(start, end + 1),
        indices: [start, end]
    };
}
```

## 6. Unique Paths ⭐⭐⭐⭐
**Problem**: Find number of unique paths from top-left to bottom-right in m×n grid.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
// 2D DP approach
function uniquePaths(m, n) {
    const dp = Array(m).fill().map(() => Array(n).fill(1));
    
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    
    return dp[m-1][n-1];
}

// Space optimized - O(n) space
function uniquePathsOptimized(m, n) {
    let dp = new Array(n).fill(1);
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[j] += dp[j-1];
        }
    }
    return dp[n-1];
}

// Mathematical approach - O(1) space
function uniquePathsMath(m, n) {
    let result = 1;
    for (let i = 0; i < Math.min(m-1, n-1); i++) {
        result = result * (m + n - 2 - i) / (i + 1);
    }
    return Math.round(result);
}
```

## 7. Range Sum Query - Immutable ⭐⭐⭐
**Problem**: Calculate sum of elements between indices i and j (inclusive).
**Companies**: Google, Microsoft, Amazon

```javascript
class NumArray {
    constructor(nums) {
        this.prefixSum = new Array(nums.length + 1).fill(0);
        for (let i = 0; i < nums.length; i++) {
            this.prefixSum[i + 1] = this.prefixSum[i] + nums[i];
        }
    }
    
    sumRange(left, right) {
        return this.prefixSum[right + 1] - this.prefixSum[left];
    }
}
```

## 8. Counting Bits ⭐⭐⭐⭐
**Problem**: Count number of 1s in binary representation of numbers 0 to n.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
function countBits(n) {
    const dp = new Array(n + 1).fill(0);
    
    for (let i = 1; i <= n; i++) {
        dp[i] = dp[i >> 1] + (i & 1);
    }
    
    return dp;
}

// Alternative approach
function countBitsAlt(n) {
    const dp = new Array(n + 1).fill(0);
    
    for (let i = 1; i <= n; i++) {
        dp[i] = dp[i & (i - 1)] + 1;
    }
    
    return dp;
}
```

---

# Medium Level Questions

## 1. Longest Increasing Subsequence ⭐⭐⭐⭐⭐
**Problem**: Find length of longest increasing subsequence.
**Companies**: Google, Microsoft, Amazon, Facebook, Netflix

```javascript
// O(n²) DP solution
function lengthOfLIS(nums) {
    const n = nums.length;
    const dp = new Array(n).fill(1);
    
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }
    
    return Math.max(...dp);
}

// O(n log n) optimized solution
function lengthOfLISOptimized(nums) {
    const tails = [];
    
    for (const num of nums) {
        let left = 0, right = tails.length;
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            if (tails[mid] < num) left = mid + 1;
            else right = mid;
        }
        
        if (left === tails.length) tails.push(num);
        else tails[left] = num;
    }
    
    return tails.length;
}

// Return actual LIS
function findLIS(nums) {
    const n = nums.length;
    const dp = new Array(n).fill(1);
    const prev = new Array(n).fill(-1);
    
    let maxLength = 1, maxIndex = 0;
    
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i] && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1;
                prev[i] = j;
            }
        }
        if (dp[i] > maxLength) {
            maxLength = dp[i];
            maxIndex = i;
        }
    }
    
    // Reconstruct LIS
    const lis = [];
    let current = maxIndex;
    while (current !== -1) {
        lis.unshift(nums[current]);
        current = prev[current];
    }
    
    return lis;
}
```

## 2. Coin Change ⭐⭐⭐⭐⭐
**Problem**: Find minimum number of coins to make amount.
**Companies**: Google, Microsoft, Amazon, Facebook, Apple

```javascript
// Bottom-up DP
function coinChange(coins, amount) {
    const dp = new Array(amount + 1).fill(Infinity);
    dp[0] = 0;
    
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    
    return dp[amount] === Infinity ? -1 : dp[amount];
}

// Return actual coins used
function coinChangeWithPath(coins, amount) {
    const dp = new Array(amount + 1).fill(Infinity);
    const coinUsed = new Array(amount + 1).fill(-1);
    dp[0] = 0;
    
    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i && dp[i - coin] + 1 < dp[i]) {
                dp[i] = dp[i - coin] + 1;
                coinUsed[i] = coin;
            }
        }
    }
    
    if (dp[amount] === Infinity) return null;
    
    // Reconstruct solution
    const result = [];
    let current = amount;
    while (current > 0) {
        const coin = coinUsed[current];
        result.push(coin);
        current -= coin;
    }
    
    return result;
}
```

## 3. Coin Change 2 ⭐⭐⭐⭐
**Problem**: Count number of ways to make amount using coins.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
function change(amount, coins) {
    const dp = new Array(amount + 1).fill(0);
    dp[0] = 1;
    
    for (const coin of coins) {
        for (let i = coin; i <= amount; i++) {
            dp[i] += dp[i - coin];
        }
    }
    
    return dp[amount];
}

// 2D DP version for clarity
function change2D(amount, coins) {
    const n = coins.length;
    const dp = Array(n + 1).fill().map(() => Array(amount + 1).fill(0));
    
    // Base case: one way to make amount 0
    for (let i = 0; i <= n; i++) {
        dp[i][0] = 1;
    }
    
    for (let i = 1; i <= n; i++) {
        for (let j = 1; j <= amount; j++) {
            dp[i][j] = dp[i-1][j]; // Don't use coin i
            if (j >= coins[i-1]) {
                dp[i][j] += dp[i][j - coins[i-1]]; // Use coin i
            }
        }
    }
    
    return dp[n][amount];
}
```

## 4. Longest Common Subsequence ⭐⭐⭐⭐⭐
**Problem**: Find length of longest common subsequence between two strings.
**Companies**: Google, Microsoft, Amazon, Facebook, Netflix

```javascript
function longestCommonSubsequence(text1, text2) {
    const m = text1.length, n = text2.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i-1] === text2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    
    return dp[m][n];
}

// Space optimized version
function longestCommonSubsequenceOptimized(text1, text2) {
    const n = text2.length;
    let prev = new Array(n + 1).fill(0);
    let curr = new Array(n + 1).fill(0);
    
    for (let i = 1; i <= text1.length; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i-1] === text2[j-1]) {
                curr[j] = prev[j-1] + 1;
            } else {
                curr[j] = Math.max(prev[j], curr[j-1]);
            }
        }
        [prev, curr] = [curr, prev];
    }
    
    return prev[n];
}

// Return actual LCS
function findLCS(text1, text2) {
    const m = text1.length, n = text2.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i-1] === text2[j-1]) {
                dp[i][j] = dp[i-1][j-1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
    }
    
    // Reconstruct LCS
    let lcs = "";
    let i = m, j = n;
    while (i > 0 && j > 0) {
        if (text1[i-1] === text2[j-1]) {
            lcs = text1[i-1] + lcs;
            i--;
            j--;
        } else if (dp[i-1][j] > dp[i][j-1]) {
            i--;
        } else {
            j--;
        }
    }
    
    return lcs;
}
```

## 5. Word Break ⭐⭐⭐⭐⭐
**Problem**: Check if string can be segmented into dictionary words.
**Companies**: Google, Microsoft, Amazon, Facebook, Apple

```javascript
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict);
    const dp = new Array(s.length + 1).fill(false);
    dp[0] = true;
    
    for (let i = 1; i <= s.length; i++) {
        for (let j = 0; j < i; j++) {
            if (dp[j] && wordSet.has(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[s.length];
}

// Return all possible sentences
function wordBreakII(s, wordDict) {
    const wordSet = new Set(wordDict);
    const memo = new Map();
    
    function backtrack(start) {
        if (start === s.length) return [''];
        if (memo.has(start)) return memo.get(start);
        
        const result = [];
        for (let end = start + 1; end <= s.length; end++) {
            const word = s.substring(start, end);
            if (wordSet.has(word)) {
                const suffixes = backtrack(end);
                for (const suffix of suffixes) {
                    result.push(word + (suffix === '' ? '' : ' ' + suffix));
                }
            }
        }
        
        memo.set(start, result);
        return result;
    }
    
    return backtrack(0);
}
```

## 6. House Robber II ⭐⭐⭐⭐
**Problem**: Houses arranged in circle, can't rob adjacent houses.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
function rob(nums) {
    const n = nums.length;
    if (n === 1) return nums[0];
    if (n === 2) return Math.max(nums[0], nums[1]);
    
    // Two scenarios: rob house 0 or not rob house 0
    const robFirst = robLinear(nums.slice(0, n-1));
    const skipFirst = robLinear(nums.slice(1));
    
    return Math.max(robFirst, skipFirst);
}

function robLinear(nums) {
    let prev2 = 0, prev1 = 0;
    for (const num of nums) {
        [prev2, prev1] = [prev1, Math.max(prev1, prev2 + num)];
    }
    return prev1;
}
```

## 7. Decode Ways ⭐⭐⭐⭐
**Problem**: Count ways to decode string where A=1, B=2, ..., Z=26.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
function numDecodings(s) {
    if (s[0] === '0') return 0;
    
    const n = s.length;
    let prev2 = 1, prev1 = 1;
    
    for (let i = 1; i < n; i++) {
        let curr = 0;
        
        // Single digit
        if (s[i] !== '0') {
            curr += prev1;
        }
        
        // Two digits
        const twoDigit = parseInt(s.substring(i-1, i+1));
        if (twoDigit >= 10 && twoDigit <= 26) {
            curr += prev2;
        }
        
        prev2 = prev1;
        prev1 = curr;
    }
    
    return prev1;
}

// With memoization
function numDecodingsMemo(s) {
    const memo = new Map();
    
    function decode(index) {
        if (index === s.length) return 1;
        if (s[index] === '0') return 0;
        if (memo.has(index)) return memo.get(index);
        
        let result = decode(index + 1);
        
        if (index + 1 < s.length) {
            const twoDigit = parseInt(s.substring(index, index + 2));
            if (twoDigit <= 26) {
                result += decode(index + 2);
            }
        }
        
        memo.set(index, result);
        return result;
    }
    
    return decode(0);
}
```

## 8. Unique Paths II ⭐⭐⭐⭐
**Problem**: Find unique paths with obstacles in grid.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
function uniquePathsWithObstacles(obstacleGrid) {
    const m = obstacleGrid.length;
    const n = obstacleGrid[0].length;
    
    if (obstacleGrid[0][0] === 1) return 0;
    
    const dp = Array(m).fill().map(() => Array(n).fill(0));
    dp[0][0] = 1;
    
    // Fill first row
    for (let j = 1; j < n; j++) {
        dp[0][j] = obstacleGrid[0][j] === 1 ? 0 : dp[0][j-1];
    }
    
    // Fill first column
    for (let i = 1; i < m; i++) {
        dp[i][0] = obstacleGrid[i][0] === 1 ? 0 : dp[i-1][0];
    }
    
    // Fill rest
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (obstacleGrid[i][j] === 1) {
                dp[i][j] = 0;
            } else {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
    }
    
    return dp[m-1][n-1];
}
```

## 9. Minimum Path Sum ⭐⭐⭐⭐
**Problem**: Find minimum sum path from top-left to bottom-right.
**Companies**: Google, Microsoft, Amazon, Facebook

```javascript
function minPathSum(grid) {
    const m = grid.length, n = grid[0].length;
    const dp = Array(m).fill().map(() => Array(n).fill(0));
    
    dp[0][0] = grid[0][0];
    
    // Fill first row
    for (let j = 1; j < n; j++) {
        dp[0][j] = dp[0][j-1] + grid[0][j];
    }
    
    // Fill first column
    for (let i = 1; i < m; i++) {
        dp[i][0] = dp[i-1][0] + grid[i][0];
    }
    
    // Fill rest
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
        }
    }
    
    return dp[m-1][n-1];
}

// Space optimized
function minPathSumOptimized(grid) {
    const n = grid[0].length;
    let dp = [...grid[0]];
    
    // Process cumulative sum for first row
    for (let j = 1; j < n; j++) {
        dp[j] += dp[j-1];
    }
    
    // Process remaining rows
    for (let i = 1; i < grid.length; i++) {
        dp[0] += grid[i][0];
        for (let j = 1; j < n; j++) {
            dp[j] = Math.min(dp[j], dp[j-1]) + grid[i][j];
        }
    }
    
    return dp[n-1];
}
```

## 10. Maximum Product Subarray ⭐⭐⭐⭐⭐
**Problem**: Find contiguous subarray with maximum product.
**Companies**: Google, Microsoft, Amazon, Facebook, Apple

```javascript
function maxProduct(nums) {
    let maxSoFar = nums[0];
    let maxEndingHere = nums[0];
    let minEndingHere = nums[0];
    
    for (let i = 1; i < nums.length; i++) {
        const curr = nums[i];
        const tempMax = Math.max(curr, maxEndingHere * curr, minEndingHere * curr);
        minEndingHere = Math.min(curr, maxEndingHere * curr, minEndingHere * curr);
        maxEndingHere = tempMax;
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
```

---

# Hard Level Questions

### 1. Regular Expression Matching ⭐⭐⭐⭐⭐
**Problem**: Given an input string `s` and a pattern `p`, implement regular expression matching with support for `.` and `*`.

**Company**: Google, Facebook, Microsoft, Amazon

```javascript
function isMatch(s, p) {
    const m = s.length, n = p.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(false));
    
    dp[0][0] = true;
    
    // Handle patterns like a*, a*b*, a*b*c*
    for (let j = 1; j <= n; j++) {
        if (p[j - 1] === '*') {
            dp[0][j] = dp[0][j - 2];
        }
    }
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (p[j - 1] === '*') {
                dp[i][j] = dp[i][j - 2]; // Zero occurrences
                if (matches(s, p, i, j - 1)) {
                    dp[i][j] = dp[i][j] || dp[i - 1][j]; // One or more occurrences
                }
            } else if (matches(s, p, i, j)) {
                dp[i][j] = dp[i - 1][j - 1];
            }
        }
    }
    
    return dp[m][n];
}

function matches(s, p, i, j) {
    return p[j - 1] === '.' || s[i - 1] === p[j - 1];
}
```

**Time**: O(m×n) | **Space**: O(m×n)

---

### 2. Wildcard Matching ⭐⭐⭐⭐⭐
**Problem**: Given an input string `s` and a pattern `p`, implement wildcard pattern matching with support for `?` and `*`.

**Company**: Google, Facebook, Microsoft

```javascript
function isMatch(s, p) {
    const m = s.length, n = p.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(false));
    
    dp[0][0] = true;
    
    // Handle leading * in pattern
    for (let j = 1; j <= n; j++) {
        if (p[j - 1] === '*') {
            dp[0][j] = dp[0][j - 1];
        }
    }
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (p[j - 1] === '*') {
                dp[i][j] = dp[i - 1][j] || dp[i][j - 1] || dp[i - 1][j - 1];
            } else if (p[j - 1] === '?' || s[i - 1] === p[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            }
        }
    }
    
    return dp[m][n];
}
```

**Time**: O(m×n) | **Space**: O(m×n)

---

### 3. Burst Balloons ⭐⭐⭐⭐⭐
**Problem**: Given n balloons, each with a number, burst all balloons to get maximum coins. When you burst balloon i, you get `nums[left] * nums[i] * nums[right]` coins.

**Company**: Google, Amazon, Facebook

```javascript
function maxCoins(nums) {
    const arr = [1, ...nums, 1];
    const n = arr.length;
    const dp = Array(n).fill().map(() => Array(n).fill(0));
    
    for (let len = 3; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            for (let k = i + 1; k < j; k++) {
                dp[i][j] = Math.max(dp[i][j], 
                    dp[i][k] + dp[k][j] + arr[i] * arr[k] * arr[j]);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

**Time**: O(n³) | **Space**: O(n²)

---

### 4. Edit Distance ⭐⭐⭐⭐⭐
**Problem**: Given two strings, find the minimum number of operations (insert, delete, replace) to convert one string to another.

**Company**: Google, Microsoft, Amazon, Facebook

```javascript
function minDistance(word1, word2) {
    const m = word1.length, n = word2.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    // Base cases
    for (let i = 0; i <= m; i++) dp[i][0] = i;
    for (let j = 0; j <= n; j++) dp[0][j] = j;
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i - 1][j],     // Delete
                    dp[i][j - 1],     // Insert
                    dp[i - 1][j - 1]  // Replace
                );
            }
        }
    }
    
    return dp[m][n];
}
```

**Time**: O(m×n) | **Space**: O(m×n)

---

### 5. Longest Valid Parentheses ⭐⭐⭐⭐⭐
**Problem**: Given a string containing just parentheses, find the length of the longest valid parentheses substring.

**Company**: Google, Microsoft, Amazon

```javascript
function longestValidParentheses(s) {
    const n = s.length;
    const dp = new Array(n).fill(0);
    let maxLen = 0;
    
    for (let i = 1; i < n; i++) {
        if (s[i] === ')') {
            if (s[i - 1] === '(') {
                dp[i] = (i >= 2 ? dp[i - 2] : 0) + 2;
            } else if (dp[i - 1] > 0) {
                const match = i - dp[i - 1] - 1;
                if (match >= 0 && s[match] === '(') {
                    dp[i] = dp[i - 1] + 2 + (match > 0 ? dp[match - 1] : 0);
                }
            }
        }
        maxLen = Math.max(maxLen, dp[i]);
    }
    
    return maxLen;
}
```

**Time**: O(n) | **Space**: O(n)

---

### 6. Interleaving String ⭐⭐⭐⭐⭐
**Problem**: Given strings s1, s2, and s3, find whether s3 is formed by interleaving s1 and s2.

**Company**: Google, Microsoft, Amazon

```javascript
function isInterleave(s1, s2, s3) {
    if (s1.length + s2.length !== s3.length) return false;
    
    const m = s1.length, n = s2.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(false));
    
    dp[0][0] = true;
    
    // Fill first row
    for (let j = 1; j <= n; j++) {
        dp[0][j] = dp[0][j - 1] && s2[j - 1] === s3[j - 1];
    }
    
    // Fill first column
    for (let i = 1; i <= m; i++) {
        dp[i][0] = dp[i - 1][0] && s1[i - 1] === s3[i - 1];
    }
    
    // Fill rest of the table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            dp[i][j] = (dp[i - 1][j] && s1[i - 1] === s3[i + j - 1]) ||
                      (dp[i][j - 1] && s2[j - 1] === s3[i + j - 1]);
        }
    }
    
    return dp[m][n];
}
```

**Time**: O(m×n) | **Space**: O(m×n)

---

### 7. Distinct Subsequences ⭐⭐⭐⭐⭐
**Problem**: Given two strings s and t, return the number of distinct subsequences of s which equals t.

**Company**: Google, Microsoft, Amazon

```javascript
function numDistinct(s, t) {
    const m = s.length, n = t.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    // Base case: empty string t can be formed in 1 way
    for (let i = 0; i <= m; i++) {
        dp[i][0] = 1;
    }
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s[i - 1] === t[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    
    return dp[m][n];
}
```

**Time**: O(m×n) | **Space**: O(m×n)

---

### 8. Best Time to Buy and Sell Stock IV ⭐⭐⭐⭐⭐
**Problem**: You may complete at most k transactions. Find the maximum profit.

**Company**: Google, Facebook, Microsoft, Amazon

```javascript
function maxProfit(k, prices) {
    const n = prices.length;
    if (n <= 1 || k === 0) return 0;
    
    // If k is large enough, it's like unlimited transactions
    if (k >= n / 2) {
        let profit = 0;
        for (let i = 1; i < n; i++) {
            if (prices[i] > prices[i - 1]) {
                profit += prices[i] - prices[i - 1];
            }
        }
        return profit;
    }
    
    // buy[i][j] = max profit after at most i transactions, currently holding stock
    // sell[i][j] = max profit after at most i transactions, not holding stock
    const buy = Array(k + 1).fill().map(() => Array(n).fill(0));
    const sell = Array(k + 1).fill().map(() => Array(n).fill(0));
    
    for (let i = 1; i <= k; i++) {
        buy[i][0] = -prices[0];
        for (let j = 1; j < n; j++) {
            buy[i][j] = Math.max(buy[i][j - 1], sell[i - 1][j - 1] - prices[j]);
            sell[i][j] = Math.max(sell[i][j - 1], buy[i][j - 1] + prices[j]);
        }
    }
    
    return sell[k][n - 1];
}
```

**Time**: O(k×n) | **Space**: O(k×n)

---

### 9. Palindrome Partitioning II ⭐⭐⭐⭐⭐
**Problem**: Given a string s, partition s such that every substring is a palindrome. Return minimum cuts needed.

**Company**: Google, Microsoft, Amazon

```javascript
function minCut(s) {
    const n = s.length;
    const dp = new Array(n).fill(0).map((_, i) => i); // worst case: all single chars
    const isPalindrome = Array(n).fill().map(() => Array(n).fill(false));
    
    // Build palindrome table
    for (let i = 0; i < n; i++) {
        isPalindrome[i][i] = true;
    }
    
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            if (s[i] === s[j]) {
                isPalindrome[i][j] = len === 2 || isPalindrome[i + 1][j - 1];
            }
        }
    }
    
    // DP for minimum cuts
    for (let i = 0; i < n; i++) {
        if (isPalindrome[0][i]) {
            dp[i] = 0;
        } else {
            for (let j = 0; j < i; j++) {
                if (isPalindrome[j + 1][i]) {
                    dp[i] = Math.min(dp[i], dp[j] + 1);
                }
            }
        }
    }
    
    return dp[n - 1];
}
```

**Time**: O(n²) | **Space**: O(n²)

---

### 10. Scramble String ⭐⭐⭐⭐⭐
**Problem**: Given two strings s1 and s2, return true if s2 is a scrambled string of s1.

**Company**: Google, Microsoft

```javascript
function isScramble(s1, s2) {
    if (s1.length !== s2.length) return false;
    if (s1 === s2) return true;
    
    const n = s1.length;
    const dp = Array(n).fill().map(() => 
        Array(n).fill().map(() => 
            Array(n + 1).fill(false)
        )
    );
    
    // Base case: length 1
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            dp[i][j][1] = s1[i] === s2[j];
        }
    }
    
    // Fill for increasing lengths
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            for (let j = 0; j <= n - len; j++) {
                for (let k = 1; k < len; k++) {
                    // No swap case
                    if (dp[i][j][k] && dp[i + k][j + k][len - k]) {
                        dp[i][j][len] = true;
                        break;
                    }
                    // Swap case
                    if (dp[i][j + len - k][k] && dp[i + k][j][len - k]) {
                        dp[i][j][len] = true;
                        break;
                    }
                }
            }
        }
    }
    
    return dp[0][0][n];
}
```

**Time**: O(n⁴) | **Space**: O(n³)

---

### 11. Cherry Pickup ⭐⭐⭐⭐⭐
**Problem**: Given an n×n grid with cherries, collect maximum cherries by going from (0,0) to (n-1,n-1) and back.

**Company**: Google, Amazon

```javascript
function cherryPickup(grid) {
    const n = grid.length;
    const dp = Array(n).fill().map(() => 
        Array(n).fill().map(() => 
            Array(n).fill(-1)
        )
    );
    
    function solve(r1, c1, r2) {
        const c2 = r1 + c1 - r2;
        
        // Out of bounds or thorns
        if (r1 >= n || c1 >= n || r2 >= n || c2 >= n || 
            grid[r1][c1] === -1 || grid[r2][c2] === -1) {
            return -Infinity;
        }
        
        // Reached destination
        if (r1 === n - 1 && c1 === n - 1) {
            return grid[r1][c1];
        }
        
        // Already computed
        if (dp[r1][c1][r2] !== -1) {
            return dp[r1][c1][r2];
        }
        
        // Collect cherries
        let result = grid[r1][c1];
        if (r1 !== r2) result += grid[r2][c2];
        
        // Try all possible moves
        const moves = [
            solve(r1 + 1, c1, r2 + 1),     // Both down
            solve(r1 + 1, c1, r2),         // Person1 down, Person2 right
            solve(r1, c1 + 1, r2 + 1),     // Person1 right, Person2 down
            solve(r1, c1 + 1, r2)          // Both right
        ];
        
        result += Math.max(...moves);
        dp[r1][c1][r2] = result;
        return result;
    }
    
    return Math.max(0, solve(0, 0, 0));
}
```

**Time**: O(n³) | **Space**: O(n³)

---

### 12. Minimum Window Subsequence ⭐⭐⭐⭐⭐
**Problem**: Given strings S and T, find the minimum window in S which contains T as a subsequence.

**Company**: Google, Microsoft

```javascript
function minWindow(s, t) {
    const m = s.length, n = t.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    // Base case: empty t can be matched by any prefix of s
    for (let i = 0; i <= m; i++) {
        dp[i][0] = i;
    }
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (s[i - 1] === t[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1];
            } else {
                dp[i][j] = dp[i - 1][j];
            }
        }
    }
    
    let start = 0, len = Infinity;
    
    // Find minimum window ending at each position
    for (let i = n; i <= m; i++) {
        if (dp[i][n] !== 0) {
            if (i - dp[i][n] + 1 < len) {
                start = dp[i][n] - 1;
                len = i - dp[i][n] + 1;
            }
        }
    }
    
    return len === Infinity ? "" : s.substring(start, start + len);
}
```

**Time**: O(m×n) | **Space**: O(m×n)

---

### 13. Count Different Palindromic Subsequences ⭐⭐⭐⭐⭐
**Problem**: Given a string S, find the number of different non-empty palindromic subsequences.

**Company**: Google, Microsoft

```javascript
function countPalindromicSubsequences(s) {
    const MOD = 1e9 + 7;
    const n = s.length;
    const dp = Array(n).fill().map(() => Array(n).fill(0));
    
    // Single character palindromes
    for (let i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Length 2 palindromes
    for (let i = 0; i < n - 1; i++) {
        dp[i][i + 1] = s[i] === s[i + 1] ? 1 : 2;
    }
    
    // Length 3 and more
    for (let len = 3; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            
            if (s[i] === s[j]) {
                // Find next and previous occurrence
                let low = i + 1, high = j - 1;
                while (low <= high && s[low] !== s[i]) low++;
                while (low <= high && s[high] !== s[i]) high--;
                
                if (low > high) {
                    // No same character in between
                    dp[i][j] = (dp[i + 1][j - 1] * 2 + 2) % MOD;
                } else if (low === high) {
                    // One same character in between
                    dp[i][j] = (dp[i + 1][j - 1] * 2 + 1) % MOD;
                } else {
                    // Two or more same characters in between
                    dp[i][j] = (dp[i + 1][j - 1] * 2 - dp[low + 1][high - 1] + MOD) % MOD;
                }
            } else {
                dp[i][j] = (dp[i + 1][j] + dp[i][j - 1] - dp[i + 1][j - 1] + MOD) % MOD;
            }
        }
    }
    
    return dp[0][n - 1];
}
```

**Time**: O(n³) | **Space**: O(n²)

---

### 14. Shortest Palindrome ⭐⭐⭐⭐⭐
**Problem**: Given a string s, add characters in front of it to make it a palindrome. Find the shortest palindrome.

**Company**: Google, Microsoft

```javascript
function shortestPalindrome(s) {
    const n = s.length;
    if (n === 0) return "";
    
    // Find the longest palindromic prefix
    let end = 0;
    for (let i = n - 1; i >= 0; i--) {
        if (s[i] === s[end]) {
            end++;
        }
    }
    
    // If entire string is palindrome
    if (end === n) return s;
    
    // Recursively find palindrome for suffix
    const suffix = s.substring(end);
    const prefix = suffix.split('').reverse().join('');
    
    return prefix + shortestPalindrome(s.substring(0, end)) + suffix;
}

// Alternative KMP approach
function shortestPalindromeKMP(s) {
    const combined = s + "#" + s.split('').reverse().join('');
    const lps = computeLPS(combined);
    
    return s.split('').reverse().join('').substring(0, s.length - lps[lps.length - 1]) + s;
}

function computeLPS(s) {
    const n = s.length;
    const lps = new Array(n).fill(0);
    let len = 0;
    
    for (let i = 1; i < n; i++) {
        while (len > 0 && s[i] !== s[len]) {
            len = lps[len - 1];
        }
        if (s[i] === s[len]) {
            len++;
        }
        lps[i] = len;
    }
    
    return lps;
}
```

**Time**: O(n) | **Space**: O(n)

---

### 15. Number of Digit One ⭐⭐⭐⭐⭐
**Problem**: Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

**Company**: Google, Microsoft

```javascript
function countDigitOne(n) {
    let count = 0;
    let factor = 1;
    
    while (factor <= n) {
        const lower = Math.floor(n / (factor * 10)) * factor;
        const cur = Math.floor(n / factor) % 10;
        const higher = Math.floor(n / (factor * 10));
        
        if (cur === 0) {
            count += higher * factor;
        } else if (cur === 1) {
            count += higher * factor + (n % factor) + 1;
        } else {
            count += (higher + 1) * factor;
        }
        
        factor *= 10;
    }
    
    return count;
}
```

**Time**: O(log n) | **Space**: O(1)

---

## Interview Tips for Hard DP Problems

### 1. **Pattern Recognition**
- **Interval DP**: When you need to consider all possible splits
- **3D DP**: When you have two sequences/paths to consider
- **State Machine**: When you have multiple states at each step
- **String Matching**: Often involves 2D DP with character matching

### 2. **Common Hard DP Patterns**
```javascript
// Interval DP Pattern
for (let len = 2; len <= n; len++) {
    for (let i = 0; i <= n - len; i++) {
        const j = i + len - 1;
        for (let k = i; k < j; k++) {
            // Try all possible splits
        }
    }
}

// 3D DP Pattern (Two sequences)
for (let i = 0; i <= m; i++) {
    for (let j = 0; j <= n; j++) {
        for (let k = 0; k <= p; k++) {
            // Consider all three dimensions
        }
    }
}
```

### 3. **Optimization Strategies**
- **Memoization**: Start with recursive solution, add memoization
- **Bottom-up**: Convert to iterative for better performance
- **Space optimization**: Use rolling arrays when possible
- **Early termination**: Add pruning conditions

### 4. **Debugging Hard DP**
- **Small examples**: Test with strings of length 2-3
- **Edge cases**: Empty strings, single characters
- **State verification**: Print DP table to verify transitions
- **Recurrence check**: Manually verify first few states

---
