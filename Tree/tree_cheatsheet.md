# 🌲 Tree Data Structure - Coding Cheat Sheet

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

## 📌 Tree Traversals

| Traversal Type | Order | Use Cases |
|---|---|---|
| **Inorder** | Left → Root → Right | BST: gives sorted values |
| **Preorder** | Root → Left → Right | Copying tree, Prefix expression |
| **Postorder** | Left → Right → Root | Deleting tree, Postfix expression |
| **Level Order** | BFS (Queue) | Shortest path, level-based logic |

### 1. Depth-First Search (DFS) - Recursive
```javascript
function preorder(root) {
    if (!root) return [];
    return [root.val, ...preorder(root.left), ...preorder(root.right)];
}

function inorder(root) {
    if (!root) return [];
    return [...inorder(root.left), root.val, ...inorder(root.right)];
}

function postorder(root) {
    if (!root) return [];
    return [...postorder(root.left), ...postorder(root.right), root.val];
}
```

### 2. Breadth-First Search (BFS) - Level Order
```javascript
function levelOrder(root) {
    if (!root) return [];
    const queue = [root];
    const result = [];
    
    while (queue.length > 0) {
        const node = queue.shift();
        result.push(node.val);
        if (node.left) queue.push(node.left);
        if (node.right) queue.push(node.right);
    }
    
    return result;
}
```

## 🔄 Iterative Traversal Templates

### 🟡 Inorder (Iterative)
```javascript
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

### 🔵 Preorder (Iterative)
```javascript
function preorderIterative(root) {
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

## Common Problems & Patterns

### Check if Valid BST
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

### Invert Binary Tree (Mirror)
```javascript
function invertTree(root) {
    if (!root) return null;
    [root.left, root.right] = [invertTree(root.right), invertTree(root.left)];
    return root;
}
```

### Max Depth / Height
```javascript
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### Min Depth
```javascript
function minDepth(root) {
    if (!root) return 0;
    if (!root.left) return 1 + minDepth(root.right);
    if (!root.right) return 1 + minDepth(root.left);
    return 1 + Math.min(minDepth(root.left), minDepth(root.right));
}
```

### ⚖️ Check if Tree is Balanced
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

### Lowest Common Ancestor (LCA)
```javascript
function lowestCommonAncestor(root, p, q) {
    if (!root || root === p || root === q) return root;
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    return left && right ? root : left || right;
}
```

### Path Sum Problems
```javascript
function hasPathSum(root, targetSum) {
    if (!root) return false;
    if (!root.left && !root.right) return root.val === targetSum;
    return hasPathSum(root.left, targetSum - root.val) ||
           hasPathSum(root.right, targetSum - root.val);
}
```

### Diameter of Tree
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

## 🔍 Binary Search Tree (BST) Specific

### Search in BST
```javascript
function searchBST(root, val) {
    if (!root || root.val === val) return root;
    if (val < root.val) return searchBST(root.left, val);
    return searchBST(root.right, val);
}
```

### Insert into BST
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

### Delete from BST
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
        
        // Find inorder successor
        let successor = root.right;
        while (successor.left) successor = successor.left;
        root.val = successor.val;
        root.right = deleteNode(root.right, successor.val);
    }
    return root;
}
```

## Serialize/Deserialize Tree
```javascript
// Serialize
function serialize(root) {
    if (!root) return "null,";
    return root.val + "," + serialize(root.left) + serialize(root.right);
}

// Deserialize
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

## 📊 Key Problem-Solving Patterns

### 1. Top-Down Approach (Preorder-like)
- Pass information from parent to children
- Good for: path sum, max depth with constraints

### 2. Bottom-Up Approach (Postorder-like)
- Collect information from children first
- Good for: height, diameter, balanced check

### 3. Level-by-Level Processing
- Use BFS with queue
- Good for: level order traversal, zigzag traversal

### 4. Two-Pointer Technique
- For problems involving two nodes
- Good for: LCA, distance between nodes

## ⏱️ Time & Space Complexity
- **Traversal**: O(n) time, O(h) space (h = height)
- **Search in BST**: O(log n) average, O(n) worst case
- **Height**: O(n) time, O(h) space
- **Level Order**: O(n) time, O(w) space (w = max width)

## 🛠️ General Tips
- Always check for `null/None` nodes first
- **Use recursion** for most tree problems (unless space optimization is needed)
- **Queue** for level order (BFS)
- **Stack** for iterative DFS (inorder, preorder, postorder)
- For BSTs, leverage their **sorted nature**
- Use **global variables** cautiously in recursion
- For BST problems, leverage the sorted property
- Use helper functions with additional parameters for complex logic
- Consider both recursive and iterative approaches
- For path problems, remember to backtrack when needed
- BFS for level-related problems, DFS for path-related problems
