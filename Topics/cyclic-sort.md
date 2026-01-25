# Cyclic Sort Algorithm 🔄

## Introduction
Cyclic Sort is a pattern used to solve array problems where the array contains numbers in a given range. It's particularly efficient for problems involving arrays containing numbers from 1 to n, as it sorts the array in O(n) time without using extra space.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (array manipulation and indexing)
- Basic understanding of in-place algorithms

### Related Topics
- **Builds on**: [Arrays](arrays.md), index-value relationship concepts
- **Alternative to**: [Hashing](hashing.md) (for finding missing/duplicate numbers)
- **Related algorithms**: [Sorting](sorting.md) (comparison with other sorting methods)
- **See also**: [Linked List](linked-list.md) (Floyd's cycle detection uses similar ideas)

## Pattern Recognition Guide

### 🎯 When to Use Cyclic Sort
**Keywords in problem**: "numbers from 1 to n", "missing number", "duplicate number", "in range [1, n]"

**Use Cyclic Sort when you see**:
- Array contains numbers in range 1 to n (or 0 to n-1)
- Need to find missing or duplicate numbers
- O(1) space requirement with O(n) time
- Each number should appear at specific index

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Missing Number | "find missing", "one number missing", "disappeared" | Missing Number, Find Disappeared Numbers |
| Duplicate Number | "find duplicate", "one number repeats" | Find the Duplicate Number |
| Multiple Missing/Duplicates | "find all missing", "find all duplicates" | Find All Duplicates in Array |
| Corrupt Pair | "one missing, one duplicate", "set mismatch" | Set Mismatch |
| First Missing Positive | "first positive missing", "smallest positive" | First Missing Positive |

### ❌ When NOT to Use
- Numbers NOT in a continuous range → use [Hashing](hashing.md)
- Array contains negative numbers or zeros outside expected range
- Need to preserve original array (cyclic sort modifies in-place)
- Numbers can repeat more than once (except specific duplicate problems)

## Core Concepts

### Important Terminologies
- **Cyclic Sort**: Algorithm for sorting numbers in range 1 to n
- **In-place Swap**: Exchange elements without extra space
- **Index Matching**: Element value equals index + 1
- **Range Constraint**: Numbers limited to specific range
- **Missing Number**: Value absent from sequence
- **Duplicate Number**: Value appearing multiple times
- **Corruption**: When array violates range property
- **Natural Number**: Positive integers (1 to n)
- **Zero-based**: Array indices start from 0
- **One-based**: Numbers start from 1

### Properties of Cyclic Sort
1. **Time Efficiency**: O(n) for all cases
2. **Space Efficiency**: O(1) extra space
3. **Stability**: Not stable
4. **In-Place**: Yes
5. **Adaptive**: No
6. **Comparison-Based**: No

### Time Complexity Analysis
| Operation           | Time Complexity | Space Complexity |
|--------------------|----------------|------------------|
| Sorting            | O(n)           | O(1)             |
| Finding Duplicate  | O(n)           | O(1)             |
| Finding Missing    | O(n)           | O(1)             |
| Finding Multiple   | O(n)           | O(1)             |
| Corruption Check   | O(n)           | O(1)             |

## Implementation Patterns

### 1. Basic Cyclic Sort

**Pseudocode:**
```
1. Start at index i = 0
2. While i < array length:
   a. Calculate correct position: nums[i] - 1
   b. If nums[i] is in valid range AND nums[i] != nums[correct_pos]:
      - Swap nums[i] with nums[correct_pos]
   c. Else: move to next index (i++)
3. Each number ends up at index (value - 1)
```

```python
def cyclic_sort(nums):
    i = 0
    while i < len(nums):
        correct_pos = nums[i] - 1
        if nums[i] > 0 and nums[i] <= len(nums) and nums[i] != nums[correct_pos]:
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    return nums
```

```java
public void cyclicSort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correctPos = nums[i] - 1;
        if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[correctPos]) {
            int temp = nums[i]; nums[i] = nums[correctPos]; nums[correctPos] = temp;
        } else i++;
    }
}
```

```cpp
void cyclicSort(vector<int>& nums) {
    int i = 0;
    while (i < nums.size()) {
        int correctPos = nums[i] - 1;
        if (nums[i] > 0 && nums[i] <= nums.size() && nums[i] != nums[correctPos])
            swap(nums[i], nums[correctPos]);
        else i++;
    }
}
```

### 2. Find Missing Number

**Pseudocode:**
```
1. Apply cyclic sort: place each number at its correct index
2. After sorting, scan array:
   a. For each index i: if nums[i] != i, return i
3. If all positions correct, missing number is n (array length)
```

```python
def find_missing_number(nums):
    i = 0
    while i < len(nums):
        correct_pos = nums[i]
        if nums[i] < len(nums) and nums[i] != nums[correct_pos]:
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    
    # Find the missing number
    for i in range(len(nums)):
        if nums[i] != i:
            return i
    
    return len(nums)
```

```java
public int missingNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        if (nums[i] < nums.length && nums[i] != nums[nums[i]])
            { int t = nums[i]; nums[i] = nums[t]; nums[t] = t; }
        else i++;
    }
    for (i = 0; i < nums.length; i++) if (nums[i] != i) return i;
    return nums.length;
}
```

```cpp
int missingNumber(vector<int>& nums) {
    int i = 0;
    while (i < nums.size()) {
        if (nums[i] < nums.size() && nums[i] != nums[nums[i]])
            swap(nums[i], nums[nums[i]]);
        else i++;
    }
    for (i = 0; i < nums.size(); i++) if (nums[i] != i) return i;
    return nums.size();
}
```

### 3. Find Duplicate Number

**Pseudocode:**
```
1. Start at index i = 0
2. While i < array length:
   a. If nums[i] != i + 1:
      - Calculate correct position = nums[i] - 1
      - If nums[i] != nums[correct_pos]: swap them
      - Else: found duplicate, return nums[i]
   b. Else: move to next index (i++)
3. Return -1 if no duplicate found
```

```python
def find_duplicate(nums):
    i = 0
    while i < len(nums):
        if nums[i] != i + 1:
            correct_pos = nums[i] - 1
            if nums[i] != nums[correct_pos]:
                nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
            else:
                return nums[i]
        else:
            i += 1
    return -1
```

```java
public int findDuplicate(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        if (nums[i] != i + 1) {
            int correct = nums[i] - 1;
            if (nums[i] != nums[correct]) { int t = nums[i]; nums[i] = nums[correct]; nums[correct] = t; }
            else return nums[i];
        } else i++;
    }
    return -1;
}
```

```cpp
int findDuplicate(vector<int>& nums) {
    int i = 0;
    while (i < nums.size()) {
        if (nums[i] != i + 1) {
            int correct = nums[i] - 1;
            if (nums[i] != nums[correct]) swap(nums[i], nums[correct]);
            else return nums[i];
        } else i++;
    }
    return -1;
}
```

### 4. Find All Missing Numbers

**Pseudocode:**
```
1. Apply cyclic sort to place numbers at correct positions
2. Create empty list for missing numbers
3. Scan array: for each index i:
   a. If nums[i] != i + 1: add (i + 1) to missing list
4. Return missing numbers list
```

```python
def find_missing_numbers(nums):
    i = 0
    while i < len(nums):
        correct_pos = nums[i] - 1
        if nums[i] != nums[correct_pos]:
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    
    missing_numbers = []
    for i in range(len(nums)):
        if nums[i] != i + 1:
            missing_numbers.append(i + 1)
    
    return missing_numbers
```

```java
public List<Integer> findDisappearedNumbers(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correct = nums[i] - 1;
        if (nums[i] != nums[correct]) { int t = nums[i]; nums[i] = nums[correct]; nums[correct] = t; }
        else i++;
    }
    List<Integer> missing = new ArrayList<>();
    for (i = 0; i < nums.length; i++) if (nums[i] != i + 1) missing.add(i + 1);
    return missing;
}
```

```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {
    int i = 0;
    while (i < nums.size()) {
        int correct = nums[i] - 1;
        if (nums[i] != nums[correct]) swap(nums[i], nums[correct]);
        else i++;
    }
    vector<int> missing;
    for (i = 0; i < nums.size(); i++) if (nums[i] != i + 1) missing.push_back(i + 1);
    return missing;
}
```

### 5. Find First Missing Positive

**Pseudocode:**
```
1. Apply cyclic sort (only for positive numbers in range 1 to n):
   a. If 0 < nums[i] <= n AND nums[i] != nums[correct_pos]: swap
2. Scan array: for each index i:
   a. If nums[i] != i + 1: return i + 1 (first missing positive)
3. If all positions correct, return n + 1
```

```python
def first_missing_positive(nums):
    i = 0
    while i < len(nums):
        correct_pos = nums[i] - 1
        if 0 < nums[i] <= len(nums) and nums[i] != nums[correct_pos]:
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return i + 1
    
    return len(nums) + 1
```

```java
public int firstMissingPositive(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correct = nums[i] - 1;
        if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[correct]) {
            int t = nums[i]; nums[i] = nums[correct]; nums[correct] = t;
        } else i++;
    }
    for (i = 0; i < nums.length; i++) if (nums[i] != i + 1) return i + 1;
    return nums.length + 1;
}
```

```cpp
int firstMissingPositive(vector<int>& nums) {
    int i = 0;
    while (i < nums.size()) {
        int correct = nums[i] - 1;
        if (nums[i] > 0 && nums[i] <= nums.size() && nums[i] != nums[correct])
            swap(nums[i], nums[correct]);
        else i++;
    }
    for (i = 0; i < nums.size(); i++) if (nums[i] != i + 1) return i + 1;
    return nums.size() + 1;
}
```

## Common Techniques

### 1. Index Mapping

**Pseudocode:**
```
1. For each index i:
   a. While nums[i] != i + 1 AND nums[i] is in valid range:
      - Calculate correct position = nums[i] - 1
      - If nums[i] != nums[correct_pos]: swap them
      - Else: break (duplicate detected)
```

```python
def index_mapping_sort(nums):
    """Sort array where each number should be at index (number-1)"""
    for i in range(len(nums)):
        while nums[i] != i + 1 and 1 <= nums[i] <= len(nums):
            correct_pos = nums[i] - 1
            if nums[i] != nums[correct_pos]:
                nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
            else:
                break
```

```java
public void indexMappingSort(int[] nums) {
    for (int i = 0; i < nums.length; i++) {
        while (nums[i] != i + 1 && nums[i] >= 1 && nums[i] <= nums.length) {
            int correct = nums[i] - 1;
            if (nums[i] != nums[correct]) { int t = nums[i]; nums[i] = nums[correct]; nums[correct] = t; }
            else break;
        }
    }
}
```

```cpp
void indexMappingSort(vector<int>& nums) {
    for (int i = 0; i < nums.size(); i++) {
        while (nums[i] != i + 1 && nums[i] >= 1 && nums[i] <= nums.size()) {
            int correct = nums[i] - 1;
            if (nums[i] != nums[correct]) swap(nums[i], nums[correct]);
            else break;
        }
    }
}
```

### 2. Corruption Detection

**Pseudocode:**
```
Find duplicate and missing number:
1. Apply cyclic sort to place numbers at correct positions
2. Scan array: for each index i:
   a. If nums[i] != i + 1:
      - Duplicate = nums[i] (number at wrong position)
      - Missing = i + 1 (number that should be here)
      - Return [duplicate, missing]
```

```python
def find_corrupt_pair(nums):
    """Find the duplicate and missing number"""
    i = 0
    while i < len(nums):
        correct_pos = nums[i] - 1
        if nums[i] != nums[correct_pos]:
            nums[i], nums[correct_pos] = nums[correct_pos], nums[i]
        else:
            i += 1
    
    for i in range(len(nums)):
        if nums[i] != i + 1:
            return [nums[i], i + 1]
    
    return [-1, -1]
```

```java
public int[] findCorruptPair(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correct = nums[i] - 1;
        if (nums[i] != nums[correct]) { int t = nums[i]; nums[i] = nums[correct]; nums[correct] = t; }
        else i++;
    }
    for (i = 0; i < nums.length; i++) {
        if (nums[i] != i + 1) return new int[]{nums[i], i + 1};
    }
    return new int[]{-1, -1};
}
```

```cpp
vector<int> findCorruptPair(vector<int>& nums) {
    int i = 0;
    while (i < nums.size()) {
        int correct = nums[i] - 1;
        if (nums[i] != nums[correct]) swap(nums[i], nums[correct]);
        else i++;
    }
    for (i = 0; i < nums.size(); i++) {
        if (nums[i] != i + 1) return {nums[i], i + 1};
    }
    return {-1, -1};
}
```

## Edge Cases to Consider
1. Empty array
2. Single element array
3. All elements same
4. Already sorted array
5. Reverse sorted array
6. Array with duplicates
7. Array with missing numbers
8. Numbers out of range
9. Negative numbers
10. Maximum value scenarios

## Common Pitfalls
1. Infinite loop in swapping
2. Not handling duplicates
3. Off-by-one errors
4. Not checking array bounds
5. Incorrect position calculation
6. Modifying input when not allowed
7. Not handling out-of-range values
8. Incorrect range assumptions

## Practice Problems by Difficulty

### Easy
1. [Missing Number](https://leetcode.com/problems/missing-number/) (LC #268)
2. [Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/) (LC #448)
3. [Set Mismatch](https://leetcode.com/problems/set-mismatch/) (LC #645)

### Medium
1. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) (LC #287)
2. [Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/) (LC #442)
3. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) (LC #41)

### Hard
1. [Couples Holding Hands](https://leetcode.com/problems/couples-holding-hands/) (LC #765)
2. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) (LC #41)
3. [N-Queens](https://leetcode.com/problems/n-queens/) (LC #51)

## Real-World Applications
1. **Error Detection**:
   - Finding corrupted data
   - Validating sequences
2. **Data Validation**:
   - Checking completeness
   - Identifying duplicates
3. **Resource Management**:
   - Memory allocation
   - Process scheduling
4. **Database Systems**:
   - Record deduplication
   - Data integrity checks
5. **Network Protocols**:
   - Packet sequence validation
   - Missing packet detection

## Advanced Topics
1. **Variants of Cyclic Sort**:
   - Multi-range sort
   - Negative number handling
2. **Optimization Techniques**:
   - Cache efficiency
   - Branch reduction
3. **Related Algorithms**:
   - Counting sort
   - Radix sort
4. **Applications in Other Algorithms**:
   - Graph cycle detection
   - Linked list cycle finding

## Important Resources
1. [Cyclic Sort Pattern](https://www.educative.io/courses/grokking-coding-interview-patterns-java/qVVNDRqLZZA)
2. [In-place Sorting](https://www.geeksforgeeks.org/in-place-algorithm/)
3. [Array Rearrangement](https://www.geeksforgeeks.org/rearrange-array-arrj-becomes-arri-j/)
4. [Missing Number Problems](https://leetcode.com/problems/missing-number/solution/)
5. [Duplicate Detection](https://leetcode.com/problems/find-the-duplicate-number/solution/)

## ❓ FAQ Section

**Q: When should I use cyclic sort?**
A: Use cyclic sort when: (1) Array contains numbers in range [1, n] or [0, n-1], (2) You need O(1) space, (3) Problem asks for missing/duplicate numbers. Keywords: "numbers in range 1 to n", "missing number", "duplicate in array".

**Q: What's the key insight of cyclic sort?**
A: Each number should be at its "correct" index (number i at index i-1, or number i at index i). Swap each number to its correct position. After sorting, any number at wrong position reveals the missing/duplicate.

**Q: How do I handle duplicates in cyclic sort?**
A: When swapping, if the number at the target position equals the current number, it's a duplicate - skip the swap and move to the next index. The duplicate will be detected when we find misplaced elements.

**Q: What's the time complexity of cyclic sort?**
A: O(n). Although there's a nested while loop, each number is swapped at most once to its correct position. Total swaps ≤ n, so overall O(n) despite the nested loop appearance.

**Q: Can cyclic sort work for numbers outside [1,n] range?**
A: Partially. For finding first missing positive, ignore numbers ≤ 0 or > n (they can't be the answer). Place valid numbers at correct positions. First position with wrong number gives the answer.

## Interview Tips
1. Identify range constraints
2. Consider space requirements
3. Handle edge cases explicitly
4. Draw array state changes
5. Test with small examples
6. Consider optimization opportunities
7. Discuss time/space trade-offs
8. Handle invalid inputs
9. Consider follow-up questions
10. Think about scalability