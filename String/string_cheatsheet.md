# String Coding Interview Questions & Cheat Sheet (JavaScript)

## Common String Methods
```javascript
str.length                    // Get length
str[i] or str.charAt(i)      // Access character at index
str.slice(start, end)        // Extract substring
str.substring(start, end)    // Similar to slice
str.indexOf(char)            // Find first occurrence
str.lastIndexOf(char)        // Find last occurrence
str.includes(substring)      // Check if contains
str.split(delimiter)         // Convert to array
str.replace(old, new)        // Replace first occurrence
str.replaceAll(old, new)     // Replace all occurrences
str.toLowerCase()            // Convert to lowercase
str.toUpperCase()            // Convert to uppercase
str.trim()                   // Remove whitespace
str.repeat(count)            // Repeat a string multiple times
```

## Most Frequently Asked String Interview Questions

### EASY Level Questions

#### 1. Valid Palindrome ⭐⭐⭐
```javascript
function isPalindrome(s) {
    s = s.toLowerCase().replace(/[^a-z0-9]/g, '');
    let left = 0, right = s.length - 1;
    while (left < right) {
        if (s[left] !== s[right]) return false;
        left++;
        right--;
    }
    return true;
}
```

#### 2. Valid Anagram ⭐⭐⭐
```javascript
function isAnagram(s, t) {
    if (s.length !== t.length) return false;
    const freq = {};
    for (let c of s) freq[c] = (freq[c] || 0) + 1;
    for (let c of t) {
        if (!freq[c]) return false;
        freq[c]--;
    }
    return true;
}
```

#### 3. First Unique Character ⭐⭐
```javascript
function firstUniqChar(s) {
    const freq = {};
    for (let c of s) freq[c] = (freq[c] || 0) + 1;
    for (let i = 0; i < s.length; i++) {
        if (freq[s[i]] === 1) return i;
    }
    return -1;
}
```

#### 4. Reverse String
```javascript
function reverseString(s) {
    let left = 0, right = s.length - 1;
    while (left < right) {
        [s[left], s[right]] = [s[right], s[left]];
        left++;
        right--;
    }
}
```

#### 5. Valid Parentheses ⭐⭐⭐
```javascript
function isValidParentheses(s) {
    const stack = [];
    const pairs = {'(': ')', '[': ']', '{': '}'};
    for (let char of s) {
        if (char in pairs) {
            stack.push(char);
        } else {
            if (!stack.length || pairs[stack.pop()] !== char) return false;
        }
    }
    return stack.length === 0;
}
```

#### 6. Implement strStr() (Find Needle in Haystack)
```javascript
function strStr(haystack, needle) {
    if (!needle) return 0;
    return haystack.indexOf(needle);
}
```

#### 7. Longest Common Prefix
```javascript
function longestCommonPrefix(strs) {
    if (!strs.length) return "";
    let prefix = strs[0];
    for (let i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) !== 0) {
            prefix = prefix.slice(0, -1);
            if (!prefix) return "";
        }
    }
    return prefix;
}
```

#### 8. Roman to Integer ⭐⭐
```javascript
function romanToInt(s) {
    const map = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000};
    let result = 0;
    for (let i = 0; i < s.length; i++) {
        if (i < s.length - 1 && map[s[i]] < map[s[i + 1]]) {
            result -= map[s[i]];
        } else {
            result += map[s[i]];
        }
    }
    return result;
}
```

#### 9. Add Binary ⭐
```javascript
function addBinary(a, b) {
    let result = '';
    let carry = 0;
    let i = a.length - 1, j = b.length - 1;
    
    while (i >= 0 || j >= 0 || carry) {
        const sum = (i >= 0 ? +a[i] : 0) + (j >= 0 ? +b[j] : 0) + carry;
        result = (sum % 2) + result;
        carry = Math.floor(sum / 2);
        i--;
        j--;
    }
    return result;
}
```

#### 10. Length of Last Word
```javascript
function lengthOfLastWord(s) {
    s = s.trim();
    let len = 0;
    for (let i = s.length - 1; i >= 0 && s[i] !== ' '; i--) {
        len++;
    }
    return len;
}
```

### MEDIUM Level Questions

#### 1. Longest Substring Without Repeating Characters ⭐⭐⭐
```javascript
function lengthOfLongestSubstring(s) {
    let seen = new Set();
    let left = 0, maxLen = 0;
    for (let right = 0; right < s.length; right++) {
        while (seen.has(s[right])) {
            seen.delete(s[left]);
            left++;
        }
        seen.add(s[right]);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

#### 2. Group Anagrams ⭐⭐⭐
```javascript
function groupAnagrams(strs) {
    const map = new Map();
    for (let str of strs) {
        const key = str.split('').sort().join('');
        if (!map.has(key)) map.set(key, []);
        map.get(key).push(str);
    }
    return Array.from(map.values());
}
```

#### 3. Longest Palindromic Substring ⭐⭐⭐
```javascript
function longestPalindrome(s) {
    let start = 0, maxLen = 1;
    
    function expandAroundCenter(left, right) {
        while (left >= 0 && right < s.length && s[left] === s[right]) {
            const len = right - left + 1;
            if (len > maxLen) {
                start = left;
                maxLen = len;
            }
            left--;
            right++;
        }
    }
    
    for (let i = 0; i < s.length; i++) {
        expandAroundCenter(i, i);     // odd length
        expandAroundCenter(i, i + 1); // even length
    }
    
    return s.slice(start, start + maxLen);
}
```

#### 4. String to Integer (atoi)
```javascript
function myAtoi(s) {
    let i = 0, sign = 1, result = 0;
    const INT_MAX = 2**31 - 1, INT_MIN = -(2**31);
    
    // Skip whitespace
    while (s[i] === ' ') i++;
    
    // Check sign
    if (s[i] === '+' || s[i] === '-') {
        sign = s[i] === '-' ? -1 : 1;
        i++;
    }
    
    // Convert digits
    while (i < s.length && s[i] >= '0' && s[i] <= '9') {
        result = result * 10 + (s[i] - '0');
        if (sign * result > INT_MAX) return INT_MAX;
        if (sign * result < INT_MIN) return INT_MIN;
        i++;
    }
    
    return sign * result;
}
```

#### 5. Minimum Window Substring ⭐⭐⭐
```javascript
function minWindow(s, t) {
    const need = new Map();
    for (let c of t) need.set(c, (need.get(c) || 0) + 1);
    
    let left = 0, right = 0, valid = 0;
    let start = 0, len = Infinity;
    const window = new Map();
    
    while (right < s.length) {
        const c = s[right];
        right++;
        
        if (need.has(c)) {
            window.set(c, (window.get(c) || 0) + 1);
            if (window.get(c) === need.get(c)) valid++;
        }
        
        while (valid === need.size) {
            if (right - left < len) {
                start = left;
                len = right - left;
            }
            
            const d = s[left];
            left++;
            
            if (need.has(d)) {
                if (window.get(d) === need.get(d)) valid--;
                window.set(d, window.get(d) - 1);
            }
        }
    }
    
    return len === Infinity ? "" : s.slice(start, start + len);
}
```

#### 6. Permutation in String
```javascript
function checkInclusion(s1, s2) {
    if (s1.length > s2.length) return false;
    
    const need = new Map();
    for (let c of s1) need.set(c, (need.get(c) || 0) + 1);
    
    let left = 0, right = 0, valid = 0;
    const window = new Map();
    
    while (right < s2.length) {
        const c = s2[right];
        right++;
        
        if (need.has(c)) {
            window.set(c, (window.get(c) || 0) + 1);
            if (window.get(c) === need.get(c)) valid++;
        }
        
        while (right - left >= s1.length) {
            if (valid === need.size) return true;
            
            const d = s2[left];
            left++;
            
            if (need.has(d)) {
                if (window.get(d) === need.get(d)) valid--;
                window.set(d, window.get(d) - 1);
            }
        }
    }
    
    return false;
}
```

#### 7. Count and Say ⭐⭐
```javascript
function countAndSay(n) {
    let result = "1";
    for (let i = 1; i < n; i++) {
        result = getNext(result);
    }
    return result;
    
    function getNext(s) {
        let next = '';
        let count = 1;
        for (let j = 1; j <= s.length; j++) {
            if (j < s.length && s[j] === s[j-1]) {
                count++;
            } else {
                next += count + s[j-1];
                count = 1;
            }
        }
        return next;
    }
}
```

#### 8. Letter Combinations of Phone Number ⭐⭐⭐
```javascript
function letterCombinations(digits) {
    if (!digits) return [];
    
    const mapping = {
        '2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl',
        '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'
    };
    
    const result = [];
    
    function backtrack(index, current) {
        if (index === digits.length) {
            result.push(current);
            return;
        }
        
        const letters = mapping[digits[index]];
        for (let letter of letters) {
            backtrack(index + 1, current + letter);
        }
    }
    
    backtrack(0, '');
    return result;
}
```

#### 9. Generate Parentheses ⭐⭐⭐
```javascript
function generateParenthesis(n) {
    const result = [];
    
    function backtrack(current, open, close) {
        if (current.length === 2 * n) {
            result.push(current);
            return;
        }
        
        if (open < n) {
            backtrack(current + '(', open + 1, close);
        }
        if (close < open) {
            backtrack(current + ')', open, close + 1);
        }
    }
    
    backtrack('', 0, 0);
    return result;
}
```

### HARD Level Questions

#### 1. Edit Distance (Levenshtein Distance) ⭐⭐⭐
```javascript
function minDistance(word1, word2) {
    const m = word1.length, n = word2.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    // Initialize base cases
    for (let i = 0; i <= m; i++) dp[i][0] = i;
    for (let j = 0; j <= n; j++) dp[0][j] = j;
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i-1] === word2[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            } else {
                dp[i][j] = Math.min(
                    dp[i-1][j] + 1,    // delete
                    dp[i][j-1] + 1,    // insert
                    dp[i-1][j-1] + 1   // replace
                );
            }
        }
    }
    
    return dp[m][n];
}
```

#### 2. Regular Expression Matching ⭐⭐⭐
```javascript
function isMatch(s, p) {
    const memo = new Map();
    
    function dp(i, j) {
        const key = `${i},${j}`;
        if (memo.has(key)) return memo.get(key);
        
        let result;
        if (j === p.length) {
            result = i === s.length;
        } else {
            const firstMatch = i < s.length && (p[j] === s[i] || p[j] === '.');
            
            if (j + 1 < p.length && p[j + 1] === '*') {
                result = dp(i, j + 2) || (firstMatch && dp(i + 1, j));
            } else {
                result = firstMatch && dp(i + 1, j + 1);
            }
        }
        
        memo.set(key, result);
        return result;
    }
    
    return dp(0, 0);
}
```

#### 3. Wildcard Matching
```javascript
function isMatch(s, p) {
    const m = s.length, n = p.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(false));
    
    dp[0][0] = true;
    
    // Handle patterns like a* or *a*
    for (let j = 1; j <= n; j++) {
        if (p[j-1] === '*') dp[0][j] = dp[0][j-1];
    }
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (p[j-1] === '*') {
                dp[i][j] = dp[i-1][j] || dp[i][j-1];
            } else if (p[j-1] === '?' || s[i-1] === p[j-1]) {
                dp[i][j] = dp[i-1][j-1];
            }
        }
    }
    
    return dp[m][n];
}
```

#### 4. Substring with Concatenation of All Words
```javascript
function findSubstring(s, words) {
    if (!s || !words.length) return [];
    
    const wordLen = words[0].length;
    const totalLen = wordLen * words.length;
    const wordCount = new Map();
    
    for (let word of words) {
        wordCount.set(word, (wordCount.get(word) || 0) + 1);
    }
    
    const result = [];
    
    for (let i = 0; i <= s.length - totalLen; i++) {
        const seen = new Map();
        let j = 0;
        
        while (j < words.length) {
            const word = s.slice(i + j * wordLen, i + (j + 1) * wordLen);
            if (!wordCount.has(word)) break;
            
            seen.set(word, (seen.get(word) || 0) + 1);
            if (seen.get(word) > wordCount.get(word)) break;
            
            j++;
        }
        
        if (j === words.length) result.push(i);
    }
    
    return result;
}
```

#### 5. Text Justification ⭐⭐⭐
```javascript
function fullJustify(words, maxWidth) {
    const result = [];
    let i = 0;
    
    while (i < words.length) {
        let line = [];
        let lineLength = 0;
        
        // Pack words into current line
        while (i < words.length && lineLength + words[i].length + line.length <= maxWidth) {
            line.push(words[i]);
            lineLength += words[i].length;
            i++;
        }
        
        // Justify the line
        if (i === words.length || line.length === 1) {
            // Last line or single word - left justify
            let justified = line.join(' ');
            justified += ' '.repeat(maxWidth - justified.length);
            result.push(justified);
        } else {
            // Distribute spaces evenly
            const totalSpaces = maxWidth - lineLength;
            const gaps = line.length - 1;
            const spacePerGap = Math.floor(totalSpaces / gaps);
            const extraSpaces = totalSpaces % gaps;
            
            let justified = '';
            for (let j = 0; j < line.length - 1; j++) {
                justified += line[j];
                justified += ' '.repeat(spacePerGap + (j < extraSpaces ? 1 : 0));
            }
            justified += line[line.length - 1];
            result.push(justified);
        }
    }
    
    return result;
}
```

#### 6. Palindrome Partitioning ⭐⭐
```javascript
function partition(s) {
    const result = [];
    
    function isPalindrome(str, start, end) {
        while (start < end) {
            if (str[start] !== str[end]) return false;
            start++;
            end--;
        }
        return true;
    }
    
    function backtrack(start, current) {
        if (start === s.length) {
            result.push([...current]);
            return;
        }
        
        for (let end = start; end < s.length; end++) {
            if (isPalindrome(s, start, end)) {
                current.push(s.slice(start, end + 1));
                backtrack(end + 1, current);
                current.pop();
            }
        }
    }
    
    backtrack(0, []);
    return result;
}
```

#### 7. Word Break II ⭐⭐
```javascript
function wordBreak(s, wordDict) {
    const wordSet = new Set(wordDict);
    const memo = new Map();
    
    function backtrack(start) {
        if (memo.has(start)) return memo.get(start);
        
        if (start === s.length) return [''];
        
        const result = [];
        for (let end = start + 1; end <= s.length; end++) {
            const word = s.slice(start, end);
            if (wordSet.has(word)) {
                const suffixes = backtrack(end);
                for (let suffix of suffixes) {
                    result.push(word + (suffix ? ' ' + suffix : ''));
                }
            }
        }
        
        memo.set(start, result);
        return result;
    }
    
    return backtrack(0);
}
```

#### 8. Distinct Subsequences ⭐⭐
```javascript
function numDistinct(s, t) {
    const m = s.length, n = t.length;
    const dp = Array(m + 1).fill().map(() => Array(n + 1).fill(0));
    
    // Empty string can be formed in one way
    for (let i = 0; i <= m; i++) dp[i][0] = 1;
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            dp[i][j] = dp[i-1][j]; // Don't use s[i-1]
            if (s[i-1] === t[j-1]) {
                dp[i][j] += dp[i-1][j-1]; // Use s[i-1]
            }
        }
    }
    
    return dp[m][n];
}
```

## Core Patterns & Techniques

### Pattern 1: Two Pointers
**Use for:** Palindrome, reverse, comparison problems

```javascript
// Check palindrome
function isPalindrome(s) {
    let left = 0, right = s.length - 1;
    while (left < right) {
        if (s[left] !== s[right]) return false;
        left++;
        right--;
    }
    return true;
}

// Reverse string
function reverse(s) {
    let arr = s.split('');
    let left = 0, right = arr.length - 1;
    while (left < right) {
        [arr[left], arr[right]] = [arr[right], arr[left]];
        left++;
        right--;
    }
    return arr.join('');
}
```

### Pattern 2: Sliding Window
**Use for:** Substring problems, longest/shortest patterns

```javascript
// Longest substring without repeating characters
function lengthOfLongestSubstring(s) {
    let seen = new Set();
    let left = 0, maxLen = 0;
    
    for (let right = 0; right < s.length; right++) {
        while (seen.has(s[right])) {
            seen.delete(s[left]);
            left++;
        }
        seen.add(s[right]);
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

### Pattern 3: Hash Map/Frequency Counter
**Use for:** Anagram, character counting, grouping

```javascript
// Check if two strings are anagrams
function isAnagram(s, t) {
    if (s.length !== t.length) return false;
    
    const freq = {};
    for (let char of s) {
        freq[char] = (freq[char] || 0) + 1;
    }
    
    for (let char of t) {
        if (!freq[char]) return false;
        freq[char]--;
    }
    return true;
}

// Character frequency
function getCharFreq(s) {
    const freq = {};
    for (let char of s) {
        freq[char] = (freq[char] || 0) + 1;
    }
    return freq;
}
```

### Pattern 4: String Building/Manipulation
**Use for:** Transformation, formatting problems

```javascript
// Remove duplicates
function removeDuplicates(s) {
    const seen = new Set();
    let result = '';
    for (let char of s) {
        if (!seen.has(char)) {
            seen.add(char);
            result += char;
        }
    }
    return result;
}

// Compress string (aabcccccaaa -> a2b1c5a3)
function compress(s) {
    let result = '';
    let count = 1;
    
    for (let i = 1; i <= s.length; i++) {
        if (s[i] === s[i-1]) {
            count++;
        } else {
            result += s[i-1] + count;
            count = 1;
        }
    }
    return result;
}
```

### Pattern 5: Stack for Matching
**Use for:** Parentheses, bracket matching, undo operations

```javascript
// Valid parentheses
function isValidParentheses(s) {
    const stack = [];
    const pairs = {'(': ')', '[': ']', '{': '}'};
    
    for (let char of s) {
        if (char in pairs) {
            stack.push(char);
        } else {
            if (stack.length === 0 || pairs[stack.pop()] !== char) {
                return false;
            }
        }
    }
    return stack.length === 0;
}
```

### Pattern 6: Backtracking for String Generation
**Use for:** Generate combinations, permutations, valid sequences

```javascript
// Generate all valid IP addresses
function restoreIpAddresses(s) {
    const result = [];
    
    function isValid(segment) {
        if (segment.length > 3 || segment.length === 0) return false;
        if (segment[0] === '0' && segment.length > 1) return false;
        return parseInt(segment) <= 255;
    }
    
    function backtrack(start, parts) {
        if (parts.length === 4 && start === s.length) {
            result.push(parts.join('.'));
            return;
        }
        if (parts.length === 4) return;
        
        for (let len = 1; len <= 3 && start + len <= s.length; len++) {
            const segment = s.slice(start, start + len);
            if (isValid(segment)) {
                parts.push(segment);
                backtrack(start + len, parts);
                parts.pop();
            }
        }
    }
    
    backtrack(0, []);
    return result;
}
```

### Pattern 7: String Encoding/Decoding
**Use for:** Compression, serialization problems

```javascript
// Encode and Decode Strings
class Codec {
    encode(strs) {
        return strs.map(s => s.length + '#' + s).join('');
    }
    
    decode(s) {
        const result = [];
        let i = 0;
        
        while (i < s.length) {
            let j = i;
            while (s[j] !== '#') j++;
            
            const length = parseInt(s.slice(i, j));
            const str = s.slice(j + 1, j + 1 + length);
            result.push(str);
            i = j + 1 + length;
        }
        
        return result;
    }
}

// String Compression
function compress(chars) {
    let write = 0;
    let i = 0;
    
    while (i < chars.length) {
        const char = chars[i];
        let count = 0;
        
        while (i < chars.length && chars[i] === char) {
            i++;
            count++;
        }
        
        chars[write++] = char;
        if (count > 1) {
            for (let digit of count.toString()) {
                chars[write++] = digit;
            }
        }
    }
    
    return write;
}
```

### Pattern 8: Trie-Based String Problems
**Use for:** Prefix matching, word search problems

```javascript
// Implement Trie
class TrieNode {
    constructor() {
        this.children = {};
        this.isEnd = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }
    
    insert(word) {
        let node = this.root;
        for (let char of word) {
            if (!node.children[char]) {
                node.children[char] = new TrieNode();
            }
            node = node.children[char];
        }
        node.isEnd = true;
    }
    
    search(word) {
        let node = this.root;
        for (let char of word) {
            if (!node.children[char]) return false;
            node = node.children[char];
        }
        return node.isEnd;
    }
    
    startsWith(prefix) {
        let node = this.root;
        for (let char of prefix) {
            if (!node.children[char]) return false;
            node = node.children[char];
        }
        return true;
    }
}

// Word Search II (using Trie)
function findWords(board, words) {
    const trie = new Trie();
    words.forEach(word => trie.insert(word));
    
    const result = new Set();
    const directions = [[0,1], [1,0], [0,-1], [-1,0]];
    
    function dfs(i, j, node, word) {
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return;
        
        const char = board[i][j];
        if (!node.children[char]) return;
        
        node = node.children[char];
        word += char;
        
        if (node.isEnd) {
            result.add(word);
        }
        
        board[i][j] = '#'; // Mark as visited
        
        for (let [di, dj] of directions) {
            dfs(i + di, j + dj, node, word);
        }
        
        board[i][j] = char; // Restore
    }
    
    for (let i = 0; i < board.length; i++) {
        for (let j = 0; j < board[0].length; j++) {
            dfs(i, j, trie.root, '');
        }
    }
    
    return Array.from(result);
}
```

### Pattern 9: Rolling Hash for Pattern Matching
**Use for:** Efficient substring search, duplicate detection

```javascript
// Rabin-Karp Algorithm
function strStr(haystack, needle) {
    if (!needle) return 0;
    
    const base = 26;
    const mod = 1e9 + 7;
    const n = haystack.length, m = needle.length;
    
    if (n < m) return -1;
    
    // Calculate hash of needle and first window
    let needleHash = 0, windowHash = 0, pow = 1;
    
    for (let i = 0; i < m; i++) {
        needleHash = (needleHash * base + needle.charCodeAt(i)) % mod;
        windowHash = (windowHash * base + haystack.charCodeAt(i)) % mod;
        if (i < m - 1) pow = (pow * base) % mod;
    }
    
    // Rolling hash
    for (let i = 0; i <= n - m; i++) {
        if (windowHash === needleHash && haystack.slice(i, i + m) === needle) {
            return i;
        }
        
        if (i < n - m) {
            windowHash = (windowHash - haystack.charCodeAt(i) * pow) % mod;
            windowHash = (windowHash * base + haystack.charCodeAt(i + m)) % mod;
            if (windowHash < 0) windowHash += mod;
        }
    }
    
    return -1;
}

// Find All Duplicates in Array (using rolling hash concept)
function findRepeatedDnaSequences(s) {
    if (s.length < 10) return [];
    
    const seen = new Set();
    const result = new Set();
    
    for (let i = 0; i <= s.length - 10; i++) {
        const sequence = s.slice(i, i + 10);
        if (seen.has(sequence)) {
            result.add(sequence);
        } else {
            seen.add(sequence);
        }
    }
    
    return Array.from(result);
}
```


## Common String Algorithms

### KMP (Pattern Matching)
```javascript
function strStr(haystack, needle) {
    if (needle.length === 0) return 0;
    
    // Build LPS array
    const lps = new Array(needle.length).fill(0);
    let len = 0, i = 1;
    
    while (i < needle.length) {
        if (needle[i] === needle[len]) {
            lps[i] = ++len;
            i++;
        } else if (len > 0) {
            len = lps[len - 1];
        } else {
            lps[i] = 0;
            i++;
        }
    }
    
    // Search pattern
    i = 0; // haystack index
    let j = 0; // needle index
    
    while (i < haystack.length) {
        if (haystack[i] === needle[j]) {
            i++;
            j++;
        }
        
        if (j === needle.length) {
            return i - j;
        } else if (i < haystack.length && haystack[i] !== needle[j]) {
            if (j > 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    return -1;
}
```

## Quick Reference Patterns

| Problem Type | Pattern | Key Insight |
|--------------|---------|-------------|
| Palindrome | Two Pointers | Compare from both ends |
| Anagram | Hash Map | Count character frequencies |
| Substring | Sliding Window | Expand/contract window |
| Parentheses | Stack | LIFO matching |
| Pattern Search | KMP/Rolling Hash | Preprocessing for efficiency |
| Longest Common | DP | Build table bottom-up |

## Edge Cases to Remember
- Empty strings (`""`)
- Single character strings
- All same characters (`"aaaa"`)
- Case sensitivity
- Special characters and spaces
- Unicode characters
- Very long strings (performance)

## Time Complexities
- Character access: O(1)
- String concatenation: O(n) per operation
- Array join: O(n)
- indexOf/includes: O(n)
- Two pointers: O(n)
- Hash map operations: O(1) average
- Sliding window: O(n)

## Pro Tips
1. Use arrays for frequent modifications, join at end
2. Consider character codes for case-insensitive comparisons
3. Use Set for O(1) lookups instead of indexOf
4. Remember string immutability in JavaScript
5. Use template literals for readable string building
6. Consider regex for complex pattern matching
