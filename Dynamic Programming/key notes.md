Dynamic Programming is a favorite topic in FAANG/top-tier company interviews because it tests problem-solving, algorithmic thinking, and the ability to optimize. Beyond the classics we've covered, there are many other DP patterns and specific problems that frequently appear.

Let's break down some key concepts for solving DP smartly and then list more common DP problems.

---

## Key Concepts & Strategies for Solving DP Smartly

1.  **Recognize the DP Pattern (The "AHA!" Moment):**
    *   **Can I break this problem into smaller, self-similar problems?** (Recursive nature)
    *   **Do these smaller problems overlap?** (If not, maybe it's just Divide & Conquer).
    *   **Does an optimal solution to the problem contain optimal solutions to its subproblems?** (Optimal Substructure).
    *   **What am I trying to optimize (min/max), count, or decide?**

2.  **Define the State(s) Clearly:** This is CRUCIAL.
    *   What information do I need to carry forward to solve a larger subproblem?
    *   The state variables should uniquely identify a subproblem.
    *   Common state patterns:
        *   `dp[i]`: Solution for the first `i` elements, or considering element `i`.
        *   `dp[i][j]`: Solution considering elements up to `i` from one input and `j` from another, or a range `[i, j]`.
        *   `dp[i][state_flag]`: `state_flag` could be boolean (e.g., `can_buy_stock`, `is_previous_char_a_vowel`).
    *   **Start simple:** If you're unsure, try a state definition. If it doesn't work, think about what extra information you're missing to make decisions.

3.  **Formulate the Recurrence Relation (The "Transition"):**
    *   How can I compute `dp[state]` using previously computed `dp[smaller_states]`?
    *   Consider all possible choices or actions at the current state.
    *   **Example (Knapsack):** At item `i`, choice 1: *exclude* item `i` (state depends on `dp[i-1][w]`). Choice 2: *include* item `i` (state depends on `dp[i-1][w - weight[i]]`).
    *   **Think about the last step/decision:** How did I arrive at the solution for `dp[state]`? What was the last action taken?

4.  **Identify Base Cases:**
    *   What are the smallest, simplest subproblems whose solutions are known directly?
    *   These are often the termination conditions for recursion or the initialization values for tabulation.
    *   `dp[0]`, `dp[i][0]`, `dp[0][j]` are common.

5.  **Choose Approach: Memoization vs. Tabulation:**
    *   **Memoization (Top-Down):**
        *   Often more intuitive to write if you can think of a plain recursive solution first.
        *   Solves only the subproblems that are actually needed.
        *   Can be slightly slower due to recursion overhead and hash map lookups (if keys are complex).
    *   **Tabulation (Bottom-Up):**
        *   Requires figuring out the correct order of computation (dependencies).
        *   Often more efficient (no recursion overhead).
        *   Can be easier to optimize for space.
    *   **Practice both!** Sometimes one is clearly easier or more efficient than the other.

6.  **Order of Iteration (for Tabulation):**
    *   Ensure that when you are computing `dp[state]`, all the `dp[previous_states]` it depends on have already been computed.
    *   This often means iterating based on the size of the problem (e.g., length of substring, number of items considered).

7.  **Space Optimization:**
    *   If `dp[i]` only depends on `dp[i-1]` and `dp[i-2]`, you only need to store a few previous values, not the whole array/table.
    *   **Example:** Fibonacci, 0/1 Knapsack (1D array), LCS (2 rows).
    *   This is often an important follow-up question in interviews.

8.  **Backtracking for the Solution Path (Not Just the Value):**
    *   If the problem asks for the actual items/sequence (e.g., the LCS string, items in knapsack), you often need to backtrack through the DP table after it's filled.
    *   Store choices or look at how `dp[i][j]` was derived from previous states.

9.  **Practice Common Patterns:** Recognizing a problem as a variation of a known pattern is a huge advantage. Some patterns include:
    *   **1D DP:** Fibonacci, Max Subarray Sum, House Robber, Coin Change (ways).
    *   **2D DP:** LCS, Edit Distance, Knapsack, Matrix Chain Multiplication, Unique Paths.
    *   **Interval DP:** Matrix Chain Multiplication, Optimal Binary Search Tree, Burst Balloons. (Usually `dp[i][j]` for range `[i,j]`).
    *   **DP on Subsets/Bitmask DP:** Traveling Salesperson Problem (TSP), assignment problems where you need to track which elements are used. State can be `dp[mask][last_element]`.
    *   **DP with States:** Problems involving buying/selling stocks with cooldowns or transaction limits.
    *   **Digit DP:** Counting numbers in a range `[L, R]` satisfying certain properties based on their digits.

10. **Draw it Out / Use Small Examples:**
    *   If you're stuck, try a small example. Manually compute the first few DP states.
    *   Draw the recursion tree for memoization to see overlapping subproblems.
    *   Draw the DP table for tabulation to visualize dependencies.

11. **Time and Space Complexity Analysis:** Always be prepared to discuss this.
    *   Time: (Number of states) * (Time to compute one state/transition).
    *   Space: (Number of states) or optimized space.

---

## More Common DP Problems Asked in FAANG/Top Companies

Here's a list of problems, categorized by common patterns, often encountered:

### I. 1D DP (Often on sequences/arrays)

1.  **Coin Change (Minimum Coins):**
    *   **Problem:** Given coin denominations and a target amount, find the minimum number of coins to make that amount.
    *   **State:** `dp[i]` = min coins for amount `i`.
    *   **Transition:** `dp[i] = min(dp[i], 1 + dp[i - coin_value])` for each coin.

2.  **Coin Change (Number of Ways):**
    *   **Problem:** Given coin denominations and a target amount, find the number of distinct ways to make that amount.
    *   **State:** `dp[i]` = number of ways to make amount `i`.
    *   **Transition:** `dp[i] += dp[i - coin_value]` (careful with order of loops for distinct combinations vs permutations).
    *   *Often done with 2D DP `dp[coin_index][amount]` if order of coins matters for "distinctness" or if there are constraints on coin usage.*

3.  **House Robber (and variations like House Robber II, III):**
    *   **Problem:** Rob houses in a line. Cannot rob adjacent houses. Maximize loot.
    *   **State:** `dp[i]` = max loot robbing up to house `i`.
    *   **Transition:** `dp[i] = max(dp[i-1], nums[i] + dp[i-2])`.
    *   **House Robber II:** Houses are in a circle (first and last are adjacent). Solve twice: once excluding first, once excluding last.
    *   **House Robber III:** Houses are in a binary tree. `dp[node]` returns `[rob_node_value, skip_node_value]`.

4.  **Climbing Stairs:**
    *   **Problem:** `n` stairs. Can climb 1 or 2 steps at a time. How many distinct ways to reach the top? (Fibonacci variation).
    *   **State:** `dp[i]` = ways to reach stair `i`.
    *   **Transition:** `dp[i] = dp[i-1] + dp[i-2]`.

5.  **Word Break:**
    *   **Problem:** Given a string `s` and a dictionary of words, can `s` be segmented into a space-separated sequence of one or more dictionary words?
    *   **State:** `dp[i]` = boolean, true if `s[0...i-1]` can be segmented.
    *   **Transition:** `dp[i]` is true if there exists `j < i` such that `dp[j]` is true AND `s[j...i-1]` is in the dictionary.

6.  **Longest Increasing Subsequence (LIS):**
    *   **Problem:** Find the length of the longest strictly increasing subsequence in an array.
    *   **State:** `dp[i]` = length of LIS ending at index `i`.
    *   **Transition:** `dp[i] = 1 + max(dp[j])` for all `j < i` where `nums[j] < nums[i]`.
    *   *(There's also an O(n log n) solution using patience sorting/binary search, not strictly DP but related to optimal substructure).*

7.  **Decode Ways (Number of Decodings):**
    *   **Problem:** A message containing letters from A-Z is being encoded to numbers using `A=1, B=2, ..., Z=26`. Given an encoded string of digits, count the number of ways to decode it.
    *   **State:** `dp[i]` = number of ways to decode `s[0...i-1]`.
    *   **Transition:** Considers 1-digit and 2-digit decodings from `s[i-1]` and `s[i-2]s[i-1]`.

8.  **Best Time to Buy and Sell Stock (Variations):**
    *   **I:** One transaction allowed. (Greedy/simple scan).
    *   **II:** Unlimited transactions. (Greedy: sum of all positive price differences).
    *   **III:** At most two transactions. (DP state: `dp[k][i]` = max profit with `k` transactions up to day `i`, or state for `buy1, sell1, buy2, sell2`).
    *   **IV:** At most `k` transactions. (Similar to III, but `k` is a parameter).
    *   **With Cooldown:** After selling, must wait one day before buying again. (State: `dp[i][can_buy/can_sell/cooldown]`).
    *   **With Transaction Fee:** (State: `dp[i][holding_stock/not_holding_stock]`).

9.  **Paint House / Paint Fence:**
    *   **Problem:** Paint `n` houses with `k` colors, no two adjacent houses have the same color. Find min cost or number of ways.
    *   **State:** `dp[i][color]` = min cost to paint houses up to `i` with house `i` having `color`.
    *   **Transition:** `dp[i][c1] = cost[i][c1] + min(dp[i-1][c2])` for `c2 != c1`.

### II. 2D DP (Often on grids, two sequences, or ranges)

10. **Unique Paths (and variations):**
    *   **Problem:** Robot on `m x n` grid, starts top-left, wants to reach bottom-right. Can only move right or down. How many unique paths?
    *   **State:** `dp[i][j]` = number of paths to reach cell `(i,j)`.
    *   **Transition:** `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.
    *   **With Obstacles:** If `grid[i][j]` is obstacle, `dp[i][j] = 0`.

11. **Minimum Path Sum:**
    *   **Problem:** Grid with non-negative numbers. Find path from top-left to bottom-right with min sum of numbers along path. (Moves: right or down).
    *   **State:** `dp[i][j]` = min sum to reach `(i,j)`.
    *   **Transition:** `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

12. **Edit Distance (Levenshtein Distance):**
    *   **Problem:** Given two words, find min number of operations (insert, delete, replace) to convert one to the other.
    *   **State:** `dp[i][j]` = min operations for `word1[0...i-1]` and `word2[0...j-1]`.
    *   **Transition:**
        *   If `word1[i-1] == word2[j-1]`: `dp[i][j] = dp[i-1][j-1]`.
        *   Else: `dp[i][j] = 1 + min(dp[i-1][j],     // Delete from word1
                                     dp[i][j-1],     // Insert into word1
                                     dp[i-1][j-1])   // Replace in word1`

13. **Regular Expression Matching / Wildcard Matching:**
    *   **Problem:** Implement regex matching with `.` (any char) and `*` (zero or more of preceding).
    *   **State:** `dp[i][j]` = boolean, if `text[0...i-1]` matches `pattern[0...j-1]`.
    *   **Transition:** Complex, depends on `pattern[j-1]` (char, `.`, or `*`). `*` case requires careful handling (match zero, or match one and recurse).

14. **Interleaving String:**
    *   **Problem:** Given `s1, s2, s3`, check if `s3` is formed by interleaving `s1` and `s2`.
    *   **State:** `dp[i][j]` = boolean, if `s3[0...i+j-1]` can be formed by interleaving `s1[0...i-1]` and `s2[0...j-1]`.
    *   **Transition:** Checks if `s3[i+j-1]` matches `s1[i-1]` (then `dp[i-1][j]`) or `s2[j-1]` (then `dp[i][j-1]`).

15. **Longest Palindromic Subsequence:**
    *   **Problem:** Find the length of the longest palindromic subsequence of a given string.
    *   **Hint:** It's LCS of the string and its reverse.
    *   **State (direct):** `dp[i][j]` = length of LPS in `s[i...j]`.
    *   **Transition:** If `s[i] == s[j]`, `dp[i][j] = 2 + dp[i+1][j-1]`. Else `max(dp[i+1][j], dp[i][j-1])`.

16. **Longest Palindromic Substring:**
    *   **Problem:** Find the longest substring that is a palindrome.
    *   **State:** `dp[i][j]` = boolean, true if `s[i...j]` is a palindrome.
    *   **Transition:** `dp[i][j] = (s[i] == s[j]) AND dp[i+1][j-1]`. Base cases for length 1 and 2.
    *   *(Can also be solved with "Expand Around Center" in O(n^2) time, O(1) space).*

### III. Interval DP / Other Patterns

17. **Burst Balloons:**
    *   **Problem:** `n` balloons, each with a value. If you burst balloon `i`, you get `nums[left] * nums[i] * nums[right]` coins (where `left` and `right` are adjacent unburst balloons). Find max coins.
    *   **State:** `dp[i][j]` = max coins from bursting balloons in range `(i, j)` (exclusive of `i`, `j` which are boundaries).
    *   **Transition:** Iterate on `k` (last balloon to burst in `(i, j)`).
        `dp[i][j] = max(dp[i][k] + dp[k][j] + nums[i] * nums[k] * nums[j])`

18. **Optimal Binary Search Tree:**
    *   **Problem:** Given sorted keys and their search frequencies, construct a BST with minimum expected search cost.
    *   **State:** `dp[i][j]` = min cost for BST from keys `key[i]` to `key[j]`.
    *   **Transition:** Iterate `r` from `i` to `j` (root of subtree).
        `dp[i][j] = sum_freq(i,j) + min(dp[i][r-1] + dp[r+1][j])`

19. **Subset Sum Problem / Partition Equal Subset Sum:**
    *   **Problem (Subset Sum):** Given a set of non-negative integers and a value `sum`, determine if there is a subset with sum equal to `sum`.
    *   **State:** `dp[i][s]` = boolean, if sum `s` can be achieved using first `i` numbers. (Similar to Knapsack).
    *   **Partition Equal Subset Sum:** Can the set be partitioned into two subsets with equal sum? (Target sum is `total_sum / 2`).

### IV. Bitmask DP

20. **Traveling Salesperson Problem (TSP) - for small N:**
    *   **Problem:** Given `n` cities and distances, find the shortest tour that visits each city exactly once and returns to start.
    *   **State:** `dp[mask][i]` = min cost of path visiting all cities in `mask`, ending at city `i`. `mask` is a bitmask.
    *   **Transition:** `dp[mask][i] = min(dp[mask ^ (1<<i)][j] + dist[j][i])` over `j` in `mask ^ (1<<i)`.

21. **Assignment Problem / Maximize Compatibility Score:**
    *   **Problem:** `n` students, `m` mentors. Compatibility scores. Assign each student to one mentor, each mentor to at most one student, to maximize total score.
    *   **State:** `dp[i][mask]` = max score assigning first `i` students, using mentors represented by `mask`.

---

This list should give you a very good set of problems to practice. The key is not to memorize solutions, but to understand *how* the DP state and transition were derived for each pattern. Good luck with your FAANG preparation!
