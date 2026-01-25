# Recursion in Programming 🔁

## Introduction
Recursion is a programming concept where a function solves a problem by calling itself with smaller instances of the same problem. It's a powerful technique for solving problems that can be broken down into smaller, similar sub-problems.

## Prerequisites & Related Topics

### Prerequisites
- Functions (function calls, parameters, return values)
- [Stack](stack.md) concept (understanding call stack)
- Base case and termination conditions

### Related Topics
- **Foundation for**: [Backtracking](backtracking.md), [Dynamic Programming](dynamic-programming.md) (memoization)
- **Essential for**: [Tree](tree.md) traversals, [Graph](graph.md) (DFS)
- **Can convert to**: [Stack](stack.md) (iterative with explicit stack)
- **See also**: [Sorting](sorting.md) (merge sort, quick sort), [Math](math.md) (mathematical induction)

## Pattern Recognition Guide

### 🎯 When to Use Recursion
**Keywords in problem**: "nested", "tree", "divide", "subproblem", "self-similar"

**Use Recursion when you see**:
- Problem can be broken into smaller identical subproblems
- Tree or hierarchical data structures
- Divide and conquer opportunities
- Nested structures (parentheses, directories)
- Mathematical sequences defined recursively

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Tree Traversal | "traverse tree", "tree operations" | All tree traversals, Tree Height |
| Divide & Conquer | "merge sort", "quick sort", "binary search" | Merge Sort, Quick Sort |
| Mathematical | "factorial", "fibonacci", "power" | Climbing Stairs, Pow(x,n) |
| Nested Structures | "nested lists", "flatten", "calculator" | Nested List Iterator, Basic Calculator |
| Generate All | "combinations", "permutations", "subsets" | Subsets, Permutations |
| Linked List | "reverse recursively", "merge recursively" | Reverse Linked List (recursive) |

### ❌ When NOT to Use
- Simple iteration works → avoid recursion overhead
- Very deep recursion possible → use iteration with [Stack](stack.md)
- Overlapping subproblems → add memoization or use [DP](dynamic-programming.md)
- Tail recursion in non-optimizing language → convert to iteration

### 💡 Recursion Checklist
1. ✅ Define clear base case(s)
2. ✅ Ensure progress toward base case
3. ✅ Trust the recursive call works for smaller input
4. ✅ Consider memoization if subproblems repeat

## Core Concepts

### Important Terminologies
- **Base Case**: Condition where recursion stops
- **Recursive Case**: Function calling itself
- **Call Stack**: Memory storing function calls
- **Stack Frame**: Memory for single function call
- **Recursion Depth**: Number of recursive calls
- **Direct Recursion**: Function calls itself directly
- **Indirect Recursion**: Function A calls B, B calls A
- **Tail Recursion**: Recursive call is last operation
- **Tree Recursion**: Multiple recursive calls
- **Nested Recursion**: Recursive parameter is recursive

### Types of Recursion
1. **Linear Recursion**: One recursive call per function
2. **Binary Recursion**: Two recursive calls per function
3. **Multiple Recursion**: Multiple recursive calls
4. **Mutual Recursion**: Functions calling each other
5. **Nested Recursion**: Parameter is recursive call

### Time and Space Complexity Analysis
| Type           | Time Complexity | Space Complexity |
|----------------|----------------|------------------|
| Linear         | O(n)           | O(n)             |
| Binary         | O(2^n)         | O(n)             |
| Tail           | O(n)           | O(1)*            |
| Tree           | O(branches^depth)| O(depth)        |
| Nested         | Varies         | O(depth)         |

*Note: O(1) space for tail recursion only with proper optimization (not available in Python)

## Implementation Patterns

### 1. Basic Linear Recursion

**Pseudocode:**
```
Factorial:
1. Base case: if n <= 1, return 1
2. Recursive case: return n × factorial(n - 1)

Fibonacci:
1. Base case: if n <= 1, return n
2. Recursive case: return fib(n-1) + fib(n-2)
```

```python
def factorial(n):
    # Base case
    if n <= 1:
        return 1
    # Recursive case
    return n * factorial(n - 1)

def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

```java
public int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
public int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

```cpp
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### 2. Tail Recursion

**Pseudocode:**
```
Tail-recursive factorial (uses accumulator):
1. Base case: if n <= 1, return accumulator
2. Recursive case: return factorial(n-1, n × accumulator)
(Recursive call is last operation - enables optimization)

Tail-recursive fibonacci:
1. Base case: if n == 0, return a
2. Recursive case: return fib(n-1, b, a+b)
```

```python
def factorial_tail(n, accumulator=1):
    if n <= 1:
        return accumulator
    return factorial_tail(n - 1, n * accumulator)

def fibonacci_tail(n, a=0, b=1):
    if n == 0:
        return a
    return fibonacci_tail(n - 1, b, a + b)
```

```java
public long factorialTail(int n, long accumulator) {
    if (n <= 1) return accumulator;
    return factorialTail(n - 1, n * accumulator);
}

public int fibonacciTail(int n, int a, int b) {
    if (n == 0) return a;
    return fibonacciTail(n - 1, b, a + b);
}
```

```cpp
long factorialTail(int n, long accumulator = 1) {
    if (n <= 1) return accumulator;
    return factorialTail(n - 1, n * accumulator);
}

int fibonacciTail(int n, int a = 0, int b = 1) {
    if (n == 0) return a;
    return fibonacciTail(n - 1, b, a + b);
}
```

### 3. Binary Recursion (Divide and Conquer)

**Pseudocode:**
```
Merge Sort:
1. Base case: if length <= 1, return array
2. Split array into two halves at midpoint
3. Recursively sort left half and right half
4. Merge two sorted halves:
   a. Compare elements from both, take smaller
   b. Append remaining elements from non-empty half
```

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

```java
public int[] mergeSort(int[] arr) {
    if (arr.length <= 1) return arr;
    int mid = arr.length / 2;
    int[] left = mergeSort(Arrays.copyOfRange(arr, 0, mid));
    int[] right = mergeSort(Arrays.copyOfRange(arr, mid, arr.length));
    return merge(left, right);
}
private int[] merge(int[] left, int[] right) {
    int[] result = new int[left.length + right.length];
    int i = 0, j = 0, k = 0;
    while (i < left.length && j < right.length)
        result[k++] = left[i] <= right[j] ? left[i++] : right[j++];
    while (i < left.length) result[k++] = left[i++];
    while (j < right.length) result[k++] = right[j++];
    return result;
}
```

```cpp
vector<int> mergeSort(vector<int> arr) {
    if (arr.size() <= 1) return arr;
    int mid = arr.size() / 2;
    vector<int> left(arr.begin(), arr.begin() + mid);
    vector<int> right(arr.begin() + mid, arr.end());
    left = mergeSort(left); right = mergeSort(right);
    vector<int> result;
    int i = 0, j = 0;
    while (i < left.size() && j < right.size())
        result.push_back(left[i] <= right[j] ? left[i++] : right[j++]);
    while (i < left.size()) result.push_back(left[i++]);
    while (j < right.size()) result.push_back(right[j++]);
    return result;
}
```

### 4. Tree Recursion

**Pseudocode:**
```
Generate all binary strings of length n:
1. Base case: if string length == n, output string
2. Recursive case: 
   - Recurse with string + "0"
   - Recurse with string + "1"

Generate permutations:
1. Base case: if string empty, output prefix
2. For each character in string:
   - Remove character, recurse with remaining and prefix + char
```

```python
def print_binary_strings(n, string=""):
    if len(string) == n:
        print(string)
        return
    
    print_binary_strings(n, string + "0")
    print_binary_strings(n, string + "1")

def generate_permutations(string, prefix=""):
    if len(string) == 0:
        print(prefix)
        return
    
    for i in range(len(string)):
        rem = string[:i] + string[i+1:]
        generate_permutations(rem, prefix + string[i])
```

```java
void printBinaryStrings(int n, String s) {
    if (s.length() == n) { System.out.println(s); return; }
    printBinaryStrings(n, s + "0");
    printBinaryStrings(n, s + "1");
}

void generatePermutations(String str, String prefix) {
    if (str.isEmpty()) { System.out.println(prefix); return; }
    for (int i = 0; i < str.length(); i++) {
        String rem = str.substring(0, i) + str.substring(i + 1);
        generatePermutations(rem, prefix + str.charAt(i));
    }
}
```

```cpp
void printBinaryStrings(int n, string s = "") {
    if (s.length() == n) { cout << s << endl; return; }
    printBinaryStrings(n, s + "0");
    printBinaryStrings(n, s + "1");
}

void generatePermutations(string str, string prefix = "") {
    if (str.empty()) { cout << prefix << endl; return; }
    for (int i = 0; i < str.size(); i++) {
        string rem = str.substr(0, i) + str.substr(i + 1);
        generatePermutations(rem, prefix + str[i]);
    }
}
```

### 5. Memoization with Recursion

**Pseudocode:**
```
Fibonacci with memoization:
1. If n in memo, return memo[n]
2. Base case: if n <= 1, return n
3. Compute: memo[n] = fib(n-1, memo) + fib(n-2, memo)
4. Return memo[n]
(Transforms O(2^n) to O(n) by caching results)
```

```python
def fibonacci_memo(n, memo=None):
    if memo is None:
        memo = {}
    
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    return memo[n]
```

```java
Map<Integer, Integer> memo = new HashMap<>();
int fibonacciMemo(int n) {
    if (n <= 1) return n;
    if (memo.containsKey(n)) return memo.get(n);
    int result = fibonacciMemo(n - 1) + fibonacciMemo(n - 2);
    memo.put(n, result);
    return result;
}
```

```cpp
unordered_map<int, int> memo;
int fibonacciMemo(int n) {
    if (n <= 1) return n;
    if (memo.count(n)) return memo[n];
    return memo[n] = fibonacciMemo(n - 1) + fibonacciMemo(n - 2);
}
```

## Common Techniques

### 1. Backtracking Template

**Pseudocode:**
```
Backtracking with target sum:
1. If target < 0: return (overshot, prune this path)
2. If target == 0: add current path to results (found valid combination)
3. For each candidate starting from current position:
   a. Add candidate to path
   b. Recurse with reduced target and same/next start position
   c. Remove candidate from path (backtrack)
```

```python
def backtrack(candidates, target, path, results):
    if target < 0:
        return
    if target == 0:
        results.append(path[:])
        return
    
    for i in range(len(candidates)):
        path.append(candidates[i])
        backtrack(candidates[i:], target - candidates[i], path, results)
        path.pop()
```

```java
void backtrack(int[] candidates, int target, int start, List<Integer> path, List<List<Integer>> results) {
    if (target < 0) return;
    if (target == 0) { results.add(new ArrayList<>(path)); return; }
    for (int i = start; i < candidates.length; i++) {
        path.add(candidates[i]);
        backtrack(candidates, target - candidates[i], i, path, results);
        path.remove(path.size() - 1);
    }
}
```

```cpp
void backtrack(vector<int>& candidates, int target, int start, vector<int>& path, vector<vector<int>>& results) {
    if (target < 0) return;
    if (target == 0) { results.push_back(path); return; }
    for (int i = start; i < candidates.size(); i++) {
        path.push_back(candidates[i]);
        backtrack(candidates, target - candidates[i], i, path, results);
        path.pop_back();
    }
}
```

### 2. Tree Traversal

**Pseudocode:**
```
Pre-order traversal:
1. If node is null, return
2. Process node (print value)
3. Recurse on left subtree
4. Recurse on right subtree
(In-order: left, process, right; Post-order: left, right, process)
```

```python
def tree_traversal(root):
    if not root:
        return
    
    # Pre-order
    print(root.val)
    tree_traversal(root.left)
    tree_traversal(root.right)
```

```java
void treeTraversal(TreeNode root) {
    if (root == null) return;
    // Pre-order
    System.out.println(root.val);
    treeTraversal(root.left);
    treeTraversal(root.right);
}
```

```cpp
void treeTraversal(TreeNode* root) {
    if (!root) return;
    // Pre-order
    cout << root->val << endl;
    treeTraversal(root->left);
    treeTraversal(root->right);
}
```

## Edge Cases to Consider
1. Base case not reached
2. Stack overflow
3. Negative or zero input
4. Empty input
5. Single element input
6. Maximum recursion depth
7. Circular dependencies
8. Duplicate calculations
9. Large input causing deep recursion
10. Memory constraints

## Common Pitfalls
1. Missing base case
2. Incorrect base case
3. Infinite recursion
4. Not handling edge cases
5. Inefficient recursive solution
6. Stack overflow
7. Redundant calculations
8. Memory leaks

## Practice Problems by Difficulty

### Easy
1. [Power of Three](https://leetcode.com/problems/power-of-three/) (LC #326)
2. [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) (LC #509)
3. [Reverse String](https://leetcode.com/problems/reverse-string/) (LC #344)

### Medium
1. [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) (LC #22)
2. [Pow(x, n)](https://leetcode.com/problems/powx-n/) (LC #50)
3. [Subsets](https://leetcode.com/problems/subsets/) (LC #78)
4. [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) (LC #17)

### Hard
1. [N-Queens](https://leetcode.com/problems/n-queens/) (LC #51)
2. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) (LC #10)
3. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) (LC #37)

## Real-World Applications
1. **File Systems**: Directory traversal
2. **Compilers**: Expression parsing
3. **Graphics**: Fractal generation
4. **AI/ML**: Tree-based algorithms
5. **Games**: Game tree exploration
6. **Mathematics**: Mathematical induction
7. **Data Processing**: XML/JSON parsing

## Advanced Topics
1. **Tail Call Optimization**:
   - Implementation
   - Language support
   - Limitations
2. **Recursion to Iteration**:
   - Stack simulation
   - State machines
3. **Memory Management**:
   - Stack vs Heap
   - Memory optimization
4. **Parallel Recursion**:
   - Task parallelization
   - Work stealing

## Important Resources
1. [Recursion Visualization](https://recursion.now.sh/)
2. [Python Recursion Guide](https://realpython.com/python-recursion/)
3. [Tail Call Optimization](https://www.geeksforgeeks.org/tail-recursion/)
4. [Recursion vs Iteration](https://www.geeksforgeeks.org/recursion-vs-iteration/)
5. [Memory Management in Recursion](https://www.geeksforgeeks.org/memory-allocation-in-recursion/)

## ❓ FAQ Section

**Q: How do I identify the base case for recursion?**
A: Ask "What's the smallest/simplest input where I know the answer immediately?" For arrays: empty array or single element. For trees: null node or leaf. For numbers: 0 or 1. The base case should not require further recursion.

**Q: How do I avoid stack overflow in recursion?**
A: (1) Convert to iteration using explicit stack, (2) Use tail recursion (if language supports optimization), (3) Increase stack size, (4) Ensure base case is reachable and correct. Python has a default recursion limit of ~1000.

**Q: When should I use recursion vs iteration?**
A: Use recursion for tree/graph traversals, divide-and-conquer, and problems with natural recursive structure. Use iteration for simple loops, when stack space is a concern, or when performance is critical. Any recursion can be converted to iteration.

**Q: What is tail recursion and why does it matter?**
A: Tail recursion is when the recursive call is the last operation in the function. Some languages (not Python/Java) optimize this to avoid stack growth, making it as efficient as iteration. Structure your recursion this way when possible.

**Q: How do I convert recursion to iteration?**
A: Use an explicit stack to simulate the call stack. Push initial state, loop while stack not empty: pop state, process it, push new states for "recursive calls". For tree traversal, this is standard iterative DFS.

## Interview Tips
1. Always identify base cases first
2. Consider time/space complexity
3. Look for recursive patterns
4. Consider iterative alternatives
5. Handle edge cases explicitly
6. Use visualization for complex cases
7. Consider memoization
8. Test with small examples
9. Discuss optimization possibilities
10. Consider stack limitations