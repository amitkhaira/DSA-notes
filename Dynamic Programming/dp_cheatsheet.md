# Complete Dynamic Programming Cheat Sheet

## Table of Contents
1. [Core Concepts](#core-concepts)
2. [DP Identification](#dp-identification)
3. [Implementation Approaches](#implementation-approaches)
4. [DP Patterns & Categories](#dp-patterns--categories)
5. [Classic Problems by Pattern](#classic-problems-by-pattern)
6. [Advanced DP Techniques](#advanced-dp-techniques)
7. [Optimization Strategies](#optimization-strategies)
8. [Problem-Solving Framework](#problem-solving-framework)
9. [Complexity Analysis](#complexity-analysis)
10. [Common Pitfalls & Tips](#common-pitfalls--tips)
11. [Interview Strategy](#interview-strategy)

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

### Problem Types That Use DP
```javascript
// Optimization: Find minimum/maximum value
// Counting: Count number of ways/paths
// Existence: Check if solution exists
// Construction: Build optimal solution
```

## Implementation Approaches

### 1. Memoization (Top-Down)
```javascript
// Template
function solveMemo(params, memo = new Map()) {
    // Base case
    if (baseCondition) return baseResult;
    
    // Check if already computed
    const key = JSON.stringify(params);
    if (memo.has(key)) return memo.get(key);
    
    // Recursive computation
    let result = /* recursive calls */;
    
    // Store and return
    memo.set(key, result);
    return result;
}

// Example: Fibonacci with memoization
function fibMemo(n, memo = {}) {
    if (n <= 1) return n;
    if (n in memo) return memo[n];
    
    memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}
```

### 2. Tabulation (Bottom-Up)
```javascript
// Template
function solveTab(n) {
    // Create DP table
    const dp = new Array(n + 1);
    
    // Initialize base cases
    dp[0] = base0;
    dp[1] = base1;
    
    // Fill table iteratively  
    for (let i = 2; i <= n; i++) {
        dp[i] = /* compute using previous values */;
    }
    
    return dp[n];
}

// Example: Fibonacci with tabulation
function fibTab(n) {
    if (n <= 1) return n;
    
    const dp = [0, 1];
    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

### 3. Space Optimization
```javascript
// When only previous few values needed
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

### Memoization vs Tabulation

| Aspect | Memoization | Tabulation |
|--------|-------------|------------|
| **Approach** | Top-down | Bottom-up |
| **Storage** | Only needed states | All states |
| **Space** | O(recursion depth) extra | No recursion overhead |
| **Implementation** | More intuitive | Requires order planning |
| **Performance** | Function call overhead | Generally faster |

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

## Classic Problems by Pattern

### Linear DP Problems

#### Fibonacci Numbers
```javascript
function fibonacci(n) {
    if (n <= 1) return n;
    let a = 0, b = 1;
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}
```

#### Climbing Stairs
```javascript
function climbStairs(n) {
    if (n <= 2) return n;
    let prev2 = 1, prev1 = 2;
    for (let i = 3; i <= n; i++) {
        [prev2, prev1] = [prev1, prev1 + prev2];
    }
    return prev1;
}
```

#### House Robber
```javascript
function rob(nums) {
    let prev2 = 0, prev1 = 0;
    for (const num of nums) {
        [prev2, prev1] = [prev1, Math.max(prev1, prev2 + num)];
    }
    return prev1;
}
```

#### Maximum Subarray (Kadane's Algorithm)
```javascript
function maxSubArray(nums) {
    let maxSoFar = nums[0], maxEndingHere = nums[0];
    for (let i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

#### Longest Increasing Subsequence
```javascript
function lengthOfLIS(nums) {
    const dp = new Array(nums.length).fill(1);
    for (let i = 1; i < nums.length; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
    }
    return Math.max(...dp);
}

// Optimized O(n log n) version
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
```

### Grid DP Problems

#### Unique Paths
```javascript
function uniquePaths(m, n) {
    const dp = Array(m).fill().map(() => Array(n).fill(1));
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[i][j] = dp[i-1][j] + dp[i][j-1];
        }
    }
    return dp[m-1][n-1];
}

// Space optimized
function uniquePathsOptimized(m, n) {
    let dp = new Array(n).fill(1);
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[j] += dp[j-1];
        }
    }
    return dp[n-1];
}
```

#### Minimum Path Sum
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
```

#### Longest Common Subsequence
```javascript
function longestCommonSubsequence(text1, text2) {
    const m = text1.length, n = text2.length;
    const dp = Array(m+1).fill().map(() => Array(n+1).fill(0));
    
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
```

#### Edit Distance
```javascript
function minDistance(word1, word2) {
    const m = word1.length, n = word2.length;
    const dp = Array(m+1).fill().map(() => Array(n+1).fill(0));
    
    // Base cases
    for (let i = 0; i <= m; i++) dp[i][0] = i;
    for (let j = 0; j <= n; j++) dp[0][j] = j;
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i-1] === word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i-1][j],     // Delete
                    dp[i][j-1],     // Insert  
                    dp[i-1][j-1]    // Replace
                );
            }
        }
    }
    
    return dp[m][n];
}
```

### Knapsack Problems

#### Coin Change (Minimum Coins)
```javascript
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
```

#### Coin Change 2 (Number of Ways)
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
```

#### Partition Equal Subset Sum
```javascript
function canPartition(nums) {
    const sum = nums.reduce((a, b) => a + b, 0);
    if (sum % 2 !== 0) return false;
    
    const target = sum / 2;
    const dp = new Array(target + 1).fill(false);
    dp[0] = true;
    
    for (const num of nums) {
        for (let j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    
    return dp[target];
}
```

### Interval DP Problems

#### Matrix Chain Multiplication
```javascript
function matrixChainOrder(dims) {
    const n = dims.length - 1;
    const dp = Array(n).fill().map(() => Array(n).fill(0));
    
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i < n - len + 1; i++) {
            const j = i + len - 1;
            dp[i][j] = Infinity;
            
            for (let k = i; k < j; k++) {
                const cost = dp[i][k] + dp[k+1][j] + 
                           dims[i] * dims[k+1] * dims[j+1];
                dp[i][j] = Math.min(dp[i][j], cost);
            }
        }
    }
    
    return dp[0][n-1];
}
```

#### Burst Balloons
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
    
    return dp[0][n-1];
}
```

### String DP Problems

#### Palindromic Substrings
```javascript
function countSubstrings(s) {
    const n = s.length;
    const dp = Array(n).fill().map(() => Array(n).fill(false));
    let count = 0;
    
    // Single characters
    for (let i = 0; i < n; i++) {
        dp[i][i] = true;
        count++;
    }
    
    // Two characters
    for (let i = 0; i < n - 1; i++) {
        if (s[i] === s[i + 1]) {
            dp[i][i + 1] = true;
            count++;
        }
    }
    
    // Three or more characters
    for (let len = 3; len <= n; len++) {
        for (let i = 0; i < n - len + 1; i++) {
            const j = i + len - 1;
            if (s[i] === s[j] && dp[i + 1][j - 1]) {
                dp[i][j] = true;
                count++;
            }
        }
    }
    
    return count;
}
```

#### Longest Palindromic Subsequence
```javascript
function longestPalindromeSubseq(s) {
    const n = s.length;
    const dp = Array(n).fill().map(() => Array(n).fill(0));
    
    // Single characters
    for (let i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Fill for increasing lengths
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i < n - len + 1; i++) {
            const j = i + len - 1;
            if (s[i] === s[j]) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

### State Machine DP Problems

#### Best Time to Buy and Sell Stock with Cooldown
```javascript
function maxProfit(prices) {
    let held = -prices[0];  // Currently holding stock
    let sold = 0;           // Just sold stock (in cooldown)
    let rest = 0;           // Not holding, can buy
    
    for (let i = 1; i < prices.length; i++) {
        const prevHeld = held;
        const prevSold = sold;
        const prevRest = rest;
        
        held = Math.max(prevHeld, prevRest - prices[i]);
        sold = prevHeld + prices[i];
        rest = Math.max(prevRest, prevSold);
    }
    
    return Math.max(sold, rest);
}
```

#### Paint House
```javascript
function minCost(costs) {
    let red = 0, blue = 0, green = 0;
    
    for (const [r, b, g] of costs) {
        const newRed = Math.min(blue, green) + r;
        const newBlue = Math.min(red, green) + b;
        const newGreen = Math.min(red, blue) + g;
        
        [red, blue, green] = [newRed, newBlue, newGreen];
    }
    
    return Math.min(red, blue, green);
}
```

## Advanced DP Techniques

### 1. Space Optimization

#### Rolling Array Technique
```javascript
// Instead of 2D array, use two 1D arrays
function optimizedLCS(text1, text2) {
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
```

#### State Compression
```javascript
// Use variables instead of arrays when possible
function fibSpaceOptimized(n) {
    if (n <= 1) return n;
    let a = 0, b = 1;
    for (let i = 2; i <= n; i++) {
        [a, b] = [b, a + b];
    }
    return b;
}
```

### 2. DP with Data Structures

#### DP with HashMap
```javascript
function combinationSum4(nums, target) {
    const dp = new Map();
    
    function solve(remaining) {
        if (remaining === 0) return 1;
        if (remaining < 0) return 0;
        if (dp.has(remaining)) return dp.get(remaining);
        
        let result = 0;
        for (const num of nums) {
            result += solve(remaining - num);
        }
        
        dp.set(remaining, result);
        return result;
    }
    
    return solve(target);
}
```

#### DP with Trie
```javascript
// Word Break II - finding all possible sentences
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

### 3. Multi-dimensional DP

#### 3D DP Example
```javascript
function cherryPick(grid) {
    const m = grid.length, n = grid[0].length;
    const dp = Array(m).fill().map(() => 
        Array(n).fill().map(() => 
            Array(n).fill(-1)));
    
    dp[0][0][n-1] = grid[0][0] + (0 === n-1 ? 0 : grid[0][n-1]);
    
    for (let r = 1; r < m; r++) {
        for (let c1 = 0; c1 < n; c1++) {
            for (let c2 = 0; c2 < n; c2++) {
                let val = grid[r][c1];
                if (c1 !== c2) val += grid[r][c2];
                
                for (let dc1 = -1; dc1 <= 1; dc1++) {
                    for (let dc2 = -1; dc2 <= 1; dc2++) {
                        const pc1 = c1 + dc1, pc2 = c2 + dc2;
                        if (pc1 >= 0 && pc1 < n && pc2 >= 0 && pc2 < n && 
                            dp[r-1][pc1][pc2] !== -1) {
                            dp[r][c1][c2] = Math.max(dp[r][c1][c2], 
                                dp[r-1][pc1][pc2] + val);
                        }
                    }
                }
            }
        }
    }
    
    let result = 0;
    for (let c1 = 0; c1 < n; c1++) {
        for (let c2 = 0; c2 < n; c2++) {
            result = Math.max(result, dp[m-1][c1][c2]);
        }
    }
    return result;
}
```

### 4. DP on Trees

#### Tree DP Template
```javascript
function treeDP(root) {
    const memo = new Map();
    
    function dfs(node, canTake) {
        if (!node) return 0;
        
        const key = `${node.val}-${canTake}`;
        if (memo.has(key)) return memo.get(key);
        
        let result;
        if (canTake) {
            // Two choices: take current node or skip it
            const take = node.val + dfs(node.left, false) + dfs(node.right, false);
            const skip = dfs(node.left, true) + dfs(node.right, true);
            result = Math.max(take, skip);
        } else {
            // Can't take current node
            result = dfs(node.left, true) + dfs(node.right, true);
        }
        
        memo.set(key, result);
        return result;
    }
    
    return dfs(root, true);
}
```

### 5. Probability DP
```javascript
// Knight Probability in Chessboard
function knightProbability(n, k, row, col) {
    const moves = [[-2,-1],[-2,1],[-1,-2],[-1,2],[1,-2],[1,2],[2,-1],[2,1]];
    const memo = new Map();
    
    function dfs(r, c, steps) {
        if (r < 0 || r >= n || c < 0 || c >= n) return 0;
        if (steps === 0) return 1;
        
        const key = `${r}-${c}-${steps}`;
        if (memo.has(key)) return memo.get(key);
        
        let prob = 0;
        for (const [dr, dc] of moves) {
            prob += dfs(r + dr, c + dc, steps - 1);
        }
        prob /= 8;
        
        memo.set(key, prob);
        return prob;
    }
    
    return dfs(row, col, k);
}
```

### 6. Game Theory DP
```javascript
// Stone Game - Minimax DP
function stoneGame(piles) {
    const n = piles.length;
    const dp = Array(n).fill().map(() => Array(n).fill(0));
    
    // Base case: single pile
    for (let i = 0; i < n; i++) {
        dp[i][i] = piles[i];
    }
    
    // Fill for increasing lengths
    for (let len = 2; len <= n; len++) {
        for (let i = 0; i <= n - len; i++) {
            const j = i + len - 1;
            dp[i][j] = Math.max(
                piles[i] - dp[i+1][j],  // Take left pile
                piles[j] - dp[i][j-1]   // Take right pile
            );
        }
    }
    
    return dp[0][n-1] > 0;
}
```

## Optimization Strategies

### 1. Identify Redundant Computations
```javascript
// Before optimization: O(2^n)
function fibNaive(n) {
    if (n <= 1) return n;
    return fibNaive(n-1) + fibNaive(n-2);
}

// After memoization: O(n)
function fibMemo(n, memo = {}) {
    if (n <= 1) return n;
    if (n in memo) return memo[n];
    memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}
```

### 2. Space Optimization Patterns

#### Pattern 1: Only Previous Row Needed
```javascript
// 2D to 1D optimization
function uniquePathsSpaceOptimized(m, n) {
    let dp = new Array(n).fill(1);
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            dp[j] += dp[j-1];
        }
    }
    return dp[n-1];
}
```

#### Pattern 2: Only Few Previous Values Needed
```javascript
// Array to variables optimization
function climbStairsOptimized(n) {
    if (n <= 2) return n;
    let prev2 = 1, prev1 = 2;
    for (let i = 3; i <= n; i++) {
        [prev2, prev1] = [prev1, prev1 + prev2];
    }
    return prev1;
}
```

#### Pattern 3: Reverse Iteration for In-place Updates
```javascript
// Knapsack space optimization
function knapsackOptimized(weights, values, capacity) {
    const dp = new Array(capacity + 1).fill(0);
    
    for (let i = 0; i < weights.length; i++) {
        // Iterate backwards to avoid using updated values
        for (let w = capacity; w >= weights[i]; w--) {
            dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
        }
    }
    
    return dp[capacity];
}
```

### 3. Time Optimization Techniques

#### Monotonic Queue/Stack Optimization
```javascript
// Sliding Window Maximum with DP
function maxSlidingWindow(nums, k) {
    const deque = [];
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        // Remove elements outside window
        while (deque.length && deque[0] <= i - k) {
            deque.shift();
        }
        
        // Remove smaller elements
        while (deque.length && nums[deque[deque.length - 1]] <= nums[i]) {
            deque.pop();
        }
        
        deque.push(i);
        
        if (i >= k - 1) {
            result.push(nums[deque[0]]);
        }
    }
    
    return result;
}
```

#### Binary Search Optimization
```javascript
// LIS with binary search - O(n log n)
function lengthOfLISOptimized(nums) {
    const tails = [];
    
    for (const num of nums) {
        let left = 0, right = tails.length;
        
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            if (tails[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        if (left === tails.length) {
            tails.push(num);
        } else {
            tails[left] = num;
        }
    }
    
    return tails.length;
}
```

## Problem-Solving Framework

### Step 1: Problem Analysis
```javascript
/*
Questions to ask:
1. What am I optimizing? (min/max/count)
2. What are the states/subproblems?
3. What decisions am I making at each step?
4. How do current choices affect future choices?
5. What are the base cases?
*/
```

### Step 2: State Definition
```javascript
/*
Define dp[...] clearly:
- dp[i] = optimal solution for first i elements
- dp[i][j] = optimal solution for range [i, j]
- dp[i][j][k] = optimal solution with constraints i, j, k
- dp[mask] = optimal solution for subset represented by mask
*/
```

### Step 3: Recurrence Relation
```javascript
/*
Express current state in terms of previous states:
- Linear: dp[i] = f(dp[i-1], dp[i-2], ...)
- Grid: dp[i][j] = f(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
- Interval: dp[i][j] = optimize over k: f(dp[i][k], dp[k+1][j])
*/
```

### Step 4: Base Cases
```javascript
/*
Identify and handle base cases:
- Empty input
- Single element
- Boundary conditions
- Invalid states
*/
```

### Step 5: Implementation Choice
```javascript
/*
Choose between:
1. Recursive + Memoization (Top-down)
   - Pros: Natural, only computes needed states
   - Cons: Function call overhead, stack depth

2. Iterative + Tabulation (Bottom-up)  
   - Pros: No recursion overhead, often faster
   - Cons: Computes all states, need correct order

3. Space Optimization
   - When only few previous states needed
*/
```

### Step 6: Complexity Analysis
```javascript
/*
Time Complexity:
- Number of states × Work per state
- Consider optimization opportunities

Space Complexity:
- Storage for DP table
- Can it be optimized?
*/
```

## Complexity Analysis

### Time Complexity Patterns

| DP Type | Time Complexity | Example |
|---------|----------------|---------|
| Linear 1D | O(n) | Fibonacci, Climbing Stairs |
| Linear with inner loop | O(n²) | LIS, House Robber variants |
| Grid 2D | O(m×n) | Unique Paths, LCS |
| Knapsack | O(n×W) | 0/1 Knapsack, Coin Change |
| Interval | O(n³) | Matrix Chain, Burst Balloons |
| Tree | O(n) | Tree DP problems |
| Bitmask | O(n×2ⁿ) | TSP, Assignment problems |

### Space Complexity Optimization

| Original | Optimized | Technique |
|----------|-----------|-----------|
| O(n) | O(1) | Use variables instead of array |
| O(m×n) | O(min(m,n)) | Use 1D array, iterate over smaller dimension |
| O(n²) | O(n) | Rolling array technique |

## Common Pitfalls & Tips

### 1. Common Mistakes

#### Index Errors
```javascript
// Wrong: Off-by-one error
for (let i = 1; i < n; i++) {
    dp[i] = dp[i-1] + dp[i-2]; // dp[i-2] when i=1 is dp[-1]
}

// Correct: Proper bounds checking
for (let i = 2; i < n; i++) {
    dp[i] = dp[i-1] + dp[i-2];
}
```

#### Wrong Base Cases
```javascript
// Wrong: Incomplete base cases
function coinChange(coins, amount) {
    const dp = new Array(amount + 1).fill(0); // Should be Infinity
    dp[0] = 0;
    // ...
}

// Correct: Proper initialization
function coinChange(coins, amount) {
    const dp = new Array(amount + 1).fill(Infinity);
    dp[0] = 0;
    // ...
}
```

#### State Definition Issues
```javascript
// Wrong: Ambiguous state
// dp[i] = ? (unclear what it represents)

// Correct: Clear state definition  
// dp[i] = minimum cost to reach index i
```

### 2. Debugging Strategies

#### Print DP Table
```javascript
function debugDP(dp) {
    console.log('DP Table:');
    if (Array.isArray(dp[0])) {
        // 2D array
        for (let i = 0; i < dp.length; i++) {
            console.log(`Row ${i}: [${dp[i].join(', ')}]`);
        }
    } else {
        // 1D array
        console.log(`[${dp.join(', ')}]`);
    }
}
```

#### Test with Small Examples
```javascript
// Always test with:
// - Empty input: []
// - Single element: [x]
// - Two elements: [x, y] 
// - Simple known cases
```

#### Verify Recurrence
```javascript
// Check recurrence by hand:
// If dp[i] depends on dp[i-1] and dp[i-2]
// Manually compute first few values
```

### 3. Performance Tips

#### Choose Right Data Structure
```javascript
// Use Map for complex keys
const memo = new Map();
const key = JSON.stringify([i, j, k]);

// Use object for simple keys
const memo = {};
const key = `${i}-${j}-${k}`;

// Use array for integer indices
const dp = new Array(n);
```

#### Avoid Redundant Work
```javascript
// Bad: Recomputing same values
function solve(i, j) {
    const result1 = helper(i-1, j) + helper(i, j-1);
    const result2 = helper(i-1, j) + helper(i-1, j-1); // helper(i-1, j) computed twice
}

// Good: Store intermediate results
function solve(i, j) {
    const left = helper(i-1, j);
    const up = helper(i, j-1);
    const diag = helper(i-1, j-1);
    return Math.min(left + up, left + diag);
}
```

## Interview Strategy

### 1. Problem Recognition
```javascript
/*
Quick identification checklist:
□ Contains "optimal", "minimum", "maximum"?
□ Multiple ways to solve subproblems?
□ Counting number of ways?
□ Making sequence of decisions?
□ Can be broken into smaller similar problems?
□ Brute force is exponential?
*/
```

### 2. Communication Template
```javascript
/*
1. "This looks like a DP problem because..."
2. "Let me define the state as dp[i] = ..."
3. "The recurrence relation would be..."
4. "Base cases are..."
5. "Let me trace through a small example..."
6. "Time complexity is O(...), space is O(...)"
7. "We can optimize space by..."
*/
```

### 3. Implementation Strategy
```javascript
/*
Step 1: Start with recursive solution
Step 2: Add memoization (top-down DP)
Step 3: Convert to iterative if needed (bottom-up DP)
Step 4: Optimize space if possible
Step 5: Handle edge cases
*/
```

### 4. Testing Strategy
```javascript
/*
Test cases to consider:
1. Empty input
2. Single element
3. All same elements
4. Sorted input (ascending/descending)
5. Maximum constraints
6. Known simple cases
*/
```

### 5. Follow-up Questions to Expect
```javascript
/*
Common follow-ups:
1. "Can you optimize the space complexity?"
2. "What if we need to return the actual solution, not just the value?"
3. "How would you handle negative numbers?"
4. "What if the constraints were much larger?"
5. "Can you solve this iteratively instead of recursively?"
*/
```

### 6. Building the Solution Path
```javascript
// Template for reconstructing solution
function solveProblemWithPath(input) {
    // 1. Compute DP table
    const dp = computeDP(input);
    
    // 2. Backtrack to find solution
    const path = [];
    let i = input.length - 1;
    
    while (i >= 0) {
        if (/* condition for choice A */) {
            path.push('Choice A');
            i = /* new index based on choice A */;
        } else {
            path.push('Choice B');
            i = /* new index based on choice B */;
        }
    }
    
    return {
        value: dp[input.length - 1],
        path: path.reverse()
    };
}
```

## Quick Reference Formulas

### Common Recurrence Relations
```javascript
// Fibonacci-like: F(n) = F(n-1) + F(n-2)
// Coin Change: dp[i] = min(dp[i], dp[i-coin] + 1)
// LCS: dp[i][j] = dp[i-1][j-1] + 1 if match, else max(dp[i-1][j], dp[i][j-1])
// Knapsack: dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight] + value)
// Edit Distance: dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
```

### Space Optimization Patterns
```javascript
// Pattern 1: dp[i] only depends on dp[i-1]
let prev = base;
for (let i = 1; i <= n; i++) {
    const curr = f(prev);
    prev = curr;
}

// Pattern 2: dp[i] depends on dp[i-1] and dp[i-2]  
let prev2 = base1, prev1 = base2;
for (let i = 2; i <= n; i++) {
    const curr = f(prev1, prev2);
    [prev2, prev1] = [prev1, curr];
}

// Pattern 3: 2D to 1D (row by row)
let dp = new Array(n).fill(base);
for (let i = 1; i < m; i++) {
    for (let j = 1; j < n; j++) {
        dp[j] = f(dp[j], dp[j-1]); // dp[j] is previous row, dp[j-1] is current row
    }
}
```
