# JavaScript Array Coding Interview Cheat Sheet

## Common Array Methods

### Basic Operations
```js
arr.length              // Get array length
arr[i]                  // Access element at index i
arr.push(x)             // Add to end - O(1)
arr.pop()               // Remove from end - O(1)
arr.unshift(x)          // Add to start - O(n)
arr.shift()             // Remove from start - O(n)
arr.splice(i, n, ...x)  // Remove n elements from index i, insert x
```

### Search & Find
```js
arr.indexOf(x)          // First index of x, -1 if not found
arr.includes(x)         // Boolean check if x exists
arr.find(fn)            // First element matching condition
arr.findIndex(fn)       // Index of first element matching condition
```

### Transform & Filter
```js
arr.map(fn)             // Transform each element
arr.filter(fn)          // Keep elements matching condition
arr.reduce(fn, initial) // Reduce to single value
arr.sort(compareFn)     // Sort in place
arr.reverse()           // Reverse in place
```

## Key Patterns & Techniques

### Two Pointers
```js
// Opposite direction
let left = 0, right = arr.length - 1;
while (left < right) {
    // Process arr[left] and arr[right]
    left++;
    right--;
}

// Same direction (slow/fast)
let slow = 0;
for (let fast = 0; fast < arr.length; fast++) {
    if (condition) {
        arr[slow] = arr[fast];
        slow++;
    }
}
```

### Sliding Window
```js
// Fixed size window
let sum = 0;
for (let i = 0; i < k; i++) sum += arr[i];
let maxSum = sum;

for (let i = k; i < arr.length; i++) {
    sum = sum - arr[i - k] + arr[i];
    maxSum = Math.max(maxSum, sum);
}

// Variable size window
let left = 0, sum = 0;
for (let right = 0; right < arr.length; right++) {
    sum += arr[right];
    while (sum > target) {
        sum -= arr[left];
        left++;
    }
    // Process current window [left, right]
}
```

### Prefix Sum
```js
// Build prefix sum array
const prefixSum = [0];
for (let i = 0; i < arr.length; i++) {
    prefixSum[i + 1] = prefixSum[i] + arr[i];
}

// Range sum query [i, j]
const rangeSum = prefixSum[j + 1] - prefixSum[i];
```

### Hash Map for Frequency/Indices
```js
const map = new Map();
for (let i = 0; i < arr.length; i++) {
    if (map.has(target - arr[i])) {
        return [map.get(target - arr[i]), i];
    }
    map.set(arr[i], i);
}
```

## Common Problem Types

### Searching
```js
// Binary Search
let left = 0, right = arr.length - 1;
while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (arr[mid] === target) return mid;
    if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
}
return -1;
```

### Sorting Patterns
```js
// Custom comparator
arr.sort((a, b) => a - b);        // Ascending numbers
arr.sort((a, b) => b - a);        // Descending numbers
arr.sort((a, b) => a.localeCompare(b)); // Strings

// Sort by property
arr.sort((a, b) => a.value - b.value);
```

### Remove Duplicates
```js
// In-place removal (sorted array)
let j = 1;
for (let i = 1; i < arr.length; i++) {
    if (arr[i] !== arr[i-1]) {
        arr[j] = arr[i];
        j++;
    }
}
return j; // new length

// Using Set
const unique = [...new Set(arr)];
```

### Array Rotation
```js
// Rotate right by k positions
function rotate(arr, k) {
    k = k % arr.length;
    reverse(arr, 0, arr.length - 1);
    reverse(arr, 0, k - 1);
    reverse(arr, k, arr.length - 1);
}

function reverse(arr, start, end) {
    while (start < end) {
        [arr[start], arr[end]] = [arr[end], arr[start]];
        start++;
        end--;
    }
}
```

### Subarray Problems
```js
// Maximum subarray sum (Kadane's algorithm)
let maxSum = arr[0], currentSum = arr[0];
for (let i = 1; i < arr.length; i++) {
    currentSum = Math.max(arr[i], currentSum + arr[i]);
    maxSum = Math.max(maxSum, currentSum);
}
```

## Utility Functions

### Array Creation
```js
new Array(n).fill(0)              // Array of n zeros
Array.from({length: n}, (_, i) => i) // [0, 1, 2, ..., n-1]
Array.from({length: n}, () => [])    // Array of n empty arrays
```

### Common Checks
```js
// Check if array is sorted
const isSorted = arr.every((val, i) => i === 0 || arr[i-1] <= val);

// Check if all elements are unique
const isUnique = arr.length === new Set(arr).size;
```


# Array Interview Questions by Difficulty Level

## Easy Level Questions

### 1. Two Sum ⭐⭐⭐⭐⭐
**Problem**: Find two numbers in array that add up to target
```js
function twoSum(nums, target) {
    const map = new Map();
    for (let i = 0; i < nums.length; i++) {
        const complement = target - nums[i];
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        map.set(nums[i], i);
    }
    return [];
}
// Time: O(n), Space: O(n)
```

### 2. Remove Duplicates from Sorted Array ⭐⭐⭐⭐
**Problem**: Remove duplicates in-place, return new length
```js
function removeDuplicates(nums) {
    if (nums.length === 0) return 0;
    let j = 1;
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] !== nums[i-1]) {
            nums[j] = nums[i];
            j++;
        }
    }
    return j;
}
// Time: O(n), Space: O(1)
```

### 3. Best Time to Buy and Sell Stock ⭐⭐⭐⭐⭐
**Problem**: Find maximum profit from buying and selling stock once
```js
function maxProfit(prices) {
    let minPrice = prices[0];
    let maxProfit = 0;
    for (let i = 1; i < prices.length; i++) {
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else {
            maxProfit = Math.max(maxProfit, prices[i] - minPrice);
        }
    }
    return maxProfit;
}
// Time: O(n), Space: O(1)
```

### 4. Contains Duplicate ⭐⭐⭐⭐⭐
**Problem**: Check if array contains any duplicate values
```js
function containsDuplicate(nums) {
    return new Set(nums).size !== nums.length;
}
// Time: O(n), Space: O(n)
```

### 5. Maximum Subarray (Easy version) ⭐⭐⭐
**Problem**: Find sum of contiguous subarray with largest sum
```js
function maxSubArray(nums) {
    let maxSum = nums[0];
    let currentSum = nums[0];
    for (let i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
// Time: O(n), Space: O(1)
```

### 6. Merge Sorted Array ⭐⭐⭐⭐
**Problem**: Merge two sorted arrays in-place
```js
function merge(nums1, m, nums2, n) {
    let i = m - 1, j = n - 1, k = m + n - 1;
    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[k--] = nums1[i--];
        } else {
            nums1[k--] = nums2[j--];
        }
    }
    while (j >= 0) {
        nums1[k--] = nums2[j--];
    }
}
// Time: O(m+n), Space: O(1)
```

### 7. Plus One ⭐⭐⭐
**Problem**: Add 1 to number represented as array of digits
```js
function plusOne(digits) {
    for (let i = digits.length - 1; i >= 0; i--) {
        if (digits[i] < 9) {
            digits[i]++;
            return digits;
        }
        digits[i] = 0;
    }
    return [1, ...digits];
}
// Time: O(n), Space: O(1) amortized
```

### 8. Move Zeroes ⭐⭐⭐⭐
**Problem**: Move all zeros to end while maintaining relative order
```js
function moveZeroes(nums) {
    let writeIndex = 0;
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] !== 0) {
            nums[writeIndex] = nums[i];
            writeIndex++;
        }
    }
    for (let i = writeIndex; i < nums.length; i++) {
        nums[i] = 0;
    }
}
// Time: O(n), Space: O(1)
```

### 9. Intersection of Two Arrays II ⭐⭐⭐
**Problem**: Find intersection with frequency count
```js
function intersect(nums1, nums2) {
    const map = new Map();
    const result = [];
    
    for (let num of nums1) {
        map.set(num, (map.get(num) || 0) + 1);
    }
    
    for (let num of nums2) {
        if (map.get(num) > 0) {
            result.push(num);
            map.set(num, map.get(num) - 1);
        }
    }
    return result;
}
// Time: O(m+n), Space: O(min(m,n))
```

### 10. Valid Mountain Array ⭐⭐
**Problem**: Check if array forms a valid mountain
```js
function validMountainArray(arr) {
    if (arr.length < 3) return false;
    
    let i = 0;
    // Walk up
    while (i < arr.length - 1 && arr[i] < arr[i + 1]) {
        i++;
    }
    
    // Peak can't be first or last
    if (i === 0 || i === arr.length - 1) return false;
    
    // Walk down
    while (i < arr.length - 1 && arr[i] > arr[i + 1]) {
        i++;
    }
    
    return i === arr.length - 1;
}
// Time: O(n), Space: O(1)
```

---

## Medium Level Questions

### 1. 3Sum ⭐⭐⭐⭐⭐
**Problem**: Find all unique triplets that sum to zero
```js
function threeSum(nums) {
    nums.sort((a, b) => a - b);
    const result = [];
    
    for (let i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] === nums[i-1]) continue;
        
        let left = i + 1, right = nums.length - 1;
        while (left < right) {
            const sum = nums[i] + nums[left] + nums[right];
            if (sum === 0) {
                result.push([nums[i], nums[left], nums[right]]);
                while (left < right && nums[left] === nums[left+1]) left++;
                while (left < right && nums[right] === nums[right-1]) right--;
                left++;
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    return result;
}
// Time: O(n²), Space: O(1)
```

### 2. Product of Array Except Self ⭐⭐⭐⭐⭐
**Problem**: Return array where each element is product of all other elements
```js
function productExceptSelf(nums) {
    const result = new Array(nums.length);
    
    // Left pass
    result[0] = 1;
    for (let i = 1; i < nums.length; i++) {
        result[i] = result[i-1] * nums[i-1];
    }
    
    // Right pass
    let right = 1;
    for (let i = nums.length - 1; i >= 0; i--) {
        result[i] *= right;
        right *= nums[i];
    }
    
    return result;
}
// Time: O(n), Space: O(1)
```

### 3. Container With Most Water ⭐⭐⭐⭐⭐
**Problem**: Find two lines that form container holding most water
```js
function maxArea(height) {
    let left = 0, right = height.length - 1;
    let maxWater = 0;
    
    while (left < right) {
        const width = right - left;
        const minHeight = Math.min(height[left], height[right]);
        maxWater = Math.max(maxWater, width * minHeight);
        
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    return maxWater;
}
// Time: O(n), Space: O(1)
```

### 4. Find All Duplicates in Array ⭐⭐⭐
**Problem**: Find all elements that appear twice (nums[i] in range [1,n])
```js
function findDuplicates(nums) {
    const result = [];
    for (let i = 0; i < nums.length; i++) {
        const index = Math.abs(nums[i]) - 1;
        if (nums[index] < 0) {
            result.push(index + 1);
        } else {
            nums[index] = -nums[index];
        }
    }
    return result;
}
// Time: O(n), Space: O(1)
```

### 5. Subarray Sum Equals K ⭐⭐⭐⭐
**Problem**: Count number of subarrays with sum equal to k
```js
function subarraySum(nums, k) {
    const map = new Map();
    map.set(0, 1);
    let count = 0, sum = 0;
    
    for (let num of nums) {
        sum += num;
        if (map.has(sum - k)) {
            count += map.get(sum - k);
        }
        map.set(sum, (map.get(sum) || 0) + 1);
    }
    return count;
}
// Time: O(n), Space: O(n)
```

### 6. Rotate Array ⭐⭐⭐⭐
**Problem**: Rotate array to right by k steps
```js
function rotate(nums, k) {
    k = k % nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

function reverse(nums, start, end) {
    while (start < end) {
        [nums[start], nums[end]] = [nums[end], nums[start]];
        start++;
        end--;
    }
}
// Time: O(n), Space: O(1)
```

### 7. Set Matrix Zeroes ⭐⭐⭐⭐
**Problem**: Set entire row and column to zero if element is zero
```js
function setZeroes(matrix) {
    const m = matrix.length, n = matrix[0].length;
    let firstRowZero = false, firstColZero = false;
    
    // Check if first row/col should be zero
    for (let i = 0; i < m; i++) {
        if (matrix[i][0] === 0) firstColZero = true;
    }
    for (let j = 0; j < n; j++) {
        if (matrix[0][j] === 0) firstRowZero = true;
    }
    
    // Use first row/col as markers
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][j] === 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }
    
    // Set zeros based on markers
    for (let i = 1; i < m; i++) {
        for (let j = 1; j < n; j++) {
            if (matrix[i][0] === 0 || matrix[0][j] === 0) {
                matrix[i][j] = 0;
            }
        }
    }
    
    // Handle first row/col
    if (firstRowZero) {
        for (let j = 0; j < n; j++) matrix[0][j] = 0;
    }
    if (firstColZero) {
        for (let i = 0; i < m; i++) matrix[i][0] = 0;
    }
}
// Time: O(m×n), Space: O(1)
```

### 8. Next Permutation ⭐⭐⭐
**Problem**: Find lexicographically next greater permutation
```js
function nextPermutation(nums) {
    let i = nums.length - 2;
    
    // Find first decreasing element from right
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }
    
    if (i >= 0) {
        // Find smallest element greater than nums[i]
        let j = nums.length - 1;
        while (nums[j] <= nums[i]) {
            j--;
        }
        [nums[i], nums[j]] = [nums[j], nums[i]];
    }
    
    // Reverse the suffix
    reverse(nums, i + 1, nums.length - 1);
}

function reverse(nums, start, end) {
    while (start < end) {
        [nums[start], nums[end]] = [nums[end], nums[start]];
        start++;
        end--;
    }
}
// Time: O(n), Space: O(1)
```

### 9. Find Peak Element ⭐⭐⭐
**Problem**: Find any peak element in O(log n) time
```js
function findPeakElement(nums) {
    let left = 0, right = nums.length - 1;
    
    while (left < right) {
        const mid = Math.floor((left + right) / 2);
        if (nums[mid] > nums[mid + 1]) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
// Time: O(log n), Space: O(1)
```

### 10. Longest Consecutive Sequence ⭐⭐⭐⭐
**Problem**: Find length of longest consecutive sequence
```js
function longestConsecutive(nums) {
    const numSet = new Set(nums);
    let maxLength = 0;
    
    for (let num of numSet) {
        // Only start counting if it's the beginning of a sequence
        if (!numSet.has(num - 1)) {
            let currentNum = num;
            let currentLength = 1;
            
            while (numSet.has(currentNum + 1)) {
                currentNum++;
                currentLength++;
            }
            
            maxLength = Math.max(maxLength, currentLength);
        }
    }
    return maxLength;
}
// Time: O(n), Space: O(n)
```

### 11. Minimum Window Substring (Array Pattern) ⭐⭐
**Problem**: Find minimum window containing all characters
```js
function minWindow(s, t) {
    const need = new Map();
    const window = new Map();
    
    for (let c of t) {
        need.set(c, (need.get(c) || 0) + 1);
    }
    
    let left = 0, right = 0;
    let valid = 0;
    let start = 0, len = Infinity;
    
    while (right < s.length) {
        let c = s[right];
        right++;
        
        if (need.has(c)) {
            window.set(c, (window.get(c) || 0) + 1);
            if (window.get(c) === need.get(c)) {
                valid++;
            }
        }
        
        while (valid === need.size) {
            if (right - left < len) {
                start = left;
                len = right - left;
            }
            
            let d = s[left];
            left++;
            
            if (need.has(d)) {
                if (window.get(d) === need.get(d)) {
                    valid--;
                }
                window.set(d, window.get(d) - 1);
            }
        }
    }
    
    return len === Infinity ? "" : s.substr(start, len);
}
// Time: O(m+n), Space: O(m+n)
```

---

## Hard Level Questions

### 1. Trapping Rain Water ⭐⭐⭐⭐⭐
**Problem**: Calculate how much rain water can be trapped
```js
function trap(height) {
    if (height.length === 0) return 0;
    
    let left = 0, right = height.length - 1;
    let leftMax = 0, rightMax = 0;
    let water = 0;
    
    while (left < right) {
        if (height[left] < height[right]) {
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                water += leftMax - height[left];
            }
            left++;
        } else {
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                water += rightMax - height[right];
            }
            right--;
        }
    }
    return water;
}
// Time: O(n), Space: O(1)
```

### 2. First Missing Positive ⭐⭐⭐⭐⭐
**Problem**: Find smallest missing positive integer
```js
function firstMissingPositive(nums) {
    const n = nums.length;
    
    // Place each positive number i at index i-1
    for (let i = 0; i < n; i++) {
        while (nums[i] > 0 && nums[i] <= n && nums[nums[i] - 1] !== nums[i]) {
            [nums[nums[i] - 1], nums[i]] = [nums[i], nums[nums[i] - 1]];
        }
    }
    
    // Find first missing positive
    for (let i = 0; i < n; i++) {
        if (nums[i] !== i + 1) {
            return i + 1;
        }
    }
    return n + 1;
}
// Time: O(n), Space: O(1)
```

### 3. Median of Two Sorted Arrays ⭐⭐⭐⭐
**Problem**: Find median of two sorted arrays in O(log(m+n))
```js
function findMedianSortedArrays(nums1, nums2) {
    if (nums1.length > nums2.length) {
        [nums1, nums2] = [nums2, nums1];
    }
    
    const m = nums1.length, n = nums2.length;
    let left = 0, right = m;
    
    while (left <= right) {
        const partitionX = Math.floor((left + right) / 2);
        const partitionY = Math.floor((m + n + 1) / 2) - partitionX;
        
        const maxLeftX = partitionX === 0 ? -Infinity : nums1[partitionX - 1];
        const maxLeftY = partitionY === 0 ? -Infinity : nums2[partitionY - 1];
        
        const minRightX = partitionX === m ? Infinity : nums1[partitionX];
        const minRightY = partitionY === n ? Infinity : nums2[partitionY];
        
        if (maxLeftX <= minRightY && maxLeftY <= minRightX) {
            if ((m + n) % 2 === 0) {
                return (Math.max(maxLeftX, maxLeftY) + Math.min(minRightX, minRightY)) / 2;
            } else {
                return Math.max(maxLeftX, maxLeftY);
            }
        } else if (maxLeftX > minRightY) {
            right = partitionX - 1;
        } else {
            left = partitionX + 1;
        }
    }
}
// Time: O(log(min(m,n))), Space: O(1)
```

### 4. Largest Rectangle in Histogram ⭐⭐⭐
**Problem**: Find largest rectangle area in histogram
```js
function largestRectangleArea(heights) {
    const stack = [];
    let maxArea = 0;
    
    for (let i = 0; i <= heights.length; i++) {
        const currentHeight = i === heights.length ? 0 : heights[i];
        
        while (stack.length && heights[stack[stack.length - 1]] > currentHeight) {
            const height = heights[stack.pop()];
            const width = stack.length === 0 ? i : i - stack[stack.length - 1] - 1;
            maxArea = Math.max(maxArea, height * width);
        }
        stack.push(i);
    }
    return maxArea;
}
// Time: O(n), Space: O(n)
```

### 5. Sliding Window Maximum ⭐⭐⭐⭐
**Problem**: Find maximum in each sliding window of size k
```js
function maxSlidingWindow(nums, k) {
    const deque = [];
    const result = [];
    
    for (let i = 0; i < nums.length; i++) {
        // Remove elements outside current window
        while (deque.length && deque[0] <= i - k) {
            deque.shift();
        }
        
        // Remove smaller elements from back
        while (deque.length && nums[deque[deque.length - 1]] <= nums[i]) {
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
// Time: O(n), Space: O(k)
```

### 6. Maximum Gap ⭐⭐
**Problem**: Find maximum difference between successive elements in sorted form
```js
function maximumGap(nums) {
    if (nums.length < 2) return 0;
    
    const n = nums.length;
    const minVal = Math.min(...nums);
    const maxVal = Math.max(...nums);
    
    if (minVal === maxVal) return 0;
    
    const bucketSize = Math.max(1, Math.floor((maxVal - minVal) / (n - 1)));
    const bucketCount = Math.floor((maxVal - minVal) / bucketSize) + 1;
    
    const buckets = Array(bucketCount).fill(null).map(() => ({ min: null, max: null }));
    
    for (let num of nums) {
        const bucketIndex = Math.floor((num - minVal) / bucketSize);
        const bucket = buckets[bucketIndex];
        bucket.min = bucket.min === null ? num : Math.min(bucket.min, num);
        bucket.max = bucket.max === null ? num : Math.max(bucket.max, num);
    }
    
    let maxGap = 0;
    let prevMax = minVal;
    
    for (let bucket of buckets) {
        if (bucket.min !== null) {
            maxGap = Math.max(maxGap, bucket.min - prevMax);
            prevMax = bucket.max;
        }
    }
    
    return maxGap;
}
// Time: O(n), Space: O(n)
```

### 7. Count of Smaller Numbers After Self ⭐⭐
**Problem**: Count how many numbers after current are smaller
```js
function countSmaller(nums) {
    const result = new Array(nums.length).fill(0);
    const indices = Array.from({length: nums.length}, (_, i) => i);
    
    function mergeSort(start, end) {
        if (start >= end) return;
        
        const mid = Math.floor((start + end) / 2);
        mergeSort(start, mid);
        mergeSort(mid + 1, end);
        
        const temp = [];
        let i = start, j = mid + 1, k = 0;
        
        while (i <= mid && j <= end) {
            if (nums[indices[j]] < nums[indices[i]]) {
                temp[k++] = indices[j++];
            } else {
                result[indices[i]] += j - mid - 1;
                temp[k++] = indices[i++];
            }
        }
        
        while (i <= mid) {
            result[indices[i]] += j - mid - 1;
            temp[k++] = indices[i++];
        }
        
        while (j <= end) {
            temp[k++] = indices[j++];
        }
        
        for (let p = start; p <= end; p++) {
            indices[p] = temp[p - start];
        }
    }
    
    mergeSort(0, nums.length - 1);
    return result;
}
// Time: O(n log n), Space: O(n)
```

### 8. Russian Doll Envelopes ⭐⭐⭐
**Problem**: Maximum number of envelopes you can Russian doll
```js
function maxEnvelopes(envelopes) {
    // Sort by width ascending, height descending
    envelopes.sort((a, b) => a[0] === b[0] ? b[1] - a[1] : a[0] - b[0]);
    
    // Find LIS on heights
    const heights = envelopes.map(env => env[1]);
    return lengthOfLIS(heights);
}

function lengthOfLIS(nums) {
    const dp = [];
    
    for (let num of nums) {
        let left = 0, right = dp.length;
        
        while (left < right) {
            const mid = Math.floor((left + right) / 2);
            if (dp[mid] < num) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        
        if (left === dp.length) {
            dp.push(num);
        } else {
            dp[left] = num;
        }
    }
    
    return dp.length;
}
// Time: O(n log n), Space: O(n)
```

## Missing Pattern Categories Added

### **Bucket Sort Pattern** (Maximum Gap)
### **Merge Sort with Index Tracking** (Count Smaller Numbers)
### **LIS with Binary Search** (Russian Doll Envelopes)  
### **Lexicographic Permutation** (Next Permutation)
### **Peak Finding with Binary Search** (Find Peak Element)
### **Union-Find Alternative** (Longest Consecutive Sequence)
### **Advanced Sliding Window** (Minimum Window Substring)

## Interview Tips

### Easy Level Focus
- Master basic array operations and two-pointer technique
- Practice with hash maps for O(1) lookups
- Understand in-place modifications

### Medium Level Focus
- Learn prefix sums and sliding window patterns
- Practice multiple pointer techniques
- Master sorting and binary search applications

### Hard Level Focus
- Study advanced data structures (stacks, deques)
- Practice divide and conquer approaches
- Master space optimization techniques

  
## Time Complexities
- Access: O(1)
- Search: O(n) unsorted, O(log n) sorted with binary search
- Insertion: O(1) at end, O(n) at beginning/middle
- Deletion: O(1) at end, O(n) at beginning/middle
- Sort: O(n log n)

## Space Optimization Tips
- Use two pointers to modify array in-place instead of creating new array
- Use index mapping instead of additional data structures when possible
- Consider using the array itself as a hash map for problems with limited range
