# 🔥 Complete Stack Cheat Sheet - Interview Edition

## 1. Stack Fundamentals ⭐⭐⭐⭐⭐

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

## 2. Stack Implementations ⭐⭐⭐⭐

### Array-based Stack (Most Common)
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
}
```

---

# 🟢 EASY LEVEL PROBLEMS ⭐⭐⭐

## Easy Level Questions

### 1. Valid Parentheses ⭐⭐⭐⭐⭐
**Problem**: Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
**Companies**: Google, Microsoft, Amazon, Facebook, Apple
```js
function isValid(s) {
    const stack = [];
    const map = { ')': '(', '}': '{', ']': '[' };
    
    for (let char of s) {
        if (char in map) {
            if (stack.length === 0 || stack.pop() !== map[char]) {
                return false;
            }
        } else {
            stack.push(char);
        }
    }
    
    return stack.length === 0;
}

// Test cases
console.log(isValid("()")); // true
console.log(isValid("()[]{}")); // true
console.log(isValid("(]")); // false
```

### 2. Implement Stack using Arrays ⭐⭐⭐⭐
**Problem**: Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
**Companies**: Amazon, Microsoft, Google
```js
class MyStack {
    constructor() {
        this.data = [];
    }
    
    push(x) {
        this.data.push(x);
    }
    
    pop() {
        if (this.empty()) return -1;
        return this.data.pop();
    }
    
    top() {
        if (this.empty()) return -1;
        return this.data[this.data.length - 1];
    }
    
    empty() {
        return this.data.length === 0;
    }
}
```

### 3. Remove All Adjacent Duplicates ⭐⭐⭐⭐
**Problem**: Remove all adjacent duplicate letters from a string.
**Companies**: Amazon, Google, Facebook
```js
function removeDuplicates(s) {
    const stack = [];
    
    for (let char of s) {
        if (stack.length > 0 && stack[stack.length - 1] === char) {
            stack.pop();
        } else {
            stack.push(char);
        }
    }
    
    return stack.join('');
}

// Test cases
console.log(removeDuplicates("abbaca")); // "ca"
console.log(removeDuplicates("azxxzy")); // "ay"
```

### 4. Baseball Game ⭐⭐⭐
**Problem**: Calculate the sum of scores after applying operations represented by strings.
**Companies**: Amazon, Google
```js
function calPoints(ops) {
    const stack = [];
    
    for (let op of ops) {
        if (op === 'C') {
            stack.pop();
        } else if (op === 'D') {
            stack.push(stack[stack.length - 1] * 2);
        } else if (op === '+') {
            const last = stack[stack.length - 1];
            const secondLast = stack[stack.length - 2];
            stack.push(last + secondLast);
        } else {
            stack.push(parseInt(op));
        }
    }
    
    return stack.reduce((sum, score) => sum + score, 0);
}
```

### 5. Reverse String using Stack ⭐⭐⭐
**Problem**: Reverse a string using stack data structure.
**Companies**: Microsoft, Amazon
```js
function reverseString(str) {
    const stack = [];
    
    // Push all characters
    for (let char of str) {
        stack.push(char);
    }
    
    // Pop all characters
    let reversed = '';
    while (stack.length > 0) {
        reversed += stack.pop();
    }
    
    return reversed;
}

// Test cases
console.log(reverseString("hello")); // "olleh"
console.log(reverseString("world")); // "dlrow"
```

---

# 🟡 MEDIUM LEVEL PROBLEMS ⭐⭐⭐⭐

## Medium Level Questions

### 1. Min Stack ⭐⭐⭐⭐⭐
**Problem**: Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
**Companies**: Amazon, Microsoft, Google, Facebook, Apple
```js
class MinStack {
    constructor() {
        this.stack = [];
        this.minStack = [];
    }
    
    push(val) {
        this.stack.push(val);
        
        if (this.minStack.length === 0 || val <= this.minStack[this.minStack.length - 1]) {
            this.minStack.push(val);
        }
    }
    
    pop() {
        const popped = this.stack.pop();
        
        if (popped === this.minStack[this.minStack.length - 1]) {
            this.minStack.pop();
        }
        
        return popped;
    }
    
    top() {
        return this.stack[this.stack.length - 1];
    }
    
    getMin() {
        return this.minStack[this.minStack.length - 1];
    }
}
```

### 2. Evaluate Postfix Expression ⭐⭐⭐⭐
**Problem**: Evaluate the value of an arithmetic expression in Reverse Polish Notation.
**Companies**: Google, Microsoft, Amazon
```js
function evalRPN(tokens) {
    const stack = [];
    
    for (let token of tokens) {
        if (['+', '-', '*', '/'].includes(token)) {
            const b = stack.pop();
            const a = stack.pop();
            
            switch (token) {
                case '+': stack.push(a + b); break;
                case '-': stack.push(a - b); break;
                case '*': stack.push(a * b); break;
                case '/': stack.push(Math.trunc(a / b)); break;
            }
        } else {
            stack.push(parseInt(token));
        }
    }
    
    return stack[0];
}

// Test cases
console.log(evalRPN(["2","1","+","3","*"])); // 9
console.log(evalRPN(["4","13","5","/","+"])); // 6
```

### 3. Next Greater Element I ⭐⭐⭐⭐
**Problem**: Find the next greater element for each element in nums1 from nums2.
**Companies**: Amazon, Google, Microsoft
```js
function nextGreaterElement(nums1, nums2) {
    const stack = [];
    const map = {};
    
    // Find next greater elements for nums2
    for (let i = 0; i < nums2.length; i++) {
        while (stack.length > 0 && nums2[stack[stack.length - 1]] < nums2[i]) {
            const index = stack.pop();
            map[nums2[index]] = nums2[i];
        }
        stack.push(i);
    }
    
    // Build result for nums1
    return nums1.map(num => map[num] || -1);
}

// Test cases
console.log(nextGreaterElement([4,1,2], [1,3,4,2])); // [-1,3,-1]
console.log(nextGreaterElement([2,4], [1,2,3,4])); // [3,-1]
```

### 4. Daily Temperatures ⭐⭐⭐⭐⭐
**Problem**: Given daily temperatures, return array showing how many days you have to wait until a warmer temperature.
**Companies**: Google, Facebook, Amazon, Microsoft
```js
function dailyTemperatures(temperatures) {
    const stack = [];
    const result = new Array(temperatures.length).fill(0);
    
    for (let i = 0; i < temperatures.length; i++) {
        while (stack.length > 0 && temperatures[stack[stack.length - 1]] < temperatures[i]) {
            const index = stack.pop();
            result[index] = i - index;
        }
        stack.push(i);
    }
    
    return result;
}

// Test cases
console.log(dailyTemperatures([73,74,75,71,69,72,76,73])); // [1,1,4,2,1,1,0,0]
```

### 5. Implement Queue using Stacks ⭐⭐⭐⭐
**Problem**: Implement a first in first out (FIFO) queue using only two stacks.
**Companies**: Amazon, Microsoft, Google, Apple
```js
class MyQueue {
    constructor() {
        this.stack1 = []; // for enqueue
        this.stack2 = []; // for dequeue
    }
    
    push(x) {
        this.stack1.push(x);
    }
    
    pop() {
        this.peek();
        return this.stack2.pop();
    }
    
    peek() {
        if (this.stack2.length === 0) {
            while (this.stack1.length > 0) {
                this.stack2.push(this.stack1.pop());
            }
        }
        return this.stack2[this.stack2.length - 1];
    }
    
    empty() {
        return this.stack1.length === 0 && this.stack2.length === 0;
    }
}
```

### 6. Stock Span Problem ⭐⭐⭐⭐
**Problem**: Calculate the span of stock prices for each day (consecutive days with price <= current day).
**Companies**: Amazon, Microsoft, Google
```js
class StockSpanner {
    constructor() {
        this.stack = []; // [price, span]
    }
    
    next(price) {
        let span = 1;
        
        while (this.stack.length > 0 && this.stack[this.stack.length - 1][0] <= price) {
            span += this.stack.pop()[1];
        }
        
        this.stack.push([price, span]);
        return span;
    }
}

// Test cases
const stockSpanner = new StockSpanner();
console.log(stockSpanner.next(100)); // 1
console.log(stockSpanner.next(80));  // 1
console.log(stockSpanner.next(60));  // 1
console.log(stockSpanner.next(70));  // 2
console.log(stockSpanner.next(60));  // 1
console.log(stockSpanner.next(75));  // 4
console.log(stockSpanner.next(85));  // 6
```

### 7. Decode String ⭐⭐⭐⭐
**Problem**: Decode a string where numbers indicate how many times to repeat the enclosed string.
**Companies**: Google, Amazon, Microsoft, Facebook
```js
function decodeString(s) {
    const numStack = [];
    const strStack = [];
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

// Test cases
console.log(decodeString("3[a]2[bc]")); // "aaabcbc"
console.log(decodeString("3[a2[c]]")); // "accaccacc"
console.log(decodeString("2[abc]3[cd]ef")); // "abcabccdcdcdef"
```

---

# 🔴 HARD LEVEL PROBLEMS ⭐⭐⭐⭐⭐

## Hard Level Questions

### 1. Largest Rectangle in Histogram ⭐⭐⭐⭐⭐
**Problem**: Find the area of the largest rectangle that can be formed in a histogram.
**Companies**: Google, Amazon, Microsoft, Facebook, Apple
```js
function largestRectangleArea(heights) {
    const stack = [];
    let maxArea = 0;
    
    for (let i = 0; i <= heights.length; i++) {
        const currentHeight = i === heights.length ? 0 : heights[i];
        
        while (stack.length > 0 && heights[stack[stack.length - 1]] > currentHeight) {
            const height = heights[stack.pop()];
            const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        
        stack.push(i);
    }
    
    return maxArea;
}

// Test cases
console.log(largestRectangleArea([2,1,5,6,2,3])); // 10
console.log(largestRectangleArea([2,4])); // 4
```

### 2. Trapping Rain Water ⭐⭐⭐⭐⭐
**Problem**: Calculate how much water can be trapped after raining given elevation heights.
**Companies**: Amazon, Google, Microsoft, Facebook, Apple
```js
function trap(height) {
    const stack = [];
    let water = 0;
    
    for (let i = 0; i < height.length; i++) {
        while (stack.length > 0 && height[i] > height[stack[stack.length - 1]]) {
            const top = stack.pop();
            
            if (stack.length === 0) break;
            
            const boundedHeight = Math.min(height[i], height[stack[stack.length - 1]]) - height[top];
            water += distance * boundedHeight;
        }
        
        stack.push(i);
    }
    
    return water;
}

// Test cases
console.log(trap([0,1,0,2,1,0,1,3,2,1,2,1])); // 6
console.log(trap([4,2,0,3,2,5])); // 9
```

### 3. Basic Calculator ⭐⭐⭐⭐⭐
**Problem**: Implement a basic calculator to evaluate a mathematical expression string.
**Companies**: Google, Microsoft, Amazon, Facebook

```js
function calculate(s) {
    const stack = [];
    let num = 0;
    let sign = '+';
    
    for (let i = 0; i < s.length; i++) {
        const char = s[i];
        
        if (char >= '0' && char <= '9') {
            num = num * 10 + Number(char);
        }
        
        if (char === '+' || char === '-' || char === '*' || char === '/' || i === s.length - 1) {
            if (sign === '+') {
                stack.push(num);
            } else if (sign === '-') {
                stack.push(-num);
            } else if (sign === '*') {
                stack.push(stack.pop() * num);
            } else if (sign === '/') {
                stack.push(Math.trunc(stack.pop() / num));
            }
            
            sign = char;
            num = 0;
        }
    }
    
    return stack.reduce((sum, val) => sum + val, 0);
}

// Test cases
console.log(calculate("3+2*2")); // 7
console.log(calculate(" 3/2 ")); // 1
console.log(calculate(" 3+5 / 2 ")); // 5
```

### 4. Remove K Digits ⭐⭐⭐⭐⭐
**Problem**: Remove k digits from a number to make it as small as possible.
**Companies**: Google, Amazon, Microsoft, Facebook

```js
function removeKdigits(num, k) {
    const stack = [];
    
    for (let digit of num) {
        while (k > 0 && stack.length > 0 && stack[stack.length - 1] > digit) {
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
    const result = stack.join('');
    return result === '' || result === '0'.repeat(result.length) ? '0' : result;
}

// Test cases
console.log(removeKdigits("1432219", 3)); // "1219"
console.log(removeKdigits("10200", 1)); // "200"
console.log(removeKdigits("10", 2)); // "0"
```

### 5. Sliding Window Maximum ⭐⭐⭐⭐⭐
**Problem**: Find the maximum element in each sliding window of size k.
**Companies**: Google, Amazon, Microsoft, Facebook

```js
function maxSlidingWindow(nums, k) {
    const deque = []; // stores indices
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        // Remove elements outside current window
        while (deque.length > 0 && deque[0] < i - k + 1) {
            deque.shift();
        }
        
        // Remove smaller elements from back
        while (deque.length > 0 && nums[deque[deque.length - 1]] < nums[i]) {
            deque.pop();
        }
        
        deque.push(i);
        
        // Add to result if window is complete
        if (i >= k - 1) {
            result.push(nums[deque[0]]);
        }
    }
    
    return result;
}

// Test cases
console.log(maxSlidingWindow([1,3,-1,-3,5,3,6,7], 3)); // [3,3,5,5,6,7]
console.log(maxSlidingWindow([1], 1)); // [1]
```

### 6. Longest Valid Parentheses ⭐⭐⭐⭐⭐
**Problem**: Find the length of the longest valid parentheses substring.
**Companies**: Google, Amazon, Microsoft, Facebook

```js
function longestValidParentheses(s) {
    const stack = [-1]; // Initialize with -1 for base case
    let maxLength = 0;
    
    for (let i = 0; i < s.length; i++) {
        if (s[i] === '(') {
            stack.push(i);
        } else {
            stack.pop();
            
            if (stack.length === 0) {
                stack.push(i); // New base
            } else {
                maxLength = Math.max(maxLength, i - stack[stack.length - 1]);
            }
        }
    }
    
    return maxLength;
}

// Test cases
console.log(longestValidParentheses("(()")); // 2
console.log(longestValidParentheses(")()())")); // 4
console.log(longestValidParentheses("")); // 0
```

---

## 🎯 Additional Important Questions for Interviews

### Easy Level
- **Remove Outermost Parentheses** (Amazon, Google)
- **Build Array With Stack Operations** (Microsoft)
- **Make The String Great** (Facebook)
- **Crawler Log Folder** (Google)
- **Final Prices With Special Discount** (Amazon)

### Medium Level  
- **Next Greater Element II** (Amazon, Google)
- **Asteroid Collision** (Google, Microsoft)
- **Validate Stack Sequences** (Amazon, Facebook)
- **132 Pattern** (Google, Microsoft)
- **Sum of Subarray Minimums** (Amazon, Google)

### Hard Level
- **Maximum Rectangle** (Google, Amazon, Microsoft)
- **Remove Invalid Parentheses** (Facebook, Google)
- **Expression Add Operators** (Google, Microsoft)
- **Reverse Substrings Between Each Pair of Parentheses** (Amazon)
- **Minimum Remove to Make Valid Parentheses** (Facebook, Google)

---

## 📊 Interview Tips & Patterns

### Pattern Recognition
1. **Balanced Parentheses Pattern**: Use stack to match opening/closing pairs
2. **Next Greater/Smaller Pattern**: Monotonic stack approach
3. **Expression Evaluation**: Stack for operators and operands
4. **Histogram Problems**: Stack to find previous/next smaller elements
5. **String Reversal**: Stack for LIFO behavior

### Common Mistakes to Avoid
- Not checking for empty stack before pop/peek operations
- Forgetting to handle edge cases (empty strings, single elements)
- Incorrect precedence handling in expression evaluation
- Not considering integer overflow in calculations
- Missing boundary conditions in sliding window problems

### Time & Space Complexity Analysis
- Most stack operations: **O(1)** time
- Stack traversal problems: **O(n)** time, **O(n)** space
- Monotonic stack problems: **O(n)** amortized time
- Expression evaluation: **O(n)** time, **O(n)** space

---

## 🚀 Advanced Stack Applications

### System Design Applications
- **Function Call Stack**: Runtime execution management
- **Undo/Redo Operations**: Text editors, image editing
- **Browser History**: Back/forward navigation
- **Expression Parsers**: Compilers, calculators
- **Backtracking Algorithms**: N-Queens, maze solving

### JavaScript Built-in Stack Usage
```js
// Using Array as Stack (most common)
const stack = [];
stack.push(1);      // [1]
stack.push(2);      // [1, 2]
const top = stack.pop(); // 2, stack = [1]
const peek = stack[stack.length - 1]; // 1

// Check if empty
const isEmpty = stack.length === 0;
```
