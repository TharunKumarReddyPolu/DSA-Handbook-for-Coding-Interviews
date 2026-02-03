# Arrays in Data Structures and Algorithms 📌

## Introduction
Arrays are fundamental data structures that store elements in contiguous memory locations. They are the building blocks for many other data structures and algorithms.

## Prerequisites & Related Topics

### Prerequisites
- Basic programming concepts (variables, loops, functions)
- Understanding of memory and indexing

### Related Topics
- **Builds on**: Basic programming fundamentals
- **Used in**: [Hashing](hashing.md), [Sorting](sorting.md), [Matrix](matrix.md), [Heap](heap-pq.md), [Stack](stack.md), [Queue](queue.md)
- **Techniques often combined**: Two Pointers, Sliding Window, Prefix Sum
- **See also**: [Strings](strings.md) (character arrays), [Linked List](linked-list.md) (alternative linear structure)

## Pattern Recognition Guide

### 🎯 When to Use Arrays
**Keywords in problem**: "contiguous", "subarray", "consecutive", "in-place", "rotate", "reverse"

**Use Arrays when you see**:
- Need O(1) random access by index
- Working with fixed-size collections
- Problems involving subarrays or subsequences
- Sliding window problems (fixed or variable size)
- Two pointer problems on sorted data
- Prefix/suffix computations

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Two Pointers | "sorted array", "pair with sum", "remove duplicates" | Two Sum II, Container With Most Water |
| Sliding Window | "subarray of size k", "longest/shortest substring", "maximum sum" | Max Sum Subarray, Minimum Window |
| Prefix Sum | "range sum", "cumulative", "sum between indices" | Range Sum Query, Subarray Sum Equals K |
| Kadane's | "maximum subarray", "contiguous sum" | Maximum Subarray |
| In-place | "O(1) space", "modify in place", "without extra space" | Rotate Array, Move Zeroes |

### ❌ When NOT to Use
- Frequent insertions/deletions in middle → use [Linked List](linked-list.md)
- Need fast lookups by value → use [Hashing](hashing.md)
- Data is hierarchical → use [Tree](tree.md)
- Need sorted order with dynamic insertions → use [Heap](heap-pq.md)

## Core Concepts

### Important Terminologies
- **Array**: A collection of elements stored at contiguous memory locations
- **Subarray**: Contiguous elements within an array (e.g., [1,2,3] in [1,2,3,4])
- **Subsequence**: Non-contiguous elements maintaining relative order (e.g., [1,3,4] in [1,2,3,4])
- **Bitonic Array**: Array which first increases then decreases (e.g., [1,3,5,4,2])
- **Circular Array**: Array where we consider the first element as next to the last element

### Time Complexity Analysis
| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Access    | O(1)         | O(1)       |
| Search    | O(n)         | O(n)       |
| Insertion | O(n)         | O(n)       |
| Deletion  | O(n)         | O(n)       |
| Sort      | O(n log n)   | O(n²)      |

### Space Complexity
- **Linear Arrays**: O(n) where n is the size of array
- **2D Arrays**: O(m×n) where m and n are dimensions

## Common Techniques

### 1. Two Pointer Technique
Used when dealing with sorted arrays or pairs in array.

**Pseudocode:**
```
1. Initialize two pointers: left at start (index 0), right at end (last index)
2. While left pointer is less than right pointer:
   a. Calculate current sum = element at left + element at right
   b. If current sum equals target, return the indices [left, right]
   c. If current sum is less than target, move left pointer right (left++)
   d. If current sum is greater than target, move right pointer left (right--)
3. If no pair found, return empty result
```

```python
def twoSum(nums, target):
    left, right = 0, len(nums) - 1
    while left < right:
        curr_sum = nums[left] + nums[right]
        if curr_sum == target:
            return [left, right]
        elif curr_sum < target:
            left += 1
        else:
            right -= 1
    return []
```

```java
public int[] twoSum(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) return new int[]{left, right};
        else if (sum < target) left++;
        else right--;
    }
    return new int[]{};
}
```

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) return {left, right};
        else if (sum < target) left++;
        else right--;
    }
    return {};
}
```

### 2. Sliding Window
Useful for problems involving contiguous subarrays.

🔹 Fixed Size Sliding Window
Used when the window size `k` is constant.
**Pseudocode:**
```
1. Calculate the sum of first k elements (initial window)
2. Set max_sum = initial window sum
3. Slide the window from left to right:
   a. For each position i from k to end of array:
      - Subtract the element leaving the window (leftmost element)
      - Add the element entering the window (rightmost element)
      - Update max_sum if current window sum is greater
4. Return max_sum
```

```python
def maxSumSubarray(nums, k):
    window_sum = sum(nums[:k])
    max_sum = window_sum
    
    for i in range(len(nums) - k):
        window_sum = window_sum - nums[i] + nums[i + k]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

```java
public int maxSumSubarray(int[] nums, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) windowSum += nums[i];
    int maxSum = windowSum;
    for (int i = k; i < nums.length; i++) {
        windowSum += nums[i] - nums[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

```cpp
int maxSumSubarray(vector<int>& nums, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) windowSum += nums[i];
    int maxSum = windowSum;
    for (int i = k; i < nums.size(); i++) {
        windowSum += nums[i] - nums[i - k];
        maxSum = max(maxSum, windowSum);
    }
    return maxSum;
}
```
🔹 Variable Size Sliding Window

Used when the window size is dynamic and depends on a condition.

**Core Idea:**

Expand the window using the right pointer.
If the window violates the condition, shrink it using the left pointer.
We shrink the window whenever a duplicate character is found, and continue shrinking until the window becomes valid again.
Continue until the window becomes valid again.
Update the result during valid states.

**Pseudocode:**
1. Initialize left = 0
2. For each right from 0 to n-1:
   a. Add element at right to the window
   b. While window violates condition:
      - Remove element at left from window
      - Increment left
   c. Update answer
3. Return result

**Time Complexity:** O(n)  
Each element is added to the window once and removed at most once.

**Space Complexity:** O(n)  
In worst case, the set stores all unique characters.

```python
def lengthOfLongestSubstring(s):
    char_set = set()
    left = 0
    max_length = 0

    for right in range(len(s)):
        while s[right] in char_set:
            char_set.remove(s[left])
            left += 1
        
        char_set.add(s[right])
        max_length = max(max_length, right - left + 1)

    return max_length
```

```cpp
int lengthOfLongestSubstring(string s) {
    unordered_set<char> charSet;
    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {
        while (charSet.count(s[right])) {
            charSet.erase(s[left]);
            left++;
        }

        charSet.insert(s[right]);
        maxLength = max(maxLength, right - left + 1);
    }

    return maxLength;
}
```

```java
public int lengthOfLongestSubstring(String s) {
    HashSet<Character> set = new HashSet<>();
    int left = 0;
    int maxLength = 0;

    for (int right = 0; right < s.length(); right++) {
        while (set.contains(s.charAt(right))) {
            set.remove(s.charAt(left));
            left++;
        }

        set.add(s.charAt(right));
        maxLength = Math.max(maxLength, right - left + 1);
    }

    return maxLength;
}
```
#### Fixed vs Variable Sliding Window

| Feature | Fixed Size | Variable Size |
|----------|------------|---------------|
| Window size | Constant (k) | Dynamic |
| Trigger | Size based | Condition based |
| Example | Max sum of size k | Longest substring without repeating characters |

### 3. Prefix Sum
Optimal for range queries and cumulative computations.

**Pseudocode:**
```
1. Create a prefix array of size (n + 1), initialized with zeros
2. For each index i from 0 to n-1:
   a. prefix[i + 1] = prefix[i] + nums[i]
   (Each position stores sum of all elements from start up to that index)
3. Return the prefix array
   (To get sum of range [i, j]: prefix[j+1] - prefix[i])
```

```python
def buildPrefixSum(nums):
    prefix = [0] * (len(nums) + 1)
    for i in range(len(nums)):
        prefix[i + 1] = prefix[i] + nums[i]
    return prefix
```

```java
public int[] buildPrefixSum(int[] nums) {
    int[] prefix = new int[nums.length + 1];
    for (int i = 0; i < nums.length; i++) {
        prefix[i + 1] = prefix[i] + nums[i];
    }
    return prefix;
}
```

```cpp
vector<int> buildPrefixSum(vector<int>& nums) {
    vector<int> prefix(nums.size() + 1, 0);
    for (int i = 0; i < nums.size(); i++) {
        prefix[i + 1] = prefix[i] + nums[i];
    }
    return prefix;
}
```

### 4. Kadane's Algorithm
For maximum subarray problems.

**Pseudocode:**
```
1. Initialize max_sum and current_sum with first element
2. For each element from index 1 to end:
   a. current_sum = maximum of (current element, current_sum + current element)
      (Decide: start fresh from current element OR extend previous subarray)
   b. max_sum = maximum of (max_sum, current_sum)
      (Track the best sum seen so far)
3. Return max_sum
```

```python
def kadane(nums):
    max_sum = current_sum = nums[0]
    for num in nums[1:]:
        current_sum = max(num, current_sum + num)
        max_sum = max(max_sum, current_sum)
    return max_sum
```

```java
public int kadane(int[] nums) {
    int maxSum = nums[0], currentSum = nums[0];
    for (int i = 1; i < nums.length; i++) {
        currentSum = Math.max(nums[i], currentSum + nums[i]);
        maxSum = Math.max(maxSum, currentSum);
    }
    return maxSum;
}
```

```cpp
int kadane(vector<int>& nums) {
    int maxSum = nums[0], currentSum = nums[0];
    for (int i = 1; i < nums.size(); i++) {
        currentSum = max(nums[i], currentSum + nums[i]);
        maxSum = max(maxSum, currentSum);
    }
    return maxSum;
}
```

## Edge Cases to Consider
- Empty array
- Array with single element
- Array with all negative values
- Array with duplicates
- Array with overflow potential
- Sorted vs unsorted arrays
- Circular array scenarios

## Common Pitfalls
1. Off-by-one errors in array indexing
2. Not handling array bounds properly
3. Assuming array is sorted when it's not
4. Ignoring integer overflow
5. Not considering memory constraints for large arrays

## Practice Problems by Difficulty

### Easy
1. [Two Sum](https://leetcode.com/problems/two-sum/) (LC #1)
2. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) (LC #121)
3. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) (LC #217)

### Medium
1. [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) (LC #53)
2. [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) (LC #438)
3. [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) (LC #11)
4. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) (LC #560)

### Hard
1. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) (LC #41)
2. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) (LC #4)
3. [Maximum Rectangle](https://leetcode.com/problems/maximal-rectangle/) (LC #85)

## Real-World Applications
1. **Image Processing**: Pixels in images are stored as 2D arrays
2. **Database Systems**: Array-based indices for faster lookups
3. **Memory Management**: Contiguous memory allocation
4. **Video Games**: Collision detection using grid arrays
5. **Scientific Computing**: Matrix operations in numerical analysis

## Advanced Topics
1. **Dynamic Arrays**: Implementation and amortized analysis
2. **Sparse Arrays**: Efficient storage for arrays with many default values
3. **Array-based Data Structures**: 
   - Heap
   - Hash Table
   - Stack
   - Queue

## Important Resources
1. [Python Arrays Documentation](https://docs.python.org/3/library/array.html)
2. [Python bisect module](https://docs.python.org/3/library/bisect.html)
3. [Sliding Window Technique](https://leetcode.com/problems/minimum-size-subarray-sum/solution/)
4. [Prefix Sum Array Tutorial](https://www.geeksforgeeks.org/prefix-sum-array-implementation-applications-competitive-programming/)
5. [Two Pointer Technique](https://leetcode.com/articles/two-pointer-technique/)

## ❓ FAQ Section

**Q: When should I use Two Pointers vs Sliding Window?**
A: Use Two Pointers when you need to find pairs or compare elements from both ends (e.g., sorted array, palindrome). Use Sliding Window for problems involving contiguous subarrays of fixed or variable size (e.g., max sum subarray, longest substring).

**Q: How do I decide between O(n²) brute force and O(n) optimized solution?**
A: Check constraints. If n ≤ 1000, O(n²) is usually acceptable. For n ≤ 10⁵ or 10⁶, you need O(n) or O(n log n). Always start with brute force to understand the problem, then optimize.

**Q: What's the difference between subarray and subsequence?**
A: Subarray is contiguous (elements must be adjacent), e.g., [2,3] in [1,2,3,4]. Subsequence maintains relative order but doesn't need to be contiguous, e.g., [1,3] in [1,2,3,4].

**Q: How do I handle array index out of bounds errors?**
A: Always validate indices before accessing. Use conditions like `if (i >= 0 && i < n)`. For sliding windows, ensure `right < n` before expansion and `left <= right` during contraction.

**Q: When should I sort the array first?**
A: Sort when the problem involves finding pairs/triplets with target sum, removing duplicates, or when ordering doesn't affect the answer but enables efficient algorithms like binary search or two pointers.

## Interview Tips
1. Always clarify input constraints and array properties
2. Consider time/space complexity requirements
3. Look for sorting requirements or if input is already sorted
4. Consider using standard library functions when appropriate
5. Test your solution with edge cases before finalizing