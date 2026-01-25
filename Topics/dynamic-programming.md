# Dynamic Programming 🧠

## Introduction
Dynamic Programming (DP) is a method for solving complex problems by breaking them down into simpler subproblems. It is applicable when subproblems overlap and have optimal substructure. DP combines the solutions to subproblems to solve the original problem.

## Prerequisites & Related Topics

### Prerequisites
- [Recursion](recursion.md) (essential for top-down/memoization approach)
- [Arrays](arrays.md) (for tabulation and state storage)
- [Math](math.md) (for understanding recurrence relations)

### Related Topics
- **Builds on**: [Recursion](recursion.md) (memoization), [Math](math.md) (optimal substructure)
- **Compare with**: [Greedy](greedy.md) (when greedy choice property doesn't hold)
- **Often combined with**: [Strings](strings.md) (LCS, edit distance), [Graph](graph.md) (shortest paths), [Tree](tree.md) (tree DP)
- **See also**: [Bit Manipulation](bit-manipulation.md) (bitmask DP), [Matrix](matrix.md) (grid DP)

## Pattern Recognition Guide

### 🎯 When to Use Dynamic Programming
**Keywords in problem**: "minimum/maximum", "count ways", "longest/shortest", "is it possible", "optimal"

**Use DP when you see**:
- Overlapping subproblems (same computation repeated)
- Optimal substructure (optimal solution uses optimal sub-solutions)
- Counting problems (number of ways to...)
- Optimization problems (min/max cost, length, etc.)
- Decision problems (can we achieve X?)

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Linear DP | "climbing stairs", "house robber", "sequence" | Climbing Stairs, House Robber |
| 2D Grid DP | "grid path", "minimum path sum", "unique paths" | Unique Paths, Minimum Path Sum |
| String DP | "edit distance", "longest common", "palindrome" | Edit Distance, LCS, Longest Palindrome |
| Knapsack | "capacity", "weight limit", "select items", "subset sum" | 0/1 Knapsack, Coin Change |
| Interval DP | "merge", "burst", "range", "partition" | Burst Balloons, Matrix Chain |
| State Machine | "buy/sell stock", "states", "cooldown" | Best Time to Buy and Sell Stock |

### ❌ When NOT to Use
- Problem has greedy choice property → use [Greedy](greedy.md) (simpler)
- No overlapping subproblems → use [Recursion](recursion.md) or [Backtracking](backtracking.md)
- Need all solutions, not just count/optimal → use [Backtracking](backtracking.md)
- State space is too large even with memoization

## Core Concepts

### Important Terminologies
- **Overlapping Subproblems**: Same subproblems occur repeatedly
- **Optimal Substructure**: Optimal solution contains optimal subsolutions
- **State**: Variables needed to uniquely identify subproblem
- **Transition**: Way to move from one state to another
- **Base Case**: Smallest subproblem with known solution
- **Memoization**: Top-down approach with recursion + caching
- **Tabulation**: Bottom-up approach with iteration
- **State Space**: Set of all possible states
- **Recurrence Relation**: Formula relating states
- **Subproblem Graph**: Dependencies between subproblems

### Approaches to DP
1. **Top-Down (Memoization)**:
   - Recursive approach
   - Cache results
   - Natural to implement
   - Stack space overhead

2. **Bottom-Up (Tabulation)**:
   - Iterative approach
   - Build table
   - More space efficient
   - Order of computation important

### Time and Space Complexity Analysis
| Pattern                    | Time Complexity | Space Complexity |
|---------------------------|-----------------|------------------|
| Linear DP                 | O(n)           | O(n)             |
| Matrix Chain              | O(n³)          | O(n²)            |
| 0/1 Knapsack             | O(nW)          | O(nW)            |
| LCS                      | O(mn)          | O(mn)            |
| Edit Distance            | O(mn)          | O(mn)            |
| Matrix Path              | O(mn)          | O(mn)            |
| Coin Change              | O(nA)          | O(A)             |
| Longest Increasing Seq   | O(n²)          | O(n)             |

Where:
- n, m = input sizes
- W = weight capacity
- A = target amount

## Implementation Patterns

### 1. Fibonacci with Both Approaches

**Pseudocode:**
```
Top-down (Memoization):
1. If n is in memo, return memo[n]
2. Base case: if n <= 1, return n
3. memo[n] = fib(n-1) + fib(n-2)
4. Return memo[n]

Bottom-up (Tabulation):
1. Create dp array of size n+1, set dp[1] = 1
2. For i from 2 to n: dp[i] = dp[i-1] + dp[i-2]
3. Return dp[n]
```

```python
# Top-down (Memoization)
def fib_memo(n, memo=None):
    if memo is None:
        memo = {}
    
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fib_memo(n-1, memo) + fib_memo(n-2, memo)
    return memo[n]

# Bottom-up (Tabulation)
def fib_tab(n):
    if n <= 1:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]
```

```java
// Bottom-up (Tabulation)
public int fib(int n) {
    if (n <= 1) return n;
    int[] dp = new int[n + 1];
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

```cpp
// Bottom-up (Tabulation)
int fib(int n) {
    if (n <= 1) return n;
    vector<int> dp(n + 1);
    dp[1] = 1;
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

### 2. 0/1 Knapsack

**Pseudocode:**
```
1. Create 2D dp table: dp[i][w] = max value using first i items with capacity w
2. For each item i from 1 to n:
   a. For each capacity w from 0 to max_capacity:
      - If weight[i-1] <= w:
        dp[i][w] = max(take item: value[i-1] + dp[i-1][w-weight[i-1]], 
                       skip item: dp[i-1][w])
      - Else: dp[i][w] = dp[i-1][w] (can't take item)
3. Return dp[n][capacity]
```

```python
def knapsack(values, weights, capacity):
    n = len(values)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w-weights[i-1]],  # Take item
                    dp[i-1][w]  # Skip item
                )
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]
```

```java
public int knapsack(int[] values, int[] weights, int capacity) {
    int n = values.length;
    int[][] dp = new int[n + 1][capacity + 1];
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            if (weights[i-1] <= w)
                dp[i][w] = Math.max(values[i-1] + dp[i-1][w - weights[i-1]], dp[i-1][w]);
            else
                dp[i][w] = dp[i-1][w];
        }
    }
    return dp[n][capacity];
}
```

```cpp
int knapsack(vector<int>& values, vector<int>& weights, int capacity) {
    int n = values.size();
    vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= capacity; w++) {
            if (weights[i-1] <= w)
                dp[i][w] = max(values[i-1] + dp[i-1][w - weights[i-1]], dp[i-1][w]);
            else
                dp[i][w] = dp[i-1][w];
        }
    }
    return dp[n][capacity];
}
```

### 3. Longest Common Subsequence

**Pseudocode:**
```
1. Create 2D dp table: dp[i][j] = LCS length of text1[0:i] and text2[0:j]
2. For i from 1 to m, for j from 1 to n:
   a. If text1[i-1] == text2[j-1]:
      dp[i][j] = dp[i-1][j-1] + 1  (extend LCS)
   b. Else:
      dp[i][j] = max(dp[i-1][j], dp[i][j-1])  (take best without one char)
3. Return dp[m][n]
```

```python
def lcs(text1: str, text2: str) -> int:
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]
```

```java
public int longestCommonSubsequence(String text1, String text2) {
    int m = text1.length(), n = text2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1.charAt(i-1) == text2.charAt(j-1))
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[m][n];
}
```

```cpp
int longestCommonSubsequence(string text1, string text2) {
    int m = text1.size(), n = text2.size();
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (text1[i-1] == text2[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[m][n];
}
```

### 4. Matrix Path Problems

**Pseudocode:**
```
Minimum Path Sum:
1. Create dp table, set dp[0][0] = grid[0][0]
2. Fill first row: dp[0][j] = dp[0][j-1] + grid[0][j]
3. Fill first column: dp[i][0] = dp[i-1][0] + grid[i][0]
4. For each cell (i,j): dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
5. Return dp[m-1][n-1]
```

```python
def min_path_sum(grid):
    if not grid:
        return 0
    
    m, n = len(grid), len(grid[0])
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = grid[0][0]
    
    # Fill first row
    for j in range(1, n):
        dp[0][j] = dp[0][j-1] + grid[0][j]
    
    # Fill first column
    for i in range(1, m):
        dp[i][0] = dp[i-1][0] + grid[i][0]
    
    # Fill rest of the table
    for i in range(1, m):
        for j in range(1, n):
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j]
    
    return dp[m-1][n-1]
```

```java
public int minPathSum(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    int[][] dp = new int[m][n];
    dp[0][0] = grid[0][0];
    for (int j = 1; j < n; j++) dp[0][j] = dp[0][j-1] + grid[0][j];
    for (int i = 1; i < m; i++) dp[i][0] = dp[i-1][0] + grid[i][0];
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = Math.min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
    return dp[m-1][n-1];
}
```

```cpp
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<int>> dp(m, vector<int>(n));
    dp[0][0] = grid[0][0];
    for (int j = 1; j < n; j++) dp[0][j] = dp[0][j-1] + grid[0][j];
    for (int i = 1; i < m; i++) dp[i][0] = dp[i-1][0] + grid[i][0];
    for (int i = 1; i < m; i++)
        for (int j = 1; j < n; j++)
            dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
    return dp[m-1][n-1];
}
```

### 5. State Compression DP

**Pseudocode:**
```
Partition Equal Subset Sum:
1. Calculate total sum; if odd, return false
2. Target = total / 2
3. Use boolean dp array where dp[j] = can we make sum j?
4. Initialize dp[0] = true
5. For each number: traverse j from target down to num
   a. dp[j] = dp[j] OR dp[j - num]
6. Return dp[target]
```

```python
def can_partition(nums):
    total = sum(nums)
    if total % 2:
        return False
    
    target = total // 2
    dp = 1 << target
    
    for num in nums:
        dp |= (dp << num) & ((1 << (target + 1)) - 1)
    
    return dp & (1 << target)
```

```java
public boolean canPartition(int[] nums) {
    int total = Arrays.stream(nums).sum();
    if (total % 2 != 0) return false;
    int target = total / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;
    for (int num : nums)
        for (int j = target; j >= num; j--)
            dp[j] = dp[j] || dp[j - num];
    return dp[target];
}
```

```cpp
bool canPartition(vector<int>& nums) {
    int total = accumulate(nums.begin(), nums.end(), 0);
    if (total % 2 != 0) return false;
    int target = total / 2;
    vector<bool> dp(target + 1, false);
    dp[0] = true;
    for (int num : nums)
        for (int j = target; j >= num; j--)
            dp[j] = dp[j] || dp[j - num];
    return dp[target];
}
```

## Common DP Patterns

### 1. Linear DP
- One-dimensional state
- Linear state transition
- Example: Fibonacci, Climbing Stairs

### 2. Matrix DP
- Two-dimensional state
- Grid-based problems
- Example: Unique Paths, Minimum Path Sum

### 3. Interval DP
- State represents interval
- Usually O(n²) or O(n³)
- Example: Matrix Chain Multiplication

### 4. Decision Making
- Choose or not choose at each step
- Example: House Robber, 0/1 Knapsack

### 5. String DP
- States based on string indices
- Example: Edit Distance, LCS

## Edge Cases to Consider
1. Empty input
2. Single element input
3. Negative numbers
4. Integer overflow
5. Maximum recursion depth
6. Large input size
7. Equal values
8. Boundary conditions
9. Invalid input
10. Memory constraints

## Common Pitfalls
1. Incorrect base cases
2. Missing state variables
3. Wrong recurrence relation
4. Not handling edge cases
5. Inefficient state representation
6. Memory limit exceeded
7. Stack overflow in recursion
8. Incorrect initialization

## Practice Problems by Difficulty

### Easy
1. [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) (LC #70)
2. [House Robber](https://leetcode.com/problems/house-robber/) (LC #198)
3. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) (LC #121)

### Medium
1. [Unique Paths](https://leetcode.com/problems/unique-paths/) (LC #62)
2. [Coin Change](https://leetcode.com/problems/coin-change/) (LC #322)
3. [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) (LC #300)
4. [Target Sum](https://leetcode.com/problems/target-sum/) (LC #494)

### Hard
1. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) (LC #10)
2. [Edit Distance](https://leetcode.com/problems/edit-distance/) (LC #72)
3. [Burst Balloons](https://leetcode.com/problems/burst-balloons/) (LC #312)

## Real-World Applications
1. **Resource Allocation**:
   - Job scheduling
   - Memory management
2. **Financial Planning**:
   - Investment strategies
   - Portfolio optimization
3. **Bioinformatics**:
   - Sequence alignment
   - Protein folding
4. **Game Theory**:
   - Optimal strategies
   - Decision making
5. **Computer Graphics**:
   - Image processing
   - Path finding
6. **Natural Language Processing**:
   - Text similarity
   - Speech recognition

## Advanced Topics
1. **Probability DP**:
   - Expected value
   - State probability
2. **Tree DP**:
   - Tree diameter
   - Tree coloring
3. **Bitmask DP**:
   - State compression
   - Subset problems
4. **Digit DP**:
   - Number range problems
   - Digit counting
5. **Connection with Other Techniques**:
   - DP with binary search
   - DP with segment trees

## Important Resources
1. [Dynamic Programming Patterns](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns)
2. [How to Solve DP Problems](https://www.freecodecamp.org/news/demystifying-dynamic-programming-3efafb8d4296/)
3. [TopCoder DP Tutorial](https://www.topcoder.com/community/competitive-programming/tutorials/dynamic-programming-from-novice-to-advanced/)
4. [Algorithms Live! DP](https://www.youtube.com/watch?v=YBSt1jYwVfU)
5. [MIT OpenCourseWare DP](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-006-introduction-to-algorithms-fall-2011/lecture-videos/lecture-19-dynamic-programming-i-fibonacci-shortest-paths/)

## ❓ FAQ Section

**Q: How do I know if a problem can be solved with DP?**
A: Look for two properties: (1) Overlapping subproblems - same subproblems are solved multiple times, (2) Optimal substructure - optimal solution can be built from optimal solutions of subproblems. Keywords: "minimum", "maximum", "count ways", "longest", "shortest".

**Q: When should I use top-down (memoization) vs bottom-up (tabulation)?**
A: Top-down is easier to write (just add caching to recursion) and only computes needed states. Bottom-up is more efficient (no recursion overhead), avoids stack overflow, and is easier to optimize for space. Start with top-down, convert to bottom-up if needed.

**Q: How do I define the DP state?**
A: Ask "What information do I need to uniquely identify a subproblem?" Common states: current index, remaining capacity, previous choice, substring indices. State should capture everything needed to make the next decision.

**Q: How can I optimize DP space complexity?**
A: If current state only depends on previous row/state, use rolling arrays. For 2D DP that only uses dp[i-1], use two 1D arrays or a single array traversed carefully. For Fibonacci-like, use just 2-3 variables.

**Q: What's the difference between DP and Greedy?**
A: Greedy makes locally optimal choices hoping for global optimum (faster but may not work). DP explores all options and combines optimal subproblems (always works when applicable but slower). If greedy works, prefer it; otherwise use DP.

## Interview Tips
1. Identify overlapping subproblems
2. Define clear state representation
3. Write recurrence relation
4. Consider space optimization
5. Handle base cases properly
6. Draw state transition diagram
7. Test with small examples
8. Analyze time/space complexity
9. Consider both approaches
10. Optimize final solution