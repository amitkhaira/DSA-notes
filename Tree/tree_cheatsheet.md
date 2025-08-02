# 🌲 Tree Data Structure - Interview Cheat Sheet

## 📌 Basic Tree Node Structure
```javascript
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## 📌 Tree Traversals Reference

| Traversal Type | Order | Use Cases |
|---|---|---|
| **Inorder** | Left → Root → Right | BST: gives sorted values |
| **Preorder** | Root → Left → Right | Copying tree, Prefix expression |
| **Postorder** | Left → Right → Root | Deleting tree, Postfix expression |
| **Level Order** | BFS (Queue) | Shortest path, level-based logic |

---

## 🟢 Easy Level Questions

### 1. Maximum Depth of Binary Tree ⭐⭐⭐⭐⭐
**Problem**: Find the maximum depth of a binary tree
```javascript
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### 2. Invert Binary Tree ⭐⭐⭐⭐⭐
**Problem**: Invert a binary tree (mirror image)
```javascript
function invertTree(root) {
    if (!root) return null;
    [root.left, root.right] = [invertTree(root.right), invertTree(root.left)];
    return root;
}
```

### 3. Same Tree ⭐⭐⭐⭐
**Problem**: Check if two binary trees are identical
```javascript
function isSameTree(p, q) {
    if (!p && !q) return true;
    if (!p || !q) return false;
    return p.val === q.val && 
           isSameTree(p.left, q.left) && 
           isSameTree(p.right, q.right);
}
```

### 4. Symmetric Tree ⭐⭐⭐⭐
**Problem**: Check if a binary tree is symmetric around its center
```javascript
function isSymmetric(root) {
    function isMirror(left, right) {
        if (!left && !right) return true;
        if (!left || !right) return false;
        return left.val === right.val && 
               isMirror(left.left, right.right) && 
               isMirror(left.right, right.left);
    }
    
    return !root || isMirror(root.left, root.right);
}
```

### 5. Binary Tree Inorder Traversal ⭐⭐⭐⭐
**Problem**: Return inorder traversal of binary tree
```javascript
// Recursive
function inorderTraversal(root) {
    if (!root) return [];
    return [...inorderTraversal(root.left), root.val, ...inorderTraversal(root.right)];
}

// Iterative
function inorderIterative(root) {
    const stack = [], result = [];
    let curr = root;
    
    while (curr || stack.length) {
        while (curr) {
            stack.push(curr);
            curr = curr.left;
        }
        curr = stack.pop();
        result.push(curr.val);
        curr = curr.right;
    }
    return result;
}
```

### 6. Binary Tree Preorder Traversal ⭐⭐⭐
**Problem**: Return preorder traversal of binary tree
```javascript
// Recursive
function preorder(root) {
    if (!root) return [];
    return [root.val, ...preorder(root.left), ...preorder(root.right)];
}

// Iterative
function preorderTraversal(root) {
    if (!root) return [];
    const stack = [root], result = [];
    
    while (stack.length) {
        const node = stack.pop();
        result.push(node.val);
        if (node.right) stack.push(node.right);
        if (node.left) stack.push(node.left);
    }
    return result;
}
```

### 7. Binary Tree Postorder Traversal ⭐⭐⭐
**Problem**: Return postorder traversal of binary tree
```javascript
function postorderTraversal(root) {
    if (!root) return [];
    return [...postorderTraversal(root.left), ...postorderTraversal(root.right), root.val];
}
```

### 8. Search in Binary Search Tree ⭐⭐⭐
**Problem**: Find a node in BST
```javascript
function searchBST(root, val) {
    if (!root || root.val === val) return root;
    if (val < root.val) return searchBST(root.left, val);
    return searchBST(root.right, val);
}
```

### 9. Minimum Depth of Binary Tree ⭐⭐⭐
**Problem**: Find minimum depth to a leaf node
```javascript
function minDepth(root) {
    if (!root) return 0;
    if (!root.left) return 1 + minDepth(root.right);
    if (!root.right) return 1 + minDepth(root.left);
    return 1 + Math.min(minDepth(root.left), minDepth(root.right));
}
```

### 10. Path Sum ⭐⭐⭐
**Problem**: Check if there's a root-to-leaf path with given sum
```javascript
function hasPathSum(root, targetSum) {
    if (!root) return false;
    if (!root.left && !root.right) return root.val === targetSum;
    return hasPathSum(root.left, targetSum - root.val) ||
           hasPathSum(root.right, targetSum - root.val);
}
```

---

## 🟡 Medium Level Questions

### 1. Validate Binary Search Tree ⭐⭐⭐⭐⭐
**Problem**: Check if a binary tree is a valid BST
```javascript
function isValidBST(root) {
    function validate(node, min, max) {
        if (!node) return true;
        if (node.val <= min || node.val >= max) return false;
        return validate(node.left, min, node.val) && 
               validate(node.right, node.val, max);
    }
    return validate(root, -Infinity, Infinity);
}
```

### 2. Lowest Common Ancestor ⭐⭐⭐⭐⭐
**Problem**: Find LCA of two nodes in binary tree
```javascript
function lowestCommonAncestor(root, p, q) {
    if (!root || root === p || root === q) return root;
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    return left && right ? root : left || right;
}
```

### 3. Binary Tree Level Order Traversal ⭐⭐⭐⭐⭐
**Problem**: Return level order traversal as 2D array
```javascript
function levelOrder(root) {
    if (!root) return [];
    const result = [];
    const queue = [root];
    
    while (queue.length) {
        const size = queue.length;
        const level = [];
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            level.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        result.push(level);
    }
    return result;
}
```

### 4. Binary Tree Right Side View ⭐⭐⭐⭐
**Problem**: Return values of nodes you can see from right side
```javascript
function rightSideView(root) {
    if (!root) return [];
    const result = [];
    const queue = [root];
    
    while (queue.length) {
        const size = queue.length;
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            if (i === size - 1) result.push(node.val);
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
    }
    return result;
}
```

### 5. Diameter of Binary Tree ⭐⭐⭐⭐
**Problem**: Find longest path between any two nodes
```javascript
function diameterOfBinaryTree(root) {
    let maxDiameter = 0;
    
    function depth(node) {
        if (!node) return 0;
        const left = depth(node.left);
        const right = depth(node.right);
        maxDiameter = Math.max(maxDiameter, left + right);
        return 1 + Math.max(left, right);
    }
    
    depth(root);
    return maxDiameter;
}
```

### 6. Balanced Binary Tree ⭐⭐⭐⭐
**Problem**: Check if tree is height-balanced
```javascript
function isBalanced(root) {
    function height(node) {
        if (!node) return 0;
        const left = height(node.left);
        const right = height(node.right);
        if (left === -1 || right === -1 || Math.abs(left - right) > 1) {
            return -1;
        }
        return 1 + Math.max(left, right);
    }
    
    return height(root) !== -1;
}
```

### 7. Path Sum II ⭐⭐⭐⭐
**Problem**: Find all root-to-leaf paths with given sum
```javascript
function pathSum(root, targetSum) {
    const result = [];
    
    function dfs(node, sum, path) {
        if (!node) return;
        path.push(node.val);
        if (!node.left && !node.right && sum === node.val) {
            result.push([...path]);
        } else {
            dfs(node.left, sum - node.val, path);
            dfs(node.right, sum - node.val, path);
        }
        path.pop();
    }
    
    dfs(root, targetSum, []);
    return result;
}
```

### 8. Subtree of Another Tree ⭐⭐⭐⭐
**Problem**: Check if one tree is subtree of another
```javascript
function isSubtree(root, subRoot) {
    if (!root) return false;
    if (isSameTree(root, subRoot)) return true;
    return isSubtree(root.left, subRoot) || isSubtree(root.right, subRoot);
}
```

### 9. Construct Binary Tree from Preorder and Inorder ⭐⭐⭐⭐
**Problem**: Build tree from preorder and inorder traversals
```javascript
function buildTree(preorder, inorder) {
    if (!preorder.length || !inorder.length) return null;
    
    const root = new TreeNode(preorder[0]);
    const rootIndex = inorder.indexOf(preorder[0]);
    
    root.left = buildTree(preorder.slice(1, rootIndex + 1), inorder.slice(0, rootIndex));
    root.right = buildTree(preorder.slice(rootIndex + 1), inorder.slice(rootIndex + 1));
    
    return root;
}
```

### 10. Binary Tree Zigzag Level Order Traversal ⭐⭐⭐⭐
**Problem**: Level order traversal in zigzag pattern
```javascript
function zigzagLevelOrder(root) {
    if (!root) return [];
    const result = [];
    const queue = [root];
    let leftToRight = true;
    
    while (queue.length) {
        const size = queue.length;
        const level = [];
        for (let i = 0; i < size; i++) {
            const node = queue.shift();
            if (leftToRight) {
                level.push(node.val);
            } else {
                level.unshift(node.val);
            }
            if (node.left) queue.push(node.left);
            if (node.right) queue.push(node.right);
        }
        result.push(level);
        leftToRight = !leftToRight;
    }
    return result;
}
```

### 11. Flatten Binary Tree to Linked List ⭐⭐⭐
**Problem**: Flatten tree to linked list in preorder
```javascript
function flatten(root) {
    if (!root) return;
    
    flatten(root.left);
    flatten(root.right);
    
    const rightSubtree = root.right;
    root.right = root.left;
    root.left = null;
    
    let current = root;
    while (current.right) {
        current = current.right;
    }
    current.right = rightSubtree;
}
```

### 12. Kth Smallest Element in BST ⭐⭐⭐⭐
**Problem**: Find kth smallest element in BST
```javascript
function kthSmallest(root, k) {
    const stack = [];
    let current = root;
    
    while (current || stack.length) {
        while (current) {
            stack.push(current);
            current = current.left;
        }
        current = stack.pop();
        k--;
        if (k === 0) return current.val;
        current = current.right;
    }
}
```

---

## 🔴 Hard Level Questions

### 1. Serialize and Deserialize Binary Tree ⭐⭐⭐⭐⭐
**Problem**: Serialize tree to string and deserialize back
```javascript
function serialize(root) {
    if (!root) return "null,";
    return root.val + "," + serialize(root.left) + serialize(root.right);
}

function deserialize(data) {
    const values = data.split(',');
    let index = 0;
    
    function buildTree() {
        if (values[index] === "null") {
            index++;
            return null;
        }
        const node = new TreeNode(parseInt(values[index++]));
        node.left = buildTree();
        node.right = buildTree();
        return node;
    }
    
    return buildTree();
}
```

### 2. Binary Tree Maximum Path Sum ⭐⭐⭐⭐⭐
**Problem**: Find maximum sum path between any two nodes
```javascript
function maxPathSum(root) {
    let maxSum = -Infinity;
    
    function maxGain(node) {
        if (!node) return 0;
        
        const leftGain = Math.max(maxGain(node.left), 0);
        const rightGain = Math.max(maxGain(node.right), 0);
        
        const priceNewPath = node.val + leftGain + rightGain;
        maxSum = Math.max(maxSum, priceNewPath);
        
        return node.val + Math.max(leftGain, rightGain);
    }
    
    maxGain(root);
    return maxSum;
}
```

### 3. Recover Binary Search Tree ⭐⭐⭐⭐
**Problem**: Fix BST where exactly two nodes are swapped
```javascript
function recoverTree(root) {
    let first = null, second = null, prev = null;
    
    function inorder(node) {
        if (!node) return;
        
        inorder(node.left);
        
        if (prev && prev.val > node.val) {
            if (!first) first = prev;
            second = node;
        }
        prev = node;
        
        inorder(node.right);
    }
    
    inorder(root);
    [first.val, second.val] = [second.val, first.val];
}
```

### 4. Binary Tree Postorder Traversal (Iterative) ⭐⭐⭐⭐
**Problem**: Postorder traversal using iteration
```javascript
function postorderTraversal(root) {
    if (!root) return [];
    const stack = [root], result = [];
    let lastVisited = null;
    
    while (stack.length) {
        const node = stack[stack.length - 1];
        if ((!node.left && !node.right) || 
            (lastVisited && (lastVisited === node.left || lastVisited === node.right))) {
            result.push(node.val);
            lastVisited = stack.pop();
        } else {
            if (node.right) stack.push(node.right);
            if (node.left) stack.push(node.left);
        }
    }
    return result;
}
```

### 5. Binary Tree Cameras ⭐⭐⭐⭐
**Problem**: Minimum cameras needed to monitor all nodes
```javascript
function minCameraCover(root) {
    let cameras = 0;
    
    function dfs(node) {
        if (!node) return 2; // covered
        
        const left = dfs(node.left);
        const right = dfs(node.right);
        
        if (left === 0 || right === 0) {
            cameras++;
            return 1; // has camera
        }
        
        return left === 1 || right === 1 ? 2 : 0; // covered or not monitored
    }
    
    return dfs(root) === 0 ? cameras + 1 : cameras;
}
```

### 6. Vertical Order Traversal ⭐⭐⭐⭐
**Problem**: Return vertical order traversal of binary tree
```javascript
function verticalTraversal(root) {
    if (!root) return [];
    
    const columnTable = new Map();
    const queue = [[root, 0, 0]]; // [node, row, col]
    
    while (queue.length) {
        const [node, row, col] = queue.shift();
        
        if (!columnTable.has(col)) {
            columnTable.set(col, []);
        }
        columnTable.get(col).push([row, node.val]);
        
        if (node.left) queue.push([node.left, row + 1, col - 1]);
        if (node.right) queue.push([node.right, row + 1, col + 1]);
    }
    
    const result = [];
    const sortedColumns = Array.from(columnTable.keys()).sort((a, b) => a - b);
    
    for (const col of sortedColumns) {
        const column = columnTable.get(col);
        column.sort((a, b) => a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]);
        result.push(column.map(item => item[1]));
    }
    
    return result;
}
```

### 7. House Robber III ⭐⭐⭐⭐
**Problem**: Maximum money robbed without robbing parent-child nodes
```javascript
function rob(root) {
    function robHelper(node) {
        if (!node) return [0, 0]; // [rob, notRob]
        
        const left = robHelper(node.left);
        const right = robHelper(node.right);
        
        const rob = node.val + left[1] + right[1];
        const notRob = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        
        return [rob, notRob];
    }
    
    const result = robHelper(root);
    return Math.max(result[0], result[1]);
}
```

### 8. Count Complete Tree Nodes ⭐⭐⭐
**Problem**: Count nodes in complete binary tree efficiently
```javascript
function countNodes(root) {
    if (!root) return 0;
    
    let leftHeight = 0, rightHeight = 0;
    let left = root, right = root;
    
    while (left) {
        leftHeight++;
        left = left.left;
    }
    
    while (right) {
        rightHeight++;
        right = right.right;
    }
    
    if (leftHeight === rightHeight) {
        return Math.pow(2, leftHeight) - 1;
    }
    
    return 1 + countNodes(root.left) + countNodes(root.right);
}
```

---

## 🔍 Binary Search Tree (BST) Specific Problems

### Insert into BST ⭐⭐⭐
```javascript
function insertIntoBST(root, val) {
    if (!root) return new TreeNode(val);
    if (val < root.val) {
        root.left = insertIntoBST(root.left, val);
    } else {
        root.right = insertIntoBST(root.right, val);
    }
    return root;
}
```

### Delete Node in BST ⭐⭐⭐⭐
```javascript
function deleteNode(root, key) {
    if (!root) return null;
    
    if (key < root.val) {
        root.left = deleteNode(root.left, key);
    } else if (key > root.val) {
        root.right = deleteNode(root.right, key);
    } else {
        if (!root.left) return root.right;
        if (!root.right) return root.left;
        
        let successor = root.right;
        while (successor.left) successor = successor.left;
        root.val = successor.val;
        root.right = deleteNode(root.right, successor.val);
    }
    return root;
}
```

---

## 📊 Key Problem-Solving Patterns

### 1. **Top-Down Approach (Preorder-like)**
- Pass information from parent to children
- Good for: path sum, max depth with constraints

### 2. **Bottom-Up Approach (Postorder-like)**
- Collect information from children first
- Good for: height, diameter, balanced check

### 3. **Level-by-Level Processing**
- Use BFS with queue
- Good for: level order traversal, zigzag traversal

### 4. **Two-Pointer Technique**
- For problems involving two nodes
- Good for: LCA, distance between nodes

## ⏱️ Time & Space Complexity Reference
- **Traversal**: O(n) time, O(h) space (h = height)
- **Search in BST**: O(log n) average, O(n) worst case
- **Height**: O(n) time, O(h) space
- **Level Order**: O(n) time, O(w) space (w = max width)

## 🎯 Interview Tips
- **Always check for null nodes first**
- **Use recursion for most tree problems** (unless space optimization needed)
- **Queue for BFS, Stack for DFS**
- **For BSTs, leverage their sorted property**
- **Practice both recursive and iterative solutions**
- **Remember to handle edge cases** (empty tree, single node, etc.)
- **For path problems, consider backtracking**
- **BFS for level problems, DFS for path problems**
