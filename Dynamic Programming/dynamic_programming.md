Okay, let's switch gears from making locally optimal choices with Greedy to a more exhaustive (but clever) approach: **Dynamic Programming (DP)**.

DP is a very powerful technique for solving optimization, counting, or decision problems by breaking them down into simpler, overlapping subproblems.

---

## Dynamic Programming: Key Notes

### 1. What is Dynamic Programming?
Dynamic Programming is an algorithmic technique for solving an optimization problem by breaking it down into simpler subproblems and utilizing the fact that the optimal solution to the overall problem depends upon the optimal solutions to its subproblems.

Think of it like this: if you've solved a small piece of a puzzle already, you don't want to solve it again when you encounter that same piece in a slightly larger context. You remember (memoize) or pre-calculate (tabulate) its solution.

### 2. Core Idea / Key Principles
For a problem to be solvable using Dynamic Programming, it must typically exhibit two key properties:

*   **a. Overlapping Subproblems:**
    A problem has overlapping subproblems if it can be broken down into subproblems that are reused multiple times. DP solves each subproblem just once and then stores its solution in a table (or cache) to avoid the work of recomputing the solution every time the same subproblem is encountered.
    *   *Contrast with Divide and Conquer:* In Divide and Conquer (like Merge Sort), subproblems are generally disjoint (independent).

*   **b. Optimal Substructure:**
    A problem exhibits optimal substructure if an optimal solution to the problem can be constructed from optimal solutions to its subproblems.
    *   *Example:* The shortest path between two points `A` and `C` that goes through an intermediate point `B` is the sum of the shortest path from `A` to `B` and the shortest path from `B` to `C`.

### 3. Approaches to Dynamic Programming
There are two main ways to implement a DP solution:

*   **a. Memoization (Top-Down Approach):**
    1.  Write a recursive function that solves the problem by breaking it down into subproblems.
    2.  Store the result of each subproblem in a lookup table (e.g., an array, hash map) the first time it's computed.
    3.  Before computing a subproblem, check if its solution is already in the table. If so, return the stored value. Otherwise, compute it, store it, and then return it.
    *   This approach solves subproblems only when they are needed.

    **General Pseudo-code (Memoization):**
    ```
    memo = {} // or array initialized with a special value

    function solveDP(params):
        if params in memo:
            return memo[params]
        
        if base_case(params):
            result = solve_base_case(params)
        else:
            // Recurse based on subproblems
            result = compute_using_solveDP(subproblem1_params) ... 
        
        memo[params] = result
        return result
    ```

*   **b. Tabulation (Bottom-Up Approach):**
    1.  Identify the subproblems and the order in which they should be solved (typically starting from the smallest/simplest).
    2.  Create a table (usually an array or a 2D array) to store the solutions to subproblems.
    3.  Iteratively fill the table, solving subproblems in order, using previously computed results to solve larger/later subproblems.
    4.  The final entry in the table (or one of the entries) will be the solution to the original problem.
    *   This approach solves all related subproblems, often in a specific bottom-up order.

    **General Pseudo-code (Tabulation):**
    ```
    dp_table = new Array(...) // Initialize with base case values or 0s/Infinities

    // Iterate through states to fill the table
    for i from smallest_state to largest_state:
        for j from ... :
            // Compute dp_table[i][j] based on previous entries
            // (e.g., dp_table[i-1][j], dp_table[i][j-1], etc.)
            dp_table[i][j] = compute_using(dp_table[previous_states])
    
    return dp_table[final_state]
    ```

### 4. When to Use DP?
As mentioned, look for:
1.  **Overlapping Subproblems:** Without this, DP is overkill; simple recursion or divide and conquer might be fine.
2.  **Optimal Substructure:** If optimal solutions to subproblems don't guarantee an optimal solution to the overall problem, DP won't work.

DP is often applicable to problems that involve:
*   Finding the minimum or maximum value.
*   Counting the number of ways to do something.
*   Determining if something is possible.

### 5. General Steps to Solve a DP Problem:
1.  **Identify if it's a DP Problem:** Check for overlapping subproblems and optimal substructure. Think if you can define the problem in terms of smaller, similar problems.
2.  **Define the State(s):** Determine what parameters are needed to uniquely describe a subproblem. The "state" is essentially the set of parameters passed to your recursive function (for memoization) or the indices of your DP table (for tabulation). E.g., `dp[i]` could be the solution for the first `i` elements. `dp[i][j]` could be the solution considering elements up to `i` with some capacity `j`.
3.  **Formulate the Recurrence Relation (Transition):** Express the solution to a state in terms of solutions to smaller/previous states. This is the heart of DP.
4.  **Identify Base Cases:** Determine the solutions for the smallest, indivisible subproblems. These are the starting points for your recursion or tabulation.
5.  **Decide an Approach (Memoization or Tabulation):**
    *   Memoization is often more intuitive if you can first think of a plain recursive solution.
    *   Tabulation can be more efficient (avoids recursion overhead) and sometimes easier to reason about space optimization.
6.  **Implement:** Write the code.
7.  **Optimize (Optional):** Sometimes, you can optimize space complexity (e.g., if `dp[i]` only depends on `dp[i-1]` and `dp[i-2]`, you might not need the whole table).

### 6. Advantages of Dynamic Programming
*   **Guaranteed Optimality:** If the problem has optimal substructure and overlapping subproblems, DP typically finds the globally optimal solution.
*   **Efficiency:** By solving each subproblem only once, DP significantly reduces computation time compared to naive recursive approaches (which might recompute the same subproblems exponentially many times).

### 7. Disadvantages/Considerations of Dynamic Programming
*   **Space Complexity:** DP often requires extra space to store the solutions to subproblems in the DP table. This can be substantial for problems with many states.
*   **Problem Formulation:** Identifying the states and the correct recurrence relation can be challenging and is often the hardest part.
*   **Not Always Applicable:** DP is not a silver bullet; it only works for problems exhibiting the two key properties.
*   **Order of Computation (Tabulation):** Figuring out the correct order to fill the table in tabulation can sometimes be tricky.

---

## Classic DP Problems & Code Examples (JavaScript)

### 1. Fibonacci Sequence

**Problem:** Compute the N-th Fibonacci number. `F(0) = 0, F(1) = 1, F(n) = F(n-1) + F(n-2)` for `n > 1`.
This is the "Hello World" of DP, clearly showing overlapping subproblems (`F(2)` is needed for `F(4)` and `F(3)`).

**a. Memoization (Top-Down)**
```javascript
function fibMemo(n, memo = {}) {
    if (n in memo) return memo[n];
    if (n <= 0) return 0;
    if (n === 1) return 1;

    memo[n] = fibMemo(n - 1, memo) + fibMemo(n - 2, memo);
    return memo[n];
}

console.log("Fibonacci (Memoization):");
console.log("F(6) =", fibMemo(6)); // Expected: 8
console.log("F(10) =", fibMemo(10)); // Expected: 55
```

**b. Tabulation (Bottom-Up)**
```javascript
function fibTab(n) {
    if (n <= 0) return 0;
    if (n === 1) return 1;

    const dp = new Array(n + 1);
    dp[0] = 0;
    dp[1] = 1;

    for (let i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}

console.log("\nFibonacci (Tabulation):");
console.log("F(6) =", fibTab(6)); // Expected: 8
console.log("F(10) =", fibTab(10)); // Expected: 55

// Space Optimized Tabulation
function fibTabOptimized(n) {
    if (n <= 0) return 0;
    if (n === 1) return 1;

    let prev2 = 0; // F(0)
    let prev1 = 1; // F(1)
    let current;

    for (let i = 2; i <= n; i++) {
        current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    return current;
}
console.log("\nFibonacci (Tabulation - Space Optimized):");
console.log("F(6) =", fibTabOptimized(6)); // Expected: 8
console.log("F(10) =", fibTabOptimized(10)); // Expected: 55
```

Alright, let's dive deep into these Dynamic Programming problems and their variations, and also cover Max Subarray Sum with its different approaches. This is going to be quite comprehensive!

---

## 2. 0/1 Knapsack Problem

**Problem Statement (Recap):** Given `n` items, each with a weight `w_i` and a value `v_i`, and a knapsack with a maximum weight capacity `W`, determine the subset of items to include in the knapsack such that the total value is maximized, without exceeding the capacity. You can either take an item entirely or not at all (0/1 property).

---

### a. 0/1 Knapsack using Recursion (Naive)

This is the most straightforward way to think about the problem: for each item, you either include it or exclude it.

**Approach:**
Define a recursive function, say `knapsackRecursive(weights, values, capacity, index)`, which returns the maximum value that can be obtained using items from `index` down to `0` (or `index` up to `n-1`, depending on iteration direction) with the given `capacity`.

*   **Base Cases:**
    *   If `capacity <= 0` (no more capacity), return 0.
    *   If `index < 0` (no more items to consider), return 0.

*   **Recursive Step:** For the item at `index`:
    1.  **Exclude the item:** The value is `knapsackRecursive(weights, values, capacity, index - 1)`.
    2.  **Include the item (if it fits):** If `weights[index] <= capacity`, the value is `values[index] + knapsackRecursive(weights, values, capacity - weights[index], index - 1)`.
    3.  Return the maximum of these two options.

**JavaScript Code:**
```javascript
function knapsackRecursive(values, weights, capacity, index) {
    // Base Cases
    if (capacity <= 0 || index < 0) {
        return 0;
    }

    // If current item's weight is more than the knapsack capacity,
    // then it cannot be included in the optimal solution.
    if (weights[index] > capacity) {
        return knapsackRecursive(values, weights, capacity, index - 1);
    } else {
        // Option 1: Include the current item
        const valueIncluded = values[index] + knapsackRecursive(values, weights, capacity - weights[index], index - 1);
        
        // Option 2: Exclude the current item
        const valueExcluded = knapsackRecursive(values, weights, capacity, index - 1);

        return Math.max(valueIncluded, valueExcluded);
    }
}

// Example Usage:
const ksValues = [60, 100, 120];
const ksWeights = [10, 20, 30];
const ksCapacity = 50;

console.log("0/1 Knapsack (Recursive - Naive):");
// Start with the last item (index = values.length - 1)
const maxValueRecursive = knapsackRecursive(ksValues, ksWeights, ksCapacity, ksValues.length - 1);
console.log(`Max value = ${maxValueRecursive}`); // Expected: 220 (but very slow for larger inputs)
```

**Time Complexity:** `O(2^n)` in the worst case, because each item has two choices (include or exclude), leading to an exponential number of paths in the recursion tree.
**Space Complexity:** `O(n)` due to the recursion call stack depth.

**Drawback:** Suffers from re-computing solutions for the same subproblems (same `capacity` and `index`) multiple times.

---

### b. 0/1 Knapsack using Memoization (Top-Down DP)

We store the results of solved subproblems to avoid redundant calculations.

**Approach:**
Use the same recursive structure as above, but add a `memo` table (e.g., a 2D array or a map) to store results. `memo[index][capacity]` would store the result for `knapsack(capacity, index)`.

**JavaScript Code:**
(This was covered in the previous response, presented here for completeness within this section)
```javascript
function knapsackMemo(values, weights, capacity, index, memo) {
    // Base Cases
    if (capacity <= 0 || index < 0) {
        return 0;
    }

    // Check if the result for this state is already memoized
    if (memo[index] && memo[index][capacity] !== undefined) {
        return memo[index][capacity];
    }

    let result;
    if (weights[index] > capacity) {
        result = knapsackMemo(values, weights, capacity, index - 1, memo);
    } else {
        const valueIncluded = values[index] + knapsackMemo(values, weights, capacity - weights[index], index - 1, memo);
        const valueExcluded = knapsackMemo(values, weights, capacity, index - 1, memo);
        result = Math.max(valueIncluded, valueExcluded);
    }

    // Store the result in memo before returning
    if (!memo[index]) {
        memo[index] = {}; // Or initialize memo as a 2D array of right size filled with null/undefined
    }
    memo[index][capacity] = result;
    return result;
}

console.log("\n0/1 Knapsack (Memoization):");
const memo = []; // Initialize memo (can be array of arrays or array of objects)
// For array of arrays: const memo = Array(ksValues.length).fill(null).map(() => Array(ksCapacity + 1).fill(undefined));

const maxValueMemo = knapsackMemo(ksValues, ksWeights, ksCapacity, ksValues.length - 1, memo);
console.log(`Max value = ${maxValueMemo}`); // Expected: 220
```

**Time Complexity:** `O(n * W)`, where `n` is the number of items and `W` is the capacity. Each subproblem `(index, capacity)` is solved only once.
**Space Complexity:** `O(n * W)` for the memoization table, plus `O(n)` for the recursion stack.

---

### c. 0/1 Knapsack using Tabulation (Bottom-Up DP)

We build a table `dp[i][w]` iteratively. `dp[i][w]` usually means the maximum value using the first `i` items with capacity `w`.

**Approach:**
*   `dp` table of size `(n+1) x (W+1)`.
*   `dp[i][w]` stores the maximum value achievable using items from `0` to `i-1` (from the original items array) with a knapsack capacity of `w`.
*   **Initialization:** `dp[0][w] = 0` for all `w` (no items, no value). `dp[i][0] = 0` for all `i` (no capacity, no value).
*   **Iteration:**
    For `i` from `1` to `n`:
        For `w` from `1` to `W`:
            If `weights[i-1] <= w`:
                `dp[i][w] = max(dp[i-1][w],  // Exclude item i-1
                               values[i-1] + dp[i-1][w - weights[i-1]]) // Include item i-1`
            Else (item `i-1` is too heavy):
                `dp[i][w] = dp[i-1][w]`

**JavaScript Code:**
(This was covered in the previous response, presented here for completeness)
```javascript
function knapsackTabulation(values, weights, capacity) {
    const n = values.length;
    // dp[i][w] will store the maximum value that can be put in a knapsack of capacity w
    // using the first i items
    const dp = Array(n + 1).fill(null).map(() => Array(capacity + 1).fill(0));

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

console.log("\n0/1 Knapsack (Tabulation):");
const maxValueTabulation = knapsackTabulation(ksValues, ksWeights, ksCapacity);
console.log(`Max value = ${maxValueTabulation}`); // Expected: 220
```

**Space Optimized Tabulation:**
Since `dp[i][w]` only depends on the previous row `dp[i-1]`, we can optimize space to `O(W)`.
```javascript
function knapsackTabulationSpaceOptimized(values, weights, capacity) {
    const n = values.length;
    let dp = Array(capacity + 1).fill(0); // Only current row needed

    for (let i = 0; i < n; i++) { // Iterate through items
        const currentWeight = weights[i];
        const currentValue = values[i];
        // Create a new row for this iteration or update in place carefully
        // To update in place, iterate w from right to left
        for (let w = capacity; w >= currentWeight; w--) {
            dp[w] = Math.max(
                dp[w],                           // Excluded (value from previous iteration for this w)
                currentValue + dp[w - currentWeight] // Included
            );
        }
    }
    return dp[capacity];
}

console.log("\n0/1 Knapsack (Tabulation - Space Optimized):");
const maxValueTabOpt = knapsackTabulationSpaceOptimized(ksValues, ksWeights, ksCapacity);
console.log(`Max value = ${maxValueTabOpt}`); // Expected: 220
```
**Time Complexity:** `O(n * W)`.
**Space Complexity:** `O(n * W)` for standard tabulation, `O(W)` for space-optimized.

---

### d. 0/1 Knapsack - Reconstructing the Set of Items

After computing the DP table (e.g., using the `(n+1) x (W+1)` table from `knapsackTabulation`), we can backtrack to find which items were included.

**Approach (Backtracking the DP Table):**
1.  Start from `dp[n][W]`.
2.  Let `i = n`, `w = W`.
3.  While `i > 0` and `w > 0`:
    *   If `dp[i][w]` is the same as `dp[i-1][w]`, it means item `i-1` (from the original arrays) was *not* included. Decrement `i`.
    *   Else (if `dp[i][w]` is different from `dp[i-1][w]`, it must be because item `i-1` was included):
        *   Add item `i-1` to the list of selected items.
        *   Subtract its value from the total value and its weight from the current capacity: `w = w - weights[i-1]`.
        *   Decrement `i`.

**JavaScript Code (Extending `knapsackTabulation`):**
```javascript
function knapsackTabulationWithItems(values, weights, capacity) {
    const n = values.length;
    const dp = Array(n + 1).fill(null).map(() => Array(capacity + 1).fill(0));

    for (let i = 1; i <= n; i++) {
        const currentWeight = weights[i - 1];
        const currentValue = values[i - 1];
        for (let w = 0; w <= capacity; w++) {
            if (currentWeight <= w) {
                dp[i][w] = Math.max(dp[i - 1][w], currentValue + dp[i - 1][w - currentWeight]);
            } else {
                dp[i][w] = dp[i - 1][w];
            }
        }
    }

    // Reconstruct the items
    const selectedItems = [];
    let i = n;
    let w = capacity;
    while (i > 0 && w > 0) {
        // If dp[i][w] came from dp[i-1][w], item i-1 was not included
        if (dp[i][w] === dp[i-1][w]) {
            i--;
        } else {
            // Item i-1 was included
            selectedItems.push({value: values[i-1], weight: weights[i-1], index: i-1});
            w -= weights[i-1];
            i--;
        }
    }
    
    return { maxValue: dp[n][capacity], selectedItems: selectedItems.reverse() };
}

console.log("\n0/1 Knapsack (Tabulation - With Items):");
const resultWithItems = knapsackTabulationWithItems(ksValues, ksWeights, ksCapacity);
console.log(`Max value = ${resultWithItems.maxValue}`); // Expected: 220
console.log("Selected items:", resultWithItems.selectedItems); 
// Expected: Item with value 120, weight 30 (index 2) AND Item with value 100, weight 20 (index 1)
// [{value: 100, weight: 20, index: 1}, {value: 120, weight: 30, index: 2}]
```
*Note: The "Sets Method" is not a standard standalone algorithm name. This item reconstruction is the common way to find the set.*

---

## 3. Matrix Chain Multiplication (MCM)

**Problem Statement:** Given a sequence of `n` matrices `A1, A2, ..., An`, where matrix `A_i` has dimensions `p_{i-1} x p_i`, find the most efficient way (minimum number of scalar multiplications) to multiply these matrices. We can only multiply two matrices at a time. The order of multiplication matters.

**Example:** `A (10x30), B (30x5), C (5x60)`
*   `(AB)C`: Cost of `AB` is `10*30*5 = 1500` (result `10x5`). Cost of `(AB)C` is `10*5*60 = 3000`. Total = `1500 + 3000 = 4500`.
*   `A(BC)`: Cost of `BC` is `30*5*60 = 9000` (result `30x60`). Cost of `A(BC)` is `10*30*60 = 18000`. Total = `9000 + 18000 = 27000`.
Clearly, `(AB)C` is better.

**Input:** An array `dims` where `dims[i-1]` and `dims[i]` are the dimensions of matrix `A_i`. So, for `n` matrices, `dims` has length `n+1`. `A_i` is `dims[i-1] x dims[i]`.

---

### a. Matrix Chain Multiplication using Recursion (Naive)

**Approach:**
Define `mcmRecursive(dims, i, j)` as the minimum cost to multiply matrices `A_i` through `A_j`.
The matrices involved are `A_i, A_{i+1}, ..., A_j`. The dimensions are `dims[i-1] x dims[i]`, `dims[i] x dims[i+1]`, ..., `dims[j-1] x dims[j]`.

*   **Base Case:** If `i == j` (only one matrix), cost is 0 (no multiplication needed).
*   **Recursive Step:**
    Try all possible split points `k` between `i` and `j-1`.
    The split `k` means we calculate `(A_i ... A_k) * (A_{k+1} ... A_j)`.
    Cost for a split `k` = `mcmRecursive(dims, i, k)` (cost of left part)
                        `+ mcmRecursive(dims, k+1, j)` (cost of right part)
                        `+ dims[i-1] * dims[k] * dims[j]` (cost of multiplying the two resulting matrices).
    Return the minimum cost over all `k`.

**JavaScript Code:**
```javascript
function mcmRecursive(dims, i, j) {
    // Base case: If there's only one matrix, no multiplication needed.
    if (i === j) {
        return 0;
    }

    let minCost = Infinity;

    // Try all possible split points k
    // (A_i ... A_k) * (A_{k+1} ... A_j)
    for (let k = i; k < j; k++) {
        const cost = mcmRecursive(dims, i, k) +    // Cost of (A_i...A_k)
                     mcmRecursive(dims, k + 1, j) +  // Cost of (A_{k+1}...A_j)
                     dims[i - 1] * dims[k] * dims[j]; // Cost to multiply results

        if (cost < minCost) {
            minCost = cost;
        }
    }
    return minCost;
}

// Example: A1(10x30), A2(30x5), A3(5x60)
// dims array represents p0, p1, p2, p3 ...
// A_i has dimensions dims[i-1] x dims[i]
// So for 3 matrices, we need dims of length 4.
// We want to compute M[1, n] (for 1-based indexing of matrices)
const mcmDims1 = [10, 30, 5, 60]; // A1=10x30, A2=30x5, A3=5x60. Matrices are A_1 to A_3
console.log("\nMatrix Chain Multiplication (Recursive - Naive):");
// For matrices A_1 to A_n, call with (dims, 1, n)
console.log(`Min multiplications = ${mcmRecursive(mcmDims1, 1, mcmDims1.length - 1)}`); // Expected: 4500

const mcmDims2 = [40, 20, 30, 10, 30]; // A1(40x20), A2(20x30), A3(30x10), A4(10x30)
// Matrices A_1 to A_4
console.log(`Min multiplications = ${mcmRecursive(mcmDims2, 1, mcmDims2.length - 1)}`); // Expected: 26000
```
**Time Complexity:** Exponential, roughly `O(2^n)` or worse (Catalan numbers), due to overlapping subproblems.
**Space Complexity:** `O(n)` for recursion stack.

---

### b. Matrix Chain Multiplication using Memoization (Top-Down DP)

**Approach:**
Use the same recursive structure, but store results of `mcm(i, j)` in `memo[i][j]`.

**JavaScript Code:**
```javascript
function mcmMemo(dims, i, j, memo) {
    if (i === j) {
        return 0;
    }
    if (memo[i] && memo[i][j] !== undefined) {
        return memo[i][j];
    }

    let minCost = Infinity;
    for (let k = i; k < j; k++) {
        const cost = mcmMemo(dims, i, k, memo) +
                     mcmMemo(dims, k + 1, j, memo) +
                     dims[i - 1] * dims[k] * dims[j];
        if (cost < minCost) {
            minCost = cost;
        }
    }

    if (!memo[i]) memo[i] = {};
    memo[i][j] = minCost;
    return minCost;
}

console.log("\nMatrix Chain Multiplication (Memoization):");
const memoMCM1 = []; // Or Array(mcmDims1.length).fill(null).map(() => Array(mcmDims1.length).fill(undefined));
console.log(`Min multiplications = ${mcmMemo(mcmDims1, 1, mcmDims1.length - 1, memoMCM1)}`); // Expected: 4500

const memoMCM2 = [];
console.log(`Min multiplications = ${mcmMemo(mcmDims2, 1, mcmDims2.length - 1, memoMCM2)}`); // Expected: 26000
```
**Time Complexity:** `O(n^3)`. There are `O(n^2)` subproblems `(i,j)`. Each takes `O(n)` time to compute (the loop for `k`).
**Space Complexity:** `O(n^2)` for `memo` table, plus `O(n)` for recursion stack.

---

### c. Matrix Chain Multiplication using Tabulation (Bottom-Up DP)

**Approach:**
*   `dp[i][j]` stores the minimum cost to multiply matrices `A_i` through `A_j`.
*   The table is filled based on the `length` of the chain.
*   `length = 1`: `dp[i][i] = 0` (cost to multiply a single matrix).
*   `length = 2`: `dp[i][i+1]` cost to multiply `A_i * A_{i+1}`.
*   ...
*   `length = n`: `dp[1][n]` is the final answer.

**Iteration:**
For `length` from `2` to `n` (length of the chain being considered):
  For `i` from `1` to `n - length + 1`:
    `j = i + length - 1`
    `dp[i][j] = Infinity`
    For `k` from `i` to `j-1` (split point):
      `cost = dp[i][k] + dp[k+1][j] + dims[i-1] * dims[k] * dims[j]`
      `dp[i][j] = min(dp[i][j], cost)`

**JavaScript Code:**
```javascript
function mcmTabulation(dims) {
    const n = dims.length - 1; // Number of matrices
    if (n <= 1) return 0; // 0 or 1 matrix needs 0 multiplications

    // dp[i][j] = min cost to multiply matrices A_i through A_j
    // Indices i and j are 1-based for matrices.
    const dp = Array(n + 1).fill(null).map(() => Array(n + 1).fill(0));

    // length is the chain length
    for (let len = 2; len <= n; len++) {
        for (let i = 1; i <= n - len + 1; i++) {
            const j = i + len - 1;
            dp[i][j] = Infinity;
            for (let k = i; k < j; k++) { // k is the split point
                // Cost = cost(A_i..A_k) + cost(A_{k+1}..A_j) + cost( (A_i..A_k) * (A_{k+1}..A_j) )
                // (A_i..A_k) results in dims[i-1] x dims[k]
                // (A_{k+1}..A_j) results in dims[k] x dims[j]
                const cost = dp[i][k] + dp[k + 1][j] + dims[i - 1] * dims[k] * dims[j];
                if (cost < dp[i][j]) {
                    dp[i][j] = cost;
                }
            }
        }
    }
    return dp[1][n];
}

console.log("\nMatrix Chain Multiplication (Tabulation):");
console.log(`Min multiplications = ${mcmTabulation(mcmDims1)}`); // Expected: 4500
console.log(`Min multiplications = ${mcmTabulation(mcmDims2)}`); // Expected: 26000
```
**Time Complexity:** `O(n^3)` due to three nested loops (`len`, `i`, `k`).
**Space Complexity:** `O(n^2)` for the `dp` table.

---

## 4. Longest Common Subsequence (LCS)

**Problem Statement (Recap):** Given two strings, `text1` and `text2`, find the length of the longest subsequence present in both of them. A subsequence is formed by deleting zero or more characters from the original string.

---

### a. LCS - Recursion (Naive)

**Approach:**
Define `lcsRecursive(text1, text2, i, j)` as the length of LCS of `text1[i...]` and `text2[j...]`.

*   **Base Case:** If `i == text1.length` or `j == text2.length` (one string exhausted), LCS is 0.
*   **Recursive Step:**
    *   If `text1[i] == text2[j]`: The characters match. This character is part of LCS.
      LCS = `1 + lcsRecursive(text1, text2, i + 1, j + 1)`.
    *   If `text1[i] != text2[j]`: The characters don't match.
      LCS = `max(lcsRecursive(text1, text2, i + 1, j),  // Skip char from text1
                  lcsRecursive(text1, text2, i, j + 1))   // Skip char from text2`

**JavaScript Code:**
```javascript
function lcsRecursive(text1, text2, i, j) {
    // Base case: If either string is exhausted
    if (i === text1.length || j === text2.length) {
        return 0;
    }

    if (text1[i] === text2[j]) {
        return 1 + lcsRecursive(text1, text2, i + 1, j + 1);
    } else {
        return Math.max(
            lcsRecursive(text1, text2, i + 1, j),
            lcsRecursive(text1, text2, i, j + 1)
        );
    }
}

const str1 = "AGGTAB";
const str2 = "GXTXAYB";
console.log("\nLongest Common Subsequence (Recursive - Naive):");
console.log(`LCS of "${str1}" and "${str2}" = ${lcsRecursive(str1, str2, 0, 0)}`); // Expected: 4 ("GTAB")

const str3 = "ABCDGH";
const str4 = "AEDFHR";
console.log(`LCS of "${str3}" and "${str4}" = ${lcsRecursive(str3, str4, 0, 0)}`); // Expected: 3 ("ADH")
```
**Time Complexity:** `O(2^(m+n))` in worst case, where `m` and `n` are lengths of strings. (More accurately, `O(2^m)` if `m < n`, or `O(2^n)` if `n < m`).
**Space Complexity:** `O(m+n)` for recursion stack.

---

### b. LCS - Memoization (Top-Down DP)

**Approach:**
Use the same recursive structure. Store results of `lcs(i, j)` in `memo[i][j]`.

**JavaScript Code:**
(This was covered in the previous response, presented here for completeness)
```javascript
function lcsMemo(text1, text2, i, j, memo) {
    if (i === text1.length || j === text2.length) {
        return 0;
    }
    if (memo[i] && memo[i][j] !== undefined) {
        return memo[i][j];
    }

    let result;
    if (text1[i] === text2[j]) {
        result = 1 + lcsMemo(text1, text2, i + 1, j + 1, memo);
    } else {
        result = Math.max(
            lcsMemo(text1, text2, i + 1, j, memo),
            lcsMemo(text1, text2, i, j + 1, memo)
        );
    }
    
    if (!memo[i]) memo[i] = {};
    memo[i][j] = result;
    return result;
}

console.log("\nLongest Common Subsequence (Memoization):");
const memoLCS1 = [];
console.log(`LCS of "${str1}" and "${str2}" = ${lcsMemo(str1, str2, 0, 0, memoLCS1)}`); // Expected: 4
const memoLCS2 = [];
console.log(`LCS of "${str3}" and "${str4}" = ${lcsMemo(str3, str4, 0, 0, memoLCS2)}`); // Expected: 3
```
**Time Complexity:** `O(m * n)`. Each subproblem `(i,j)` is solved once.
**Space Complexity:** `O(m * n)` for `memo` table, plus `O(m+n)` for recursion stack.

---

### c. LCS - Tabulation (Bottom-Up DP)

**Approach:**
*   `dp[i][j]` = length of LCS of `text1[0...i-1]` and `text2[0...j-1]`.
*   Table size `(m+1) x (n+1)`.
*   **Initialization:** `dp[0][j] = 0`, `dp[i][0] = 0`.
*   **Iteration:**
    For `i` from `1` to `m`:
        For `j` from `1` to `n`:
            If `text1[i-1] == text2[j-1]`: `dp[i][j] = 1 + dp[i-1][j-1]`
            Else: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`
*   The final answer is `dp[m][n]`.

**JavaScript Code (Including LCS reconstruction):**
(This was covered in the previous response, presented here for completeness)
```javascript
function lcsTabulation(text1, text2) {
    const m = text1.length;
    const n = text2.length;
    const dp = Array(m + 1).fill(null).map(() => Array(n + 1).fill(0));

    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    // Reconstruct the LCS string
    let lcsString = "";
    let i = m, j = n;
    while (i > 0 && j > 0) {
        if (text1[i-1] === text2[j-1]) {
            lcsString = text1[i-1] + lcsString;
            i--; j--;
        } else if (dp[i-1][j] > dp[i][j-1]) {
            i--; // Move up
        } else {
            j--; // Move left
        }
    }
    
    return { length: dp[m][n], lcs: lcsString };
}

console.log("\nLongest Common Subsequence (Tabulation):");
const lcsResultTab1 = lcsTabulation(str1, str2);
console.log(`LCS of "${str1}" and "${str2}" = Length: ${lcsResultTab1.length}, Sequence: "${lcsResultTab1.lcs}"`); // Expected: 4, "GTAB"
const lcsResultTab2 = lcsTabulation(str3, str4);
console.log(`LCS of "${str3}" and "${str4}" = Length: ${lcsResultTab2.length}, Sequence: "${lcsResultTab2.lcs}"`); // Expected: 3, "ADH"
```
**Time Complexity:** `O(m * n)`.
**Space Complexity:** `O(m * n)` for `dp` table. (Can be optimized to `O(min(m,n))` if only length is needed).

---

## 5. Max Subarray Sum

**Problem Statement:** Given an array of integers (can be positive, negative, or zero), find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Example:** `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]` -> Output: `6` (Subarray `[4, -1, 2, 1]`)

---

### a. Max Subarray Sum - Brute Force & "Improvement"

*   **Brute Force (Naive):** Consider all possible subarrays.
    *   Outer loop for start index `i` from `0` to `n-1`.
    *   Inner loop for end index `j` from `i` to `n-1`.
    *   Innermost loop to sum elements from `i` to `j`.
    *   Time: `O(n^3)`.

*   **"Improvement" (Slightly Better Brute Force):**
    *   Outer loop for start index `i`.
    *   Inner loop for end index `j`. Calculate sum `nums[i...j]` incrementally.
    *   Time: `O(n^2)`.

```javascript
// O(n^2) "Improved" Brute Force
function maxSubarraySumN2(nums) {
    if (!nums || nums.length === 0) return 0; // Or throw error for empty

    let maxSum = -Infinity;
    for (let i = 0; i < nums.length; i++) {
        let currentSum = 0;
        for (let j = i; j < nums.length; j++) {
            currentSum += nums[j];
            if (currentSum > maxSum) {
                maxSum = currentSum;
            }
        }
    }
    return maxSum;
}
const mssNums = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
console.log("\nMax Subarray Sum (O(n^2) Brute Force):");
console.log(`Max sum for ${mssNums} = ${maxSubarraySumN2(mssNums)}`); // Expected: 6
```

---

### b. Max Subarray Sum using DP

**State:** `dp[i]` = maximum sum of a subarray *ending at index `i`*.
**Recurrence Relation:**
To find `dp[i]`, the subarray ending at `i` either:
1.  Starts at `i` (i.e., it's just `nums[i]`).
2.  Is an extension of the maximum subarray ending at `i-1` (i.e., `dp[i-1] + nums[i]`).
So, `dp[i] = max(nums[i], dp[i-1] + nums[i])`.

**Base Case:** `dp[0] = nums[0]`.
The final answer is the maximum value in the `dp` array (since the max subarray can end anywhere).

**JavaScript Code:**
```javascript
function maxSubarraySumDP(nums) {
    if (!nums || nums.length === 0) return 0;

    const n = nums.length;
    const dp = new Array(n);
    dp[0] = nums[0];
    let overallMaxSum = dp[0];

    for (let i = 1; i < n; i++) {
        dp[i] = Math.max(nums[i], dp[i - 1] + nums[i]);
        if (dp[i] > overallMaxSum) {
            overallMaxSum = dp[i];
        }
    }
    return overallMaxSum;
}

console.log("\nMax Subarray Sum (DP):");
console.log(`Max sum for ${mssNums} = ${maxSubarraySumDP(mssNums)}`); // Expected: 6
```
**Time Complexity:** `O(n)`.
**Space Complexity:** `O(n)` for the `dp` array.

---

### c. Kadane's Max Subarray Sum Algorithm

Kadane's algorithm is essentially the space-optimized version of the DP approach above. It realizes that to calculate `dp[i]`, we only need `dp[i-1]`.

**Approach:**
Maintain two variables:
*   `currentMaxEndingHere`: Maximum sum of a subarray ending at the current position.
*   `overallMaxSoFar`: Maximum sum found so far across all subarrays.

Iterate through the array:
`currentMaxEndingHere = max(nums[i], currentMaxEndingHere + nums[i])`
`overallMaxSoFar = max(overallMaxSoFar, currentMaxEndingHere)`

**JavaScript Code:**
```javascript
function kadanesMaxSubarraySum(nums) {
    if (!nums || nums.length === 0) return 0;

    let currentMaxEndingHere = nums[0];
    let overallMaxSoFar = nums[0];

    for (let i = 1; i < nums.length; i++) {
        currentMaxEndingHere = Math.max(nums[i], currentMaxEndingHere + nums[i]);
        overallMaxSoFar = Math.max(overallMaxSoFar, currentMaxEndingHere);
    }
    return overallMaxSoFar;
}

console.log("\nKadane's Max Subarray Sum:");
console.log(`Max sum for ${mssNums} = ${kadanesMaxSubarraySum(mssNums)}`); // Expected: 6
const mssNums2 = [1];
console.log(`Max sum for ${mssNums2} = ${kadanesMaxSubarraySum(mssNums2)}`); // Expected: 1
const mssNums3 = [5, 4, -1, 7, 8];
console.log(`Max sum for ${mssNums3} = ${kadanesMaxSubarraySum(mssNums3)}`); // Expected: 23
```
**Time Complexity:** `O(n)`.
**Space Complexity:** `O(1)`.

---

### d. Max Subarray Sum using Divide and Conquer

**Approach:**
Divide the array into two halves. The maximum subarray can be in one of three places:
1.  Entirely in the left half.
2.  Entirely in the right half.
3.  Crossing the midpoint.

*   **Base Case:** If the array has only one element, that element is the max subarray sum.
*   **Recursive Step:**
    1.  Find midpoint `mid`.
    2.  Recursively find max subarray sum in left half (`arr[low...mid]`).
    3.  Recursively find max subarray sum in right half (`arr[mid+1...high]`).
    4.  Find the max subarray sum that crosses the midpoint.
        *   This involves finding the max sum ending at `mid` (iterating leftwards from `mid`) and the max sum starting at `mid+1` (iterating rightwards from `mid+1`). The sum of these two is the max crossing sum.
    5.  Return `max(left_sum, right_sum, crossing_sum)`.

**JavaScript Code:**
```javascript
function maxCrossingSum(nums, low, mid, high) {
    // Include elements on left of mid.
    let sum = 0;
    let leftSum = -Infinity;
    for (let i = mid; i >= low; i--) {
        sum += nums[i];
        if (sum > leftSum) {
            leftSum = sum;
        }
    }

    // Include elements on right of mid
    sum = 0;
    let rightSum = -Infinity;
    for (let i = mid + 1; i <= high; i++) {
        sum += nums[i];
        if (sum > rightSum) {
            rightSum = sum;
        }
    }
    // Return sum of elements on left and right of midpoint.
    // If both leftSum and rightSum are -Infinity (e.g., all negative numbers in segments),
    // this can still lead to issues if not handled carefully (e.g., if only one part is meaningful).
    // However, for a general subarray sum, they can combine. If one side is empty or non-contributing,
    // its sum would be 0 or very negative.
    // A robust crossing sum must consider a non-empty left and non-empty right.
    // But the logic here implies they must cross, so leftSum and rightSum are max sums from each side *towards* the mid.
    return leftSum + rightSum;
}

function maxSubarraySumDivideAndConquer(nums, low, high) {
    // Base Case: Only one element
    if (low === high) {
        return nums[low];
    }

    // Find middle point
    const mid = Math.floor((low + high) / 2);

    // Return maximum of following three possible cases:
    // a) Maximum subarray sum in left half
    const maxLeftSum = maxSubarraySumDivideAndConquer(nums, low, mid);
    // b) Maximum subarray sum in right half
    const maxRightSum = maxSubarraySumDivideAndConquer(nums, mid + 1, high);
    // c) Maximum subarray sum such that the subarray crosses the midpoint
    const maxCrossSum = maxCrossingSum(nums, low, mid, high);

    return Math.max(maxLeftSum, maxRightSum, maxCrossSum);
}

console.log("\nMax Subarray Sum (Divide and Conquer):");
// Initial call
console.log(`Max sum for ${mssNums} = ${maxSubarraySumDivideAndConquer(mssNums, 0, mssNums.length - 1)}`); // Expected: 6
```
**Time Complexity:** `T(n) = 2T(n/2) + O(n)` (for `maxCrossingSum`). By Master Theorem, this is `O(n log n)`.
**Space Complexity:** `O(log n)` due to recursion stack.

---

### 9. How to Identify a Dynamic Programming Problem?
There's no magic formula, but here are strong indicators:

1.  **Optimization Problems:** The problem asks for a minimum or maximum value (e.g., shortest path, max profit, min cost, longest sequence).
2.  **Counting Problems:** The problem asks to count the number of ways to do something (e.g., number of ways to make change, number of paths).
3.  **Decision Problems:** The problem asks if something is possible (e.g., can a string be segmented into dictionary words?).
4.  **"Stages" or "Choices":** The problem can be broken down into stages, and at each stage, you make a choice. The choices affect future stages.
5.  **Recursive Structure:** You can define the solution to a problem in terms of solutions to smaller instances of the same problem.
6.  **Overlapping Subproblems Hint:** If a naive recursive solution results in re-calculating the same subproblem multiple times, DP is likely applicable. Draw the recursion tree to visualize this.
7.  **Optimal Substructure Hint:** If an optimal solution to the overall problem implies that the solutions to its constituent subproblems are also optimal, DP might be a good fit.

Often, if a greedy approach seems plausible but you find counterexamples where local optima don't lead to global optima, DP might be the more robust (though often more complex) alternative.

---

Dynamic Programming is a vast topic with many patterns (e.g., DP on subsets, DP on trees, digit DP). These examples cover some fundamental types. The key is practice: the more problems you solve, the better you'll get at recognizing the patterns and formulating the states and transitions.

What would you like to tackle next within DP, or shall we move to another topic?
