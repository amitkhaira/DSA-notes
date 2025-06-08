# Complete Stack Cheat Sheet (JavaScript)

## 1. Stack Fundamentals

### Definition & Characteristics
- **LIFO**: Last In, First Out principle
- **Linear Data Structure**: Elements stacked on top of each other
- **One End Access**: Insert and remove from top only
- **Main Operations**: Push (insert), Pop (remove)

### Stack Operations & Time Complexities
| Operation | Description | Time | Space |
|-----------|-------------|------|-------|
| push() | Add element to top | O(1) | O(1) |
| pop() | Remove element from top | O(1) | O(1) |
| peek/top() | Get top element | O(1) | O(1) |
| isEmpty() | Check if stack is empty | O(1) | O(1) |
| size() | Get number of elements | O(1) | O(1) |

## 2. Stack Implementations

### 2.1 Array-based Stack (Simple & Efficient)
```js
class Stack {
    constructor() {
        this.items = [];
    }
    
    push(element) {
        this.items.push(element);
    }
    
    pop() {
        if (this.isEmpty()) return null;
        return this.items.pop();
    }
    
    peek() {
        if (this.isEmpty()) return null;
        return this.items[this.items.length - 1];
    }
    
    isEmpty() {
        return this.items.length === 0;
    }
    
    size() {
        return this.items.length;
    }
    
    clear() {
        this.items = [];
    }
    
    toArray() {
        return [...this.items];
    }
    
    toString() {
        return this.items.toString();
    }
}
```

### 2.2 Object-based Stack
```js
class Stack {
    constructor() {
        this.items = {};
        this.count = 0;
    }
    
    push(element) {
        this.items[this.count] = element;
        this.count++;
    }
    
    pop() {
        if (this.isEmpty()) return null;
        this.count--;
        const result = this.items[this.count];
        delete this.items[this.count];
        return result;
    }
    
    peek() {
        if (this.isEmpty()) return null;
        return this.items[this.count - 1];
    }
    
    isEmpty() {
        return this.count === 0;
    }
    
    size() {
        return this.count;
    }
    
    clear() {
        this.items = {};
        this.count = 0;
    }
}
```

### 2.3 Linked List Stack
```js
class Node {
    constructor(data) {
        this.data = data;
        this.next = null;
    }
}

class LinkedStack {
    constructor() {
        this.top = null;
        this.length = 0;
    }
    
    push(element) {
        const node = new Node(element);
        node.next = this.top;
        this.top = node;
        this.length++;
    }
    
    pop() {
        if (this.isEmpty()) return null;
        const poppedNode = this.top;
        this.top = this.top.next;
        this.length--;
        return poppedNode.data;
    }
    
    peek() {
        if (this.isEmpty()) return null;
        return this.top.data;
    }
    
    isEmpty() {
        return this.top === null;
    }
    
    size() {
        return this.length;
    }
}
```

## 3. Common Stack Patterns & Problems

### 3.1 Balanced Parentheses
```js
function isBalanced(str) {
    const stack = new Stack();
    const pairs = { '(': ')', '[': ']', '{': '}' };
    
    for (let char of str) {
        if (char in pairs) {
            stack.push(char);
        } else if (Object.values(pairs).includes(char)) {
            if (stack.isEmpty() || pairs[stack.pop()] !== char) {
                return false;
            }
        }
    }
    
    return stack.isEmpty();
}
```

### 3.2 Evaluate Postfix Expression
```js
function evaluatePostfix(expression) {
    const stack = new Stack();
    const tokens = expression.split(' ');
    
    for (let token of tokens) {
        if (!isNaN(token)) {
            stack.push(Number(token));
        } else {
            const b = stack.pop();
            const a = stack.pop();
            
            switch (token) {
                case '+': stack.push(a + b); break;
                case '-': stack.push(a - b); break;
                case '*': stack.push(a * b); break;
                case '/': stack.push(Math.floor(a / b)); break;
            }
        }
    }
    
    return stack.pop();
}
```

### 3.3 Infix to Postfix Conversion
```js
function infixToPostfix(infix) {
    const stack = new Stack();
    const precedence = { '+': 1, '-': 1, '*': 2, '/': 2, '^': 3 };
    let result = '';
    
    for (let char of infix) {
        if (char.match(/[a-zA-Z0-9]/)) {
            result += char;
        } else if (char === '(') {
            stack.push(char);
        } else if (char === ')') {
            while (!stack.isEmpty() && stack.peek() !== '(') {
                result += stack.pop();
            }
            stack.pop(); // Remove '('
        } else if (char in precedence) {
            while (!stack.isEmpty() && 
                   stack.peek() !== '(' && 
                   precedence[stack.peek()] >= precedence[char]) {
                result += stack.pop();
            }
            stack.push(char);
        }
    }
    
    while (!stack.isEmpty()) {
        result += stack.pop();
    }
    
    return result;
}
```

### 3.4 Next Greater Element
```js
function nextGreaterElement(nums) {
    const stack = new Stack();
    const result = new Array(nums.length).fill(-1);
    
    for (let i = 0; i < nums.length; i++) {
        while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
            const index = stack.pop();
            result[index] = nums[i];
        }
        stack.push(i);
    }
    
    return result;
}
```

### 3.5 Largest Rectangle in Histogram
```js
function largestRectangleArea(heights) {
    const stack = new Stack();
    let maxArea = 0;
    
    for (let i = 0; i <= heights.length; i++) {
        const currentHeight = i === heights.length ? 0 : heights[i];
        
        while (!stack.isEmpty() && heights[stack.peek()] > currentHeight) {
            const height = heights[stack.pop()];
            const width = stack.isEmpty() ? i : i - stack.peek() - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        
        stack.push(i);
    }
    
    return maxArea;
}
```

### 3.6 Valid Parentheses (Multiple Types)
```js
function isValid(s) {
    const stack = new Stack();
    const map = { ')': '(', '}': '{', ']': '[' };
    
    for (let char of s) {
        if (char in map) {
            if (stack.isEmpty() || stack.pop() !== map[char]) {
                return false;
            }
        } else {
            stack.push(char);
        }
    }
    
    return stack.isEmpty();
}
```

### 3.7 Min Stack (Stack with Minimum)
```js
class MinStack {
    constructor() {
        this.stack = new Stack();
        this.minStack = new Stack();
    }
    
    push(val) {
        this.stack.push(val);
        
        if (this.minStack.isEmpty() || val <= this.minStack.peek()) {
            this.minStack.push(val);
        }
    }
    
    pop() {
        const popped = this.stack.pop();
        
        if (popped === this.minStack.peek()) {
            this.minStack.pop();
        }
        
        return popped;
    }
    
    top() {
        return this.stack.peek();
    }
    
    getMin() {
        return this.minStack.peek();
    }
}
```

### 3.8 Implement Queue using Stacks
```js
class QueueUsingStacks {
    constructor() {
        this.stack1 = new Stack(); // for enqueue
        this.stack2 = new Stack(); // for dequeue
    }
    
    enqueue(element) {
        this.stack1.push(element);
    }
    
    dequeue() {
        if (this.stack2.isEmpty()) {
            while (!this.stack1.isEmpty()) {
                this.stack2.push(this.stack1.pop());
            }
        }
        
        return this.stack2.pop();
    }
    
    front() {
        if (this.stack2.isEmpty()) {
            while (!this.stack1.isEmpty()) {
                this.stack2.push(this.stack1.pop());
            }
        }
        
        return this.stack2.peek();
    }
    
    isEmpty() {
        return this.stack1.isEmpty() && this.stack2.isEmpty();
    }
}
```

### 3.9 Stock Span Problem
```js
function calculateSpan(prices) {
    const stack = new Stack();
    const span = [];
    
    for (let i = 0; i < prices.length; i++) {
        while (!stack.isEmpty() && prices[stack.peek()] <= prices[i]) {
            stack.pop();
        }
        
        span[i] = stack.isEmpty() ? i + 1 : i - stack.peek();
        stack.push(i);
    }
    
    return span;
}
```

### 3.10 Reverse String using Stack
```js
function reverseString(str) {
    const stack = new Stack();
    
    // Push all characters
    for (let char of str) {
        stack.push(char);
    }
    
    // Pop all characters
    let reversed = '';
    while (!stack.isEmpty()) {
        reversed += stack.pop();
    }
    
    return reversed;
}
```

## 4. Advanced Stack Problems

### 4.1 Trapping Rain Water
```js
function trap(height) {
    const stack = new Stack();
    let water = 0;
    
    for (let i = 0; i < height.length; i++) {
        while (!stack.isEmpty() && height[i] > height[stack.peek()]) {
            const top = stack.pop();
            
            if (stack.isEmpty()) break;
            
            const distance = i - stack.peek() - 1;
            const boundedHeight = Math.min(height[i], height[stack.peek()]) - height[top];
            water += distance * boundedHeight;
        }
        
        stack.push(i);
    }
    
    return water;
}
```

### 4.2 Decode String
```js
function decodeString(s) {
    const numStack = new Stack();
    const strStack = new Stack();
    let currentStr = '';
    let currentNum = 0;
    
    for (let char of s) {
        if (char >= '0' && char <= '9') {
            currentNum = currentNum * 10 + Number(char);
        } else if (char === '[') {
            numStack.push(currentNum);
            strStack.push(currentStr);
            currentNum = 0;
            currentStr = '';
        } else if (char === ']') {
            const prevStr = strStack.pop();
            const num = numStack.pop();
            currentStr = prevStr + currentStr.repeat(num);
        } else {
            currentStr += char;
        }
    }
    
    return currentStr;
}
```

### 4.3 Remove K Digits
```js
function removeKdigits(num, k) {
    const stack = new Stack();
    
    for (let digit of num) {
        while (k > 0 && !stack.isEmpty() && stack.peek() > digit) {
            stack.pop();
            k--;
        }
        stack.push(digit);
    }
    
    // Remove remaining digits from end
    while (k > 0) {
        stack.pop();
        k--;
    }
    
    // Build result
    const result = stack.toArray().join('');
    return result === '' || result === '0'.repeat(result.length) ? '0' : result;
}
```

## 5. JavaScript Built-in Array as Stack

### Using Array Methods
```js
const stack = [];

// Push (add to top)
stack.push(element);

// Pop (remove from top)
const element = stack.pop();

// Peek (view top element)
const top = stack[stack.length - 1];

// Check if empty
const isEmpty = stack.length === 0;

// Size
const size = stack.length;

// Clear
stack.length = 0; // or stack.splice(0)
```

## 6. Stack Applications

### Real-world Applications
- **Function Call Stack**: Managing function calls and returns
- **Undo Operations**: Text editors, browsers (back button)
- **Expression Evaluation**: Calculators, compilers
- **Syntax Parsing**: Checking balanced parentheses, XML/HTML parsing
- **Backtracking**: Maze solving, N-Queens problem
- **Memory Management**: Stack memory allocation
- **Browser History**: Forward/backward navigation

### Algorithm Applications
- **Depth-First Search (DFS)**: Graph/tree traversal
- **Tower of Hanoi**: Classic recursive problem
- **Postfix/Prefix Evaluation**: Mathematical expressions
- **Recursion Simulation**: Converting recursive to iterative
- **Parentheses Matching**: Compiler design

## 7. Common Interview Questions

### Easy Level
1. **Valid Parentheses**
2. **Implement Stack using Arrays**
3. **Reverse String using Stack**
4. **Remove All Adjacent Duplicates**

### Medium Level
5. **Min Stack / Max Stack**
6. **Evaluate Postfix Expression**
7. **Next Greater Element**
8. **Daily Temperatures**
9. **Implement Queue using Stacks**
10. **Stock Span Problem**

### Hard Level
11. **Largest Rectangle in Histogram**
12. **Trapping Rain Water**
13. **Remove K Digits**
14. **Basic Calculator**
15. **Longest Valid Parentheses**

## 8. Stack vs Other Data Structures

| Feature | Stack | Queue | Array |
|---------|-------|-------|-------|
| Access Pattern | LIFO | FIFO | Random |
| Insert/Delete | Top only | Both ends | Any position |
| Use Case | Recursion, Undo | BFS, Scheduling | General purpose |
| Memory | Sequential | Sequential | Sequential |

## 9. Tips & Best Practices

### Performance Tips
- Use arrays for simple stack operations (push/pop are O(1))
- Consider linked list for memory-constrained environments
- Avoid frequent size checks in loops

### Error Handling
```js
class SafeStack extends Stack {
    pop() {
        if (this.isEmpty()) {
            throw new Error("Stack underflow: Cannot pop from empty stack");
        }
        return super.pop();
    }
    
    peek() {
        if (this.isEmpty()) {
            throw new Error("Stack is empty: No top element");
        }
        return super.peek();
    }
}
```

### Memory Management
```js
// Clear stack properly
stack.clear(); // Custom method

// Or manually
while (!stack.isEmpty()) {
    stack.pop();
}
```

## 10. Complexity Summary

### Time Complexities
| Operation | Array Stack | Linked Stack | Object Stack |
|-----------|-------------|--------------|--------------|
| Push | O(1) | O(1) | O(1) |
| Pop | O(1) | O(1) | O(1) |
| Peek | O(1) | O(1) | O(1) |
| Search | O(n) | O(n) | O(n) |

### Space Complexity
- **All implementations**: O(n) where n is the number of elements
- **Auxiliary space**: O(1) for all operations
