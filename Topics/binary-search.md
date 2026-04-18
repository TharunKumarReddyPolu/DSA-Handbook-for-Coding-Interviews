# Binary Search 🔍

## Introduction
Binary Search is a divide-and-conquer algorithm that finds the position of a target value within a **sorted** (or monotonic) sequence in O(log n) time. At each step it eliminates half of the remaining search space by comparing the target with the middle element. Beyond searching sorted arrays, the same idea powers a broader pattern called **"binary search on the answer"**, used to solve optimization problems whose feasibility function is monotonic.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (indexing, bounds)
- [Sorting](sorting.md) (binary search requires sorted or monotonic input)
- Basic understanding of loop invariants and O(log n) complexity
- [Recursion](recursion.md) (optional — for the recursive formulation)

### Related Topics
- **Builds on**: [Arrays](arrays.md), [Sorting](sorting.md)
- **Used by**: [Dynamic Programming](dynamic-programming.md) (LIS with BS, DP + BS), [Greedy](greedy.md) (parametric search), [Matrix](matrix.md) (search in sorted matrix), [Intervals](intervals.md)
- **Enables**: O(log n) lookups, search-on-answer, rotated-array queries
- **See also**: [Tree](tree.md) (Binary Search Tree — different topic), [Hashing](hashing.md) (O(1) lookup alternative for unordered data)

## Pattern Recognition Guide

### 🎯 When to Use Binary Search
**Keywords in problem**: "sorted array", "find / search", "O(log n)", "monotonic", "minimum / maximum value such that…", "smallest / largest that satisfies", "first / last occurrence", "peak", "rotated sorted array", "find in matrix sorted row & column"

**Use Binary Search when you see**:
- Input is sorted (or can be sorted cheaply) and you need a fast lookup
- The answer lies in a numeric range and the predicate `canDo(x)` is **monotonic** (if `canDo(x)` is true, then `canDo(x+1)` is also true — or vice versa)
- "Minimize the maximum" / "Maximize the minimum" style optimization problems
- Need to find boundary: first element ≥ target, last element ≤ target, insertion point
- Rotated sorted array, bitonic array, or a "peak" element
- Problem has O(n) brute force but constraints demand O(n log n) or O(log n)

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Template |
|---------|----------------|----------|
| Exact match in sorted array | "search", "find index of target" | Standard BS |
| First true / last true | "first occurrence", "leftmost", "lower bound" | `lower_bound` template |
| Last true / upper bound | "last occurrence", "rightmost", "insertion point" | `upper_bound` template |
| Rotated sorted array | "rotated", "pivot" | Modified BS (compare with ends) |
| Peak element | "peak", "local maximum", "mountain array" | BS on slope direction |
| Search on answer | "minimum capacity", "smallest days", "split array" | BS on value range + feasibility check |
| 2D sorted matrix | "each row sorted", "staircase" | BS on flattened index / staircase from corner |
| Square root / Nth root | "integer sqrt", "kth root" | BS on answer range |

### ❌ When NOT to Use
- Input is unsorted and sorting costs more than solving the problem (often hashing is O(n))
- Data is dynamic with frequent inserts/deletes → use a balanced BST / heap / skip list instead
- The predicate is **not monotonic** — binary search will silently give a wrong answer
- Very small arrays (n ≤ 20); linear scan has less overhead
- You need all occurrences, not just a boundary → scan after finding one boundary

## Core Concepts

### Important Terminologies
- **Search Space**: The inclusive range `[lo, hi]` still being considered
- **Invariant**: A property that must stay true across iterations (e.g., "target, if it exists, is in `[lo, hi]`")
- **Midpoint**: `mid = lo + (hi - lo) / 2` — avoids integer overflow vs `(lo + hi) / 2`
- **Lower Bound**: Smallest index `i` such that `arr[i] >= target`
- **Upper Bound**: Smallest index `i` such that `arr[i] > target`
- **Monotonic Predicate**: A function `f(x)` that transitions from false to true (or true to false) exactly once across the search space
- **Binary Search on Answer (Parametric Search)**: Binary search over the *value range* of a possible answer, using a feasibility check
- **Rotated Sorted Array**: A sorted array rotated at an unknown pivot (e.g., `[4,5,6,7,0,1,2]`)

### Time and Space Complexity Analysis
| Operation | Time | Space (Iterative) | Space (Recursive) |
|-----------|------|-------------------|-------------------|
| Search in sorted array | O(log n) | O(1) | O(log n) stack |
| Lower / upper bound | O(log n) | O(1) | O(log n) |
| Rotated array search | O(log n) | O(1) | O(log n) |
| Search in m×n sorted matrix (each row sorted, first of row > last of previous) | O(log(m·n)) | O(1) | — |
| Search in m×n row-&-column sorted matrix (staircase) | O(m + n) | O(1) | — |
| Binary search on answer | O(log(range) · f(n)) where f(n) is feasibility cost | O(1) | — |

## Implementation

### 1. Standard Binary Search (Exact Match)

**Pseudocode:**
```
1. lo = 0, hi = n - 1
2. While lo <= hi:
   a. mid = lo + (hi - lo) / 2
   b. If arr[mid] == target: return mid
   c. If arr[mid] <  target: lo = mid + 1
   d. Else:                  hi = mid - 1
3. Return -1 (not found)
```

```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

```java
public int binarySearch(int[] arr, int target) {
    int lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

```cpp
int binarySearch(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

### 2. Lower Bound (First Index with `arr[i] >= target`)

**Pseudocode:**
```
1. lo = 0, hi = n   (note: hi = n, half-open interval)
2. While lo < hi:
   a. mid = lo + (hi - lo) / 2
   b. If arr[mid] < target: lo = mid + 1
   c. Else:                 hi = mid
3. Return lo   (insertion point; may equal n if target > all)
```

```python
def lower_bound(arr, target):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid
    return lo
```

```java
public int lowerBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

```cpp
int lowerBound(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] < target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
    // Equivalent STL: lower_bound(arr.begin(), arr.end(), target) - arr.begin();
}
```

### 3. Upper Bound (First Index with `arr[i] > target`)

**Pseudocode:**
```
Same as lower_bound, but use arr[mid] <= target on the left branch.
Count of target in sorted array = upper_bound(t) - lower_bound(t).
```

```python
def upper_bound(arr, target):
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] <= target:
            lo = mid + 1
        else:
            hi = mid
    return lo
```

```java
public int upperBound(int[] arr, int target) {
    int lo = 0, hi = arr.length;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] <= target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
}
```

```cpp
int upperBound(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size();
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] <= target) lo = mid + 1;
        else hi = mid;
    }
    return lo;
    // Equivalent STL: upper_bound(arr.begin(), arr.end(), target) - arr.begin();
}
```

### 4. Search in Rotated Sorted Array (No Duplicates)

**Pseudocode:**
```
1. lo = 0, hi = n - 1
2. While lo <= hi:
   a. mid = lo + (hi - lo) / 2
   b. If arr[mid] == target: return mid
   c. Decide which half is sorted by comparing arr[lo] and arr[mid]:
      - If arr[lo] <= arr[mid]:           # left half is sorted
          if arr[lo] <= target < arr[mid]: hi = mid - 1
          else:                             lo = mid + 1
      - Else:                               # right half is sorted
          if arr[mid] < target <= arr[hi]: lo = mid + 1
          else:                             hi = mid - 1
3. Return -1
```

```python
def search_rotated(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] == target:
            return mid
        if arr[lo] <= arr[mid]:
            if arr[lo] <= target < arr[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        else:
            if arr[mid] < target <= arr[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
    return -1
```

```java
public int searchRotated(int[] arr, int target) {
    int lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        if (arr[lo] <= arr[mid]) {
            if (arr[lo] <= target && target < arr[mid]) hi = mid - 1;
            else lo = mid + 1;
        } else {
            if (arr[mid] < target && target <= arr[hi]) lo = mid + 1;
            else hi = mid - 1;
        }
    }
    return -1;
}
```

```cpp
int searchRotated(vector<int>& arr, int target) {
    int lo = 0, hi = (int)arr.size() - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] == target) return mid;
        if (arr[lo] <= arr[mid]) {
            if (arr[lo] <= target && target < arr[mid]) hi = mid - 1;
            else lo = mid + 1;
        } else {
            if (arr[mid] < target && target <= arr[hi]) lo = mid + 1;
            else hi = mid - 1;
        }
    }
    return -1;
}
```

### 5. Find Peak Element

**Pseudocode:**
```
Array where arr[i] != arr[i+1]; -inf on both outer sides.
1. lo = 0, hi = n - 1
2. While lo < hi:
   a. mid = lo + (hi - lo) / 2
   b. If arr[mid] > arr[mid + 1]: hi = mid       # peak on left (inclusive)
      Else:                       lo = mid + 1   # peak on right
3. Return lo
```

```python
def find_peak(arr):
    lo, hi = 0, len(arr) - 1
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if arr[mid] > arr[mid + 1]:
            hi = mid
        else:
            lo = mid + 1
    return lo
```

```java
public int findPeak(int[] arr) {
    int lo = 0, hi = arr.length - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] > arr[mid + 1]) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}
```

```cpp
int findPeak(vector<int>& arr) {
    int lo = 0, hi = (int)arr.size() - 1;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (arr[mid] > arr[mid + 1]) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}
```

## Common Techniques

### 1. Binary Search on the Answer (Parametric Search)

**Pseudocode:**
```
Use when:
  - Answer is a number in [lo_val, hi_val]
  - feasible(x) is monotonic (e.g., false ... false true ... true)
  - You want the smallest / largest x for which feasible(x) is true

1. lo = lo_val, hi = hi_val
2. While lo < hi:
   a. mid = lo + (hi - lo) / 2
   b. If feasible(mid): hi = mid       # try to tighten toward smaller
      Else:             lo = mid + 1
3. Return lo

(For "largest x such that feasible(x)": mirror the comparisons.)
```

**Example: Koko Eating Bananas (LC #875)** — find the smallest eating speed `k` so Koko finishes in ≤ `h` hours.

```python
import math

def min_eating_speed(piles, h):
    def hours_needed(k):
        return sum(math.ceil(p / k) for p in piles)

    lo, hi = 1, max(piles)
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if hours_needed(mid) <= h:
            hi = mid
        else:
            lo = mid + 1
    return lo
```

```java
public int minEatingSpeed(int[] piles, int h) {
    int lo = 1, hi = 0;
    for (int p : piles) hi = Math.max(hi, p);
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        long hours = 0;
        for (int p : piles) hours += (p + mid - 1) / mid;
        if (hours <= h) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}
```

```cpp
int minEatingSpeed(vector<int>& piles, int h) {
    int lo = 1, hi = *max_element(piles.begin(), piles.end());
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        long long hours = 0;
        for (int p : piles) hours += (p + mid - 1) / mid;
        if (hours <= h) hi = mid;
        else lo = mid + 1;
    }
    return lo;
}
```

### 2. Integer Square Root

**Pseudocode:**
```
Find largest x such that x*x <= n.
1. lo = 0, hi = n
2. While lo < hi:
   a. mid = lo + (hi - lo + 1) / 2   # upper mid to avoid infinite loop
   b. If mid*mid <= n: lo = mid
      Else:            hi = mid - 1
3. Return lo
```

```python
def int_sqrt(n):
    if n < 2:
        return n
    lo, hi = 0, n
    while lo < hi:
        mid = lo + (hi - lo + 1) // 2
        if mid * mid <= n:
            lo = mid
        else:
            hi = mid - 1
    return lo
```

```java
public int intSqrt(int n) {
    if (n < 2) return n;
    long lo = 0, hi = n;
    while (lo < hi) {
        long mid = lo + (hi - lo + 1) / 2;
        if (mid * mid <= n) lo = mid;
        else hi = mid - 1;
    }
    return (int) lo;
}
```

```cpp
int intSqrt(int n) {
    if (n < 2) return n;
    long long lo = 0, hi = n;
    while (lo < hi) {
        long long mid = lo + (hi - lo + 1) / 2;
        if (mid * mid <= n) lo = mid;
        else hi = mid - 1;
    }
    return (int) lo;
}
```

### 3. Search in a 2D Sorted Matrix (Each Row Sorted, First-of-Row > Last-of-Previous)

**Pseudocode:**
```
Treat the m x n matrix as a flattened sorted array of length m*n.
Index i maps to (i / n, i % n).
Run standard binary search on [0, m*n - 1].
```

```python
def search_matrix(matrix, target):
    if not matrix or not matrix[0]:
        return False
    m, n = len(matrix), len(matrix[0])
    lo, hi = 0, m * n - 1
    while lo <= hi:
        mid = lo + (hi - lo) // 2
        val = matrix[mid // n][mid % n]
        if val == target:
            return True
        elif val < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return False
```

```java
public boolean searchMatrix(int[][] matrix, int target) {
    int m = matrix.length, n = matrix[0].length;
    int lo = 0, hi = m * n - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        int val = matrix[mid / n][mid % n];
        if (val == target) return true;
        else if (val < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return false;
}
```

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size(), n = matrix[0].size();
    int lo = 0, hi = m * n - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        int val = matrix[mid / n][mid % n];
        if (val == target) return true;
        else if (val < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return false;
}
```

## Edge Cases to Consider
1. Empty array → return `-1` / `0` / `false` immediately
2. Single-element array (check both equal and not-equal paths)
3. Target smaller than all elements (lower_bound returns `0`)
4. Target larger than all elements (lower_bound returns `n`)
5. Target equal to first or last element
6. Array with duplicates — do you want *any*, *first*, or *last* index?
7. Rotated array where rotation = 0 (fully sorted) or rotation = n (same as sorted)
8. Rotated array with duplicates → worst case O(n)
9. Negative numbers and zero in the search range
10. For "binary search on answer": ensure the predicate is actually monotonic
11. For integer square root / integer division: watch out for overflow in `mid * mid`
12. Floating-point binary search: use iteration count (e.g., 100 iters) or epsilon, not `lo < hi`

## Common Pitfalls
1. **Using `(lo + hi) / 2`**: can overflow for large `lo`, `hi`. Use `lo + (hi - lo) / 2`.
2. **Infinite loop from wrong mid rounding**: when you use `lo = mid` (instead of `mid + 1`), you must use the upper mid `lo + (hi - lo + 1) / 2` — otherwise `lo == hi - 1` loops forever.
3. **Inconsistent interval convention**: mixing inclusive `[lo, hi]` and half-open `[lo, hi)`. Pick one and keep the invariant consistent.
4. **Wrong loop condition**: `lo <= hi` vs `lo < hi` must match your interval convention and update rule.
5. **Off-by-one after the loop**: returning `lo` vs `lo - 1` for lower-bound-style searches.
6. **Non-monotonic predicate** in binary search on answer → silently incorrect results. Always prove or spot-check monotonicity.
7. **Forgetting unsorted input**: binary search requires the data to be sorted (or monotonic) on the axis you search.
8. **Rotated array with equal endpoints** (`arr[lo] == arr[mid] == arr[hi]`): cannot decide the sorted half — shrink with `lo++`, `hi--`, giving O(n) worst case.
9. **Using recursion on very large ranges**: O(log n) stack is fine, but iterative is usually cleaner and stack-safe.
10. **Assuming `bisect_left` == "first occurrence"**: it returns the insertion point; verify `arr[idx] == target` afterward.

## Practice Problems by Difficulty

### Easy
1. [Binary Search](https://leetcode.com/problems/binary-search/) (LC #704)
2. [First Bad Version](https://leetcode.com/problems/first-bad-version/) (LC #278)
3. [Sqrt(x)](https://leetcode.com/problems/sqrtx/) (LC #69)
4. [Search Insert Position](https://leetcode.com/problems/search-insert-position/) (LC #35)
5. [Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/) (LC #367)
6. [Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower/) (LC #374)

### Medium
1. [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/) (LC #33)
2. [Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) (LC #34)
3. [Find Peak Element](https://leetcode.com/problems/find-peak-element/) (LC #162)
4. [Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/) (LC #153)
5. [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) (LC #74)
6. [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) (LC #875)
7. [Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) (LC #1011)
8. [Time Based Key-Value Store](https://leetcode.com/problems/time-based-key-value-store/) (LC #981)
9. [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) (LC #287)
10. [Successful Pairs of Spells and Potions](https://leetcode.com/problems/successful-pairs-of-spells-and-potions/) (LC #2300)

### Hard
1. [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/) (LC #4)
2. [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) (LC #410)
3. [Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/) (LC #1095)
4. [Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) (LC #81) (with duplicates)
5. [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/) (LC #378)
6. [Aggressive Cows](https://www.spoj.com/problems/AGGRCOW/) (classic BS on answer)
7. [Minimize Max Distance to Gas Station](https://leetcode.com/problems/minimize-max-distance-to-gas-station/) (LC #774)

## Real-World Applications
1. **Database indexes**: B-tree / B+ tree search uses binary search at each node
2. **Version control**: `git bisect` uses binary search over commit history to locate a bug
3. **Autocomplete / dictionaries**: locate prefix boundaries with lower/upper bound
4. **Scheduling & capacity planning**: binary search on a feasible parameter (workers, machines, rate limits)
5. **Numerical methods**: bisection method for root finding on continuous monotonic functions
6. **Memory allocators**: buddy systems and best-fit lookups
7. **Networking**: longest-prefix-match in routing tables
8. **Game AI / timing**: finding earliest / latest safe frame for an action

## Advanced Topics
1. **Ternary search**: for unimodal (strictly increasing then decreasing) functions
2. **Exponential (galloping) search**: useful when size is unknown or unbounded — grow `hi` exponentially, then binary search
3. **Fractional cascading**: speeds up repeated searches across multiple sorted lists
4. **Binary search on floating-point answers**: use fixed iteration counts (~60–100) for numeric stability
5. **Binary indexed tree / segment tree + binary search**: find the k-th element / first prefix exceeding a threshold in O(log n)
6. **Order statistic tree**: balanced BST augmented with subtree sizes; enables "rank-of" and "k-th smallest"
7. **Binary search on sorted streams**: combined with two-pointers or sliding window
8. **Parallel binary search**: answer many offline queries by binary-searching over a shared timeline

## Important Resources
1. [LeetCode Binary Search Explore Card](https://leetcode.com/explore/learn/card/binary-search/)
2. [Powerful Ultimate Binary Search Template (LeetCode discuss)](https://leetcode.com/discuss/study-guide/786126/python-powerful-ultimate-binary-search-template-solved-many-problems)
3. [CP-Algorithms: Binary Search](https://cp-algorithms.com/num_methods/binary_search.html)
4. [Python `bisect` module](https://docs.python.org/3/library/bisect.html)
5. [C++ `lower_bound` / `upper_bound` reference](https://en.cppreference.com/w/cpp/algorithm/lower_bound)
6. [Java `Arrays.binarySearch`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Arrays.html#binarySearch(int%5B%5D,int))
7. [Visualgo: Binary Search](https://visualgo.net/en/bst)
8. [AlgoMonster: Binary Search Patterns](https://algo.monster/problems/binary_search_intro)

## ❓ FAQ Section

**Q: When should I pick the `lo <= hi` template vs the `lo < hi` template?**
A: Use `lo <= hi` with inclusive `[lo, hi]` when you search for an **exact match** and return as soon as you find it. Use `lo < hi` with half-open `[lo, hi)` when you search for a **boundary** (first-true / last-true, lower/upper bound) — it converges to a single index without special-casing.

**Q: Why `mid = lo + (hi - lo) / 2` instead of `(lo + hi) / 2`?**
A: To avoid integer overflow when `lo + hi` exceeds the integer range. In Python it doesn't matter (arbitrary precision), but it's a safe habit across languages.

**Q: How do I know if "binary search on the answer" applies?**
A: Two conditions must hold: (1) the answer is a number in a bounded range, and (2) the feasibility predicate `canDo(x)` is **monotonic** — if `canDo(x)` is true, then `canDo(x')` is true for all `x' > x` (or all `x' < x`). If both hold, binary search the value range and call `canDo(mid)` at each step.

**Q: What's the difference between lower_bound and upper_bound?**
A: `lower_bound(t)` returns the first index `i` where `arr[i] >= t`. `upper_bound(t)` returns the first index `i` where `arr[i] > t`. The count of `t` in a sorted array is `upper_bound(t) - lower_bound(t)`. For the "first occurrence" of `t`, check `lower_bound(t)` and verify `arr[i] == t`.

**Q: How do I binary-search a rotated sorted array with duplicates?**
A: When `arr[lo] == arr[mid] == arr[hi]`, you can't tell which half is sorted — shrink with `lo++` and `hi--` and continue. Worst case becomes O(n), but the average case stays O(log n).

**Q: Is recursive or iterative binary search better?**
A: Iterative. It avoids O(log n) stack frames, is easier to reason about with loop invariants, and is typically faster due to less call overhead. Use recursion only when it makes the problem (e.g., tree/divide-and-conquer variants) clearer.

**Q: Can I use binary search on a linked list?**
A: Not directly — no random access → midpoint is O(n). For sorted linked lists, other strategies (two pointers, converting to an array, skip list) are better.

## Interview Tips
1. **State the invariant out loud**: "`target`, if present, is in `[lo, hi]`." This catches off-by-one bugs early.
2. **Pick one template and stick with it.** Most interviewers accept either `lo <= hi` with `[lo, hi]` or `lo < hi` with `[lo, hi)` — pick the one you've drilled and be consistent.
3. **Confirm monotonicity before writing code** when doing binary search on the answer — verbalize *why* `canDo(x) => canDo(x+1)` (or the reverse).
4. **Dry-run with tiny inputs**: size 0, 1, 2; target at boundaries and missing. This surfaces the classic `lo = mid` infinite-loop bug.
5. **Prefer iterative** unless the problem structure is inherently recursive.
6. **Watch for overflow** on `mid` and on intermediate computations like `mid * mid` — promote to `long` when needed.
7. **Clarify duplicates** with the interviewer: any index, first, or last?
8. **Don't forget to return the right thing** when the target isn't found — `-1`, insertion index, or `false` depending on the problem statement.
