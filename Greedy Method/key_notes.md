**Greedy Method** : discuss how to approach those problems smartly, along with more examples relevant to top-tier interviews.

---

## Key Concepts & Strategies for Solving Greedy Problems Smartly

The "Greedy" approach seems simple, but proving its correctness and identifying when to use it can be tricky. Here's how to think smartly about Greedy problems:

1.  **Identify an Optimization Problem:**
    *   Greedy algorithms are almost always used for optimization problems: find the minimum/maximum, best way, most efficient, etc.

2.  **Look for a "Greedy Choice":**
    *   Can you make a decision at each step that seems locally optimal or provides the most immediate benefit according to some criterion?
    *   **What is that criterion?** Sorting is very often involved. Common criteria:
        *   Smallest/Largest value
        *   Earliest/Latest start/finish time
        *   Best ratio (e.g., value/weight)
        *   Shortest/Longest
    *   **Test simple choices:** "What if I always pick X first?" "What if I sort by Y?"

3.  **Verify the Greedy Choice Property (The Hardest Part):**
    *   **"Stays Ahead" Argument:** Try to argue that by making the greedy choice, your partial solution is always at least as good as (or "ahead of") any other partial solution.
    *   **"Exchange Argument" (Proof by Contradiction):**
        1.  Assume there's an optimal solution `OPT` that does *not* use your greedy choice at the first step where they differ.
        2.  Let `G` be the greedy choice made at that step.
        3.  Show that you can modify `OPT` by "exchanging" its choice with `G` to get a new solution `OPT'` that is:
            *   Still valid (meets problem constraints).
            *   At least as good as `OPT` (or strictly better).
            *   "Closer" to the greedy solution.
        4.  Repeat this until `OPT'` becomes the greedy solution, proving the greedy solution is also optimal.
    *   **No Backtracking:** A key characteristic of greedy is that once a choice is made, it's never undone. If you find yourself needing to reconsider past choices, it's likely not a pure greedy problem (might be DP or backtracking).

4.  **Verify Optimal Substructure (Usually Simpler):**
    *   If you make a greedy choice, are you left with a smaller version of the original problem?
    *   If the solution to this subproblem is optimal, will combining it with your greedy choice yield an optimal solution to the original problem?

5.  **Look for Counterexamples:**
    *   Before diving deep into a proof, try to break your greedy strategy with edge cases or tricky inputs.
    *   If you find a counterexample, your greedy choice is wrong, or the problem isn't solvable by a simple greedy approach.
    *   **Example:** Making change with denominations {1, 7, 10} for amount 14. Greedy (10+1+1+1+1=5 coins) vs Optimal (7+7=2 coins).

6.  **Common Scenarios Where Greedy Works:**
    *   **Selecting items based on a single sorted order:** Activity Selection (sort by finish time), Fractional Knapsack (sort by value/weight ratio).
    *   **Building something piece by piece:** Huffman Coding (merge smallest frequencies), Prim's/Kruskal's (add cheapest safe edge).
    *   **Interval Problems:** Often involve sorting by start or end times.

7.  **Contrast with Dynamic Programming:**
    *   **Greedy:** Makes one locally optimal choice, commits to it.
    *   **DP:** Explores *all* possible choices for a subproblem and then picks the best one. DP remembers results of subproblems.
    *   If a greedy choice might prevent you from making future optimal choices that outweigh the current local gain, DP might be needed. (e.g., 0/1 Knapsack vs Fractional Knapsack).

8.  **Clarity and Simplicity:**
    *   Greedy algorithms are often very elegant and simple to implement once the correct greedy choice is found. If your greedy logic becomes overly complex, question if it's truly greedy or if there's a simpler criterion.

9.  **Consider Sorting Order:**
    *   Many greedy algorithms involve sorting the input based on some property. Experiment with different sorting orders if your initial greedy choice doesn't seem to work.
    *   E.g., for interval problems, should you sort by start times, end times, or interval lengths?

10. **Focus on the "Why":**
    *   Don't just state the greedy choice; be prepared to explain *why* it leads to a global optimum (even if it's just an intuitive argument, followed by trying to formalize it with an exchange argument).

---

## More Common Greedy Problems Asked in FAANG/Top Companies

Beyond the absolute classics (Activity Selection, Fractional Knapsack, Huffman, Dijkstra, Prim, Kruskal), here are others:

1.  **Gas Station / Car Pooling / Task Scheduler (variations):**
    *   **Gas Station:**
        *   **Problem:** `n` gas stations on a circular route. `gas[i]` is gas at station `i`, `cost[i]` is cost to travel from `i` to `i+1`. Find starting station to complete the circuit, if possible.
        *   **Greedy Insight:** If total gas >= total cost, a solution exists. Iterate through stations. If you start at `i` and run out of gas before reaching `j`, then no station between `i` and `j` (exclusive of `j`) can be a valid starting point.
    *   **Task Scheduler (CPU tasks with cooldown):**
        *   **Problem:** `n` tasks, cooldown period between same tasks. Minimize time.
        *   **Greedy Insight:** At each step, schedule the most frequent task that is not on cooldown. If all available tasks are on cooldown, insert an "idle" slot. (Often uses a Max Heap).

2.  **Jump Game (I & II):**
    *   **Jump Game I:**
        *   **Problem:** Array `nums`, `nums[i]` is max jump length from `i`. Can you reach the last index?
        *   **Greedy Insight:** Keep track of the `max_reach` possible from current position. If `current_index > max_reach`, you're stuck.
    *   **Jump Game II:**
        *   **Problem:** Same setup, find minimum jumps to reach the last index.
        *   **Greedy Insight (Level-by-Level / BFS-like):** At each step (jump), find the farthest you can reach in the *next* jump.
            `current_farthest`: Farthest you can reach with current number of jumps.
            `next_farthest`: Farthest you can reach with one more jump.
            Increment jumps when `i > current_farthest`.

3.  **Merge Intervals:**
    *   **Problem:** Given a collection of intervals, merge all overlapping intervals.
    *   **Greedy Insight:** Sort intervals by their start times. Iterate through sorted intervals. If current interval overlaps with the last merged interval, extend the merged interval. Otherwise, add a new merged interval.

4.  **Non-overlapping Intervals / Erase Overlap Intervals:**
    *   **Problem:** Given intervals, find minimum intervals to remove so remaining are non-overlapping. (Equivalent to Activity Selection: find max non-overlapping, then subtract from total).
    *   **Greedy Insight:** Sort by end times. Keep the interval that finishes earliest.

5.  **Minimum Number of Arrows to Burst Balloons:**
    *   **Problem:** Balloons on a 2D plane represented by `[x_start, x_end]`. Arrow shot vertically at `x` bursts all balloons whose `x_start <= x <= x_end`. Find min arrows.
    *   **Greedy Insight:** Sort balloons by their end points. Shoot an arrow at the end point of the first balloon. This arrow will burst this balloon and any others it covers. Remove burst balloons and repeat.

6.  **Assign Cookies:**
    *   **Problem:** Children `g` (greed factor), cookies `s` (size). Cookie `j` can be given to child `i` if `s[j] >= g[i]`. Maximize satisfied children.
    *   **Greedy Insight:** Sort both `g` and `s`. Try to satisfy the least greedy child with the smallest cookie that can satisfy them. (Two-pointer approach).

7.  **Remove K Digits:**
    *   **Problem:** Given string `num` (non-negative integer) and `k`, remove `k` digits to make the smallest possible new number.
    *   **Greedy Insight:** Iterate through digits. Maintain a stack (or result string). If current digit is smaller than stack top AND `k > 0`, pop from stack (remove a larger preceding digit) and decrement `k`. This ensures digits are in increasing order as much as possible from left.

8.  **Candy:**
    *   **Problem:** `n` children with ratings. Give candies such that:
        1. Each child gets at least one candy.
        2. Children with higher rating get more candies than their *immediate* neighbors.
        Minimize total candies.
    *   **Greedy Insight (Two Passes):**
        1.  Pass left-to-right: If `rating[i] > rating[i-1]`, then `candies[i] = candies[i-1] + 1`.
        2.  Pass right-to-left: If `rating[i] > rating[i+1]`, then `candies[i] = max(candies[i], candies[i+1] + 1)`. (The `max` ensures the left-to-right condition isn't violated).

9.  **Partition Labels:**
    *   **Problem:** String `S`. Partition into as many parts as possible such that each letter appears in at most one part. Return list of sizes of these parts.
    *   **Greedy Insight:** For each character, find its last occurrence in `S`. Iterate through `S`. Maintain `current_partition_end` (max last occurrence of chars seen so far in current partition). When `current_index == current_partition_end`, a partition is complete.

10. **Two City Scheduling:**
    *   **Problem:** `2N` people. Fly them to city A or city B. `costs[i] = [costA_i, costB_i]`. Exactly `N` people to city A, `N` to city B. Minimize total cost.
    *   **Greedy Insight:** Calculate the "refund" or "extra cost" if person `i` goes to city A instead of city B (i.e., `costA_i - costB_i`). Sort people by this difference. Send the `N` people with the smallest (most negative or least positive) differences to city A. The rest go to city B.
        *   Alternatively, initially send everyone to city A. Then, select `N` people to switch to city B, choosing those who provide the biggest "savings" (`costA_i - costB_i`). The ones with the largest positive `costA_i - costB_i` means `costB_i` is much cheaper.

11. **Reorganize String / Rearrange String k Distance Apart:**
    *   **Problem:** Rearrange string so no two adjacent characters are the same (or are at least `k` distance apart).
    *   **Greedy Insight:** At each step, append the most frequent character that is different from the previously appended character (and satisfies distance constraint). Use a Max Heap to track character frequencies.

---

Remember, the key to mastering Greedy (and DP) is practice and pattern recognition. When you see a problem:

1.  Consider a Greedy approach first. Try to formulate a simple greedy choice.
2.  Spend time trying to prove it or find a counterexample. This is interview gold.
3.  If Greedy fails or seems too hard to prove, then consider DP or other techniques.

Good luck! You're building a strong foundation.
