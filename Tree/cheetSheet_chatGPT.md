# Tree Data Structure - Coding Cheat Sheet

## Basic Tree Node Structure
```javascript
class TreeNode {
    constructor(val = 0, left = null, right = null) {
        this.val = val;
        this.left = left;
        this.right = right;
    }
}
```

## Tree Traversal Patterns

### 1. Depth-First Search (DFS)
- **Preorder**: Root → Left → Right
- **Inorder**: Left → Root → Right (for BST: gives sorted order)
- **Postorder**: Left → Right → Root

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

## Common Tree Algorithms

### Height/Depth of Tree
```javascript
function maxDepth(root) {
    if (!root) return 0;
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### Check if Tree is Balanced
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
    if (!root || root === p || root === q) {
        return root;
    }
    
    const left = lowestCommonAncestor(root.left, p, q);
    const right = lowestCommonAncestor(root.right, p, q);
    
    if (left && right) return root;
    return left || right;
}
```

### Path Sum Problems
```javascript
function hasPathSum(root, targetSum) {
    if (!root) return false;
    if (!root.left && !root.right) {
        return root.val === targetSum;
    }
    
    targetSum -= root.val;
    return hasPathSum(root.left, targetSum) || hasPathSum(root.right, targetSum);
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

## Binary Search Tree (BST) Specific

### Validate BST
```javascript
function isValidBST(root) {
    function validate(node, minVal, maxVal) {
        if (!node) return true;
        if (node.val <= minVal || node.val >= maxVal) {
            return false;
        }
        return validate(node.left, minVal, node.val) && 
               validate(node.right, node.val, maxVal);
    }
    
    return validate(root, -Infinity, Infinity);
}
```

### Search in BST
```javascript
function searchBST(root, val) {
    if (!root || root.val === val) {
        return root;
    }
    
    if (val < root.val) {
        return searchBST(root.left, val);
    }
    return searchBST(root.right, val);
}
```

### Insert into BST
```javascript
function insertIntoBST(root, val) {
    if (!root) {
        return new TreeNode(val);
    }
    
    if (val < root.val) {
        root.left = insertIntoBST(root.left, val);
    } else {
        root.right = insertIntoBST(root.right, val);
    }
    
    return root;
}
```

## Key Problem-Solving Patterns

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

## Time & Space Complexity
- **Traversal**: O(n) time, O(h) space (h = height)
- **Search in BST**: O(log n) average, O(n) worst case
- **Height**: O(n) time, O(h) space
- **Level Order**: O(n) time, O(w) space (w = max width)

## Quick Tips
- Always check for `null/None` nodes first
- For BST problems, leverage the sorted property
- Use helper functions with additional parameters for complex logic
- Consider both recursive and iterative approaches
- For path problems, remember to backtrack when needed
- BFS for level-related problems, DFS for path-related problems
