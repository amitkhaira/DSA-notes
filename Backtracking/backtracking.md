Okay, let's explore the world of **Backtracking**! This is another powerful algorithmic technique, especially useful for problems involving search, permutations, combinations, and constraint satisfaction.

---

## Backtracking: Key Notes

### 1. What is Backtracking?
Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, and removing those solutions ("backtracking") that fail to satisfy the constraints of the problem at any point in time.

Think of it like navigating a maze:
*   You explore a path.
*   If you hit a dead end, you "backtrack" to the last decision point and try a different path.
*   You continue until you find a way out or exhaust all possibilities.

It's a refined form of brute-force search. Instead of generating all possible candidates and then filtering, backtracking abandons a partial candidate as soon as it determines that it cannot possibly be completed to a valid solution.

### 2. Core Idea / How it Works
1.  **Choices:** The problem involves making a sequence of choices.
2.  **Constraints:** There are rules or conditions that valid solutions must satisfy.
3.  **Exploration:** We explore potential solutions by making choices one by one.
4.  **Partial Solution:** At any point, we have a partially built candidate solution.
5.  **Check Feasibility:** After making a choice, we check if the current partial solution is still viable (doesn't violate constraints and could potentially lead to a full solution).
    *   If **viable**: Continue making the next choice (recurse).
    *   If **not viable**: "Prune" this path. Undo the last choice (backtrack) and try a different option for that choice point.
6.  **Goal State:** If a partial solution becomes a complete solution that satisfies all constraints, we have found a solution.
    *   We might need to find one solution, all solutions, or the best solution.

### 3. General Algorithm Structure (Recursive)

```javascript
function solveBacktrack(state, /* other params */) {
    // 1. Base Case(s) for successful solution
    if (isSolution(state)) {
        processSolution(state); // Add to results, count, etc.
        return; // Or return true if only one solution is needed
    }

    // 2. Base Case(s) for invalid or dead-end state (optional, sometimes covered by loop)
    if (isInvalid(state)) {
        return;
    }

    // 3. Iterate through all possible choices/moves from the current state
    for (let choice of getPossibleChoices(state)) {
        // a. Make the choice (apply to state)
        applyChoice(state, choice);
        
        // b. Recurse with the new state
        solveBacktrack(newStateFrom(state, choice), /* ... */);
        
        // c. Undo the choice (backtrack) - CRUCIAL
        //    Restore the state to what it was before making this 'choice'
        //    so that the next iteration of the loop can try a different 'choice'
        //    from the same original 'state'.
        undoChoice(state, choice); 
    }
}
```

**Key Components in the Pseudocode:**
*   `state`: Represents the current configuration or partial solution. This could be the current path in a maze, the current permutation being built, the current assignment of values, etc.
*   `isSolution(state)`: Checks if the current `state` is a complete and valid solution.
*   `processSolution(state)`: What to do when a solution is found (e.g., add it to a list of results).
*   `isInvalid(state)`: (Optional) Checks if the current state can never lead to a solution (early pruning).
*   `getPossibleChoices(state)`: Generates all valid next moves or additions from the current `state`.
*   `applyChoice(state, choice)`: Modifies the `state` by incorporating the `choice`.
*   `undoChoice(state, choice)`: Reverts the `state` to its condition before `applyChoice`. This is the "backtracking" step. This is vital for exploring other branches of the search tree.

### 4. State Space Tree
Backtracking can be visualized as a **depth-first search (DFS)** on a **state space tree**.
*   The **root** is the initial state (empty or starting configuration).
*   **Nodes** are partial solutions.
*   **Edges** represent choices made.
*   **Leaves** are either complete solutions or dead ends.
Backtracking prunes subtrees that cannot lead to a solution.

### 5. When to Use Backtracking?
Backtracking is suitable for problems where:
*   You need to find **all possible solutions** (e.g., all permutations, all combinations satisfying a condition).
*   You need to find **any one solution** (e.g., path in a maze, solving Sudoku).
*   You need to find the **optimal solution** among all possibilities, and the search space is too large for simple brute force (e.g., N-Queens problem optimized for some criteria, though often for optimization DP or branch and bound are more direct).
*   The problem involves satisfying a set of **constraints**.
*   The problem can be broken down into a sequence of decisions.

Common problem types:
*   Permutations and Combinations (with constraints).
*   Pathfinding (mazes, graphs).
*   Constraint Satisfaction Problems (Sudoku, N-Queens).
*   Parsing or splitting problems (Word Break, Expression Add Operators).

### 6. Advantages of Backtracking
*   **Systematic Exploration:** It explores the search space in an organized way (DFS).
*   **Pruning:** More efficient than naive brute force because it prunes unpromising branches early.
*   **Flexibility:** Can be adapted to find one, all, or optimal solutions.
*   **Relatively Simple to Implement:** The recursive structure is often quite natural for these types of problems once you get the hang of it.

### 7. Disadvantages/Considerations of Backtracking
*   **Time Complexity:** Can still be exponential in the worst case (`O(branches^depth)`). The effectiveness depends heavily on how much pruning can be done.
*   **Redundant Computations:** Without memoization (which would turn it into DP), it might re-explore identical sub-states if the problem structure allows it (though often states are unique due to path choices).
*   **Stack Overflow:** Deep recursion can lead to stack overflow errors for very large problem instances. (Can sometimes be converted to an iterative version using an explicit stack).
*   **Defining "State" and "Choices":** Clearly defining these and the `undo` operation is crucial and can sometimes be tricky.

### 8. Backtracking vs. Dynamic Programming vs. Greedy
*   **Greedy:** Makes one locally optimal choice at each step, never reconsiders. Aims for *one* optimal solution.
*   **Dynamic Programming:** Breaks problem into overlapping subproblems, solves each subproblem once, stores results. Typically used for optimization or counting where optimal substructure exists. Explores multiple options for subproblems to find the best.
*   **Backtracking:** Systematically searches all possible configurations. Used when you need to explore many possibilities, often with constraints. It makes a choice, explores, and if it's not good, it *undoes* the choice and tries another.

    *   *Sometimes, a backtracking solution can be optimized with memoization if subproblems overlap, effectively turning it into a DP solution (e.g., Word Break).*

---

## Classic Backtracking Problems & Code Examples (JavaScript)

The key to backtracking is the `choose -> explore -> unchoose` pattern.

Okay, let's dive deeper into these specific backtracking problems, focusing on their nuances, algorithms, and related concepts like bounding functions.

---

## 1. N-Queens Problem

### a. Introduction

**Problem Statement:**
The N-Queens puzzle is the problem of placing `N` chess queens on an `N×N` chessboard such that no two queens threaten each other. This means that no two queens can be on the same row, column, or diagonal. The task is usually to find one such configuration, all such configurations, or count the number of configurations.

**Why it's a good backtracking problem:**
*   **Choices:** For each row (or column), we need to choose a position for a queen.
*   **Constraints:** The placement of a queen is constrained by the positions of previously placed queens (no attacks).
*   **Incremental Build:** We build the solution row by row (or column by column).
*   **Pruning:** If placing a queen in a certain position immediately creates a conflict, we don't need to explore further down that path for subsequent queens.

### b. N-Queens Problem - Solution (Conceptual)

The general idea is to place queens one by one in different columns of each row.
1.  **Start with the first row (row 0).**
2.  **For each column in the current row:**
    a.  Try placing a queen at `(currentRow, currentColumn)`.
    b.  **Check if this placement is "safe"**: Does it conflict with any queens already placed in previous rows (rows `0` to `currentRow - 1`)?
        *   No other queen in the same column.
        *   No other queen on the same left-up to right-down diagonal.
        *   No other queen on the same right-up to left-down diagonal.
    c.  **If safe:**
        i.  "Place" the queen (mark it on our board representation).
        ii. If `currentRow` is the last row (`N-1`), then we have successfully placed all `N` queens. Record this solution.
        iii. Else, recursively try to place a queen in the `nextRow (currentRow + 1)`.
        iv. **Backtrack:** After the recursive call returns (meaning it has explored all possibilities from that placement), "remove" the queen from `(currentRow, currentColumn)` to allow trying other columns in the `currentRow`.
    d.  **If not safe:** Move to the next column in the `currentRow`.
3.  If all columns in the `currentRow` have been tried and no safe placement led to a solution, then this path is a dead end for the queen in the previous row, triggering further backtracking.

### c. N-Queens Algorithm (More Detailed)

We typically use a helper function for the recursive backtracking.

**Data Structures:**
*   `board`: An `N x N` 2D array (e.g., filled with `.` or `0`, and `Q` or `1` for queens).
    *   *Optimization:* We can represent the board more efficiently. Since we place one queen per row, we only need a 1D array `queenPositions[row] = col`, meaning the queen in `row` is at `col`. This inherently handles the "no two queens in the same row" constraint. We still need to check columns and diagonals.
*   `results`: A list to store valid board configurations.

**Helper Function: `solve(row)`**

```
function solveNQueens(N):
  results = []
  // board can be a 2D array of chars, or a 1D array where board[r] = c
  // Let's use 1D array `columns[row] = col_of_queen_in_that_row` for this algorithm description.
  // `columns` array of size N.
  // We also need sets/arrays to quickly check for occupied columns and diagonals.
  occupiedCols = new Set()
  occupiedDiag1 = new Set() // For diagonals where (row - col) is constant
  occupiedDiag2 = new Set() // For diagonals where (row + col) is constant

  function backtrack(row):
    if row == N: // All N queens have been placed
      // Construct and add solution to results based on `columns` array
      addSolutionToResults(columns, N, results)
      return

    for col from 0 to N-1:
      // Check if (row, col) is safe
      diag1_val = row - col
      diag2_val = row + col

      if not occupiedCols.has(col) AND
         not occupiedDiag1.has(diag1_val) AND
         not occupiedDiag2.has(diag2_val):

        // Place queen (Make choice)
        columns[row] = col
        occupiedCols.add(col)
        occupiedDiag1.add(diag1_val)
        occupiedDiag2.add(diag2_val)

        // Recurse for next row
        backtrack(row + 1)

        // Backtrack (Undo choice)
        // columns[row] will be overwritten in next iteration or when row decreases
        occupiedCols.delete(col)
        occupiedDiag1.delete(diag1_val)
        occupiedDiag2.delete(diag2_val)
  
  columns = new Array(N) // To store column position for queen in each row
  backtrack(0) // Start from row 0
  return results

// Helper to add solution (if using 1D `columns` array)
function addSolutionToResults(columns, N, results):
  current_solution_board = []
  for r from 0 to N-1:
    row_string = ""
    for c from 0 to N-1:
      if columns[r] == c:
        row_string += "Q"
      else:
        row_string += "."
    current_solution_board.push(row_string)
  results.push(current_solution_board)
```

**Explanation of Diagonal Checks:**
*   **Main Diagonal (Top-Left to Bottom-Right):** For any cell `(r, c)` on such a diagonal, the value `r - c` is constant.
*   **Anti-Diagonal (Top-Right to Bottom-Left):** For any cell `(r, c)` on such a diagonal, the value `r + c` is constant.

By using sets to store occupied column indices and these diagonal constant values, checking safety becomes an `O(1)` operation after the initial calculation.

**JavaScript Code:**
(The code provided in the previous response for N-Queens is a good example of this, using a 2D board for visualization but the core logic is the same. The version above with sets for `occupiedCols`, `occupiedDiag1`, `occupiedDiag2` is often more efficient for larger N if not constructing the full board string at each step).

```javascript
function solveNQueensOptimized(n) {
    const results = [];
    const columns = new Array(n); // columns[row] = col_of_queen
    const occupiedCols = new Set();
    const occupiedDiag1 = new Set(); // r - c
    const occupiedDiag2 = new Set(); // r + c

    function addSolution() {
        const currentBoard = [];
        for (let r = 0; r < n; r++) {
            let rowStr = "";
            for (let c = 0; c < n; c++) {
                rowStr += (columns[r] === c) ? "Q" : ".";
            }
            currentBoard.push(rowStr);
        }
        results.push(currentBoard);
    }

    function backtrack(row) {
        if (row === n) {
            addSolution();
            return;
        }

        for (let col = 0; col < n; col++) {
            const diag1Val = row - col;
            const diag2Val = row + col;

            if (!occupiedCols.has(col) && 
                !occupiedDiag1.has(diag1Val) && 
                !occupiedDiag2.has(diag2Val)) {
                
                columns[row] = col;
                occupiedCols.add(col);
                occupiedDiag1.add(diag1Val);
                occupiedDiag2.add(diag2Val);

                backtrack(row + 1);

                // No need to explicitly clear columns[row] as it will be overwritten
                occupiedCols.delete(col);
                occupiedDiag1.delete(diag1Val);
                occupiedDiag2.delete(diag2Val);
            }
        }
    }

    backtrack(0);
    return results;
}

console.log("\nN-Queens Problem (N=4, Optimized Checks):");
const nQueensSolutionsOpt = solveNQueensOptimized(4);
nQueensSolutionsOpt.forEach(solution => {
    console.log("--- Solution ---");
    solution.forEach(rowStr => console.log(rowStr));
});
```

---

## 2. Permutation Problem

### a. Introduction

**Problem Statement:**
Given a collection of items (e.g., distinct numbers or characters), find all possible orderings (permutations) of these items.

**Why it's a good backtracking problem:**
*   **Choices:** At each step of building a permutation, we choose an available item to be the next element in the sequence.
*   **Constraints:** Each item from the input set must be used exactly once in a permutation.
*   **Incremental Build:** We build the permutation one element at a time.
*   **Pruning:** Not much pruning in the basic version other than not picking an already used element. The "bounding function" concept is more about optimizing by stopping early if a partial permutation can't lead to a desired *type* of solution (e.g., if permutations need to satisfy an additional property).

### b. Permutation Problem - Bounding Function

A **bounding function** in the context of backtracking (and more generally in branch and bound algorithms) is a criterion used to determine if a particular path in the state space tree is worth exploring further. If the bound indicates that the current partial solution cannot lead to a better or valid complete solution, then that path is pruned.

**For basic permutation generation (all permutations):**
The primary "bound" or constraint is simply whether an element has already been used in the current permutation. If we try to pick an already used element, that branch is invalid.

**When Bounding Functions are More Explicit for Permutations:**
Imagine if you were looking for permutations that satisfy an additional condition, for example:
*   "Find permutations of numbers {1, 2, 3, 4} where the sum of any two adjacent numbers is less than 6."
    *   **Bounding Function:** If you've built `[1, 4, ...]` and try to add `3`, the partial permutation becomes `[1, 4, 3]`. The bounding function would check `4+3 = 7`, which is not less than 6. So, this path `[1, 4, 3, ...]` is pruned. You wouldn't explore further by adding `2`.

**Algorithm (with `used` array as implicit bounding):**
(The `permutations` JavaScript code from the previous response demonstrates this. The `if (!used[i])` check acts as the primary bounding condition for generating unique element permutations.)

```javascript
// Re-iterating the core logic:
function generatePermutations(items) {
  results = []
  currentPermutation = []
  used = new Array(items.length).fill(false)

  function backtrack():
    if currentPermutation.length == items.length:
      results.push([...currentPermutation])
      return

    for i from 0 to items.length - 1:
      if not used[i]: // Bounding condition: item not used yet
        // Choose
        currentPermutation.push(items[i])
        used[i] = true

        // Explore
        backtrack()

        // Unchoose
        used[i] = false
        currentPermutation.pop()
  
  backtrack()
  return results
}
```
**Permutations (Handling Duplicates):** If `nums` can have duplicates, sort `nums` first. Then, in the loop, if `nums[i]` is the same as `nums[i-1]` AND `used[i-1]` is false (meaning `nums[i-1]` was part of a previous permutation path that has already been fully explored and backtracked from), skip `nums[i]` to avoid duplicate permutations.
```javascript
function permutationsWithDuplicates(nums) {
    const results = [];
    const currentPermutation = [];
    const used = new Array(nums.length).fill(false);
    nums.sort((a, b) => a - b); // Sort to handle duplicates

    function backtrack() {
        if (currentPermutation.length === nums.length) {
            results.push([...currentPermutation]);
            return;
        }

        for (let i = 0; i < nums.length; i++) {
            if (used[i]) continue;
            // Skip duplicate: if current num is same as previous,
            // and previous was not used (meaning it was considered and backtracked)
            // This ensures that for a set of identical numbers, we only pick them in one order.
            if (i > 0 && nums[i] === nums[i-1] && !used[i-1]) continue;

            currentPermutation.push(nums[i]);
            used[i] = true;
            backtrack();
            used[i] = false;
            currentPermutation.pop();
        }
    }
    backtrack();
    return results;
}
console.log("\nPermutations for [1, 1, 2] (with duplicates):");
console.log(permutationsWithDuplicates([1,1,2])); // Expected: [[1,1,2], [1,2,1], [2,1,1]]
```

If the problem has specific constraints on the permutation itself (e.g., sum of elements, lexicographical order within a range), the bounding function would be an explicit check *after* `// Choose` and *before* `// Explore` (or sometimes as part of `// Choose`). If the check fails, you'd `continue` the loop or `return` early from that path without recursing.

---

## 3. Rat in a Maze Problem

### a. Introduction

**Problem Statement:**
A maze is given as an `N x M` binary matrix where `1` represents a path and `0` represents a wall. A rat starts at a source cell (e.g., `(0,0)`) and must reach a destination cell (e.g., `(N-1, M-1)`). The rat can move in certain directions (e.g., down, right, or sometimes all 4 directions: up, down, left, right). Find if a path exists, or find one such path, or all such paths.

**Why it's a good backtracking problem:**
*   **Choices:** From the current cell, the rat can choose to move to any valid adjacent cell.
*   **Constraints:**
    *   Movement must be within the maze boundaries.
    *   Cannot move into a wall (`0`).
    *   Cannot visit the same cell twice in the current path (to avoid cycles).
*   **Incremental Build:** The path is built step by step.
*   **Pruning:** If a move leads to a wall, out of bounds, or an already visited cell, that path is abandoned.

### b. Algorithm

**Data Structures:**
*   `maze[N][M]`: The input grid.
*   `solution[N][M]`: A grid of the same dimensions, initialized to `0`s, to mark the path taken (if `1`, part of path).
*   `visited[N][M]`: (Often combined with `solution` or by temporarily modifying `maze`) To keep track of cells visited in the *current* path to avoid cycles.

**Helper Function: `solveMazeUtil(x, y, solution)`**

```
function solveRatInMaze(maze, N, M, destX, destY):
  solution = new Array(N).fill(0).map(() => new Array(M).fill(0))
  // visited array is implicitly handled by modifying maze or solution, or explicitly
  // For this description, let's assume we can modify `maze` to mark visited, or use `solution` grid.

  function isSafe(x, y):
    return (x >= 0 AND x < N AND y >= 0 AND y < M AND maze[x][y] == 1) 
           // AND solution[x][y] == 0 (if using solution grid to track current path visited)

  function backtrack(currX, currY):
    // 1. Base Case: Destination reached
    if currX == destX AND currY == destY AND maze[currX][currY] == 1:
      solution[currX][currY] = 1 // Mark destination as part of path
      return true // Path found

    // 2. Check if current cell is valid to move to
    if isSafe(currX, currY):
      // a. Choose: Mark current cell as part of the solution path
      solution[currX][currY] = 1
      // (If not allowed to revisit, mark as visited in current path, e.g., maze[currX][currY] = 2 or visited[currX][currY]=true)

      // b. Explore: Try moving in allowed directions (e.g., Down, Right)
      //    Order of exploration matters if you want a specific path (e.g., lexicographically first).
      
      // Move Down
      if backtrack(currX + 1, currY):
        return true
      
      // Move Right
      if backtrack(currX, currY + 1):
        return true

      // (If other directions like Up, Left are allowed, add them here)
      // if backtrack(currX - 1, currY): return true
      // if backtrack(currX, currY - 1): return true

      // c. Unchoose (Backtrack): If none of the moves from (currX, currY) lead to a solution,
      //    unmark this cell from the solution path and return false.
      solution[currX][currY] = 0
      // (Unmark visited: maze[currX][currY] = 1 or visited[currX][currY]=false)
      return false

    return false // Not a safe/valid cell to be in

  if backtrack(0, 0): // Start from source (0,0)
    return solution // Or true, and print solution
  else:
    return "No path exists" // Or false
```

**JavaScript Code Example (Path from (0,0) to (N-1,M-1), moves: Down, Right):**
```javascript
function ratInAMaze(maze) {
    const N = maze.length;
    if (N === 0) return [];
    const M = maze[0].length;
    if (M === 0) return [];

    const solution = Array(N).fill(null).map(() => Array(M).fill(0));
    const paths = []; // To store all found paths

    function isSafe(r, c) {
        return r >= 0 && r < N && c >= 0 && c < M && maze[r][c] === 1 && solution[r][c] === 0;
    }

    // dr, dc for directions (Down, Up, Right, Left) - adapt as needed
    // For just Down and Right:
    const dr = [1, 0]; // Down, Right
    const dc = [0, 1]; // Down, Right
    // const moveNames = ["D", "R"]; // If you need to print path directions

    function backtrack(r, c /*, currentPathString = "" */) {
        if (r === N - 1 && c === M - 1 && maze[r][c] === 1) { // Destination
            solution[r][c] = 1;
            // Store a copy of the solution board for this path
            const pathBoard = solution.map(row => [...row]);
            paths.push(pathBoard);
            // paths.push(currentPathString); // if storing directions
            solution[r][c] = 0; // Backtrack for other possible paths
            return; // Don't return true if finding all paths
        }

        if (isSafe(r,c)) {
            solution[r][c] = 1; // Choose

            // Explore (Down, Right)
            if (backtrack(r + 1, c /*, currentPathString + "D" */)) {
                // if only one path is needed, return true here
            }
            if (backtrack(r, c + 1 /*, currentPathString + "R" */)) {
                // if only one path is needed, return true here
            }
            // Add other directions if allowed:
            // if (backtrack(r - 1, c)) { return true; } // Up
            // if (backtrack(r, c - 1)) { return true; } // Left


            solution[r][c] = 0; // Unchoose (Backtrack)
            // return false; // If only one path needed and none found from here
        }
        // return false; // If only one path needed
    }
    
    // Start from (0,0) if it's a valid starting cell
    if (maze[0][0] === 1) {
        backtrack(0, 0);
    }

    if (paths.length > 0) {
        console.log(`Found ${paths.length} path(s).`);
        return paths; // Return all found paths (as solution boards)
    } else {
        console.log("No path exists.");
        return [];
    }
}

const maze1 = [
    [1, 0, 0, 0],
    [1, 1, 0, 1],
    [0, 1, 0, 0],
    [1, 1, 1, 1]
];
console.log("\nRat in a Maze:");
const mazePaths = ratInAMaze(maze1);
if (mazePaths.length > 0) {
    console.log("One possible path board:");
    mazePaths[0].forEach(row => console.log(row.join(" ")));
}
// Example Output:
// Found 2 path(s).
// One possible path board:
// 1 0 0 0
// 1 1 0 0
// 0 1 0 0
// 0 1 1 1
// (Another path would go down-down-right-right-right-up-right-down) - Oh, wait, my simplified example only does Down/Right.
// If allowed D, R, U, L, the paths would be different.
// With only D,R:
// Path 1: D D R R R -> (0,0)->(1,0)->(1,1)->(2,1)->(3,1)->(3,2)->(3,3) -> No, (2,1) leads to (3,1)
// Path 1: (0,0) -> (1,0) -> (1,1) -> (2,1) -> (3,1) -> (3,2) -> (3,3)
// 1 0 0 0
// 1 1 0 0
// 0 1 0 0
// 0 1 1 1
// Path 2: (0,0) -> (1,0) -> (3,0) (invalid, as (2,0) is a wall)
// Let's re-verify with maze1.
// (0,0) -> (1,0) -> (1,1) -> (2,1) -> (3,1) -> (3,2) -> (3,3) - This is one path.
// (0,0) -> (1,0) -> (1,1) -> (3,1) from (1,1) is not direct.
// (0,0) -> (1,0) -> (1,1)
// From (1,1), can go (2,1) or (1,2) (wall). So (2,1).
// From (2,1), can go (3,1) or (2,2) (wall). So (3,1).
// From (3,1), can go (3,2).
// From (3,2), can go (3,3). Solution.
// This is the only path if only D and R are allowed from (0,0).
// The `paths.push` inside `if (r === N - 1 && c === M - 1)` will get copies.
// The backtracking of `solution[r][c] = 0;` is important for finding *all* paths correctly.
// If my `isSafe` also checks `solution[r][c] === 0`, then it correctly prevents cycles *within the current path attempt*.

```
To find all paths, the `backtrack` function should not return `true` immediately upon finding a path. It should record the path and then continue searching for other paths by allowing the `for` loop (if using directional arrays) or subsequent `if` conditions (for fixed directions) to explore other options after backtracking. My `ratInAMaze` JS code above is structured to find all paths.

---

## 4. Sudoku Solver

### a. Introduction

**Problem Statement:**
Given a partially filled `9x9` Sudoku grid, the task is to fill the remaining empty cells (usually marked with `0` or `.`) suchthat the final grid satisfies Sudoku rules:
1.  Each row contains digits `1-9` exactly once.
2.  Each column contains digits `1-9` exactly once.
3.  Each of the nine `3x3` subgrids (boxes) contains digits `1-9` exactly once.

**Why it's a good backtracking problem:**
*   **Choices:** For each empty cell, we try placing digits `1` through `9`.
*   **Constraints:** The placed digit must not violate Sudoku rules (row, column, 3x3 box uniqueness).
*   **Incremental Build:** We fill empty cells one by one.
*   **Pruning:** If placing a digit `d` in cell `(r,c)` violates a rule, we don't proceed with that `d`; we try the next digit for `(r,c)`. If no digit `1-9` works for `(r,c)`, we backtrack.

### b. Algorithm

**Helper Function: `solveSudokuUtil(board)` or `solve(row, col)`**

```
function solveSudoku(board): // board is 9x9

  function findEmptyLocation(board):
    for r from 0 to 8:
      for c from 0 to 8:
        if board[r][c] == 0: // Assuming 0 represents an empty cell
          return [r, c]
    return null // No empty cells, puzzle solved or already full

  function isSafe(board, row, col, num):
    // Check row
    for c_check from 0 to 8:
      if board[row][c_check] == num:
        return false
    // Check column
    for r_check from 0 to 8:
      if board[r_check][col] == num:
        return false
    // Check 3x3 subgrid
    startRowBox = row - (row % 3)
    startColBox = col - (col % 3)
    for r_box from 0 to 2:
      for c_box from 0 to 2:
        if board[startRowBox + r_box][startColBox + c_box] == num:
          return false
    return true

  function solve():
    emptyLoc = findEmptyLocation(board)
    if emptyLoc == null:
      return true // Puzzle is solved

    row = emptyLoc[0]
    col = emptyLoc[1]

    for num from 1 to 9:
      if isSafe(board, row, col, num):
        // Choose: Place the number
        board[row][col] = num

        // Explore: Recurse
        if solve():
          return true // Solution found

        // Unchoose (Backtrack): If recursion didn't lead to solution, reset cell
        board[row][col] = 0 
    
    return false // No number 1-9 works for this cell, trigger backtracking

  if solve():
    return board // Return the solved board
  else:
    return "No solution exists"
```

**JavaScript Code:**
```javascript
function solveSudoku(board) {
    const N = 9;

    function findEmpty(board) {
        for (let r = 0; r < N; r++) {
            for (let c = 0; c < N; c++) {
                if (board[r][c] === 0) { // Using 0 for empty
                    return [r, c];
                }
            }
        }
        return null; // No empty cells
    }

    function isSafe(board, row, col, num) {
        // Check row
        for (let c = 0; c < N; c++) {
            if (board[row][c] === num) return false;
        }
        // Check col
        for (let r = 0; r < N; r++) {
            if (board[r][col] === num) return false;
        }
        // Check 3x3 box
        const boxStartRow = Math.floor(row / 3) * 3;
        const boxStartCol = Math.floor(col / 3) * 3;
        for (let r = 0; r < 3; r++) {
            for (let c = 0; c < 3; c++) {
                if (board[boxStartRow + r][boxStartCol + c] === num) return false;
            }
        }
        return true;
    }

    function solve() {
        const emptyCell = findEmpty(board);
        if (!emptyCell) {
            return true; // All cells filled, solution found
        }
        const [row, col] = emptyCell;

        for (let num = 1; num <= N; num++) {
            if (isSafe(board, row, col, num)) {
                // Choose
                board[row][col] = num;

                // Explore
                if (solve()) {
                    return true;
                }

                // Unchoose (Backtrack)
                board[row][col] = 0; 
            }
        }
        return false; // No number works for this cell, trigger backtrack
    }

    if (solve()) {
        return board;
    } else {
        return "No solution exists"; // Or return original board / null
    }
}

const sudokuBoard = [
    [5,3,0,0,7,0,0,0,0],
    [6,0,0,1,9,5,0,0,0],
    [0,9,8,0,0,0,0,6,0],
    [8,0,0,0,6,0,0,0,3],
    [4,0,0,8,0,3,0,0,1],
    [7,0,0,0,2,0,0,0,6],
    [0,6,0,0,0,0,2,8,0],
    [0,0,0,4,1,9,0,0,5],
    [0,0,0,0,8,0,0,7,9]
];
console.log("\nSudoku Solver:");
const solvedSudoku = solveSudoku(sudokuBoard.map(row => [...row])); // Pass a copy
if (typeof solvedSudoku === "string") {
    console.log(solvedSudoku);
} else {
    console.log("Solved Sudoku:");
    solvedSudoku.forEach(row => console.log(row.join(" ")));
}
```

### 2. Subsets (Power Set)

**Problem:** Given a set of distinct integers, `nums`, return all possible subsets (the power set).

**State:** The `currentSubset` being built and the `startIndex` in `nums` to consider for adding elements.
**Choices:** For each element `nums[i]` (starting from `startIndex`), either include it in the `currentSubset` or not. (The "not include" is implicitly handled by moving to the next element without adding the current one).

**JavaScript Code:**
```javascript
function subsets(nums) {
    const results = [];
    const currentSubset = [];

    function backtrack(startIndex) {
        // 1. Add the current subset to results (every path is a valid subset)
        results.push([...currentSubset]); 

        // 2. Iterate through remaining elements to make choices
        for (let i = startIndex; i < nums.length; i++) {
            // a. Choose: Add the element to the current subset
            currentSubset.push(nums[i]);

            // b. Explore: Recurse with the next starting index
            backtrack(i + 1); // i + 1 because elements are distinct and order in subset doesn't matter

            // c. Unchoose (Backtrack): Remove the element
            currentSubset.pop();
        }
    }

    backtrack(0);
    return results;
}

console.log("\nSubsets (Power Set) for [1, 2, 3]:");
const allSubsets = subsets([1, 2, 3]);
console.log(allSubsets);
// Expected: [[], [1], [1,2], [1,2,3], [1,3], [2], [2,3], [3]] (order may vary)
```

### 4. Combination Sum

**Problem:** Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sum to target. The same repeated number may be chosen from candidates an unlimited number of times.

**State:** `currentIndex` (to start picking from in `candidates`), `currentTarget`, `currentCombination`.
**Choices:** Either include `candidates[currentIndex]` (if `currentTarget >= candidates[currentIndex]`) and recurse with the *same* `currentIndex` (allowing reuse), or skip `candidates[currentIndex]` and move to `currentIndex + 1`.

**JavaScript Code:**
```javascript
function combinationSum(candidates, target) {
    const results = [];
    
    function backtrack(startIndex, currentSum, currentCombination) {
        // Base Case: Found a combination that sums to target
        if (currentSum === target) {
            results.push([...currentCombination]);
            return;
        }
        // Base Case: Current sum exceeds target, or no more candidates
        if (currentSum > target || startIndex === candidates.length) {
            return;
        }

        // Iterate through candidates starting from startIndex
        for (let i = startIndex; i < candidates.length; i++) {
            // Optimization: if candidate[i] itself is > remaining target, skip
            if (candidates[i] > target - currentSum) continue; // Or simply check currentSum + candidates[i] > target

            // Choose: Add candidates[i]
            currentCombination.push(candidates[i]);
            
            // Explore: Recurse. Stay at current index 'i' to allow reuse of candidates[i]
            backtrack(i, currentSum + candidates[i], currentCombination);

            // Unchoose (Backtrack): Remove candidates[i]
            currentCombination.pop();
        }
    }
    // Sorting candidates can help with pruning if elements are large, but not strictly necessary for correctness here.
    // For Combination Sum II (no reuse of same element index), you'd pass `i + 1`.
    backtrack(0, 0, []);
    return results;
}

console.log("\nCombination Sum for candidates=[2,3,6,7], target=7:");
const comboSumResults = combinationSum([2, 3, 6, 7], 7);
console.log(comboSumResults); // Expected: [[2,2,3], [7]]

// For Combination Sum II (each number can be used once)
// You'd pass `i + 1` in backtrack and handle duplicates similar to Permutations II.
```

### 5. Word Search (DFS on a Grid)

**Problem:** Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid. The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**State:** Current cell `(r, c)` on the board, `k` (index of the character in `word` we are trying to match).
**Choices:** Explore adjacent cells (up, down, left, right).
**Constraints:** Cell must be within bounds, not visited in current path, and `board[r][c]` must match `word[k]`.

**JavaScript Code:**
```javascript
function exist(board, word) {
    const rows = board.length;
    const cols = board[0].length;

    function backtrack(r, c, k) {
        // Base Case: Found the entire word
        if (k === word.length) {
            return true;
        }

        // Base Case: Out of bounds or character doesn't match
        if (r < 0 || r >= rows || c < 0 || c >= cols || board[r][c] !== word[k]) {
            return false;
        }

        // Mark current cell as visited (Choose)
        const temp = board[r][c];
        board[r][c] = '#'; // Or use a separate 'visited' 2D array

        // Explore neighbors
        const found = backtrack(r + 1, c, k + 1) ||
                      backtrack(r - 1, c, k + 1) ||
                      backtrack(r, c + 1, k + 1) ||
                      backtrack(r, c - 1, k + 1);

        // Unchoose (Backtrack)
        board[r][c] = temp;

        return found;
    }

    for (let r = 0; r < rows; r++) {
        for (let c = 0; c < cols; c++) {
            if (backtrack(r, c, 0)) {
                return true;
            }
        }
    }
    return false;
}

const board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]];
console.log("\nWord Search:");
console.log(`Does "ABCCED" exist? ${exist(board, "ABCCED")}`); // Expected: true
console.log(`Does "SEE" exist? ${exist(board, "SEE")}`);       // Expected: true
const board2 = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]; // Re-init board
console.log(`Does "ABCB" exist? ${exist(board2, "ABCB")}`);   // Expected: false (cannot reuse 'B')
```

---
