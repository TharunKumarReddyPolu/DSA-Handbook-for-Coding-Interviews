# Backtracking Algorithm 🔙

## Introduction
Backtracking is an algorithmic technique that considers searching every possible combination in order to solve a computational problem. It builds candidates to the solution incrementally and abandons candidates ("backtracks") when it determines that the candidate cannot lead to a valid solution.

## Prerequisites & Related Topics

### Prerequisites
- [Recursion](recursion.md) (essential - backtracking is built on recursive calls)
- [Arrays](arrays.md) (for storing candidates and results)
- Basic understanding of tree structures (state space tree)

### Related Topics
- **Builds on**: [Recursion](recursion.md), [Tree](tree.md) traversal concepts
- **Alternative approach**: [Dynamic Programming](dynamic-programming.md) (when subproblems overlap)
- **Used in**: [Graph](graph.md) problems (DFS-based), constraint satisfaction problems
- **See also**: [Bit Manipulation](bit-manipulation.md) (for subset generation alternatives)

## Pattern Recognition Guide

### 🎯 When to Use Backtracking
**Keywords in problem**: "all combinations", "all permutations", "all possible", "generate all", "find all solutions"

**Use Backtracking when you see**:
- Need to explore ALL possible solutions
- Problems asking for permutations, combinations, or subsets
- Constraint satisfaction (Sudoku, N-Queens)
- Path finding with multiple valid paths
- Decision trees where choices can be undone

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Subsets | "all subsets", "power set", "all combinations of size k" | Subsets, Combination Sum |
| Permutations | "all arrangements", "all orderings", "permute" | Permutations, Letter Case Permutation |
| Constraint Satisfaction | "valid placement", "no conflicts", "satisfy rules" | N-Queens, Sudoku Solver |
| Path Finding | "all paths", "find path from...to", "maze" | Word Search, Path Sum II |
| Partitioning | "divide into groups", "split array", "partition" | Palindrome Partitioning |

### ❌ When NOT to Use
- Only need ONE solution or optimal solution → consider [Greedy](greedy.md) or [DP](dynamic-programming.md)
- Subproblems overlap significantly → use [Dynamic Programming](dynamic-programming.md)
- Problem has greedy choice property → use [Greedy](greedy.md)
- Solution space is too large without pruning opportunities

## Core Concepts

### Important Terminologies
- **Backtracking**: Algorithmic technique to find all solutions by incrementally building candidates
- **State Space Tree**: Tree representing all possible states
- **Pruning**: Eliminating branches that can't lead to solution
- **Constraint**: Condition that must be satisfied
- **Choice Point**: Point where decision is made
- **Dead End**: State that can't lead to solution
- **Solution Space**: Set of all possible solutions
- **Feasible Solution**: Solution satisfying all constraints
- **Optimal Solution**: Best solution among feasible ones
- **Decision Tree**: Tree showing all possible choices

### Time Complexity Analysis
| Problem Type        | Time Complexity | Space Complexity |
|--------------------|-----------------|------------------|
| Subsets            | O(2ⁿ)          | O(n)             |
| Permutations       | O(n!)          | O(n)             |
| N-Queens           | O(n!)          | O(n)             |
| Sudoku             | O(9^(n*n))     | O(n*n)          |
| Word Search        | O(4^(n*m))     | O(n*m)          |
| Combination Sum    | O(2^n)         | O(n)             |

Where n is the input size or board dimension

## Implementation Patterns

### 1. Basic Backtracking Template

**Pseudocode:**
```
1. Define base case: if current_state is a complete solution
   a. Add copy of current_state to results
   b. Return
2. For each possible choice from available options:
   a. If choice is valid for current_state:
      - Make the choice (add to current_state)
      - Recursively call backtrack with updated state
      - Undo the choice (remove from current_state) - BACKTRACK
```

```python
def backtrack(input, current_state, result):
    if is_solution(current_state):
        result.append(current_state.copy())
        return
    
    for choice in get_choices(input, current_state):
        if is_valid(choice, current_state):
            make_choice(choice, current_state)
            backtrack(input, current_state, result)
            undo_choice(choice, current_state)
```

```java
void backtrack(int[] input, List<Integer> currentState, List<List<Integer>> result) {
    if (isSolution(currentState)) {
        result.add(new ArrayList<>(currentState));
        return;
    }
    for (int choice : getChoices(input, currentState)) {
        if (isValid(choice, currentState)) {
            currentState.add(choice);  // make choice
            backtrack(input, currentState, result);
            currentState.remove(currentState.size() - 1);  // undo choice
        }
    }
}
```

```cpp
void backtrack(vector<int>& input, vector<int>& currentState, vector<vector<int>>& result) {
    if (isSolution(currentState)) {
        result.push_back(currentState);
        return;
    }
    for (int choice : getChoices(input, currentState)) {
        if (isValid(choice, currentState)) {
            currentState.push_back(choice);  // make choice
            backtrack(input, currentState, result);
            currentState.pop_back();  // undo choice
        }
    }
}
```

### 2. Subset Generation

**Pseudocode:**
```
1. Start with empty path and index 0
2. At each step, add current path to result (every path is a valid subset)
3. For each element from start index to end:
   a. Include current element in path
   b. Recursively generate subsets starting from next index
   c. Remove current element from path (backtrack)
```

```python
def subsets(nums):
    def backtrack(start, path):
        result.append(path[:])
        
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    
    result = []
    backtrack(0, [])
    return result
```

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}
private void backtrack(int[] nums, int start, List<Integer> path, List<List<Integer>> result) {
    result.add(new ArrayList<>(path));
    for (int i = start; i < nums.length; i++) {
        path.add(nums[i]);
        backtrack(nums, i + 1, path, result);
        path.remove(path.size() - 1);
    }
}
```

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> path;
    backtrack(nums, 0, path, result);
    return result;
}
void backtrack(vector<int>& nums, int start, vector<int>& path, vector<vector<int>>& result) {
    result.push_back(path);
    for (int i = start; i < nums.size(); i++) {
        path.push_back(nums[i]);
        backtrack(nums, i + 1, path, result);
        path.pop_back();
    }
}
```

### 3. Permutation Generation

**Pseudocode:**
```
1. Track which elements are used with a boolean array
2. Base case: if path length equals input length, add path to results
3. For each element in input:
   a. If element not used:
      - Mark as used, add to path
      - Recursively generate remaining permutation
      - Unmark as used, remove from path (backtrack)
```

```python
def permutations(nums):
    def backtrack(path, used):
        if len(path) == len(nums):
            result.append(path[:])
            return
        
        for i in range(len(nums)):
            if not used[i]:
                used[i] = True
                path.append(nums[i])
                backtrack(path, used)
                path.pop()
                used[i] = False
    
    result = []
    backtrack([], [False] * len(nums))
    return result
```

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, new ArrayList<>(), new boolean[nums.length], result);
    return result;
}
private void backtrack(int[] nums, List<Integer> path, boolean[] used, List<List<Integer>> result) {
    if (path.size() == nums.length) {
        result.add(new ArrayList<>(path));
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        if (!used[i]) {
            used[i] = true;
            path.add(nums[i]);
            backtrack(nums, path, used, result);
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

```cpp
vector<vector<int>> permute(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> path;
    vector<bool> used(nums.size(), false);
    backtrack(nums, path, used, result);
    return result;
}
void backtrack(vector<int>& nums, vector<int>& path, vector<bool>& used, vector<vector<int>>& result) {
    if (path.size() == nums.size()) {
        result.push_back(path);
        return;
    }
    for (int i = 0; i < nums.size(); i++) {
        if (!used[i]) {
            used[i] = true;
            path.push_back(nums[i]);
            backtrack(nums, path, used, result);
            path.pop_back();
            used[i] = false;
        }
    }
}
```

### 4. N-Queens Problem

**Pseudocode:**
```
1. Create n×n board initialized with '.'
2. Process row by row:
   a. Base case: if row == n, found valid solution, add to results
   b. For each column in current row:
      - Check if position is safe (no queen in same column, diagonals)
      - If safe: place queen ('Q'), recurse to next row, remove queen (backtrack)
3. Safety check: scan upward in column and both diagonals
```

```python
def solveNQueens(n):
    def is_safe(board, row, col):
        # Check column
        for i in range(row):
            if board[i][col] == 'Q':
                return False
        
        # Check upper left diagonal
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if board[i][j] == 'Q':
                return False
        
        # Check upper right diagonal
        for i, j in zip(range(row-1, -1, -1), range(col+1, n)):
            if board[i][j] == 'Q':
                return False
        
        return True
    
    def backtrack(row):
        if row == n:
            result.append([''.join(row) for row in board])
            return
        
        for col in range(n):
            if is_safe(board, row, col):
                board[row][col] = 'Q'
                backtrack(row + 1)
                board[row][col] = '.'
    
    board = [['.' for _ in range(n)] for _ in range(n)]
    result = []
    backtrack(0)
    return result
```

```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    for (char[] row : board) Arrays.fill(row, '.');
    backtrack(board, 0, result);
    return result;
}
private void backtrack(char[][] board, int row, List<List<String>> result) {
    if (row == board.length) {
        result.add(Arrays.stream(board).map(String::new).collect(Collectors.toList()));
        return;
    }
    for (int col = 0; col < board.length; col++) {
        if (isSafe(board, row, col)) {
            board[row][col] = 'Q';
            backtrack(board, row + 1, result);
            board[row][col] = '.';
        }
    }
}
private boolean isSafe(char[][] board, int row, int col) {
    for (int i = 0; i < row; i++) if (board[i][col] == 'Q') return false;
    for (int i = row-1, j = col-1; i >= 0 && j >= 0; i--, j--) if (board[i][j] == 'Q') return false;
    for (int i = row-1, j = col+1; i >= 0 && j < board.length; i--, j++) if (board[i][j] == 'Q') return false;
    return true;
}
```

```cpp
vector<vector<string>> solveNQueens(int n) {
    vector<vector<string>> result;
    vector<string> board(n, string(n, '.'));
    backtrack(board, 0, result);
    return result;
}
void backtrack(vector<string>& board, int row, vector<vector<string>>& result) {
    if (row == board.size()) { result.push_back(board); return; }
    for (int col = 0; col < board.size(); col++) {
        if (isSafe(board, row, col)) {
            board[row][col] = 'Q';
            backtrack(board, row + 1, result);
            board[row][col] = '.';
        }
    }
}
bool isSafe(vector<string>& board, int row, int col) {
    for (int i = 0; i < row; i++) if (board[i][col] == 'Q') return false;
    for (int i = row-1, j = col-1; i >= 0 && j >= 0; i--, j--) if (board[i][j] == 'Q') return false;
    for (int i = row-1, j = col+1; i >= 0 && j < board.size(); i--, j++) if (board[i][j] == 'Q') return false;
    return true;
}
```

### 5. Combination Sum

**Pseudocode:**
```
1. Sort candidates (optional optimization)
2. Base case: if target < 0, return; if target == 0, add path to results
3. For each candidate from start index:
   a. Add candidate to path
   b. Recurse with same start index (allows reuse) and reduced target
   c. Remove candidate from path (backtrack)
4. Optimization: break early if candidate > target (requires sorted input)
```

```python
def combinationSum(candidates, target):
    def backtrack(start, target, path):
        if target < 0:
            return
        if target == 0:
            result.append(path[:])
            return
        
        for i in range(start, len(candidates)):
            path.append(candidates[i])
            backtrack(i, target - candidates[i], path)
            path.pop()
    
    result = []
    candidates.sort()  # Optional optimization
    backtrack(0, target, [])
    return result
```

```java
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(candidates);
    backtrack(candidates, target, 0, new ArrayList<>(), result);
    return result;
}
private void backtrack(int[] candidates, int target, int start, List<Integer> path, List<List<Integer>> result) {
    if (target == 0) { result.add(new ArrayList<>(path)); return; }
    for (int i = start; i < candidates.length && candidates[i] <= target; i++) {
        path.add(candidates[i]);
        backtrack(candidates, target - candidates[i], i, path, result);
        path.remove(path.size() - 1);
    }
}
```

```cpp
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    vector<vector<int>> result;
    vector<int> path;
    sort(candidates.begin(), candidates.end());
    backtrack(candidates, target, 0, path, result);
    return result;
}
void backtrack(vector<int>& candidates, int target, int start, vector<int>& path, vector<vector<int>>& result) {
    if (target == 0) { result.push_back(path); return; }
    for (int i = start; i < candidates.size() && candidates[i] <= target; i++) {
        path.push_back(candidates[i]);
        backtrack(candidates, target - candidates[i], i, path, result);
        path.pop_back();
    }
}
```

## Common Techniques

### 1. State Management

**Pseudocode:**
```
1. Create State class with: choices list, used set
2. make_choice(choice): add to choices list, add to used set
3. undo_choice(): remove last from choices, remove from used set
4. is_valid(choice): return true if choice not in used set
```

```python
class State:
    def __init__(self):
        self.choices = []
        self.used = set()
    
    def make_choice(self, choice):
        self.choices.append(choice)
        self.used.add(choice)
    
    def undo_choice(self):
        choice = self.choices.pop()
        self.used.remove(choice)
    
    def is_valid(self, choice):
        return choice not in self.used
```

```java
class State {
    List<Integer> choices = new ArrayList<>();
    Set<Integer> used = new HashSet<>();
    
    void makeChoice(int choice) { choices.add(choice); used.add(choice); }
    void undoChoice() { int choice = choices.remove(choices.size() - 1); used.remove(choice); }
    boolean isValid(int choice) { return !used.contains(choice); }
}
```

```cpp
class State {
    vector<int> choices;
    unordered_set<int> used;
public:
    void makeChoice(int choice) { choices.push_back(choice); used.insert(choice); }
    void undoChoice() { int choice = choices.back(); choices.pop_back(); used.erase(choice); }
    bool isValid(int choice) { return !used.count(choice); }
};
```

### 2. Pruning Optimization

**Pseudocode:**
```
1. Early termination: if target < 0, return immediately
2. Success: if target == 0, add current path to results
3. For each candidate from start:
   a. Skip duplicates: if i > start AND candidate[i] == candidate[i-1], continue
   b. Prune: if candidate > target, break (requires sorted input)
   c. Add candidate, recurse with i+1 and reduced target, remove (backtrack)
```

```python
def backtrack_with_pruning(candidates, target, path, start):
    if target < 0:  # Pruning condition
        return
    if target == 0:
        result.append(path[:])
        return
    
    for i in range(start, len(candidates)):
        # Skip duplicates
        if i > start and candidates[i] == candidates[i-1]:
            continue
        
        # Pruning: if current number is too large
        if candidates[i] > target:
            break
        
        path.append(candidates[i])
        backtrack_with_pruning(candidates, target - candidates[i], path, i + 1)
        path.pop()
```

```java
void backtrackWithPruning(int[] candidates, int target, int start, List<Integer> path, List<List<Integer>> result) {
    if (target < 0) return;
    if (target == 0) { result.add(new ArrayList<>(path)); return; }
    for (int i = start; i < candidates.length; i++) {
        if (i > start && candidates[i] == candidates[i-1]) continue; // Skip duplicates
        if (candidates[i] > target) break; // Pruning
        path.add(candidates[i]);
        backtrackWithPruning(candidates, target - candidates[i], i + 1, path, result);
        path.remove(path.size() - 1);
    }
}
```

```cpp
void backtrackWithPruning(vector<int>& candidates, int target, int start, vector<int>& path, vector<vector<int>>& result) {
    if (target < 0) return;
    if (target == 0) { result.push_back(path); return; }
    for (int i = start; i < candidates.size(); i++) {
        if (i > start && candidates[i] == candidates[i-1]) continue; // Skip duplicates
        if (candidates[i] > target) break; // Pruning
        path.push_back(candidates[i]);
        backtrackWithPruning(candidates, target - candidates[i], i + 1, path, result);
        path.pop_back();
    }
}
```

## Edge Cases to Consider
1. Empty input
2. Single element input
3. Duplicate elements
4. No solution exists
5. Multiple solutions
6. Maximum recursion depth
7. Large input size
8. All elements same
9. Negative numbers
10. Boundary conditions

## Common Pitfalls
1. Not handling base cases
2. Incorrect state management
3. Missing pruning opportunities
4. Infinite recursion
5. Memory overflow
6. Not copying state properly
7. Incorrect constraint checking
8. Inefficient pruning

## Practice Problems by Difficulty

### Easy
1. [Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/) (LC #784)
2. [Binary Watch](https://leetcode.com/problems/binary-watch/) (LC #401)
3. [Find Mode in Binary Search Tree](https://leetcode.com/problems/find-mode-in-binary-search-tree/) (LC #501)

### Medium
1. [Subsets](https://leetcode.com/problems/subsets/) (LC #78)
2. [Permutations](https://leetcode.com/problems/permutations/) (LC #46)
3. [Combination Sum](https://leetcode.com/problems/combination-sum/) (LC #39)
4. [Word Search](https://leetcode.com/problems/word-search/) (LC #79)

### Hard
1. [N-Queens](https://leetcode.com/problems/n-queens/) (LC #51)
2. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) (LC #37)
3. [Word Search II](https://leetcode.com/problems/word-search-ii/) (LC #212)

## Real-World Applications
1. **Constraint Satisfaction**:
   - Scheduling problems
   - Resource allocation
2. **Game Playing**:
   - Chess engines
   - Puzzle solvers
3. **Circuit Design**:
   - VLSI design
   - Path routing
4. **AI and Machine Learning**:
   - Decision trees
   - Neural architecture search
5. **Operations Research**:
   - Vehicle routing
   - Facility location
6. **Bioinformatics**:
   - Protein folding
   - Gene sequencing

## Advanced Topics
1. **Parallel Backtracking**:
   - Task parallelization
   - Load balancing
2. **Hybrid Approaches**:
   - Backtracking with DP
   - Backtracking with Branch & Bound
3. **Optimization Techniques**:
   - Forward checking
   - Constraint propagation
4. **Heuristic Functions**:
   - State ordering
   - Variable ordering

## Important Resources
1. [Backtracking Algorithms](https://www.geeksforgeeks.org/backtracking-algorithms/)
2. [Constraint Satisfaction Problems](https://www.geeksforgeeks.org/constraint-satisfaction-problems-csp-in-artificial-intelligence/)
3. [Optimization Techniques](https://www.geeksforgeeks.org/backtracking-introduction/)
4. [Pattern Recognition](https://leetcode.com/discuss/general-discussion/136503/backtracking-pattern)
5. [Visual Backtracking](https://www.cs.usfca.edu/~galles/visualization/DFS.html)

## ❓ FAQ Section

**Q: What's the difference between backtracking and brute force?**
A: Backtracking is an optimized brute force that abandons partial solutions as soon as it determines they can't lead to valid complete solutions (pruning). Brute force checks all possibilities. Backtracking can be exponentially faster with good pruning.

**Q: When should I use backtracking vs DP?**
A: Use backtracking when you need to generate ALL solutions (permutations, combinations, subsets) or find ANY valid solution. Use DP when you need to count solutions or find the OPTIMAL solution. Sometimes they can be combined.

**Q: How do I implement the "undo" step in backtracking?**
A: After the recursive call returns, reverse whatever changes you made before the call. If you added to a list, remove. If you marked as visited, unmark. The state should be identical before and after exploring a branch.

**Q: How do I prune effectively in backtracking?**
A: (1) Sort input to enable early termination, (2) Track running sum/count to stop when target is exceeded, (3) Skip duplicates by checking if current equals previous, (4) Use constraints to eliminate invalid branches early.

**Q: What's the time complexity of backtracking?**
A: Usually O(k^n) or O(n!) where k is the branching factor and n is depth. For subsets: O(2^n), permutations: O(n!), combinations: O(C(n,k)). Pruning can significantly reduce the actual runtime but doesn't change worst-case complexity.

## Interview Tips
1. Identify problem constraints
2. Draw state space tree
3. Consider pruning opportunities
4. Handle base cases properly
5. Manage state carefully
6. Consider optimization techniques
7. Test with small examples
8. Analyze time complexity
9. Consider space optimization
10. Discuss trade-offs