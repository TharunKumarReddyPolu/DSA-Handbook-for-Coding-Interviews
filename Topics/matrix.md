# Matrix Data Structure and Algorithms 🧮

## Introduction
A matrix is a 2D array structure that organizes data in rows and columns. Matrix operations are fundamental in computer graphics, scientific computing, and many algorithmic problems. Understanding matrix manipulation is crucial for solving grid-based problems efficiently.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (matrices are 2D arrays)
- Basic understanding of nested loops
- Coordinate systems (row, column indexing)

### Related Topics
- **Builds on**: [Arrays](arrays.md) (extension to 2D)
- **Traversal uses**: [Graph](graph.md) (DFS/BFS for grid problems), [Recursion](recursion.md)
- **Optimization with**: [Dynamic Programming](dynamic-programming.md) (grid DP problems)
- **See also**: [Math](math.md) (matrix multiplication, linear algebra)

## Pattern Recognition Guide

### 🎯 When to Use Matrix Techniques
**Keywords in problem**: "grid", "matrix", "2D array", "rows and columns", "cell", "board"

**Use Matrix techniques when you see**:
- 2D grid traversal problems
- Image manipulation (rotate, flip)
- Path finding in grids
- Game boards (Sudoku, Tic-tac-toe)
- Dynamic programming on grids

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Traversal | "spiral", "diagonal", "zigzag" | Spiral Matrix, Diagonal Traverse |
| Rotation/Transformation | "rotate 90°", "flip", "transpose" | Rotate Image, Flip and Invert |
| Search in Matrix | "sorted matrix", "search element" | Search 2D Matrix, Kth Smallest |
| Island/Connected Components | "number of islands", "surrounded regions" | Number of Islands, Flood Fill |
| Path Finding | "unique paths", "minimum path sum" | Unique Paths, Minimum Path Sum |
| Game Simulation | "game of life", "battleship", "sudoku" | Game of Life, Valid Sudoku |

### ❌ When NOT to Use
- Data is 1D linear → use [Arrays](arrays.md)
- Complex relationships beyond grid → use [Graph](graph.md)
- Sparse matrix with few elements → consider [Hashing](hashing.md)

### 💡 Direction Arrays (Memorize)
```
4-directions: dx = [-1, 0, 1, 0], dy = [0, 1, 0, -1]
8-directions: Include diagonals [-1,-1], [-1,1], [1,-1], [1,1]
```

## Core Concepts

### Important Terminologies
- **Matrix**: 2D array of elements arranged in rows and columns
- **Row**: Horizontal line of elements
- **Column**: Vertical line of elements
- **Diagonal**: Elements where row index equals column index
- **Transpose**: Matrix flipped over its diagonal
- **Identity Matrix**: Square matrix with 1s on diagonal, 0s elsewhere
- **Sparse Matrix**: Matrix with mostly zero elements
- **Dense Matrix**: Matrix with mostly non-zero elements
- **Square Matrix**: Equal number of rows and columns
- **Rectangular Matrix**: Different number of rows and columns
- **Symmetric Matrix**: Equal to its transpose

### Matrix Properties
1. **Dimensions**: m × n (m rows, n columns)
2. **Access Pattern**: matrix[row][column]
3. **Memory Layout**: Contiguous in row-major or column-major order
4. **Common Operations**: Transpose, Rotation, Search, Traversal

### Time Complexity Analysis
| Operation               | Time Complexity | Space Complexity |
|------------------------|----------------|------------------|
| Access Element         | O(1)           | O(1)             |
| Row Traversal         | O(n)           | O(1)             |
| Column Traversal      | O(m)           | O(1)             |
| Matrix Multiplication | O(n³)          | O(n²)            |
| Transpose            | O(m×n)         | O(1) or O(m×n)   |
| Rotation             | O(m×n)         | O(1) or O(m×n)   |
| Search               | O(m×n)         | O(1)             |
| DFS/BFS              | O(m×n)         | O(m×n)           |

## Implementation Patterns

### 1. Matrix Creation and Basic Operations

**Pseudocode:**
```
Create matrix: initialize rows × cols 2D array with default value
Transpose: swap matrix[i][j] with matrix[j][i] for all i < j
Get dimensions: (len(matrix), len(matrix[0]))
Access element: matrix[row][col]
```

```python
def create_matrix(rows, cols, default_value=0):
    return [[default_value] * cols for _ in range(rows)]

def print_matrix(matrix):
    for row in matrix:
        print(*row)

def transpose_matrix(matrix):
    return list(map(list, zip(*matrix)))

def get_dimensions(matrix):
    return len(matrix), len(matrix[0]) if matrix else 0
```

### 2. Matrix Rotation

**Pseudocode:**
```
Rotate 90° clockwise (in-place):
1. Transpose: for i < j, swap matrix[i][j] with matrix[j][i]
2. Reverse each row

Rotate 90° counter-clockwise:
1. Transpose the matrix
2. Reverse each column (or: reverse rows first, then transpose)
```

```python
def rotate_90_clockwise(matrix):
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each row
    for i in range(n):
        matrix[i].reverse()
```

```java
public void rotate(int[][] matrix) {
    int n = matrix.length;
    // Transpose
    for (int i = 0; i < n; i++)
        for (int j = i; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    // Reverse each row
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n / 2; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][n - 1 - j];
            matrix[i][n - 1 - j] = temp;
        }
}
```

```cpp
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    // Transpose
    for (int i = 0; i < n; i++)
        for (int j = i; j < n; j++)
            swap(matrix[i][j], matrix[j][i]);
    // Reverse each row
    for (int i = 0; i < n; i++)
        reverse(matrix[i].begin(), matrix[i].end());
}
```

```python
def rotate_90_counterclockwise(matrix):
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each column
    for j in range(n):
        for i in range(n//2):
            matrix[i][j], matrix[n-1-i][j] = matrix[n-1-i][j], matrix[i][j]
```

### 3. Spiral Traversal

**Pseudocode:**
```
1. Initialize boundaries: top=0, bottom=m-1, left=0, right=n-1
2. While top <= bottom AND left <= right:
   a. Traverse right: row=top, col from left to right
   b. top++
   c. Traverse down: col=right, row from top to bottom
   d. right--
   e. If top <= bottom: traverse left: row=bottom, col from right to left
   f. bottom--
   g. If left <= right: traverse up: col=left, row from bottom to top
   h. left++
```

```python
def spiral_order(matrix):
    if not matrix:
        return []
    
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    
    while top <= bottom and left <= right:
        # Traverse right
        for j in range(left, right + 1):
            result.append(matrix[top][j])
        top += 1
        
        # Traverse down
        for i in range(top, bottom + 1):
            result.append(matrix[i][right])
        right -= 1
        
        if top <= bottom:
            # Traverse left
            for j in range(right, left - 1, -1):
                result.append(matrix[bottom][j])
            bottom -= 1
        
        if left <= right:
            # Traverse up
            for i in range(bottom, top - 1, -1):
                result.append(matrix[i][left])
            left += 1
    
    return result
```

```java
public List<Integer> spiralOrder(int[][] matrix) {
    List<Integer> result = new ArrayList<>();
    if (matrix.length == 0) return result;
    int top = 0, bottom = matrix.length - 1, left = 0, right = matrix[0].length - 1;
    while (top <= bottom && left <= right) {
        for (int j = left; j <= right; j++) result.add(matrix[top][j]);
        top++;
        for (int i = top; i <= bottom; i++) result.add(matrix[i][right]);
        right--;
        if (top <= bottom) { for (int j = right; j >= left; j--) result.add(matrix[bottom][j]); bottom--; }
        if (left <= right) { for (int i = bottom; i >= top; i--) result.add(matrix[i][left]); left++; }
    }
    return result;
}
```

```cpp
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    vector<int> result;
    if (matrix.empty()) return result;
    int top = 0, bottom = matrix.size() - 1, left = 0, right = matrix[0].size() - 1;
    while (top <= bottom && left <= right) {
        for (int j = left; j <= right; j++) result.push_back(matrix[top][j]);
        top++;
        for (int i = top; i <= bottom; i++) result.push_back(matrix[i][right]);
        right--;
        if (top <= bottom) { for (int j = right; j >= left; j--) result.push_back(matrix[bottom][j]); bottom--; }
        if (left <= right) { for (int i = bottom; i >= top; i--) result.push_back(matrix[i][left]); left++; }
    }
    return result;
}
```

### 4. Matrix Search

**Pseudocode:**
```
Search in row-wise and column-wise sorted matrix:
1. Start at top-right corner (row=0, col=n-1)
2. While within bounds:
   a. If matrix[row][col] == target: found
   b. If matrix[row][col] > target: col-- (eliminate column)
   c. If matrix[row][col] < target: row++ (eliminate row)
3. Return not found
```

```python
def search_sorted_matrix(matrix, target):
    if not matrix:
        return False
    
    m, n = len(matrix), len(matrix[0])
    row, col = 0, n - 1
    
    while row < m and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            col -= 1
        else:
            row += 1
    
    return False
```

```java
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0) return false;
    int row = 0, col = matrix[0].length - 1;
    while (row < matrix.length && col >= 0) {
        if (matrix[row][col] == target) return true;
        else if (matrix[row][col] > target) col--;
        else row++;
    }
    return false;
}
```

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if (matrix.empty()) return false;
    int row = 0, col = matrix[0].size() - 1;
    while (row < matrix.size() && col >= 0) {
        if (matrix[row][col] == target) return true;
        else if (matrix[row][col] > target) col--;
        else row++;
    }
    return false;
}
```

### 5. Island Problems (DFS)

**Pseudocode:**
```
Count islands:
1. For each cell (i, j) in grid:
   a. If cell is land ('1'):
      - Increment island count
      - DFS to mark all connected land as visited
2. DFS(i, j):
   a. If out of bounds OR not land: return
   b. Mark cell as visited (e.g., change to '#')
   c. DFS on all 4 neighbors (up, down, left, right)
```

```python
def num_islands(grid):
    if not grid:
        return 0
    
    def dfs(i, j):
        if (i < 0 or i >= len(grid) or 
            j < 0 or j >= len(grid[0]) or
            grid[i][j] != '1'):
            return
        
        grid[i][j] = '#'  # Mark as visited
        
        # Check all 4 directions
        dfs(i+1, j)
        dfs(i-1, j)
        dfs(i, j+1)
        dfs(i, j-1)
    
    count = 0
    for i in range(len(grid)):
        for j in range(len(grid[0])):
            if grid[i][j] == '1':
                dfs(i, j)
                count += 1
    
    return count
```

```java
public int numIslands(char[][] grid) {
    int count = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') { dfs(grid, i, j); count++; }
        }
    }
    return count;
}
private void dfs(char[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] != '1') return;
    grid[i][j] = '#';
    dfs(grid, i+1, j); dfs(grid, i-1, j); dfs(grid, i, j+1); dfs(grid, i, j-1);
}
```

```cpp
int numIslands(vector<vector<char>>& grid) {
    int count = 0;
    for (int i = 0; i < grid.size(); i++) {
        for (int j = 0; j < grid[0].size(); j++) {
            if (grid[i][j] == '1') { dfs(grid, i, j); count++; }
        }
    }
    return count;
}
void dfs(vector<vector<char>>& grid, int i, int j) {
    if (i < 0 || i >= grid.size() || j < 0 || j >= grid[0].size() || grid[i][j] != '1') return;
    grid[i][j] = '#';
    dfs(grid, i+1, j); dfs(grid, i-1, j); dfs(grid, i, j+1); dfs(grid, i, j-1);
}
```

## Common Techniques

### 1. Direction Arrays

**Pseudocode:**
```
4-directional movement (up, right, down, left):
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

8-directional (including diagonals):
Add: [-1,-1], [-1,1], [1,-1], [1,1]

Move: new_x = x + dx[i], new_y = y + dy[i]
Check bounds: 0 <= new_x < rows AND 0 <= new_y < cols
```

```python
# 4 directions (up, right, down, left)
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

# 8 directions (including diagonals)
dx8 = [-1, -1, -1, 0, 0, 1, 1, 1]
dy8 = [-1, 0, 1, -1, 1, -1, 0, 1]

def is_valid(x, y, rows, cols):
    return 0 <= x < rows and 0 <= y < cols
```

```java
// 4 directions (up, right, down, left)
int[] dx = {-1, 0, 1, 0};
int[] dy = {0, 1, 0, -1};
// 8 directions
int[] dx8 = {-1, -1, -1, 0, 0, 1, 1, 1};
int[] dy8 = {-1, 0, 1, -1, 1, -1, 0, 1};
boolean isValid(int x, int y, int rows, int cols) {
    return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

```cpp
// 4 directions (up, right, down, left)
int dx[] = {-1, 0, 1, 0};
int dy[] = {0, 1, 0, -1};
// 8 directions
int dx8[] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy8[] = {-1, 0, 1, -1, 1, -1, 0, 1};
bool isValid(int x, int y, int rows, int cols) {
    return x >= 0 && x < rows && y >= 0 && y < cols;
}
```

### 2. Layer by Layer Processing

**Pseudocode:**
```
Process matrix in concentric layers:
1. layers = min(m, n) / 2
2. For each layer from 0 to layers-1:
   a. first = layer (starting index)
   b. last = min(m, n) - 1 - layer (ending index)
   c. Process elements in current layer ring
```

```python
def process_matrix_layers(matrix):
    m, n = len(matrix), len(matrix[0])
    layers = min(m, n) // 2
    
    for layer in range(layers):
        first, last = layer, min(m, n) - 1 - layer
        for i in range(first, last):
            # Process current layer
            pass
```

```java
void processMatrixLayers(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int layers = Math.min(m, n) / 2;
    for (int layer = 0; layer < layers; layer++) {
        int first = layer, last = Math.min(m, n) - 1 - layer;
        for (int i = first; i < last; i++) {
            // Process current layer
        }
    }
}
```

```cpp
void processMatrixLayers(vector<vector<int>>& matrix) {
    int m = matrix.size(), n = matrix[0].size();
    int layers = min(m, n) / 2;
    for (int layer = 0; layer < layers; layer++) {
        int first = layer, last = min(m, n) - 1 - layer;
        for (int i = first; i < last; i++) {
            // Process current layer
        }
    }
}
```

## Edge Cases to Consider
1. Empty matrix
2. Single element matrix
3. Single row matrix
4. Single column matrix
5. Square vs rectangular matrix
6. Matrix with negative numbers
7. Matrix with duplicates
8. Sparse matrix
9. Identity matrix
10. Matrix with all same elements

## Common Pitfalls
1. Row/column index confusion
2. Out of bounds access
3. Incorrect rotation logic
4. Not handling empty matrix
5. Memory inefficient solutions
6. Incorrect traversal order
7. Missing edge cases
8. Modifying input when not allowed

## Practice Problems by Difficulty

### Easy
1. [Matrix Diagonal Sum](https://leetcode.com/problems/matrix-diagonal-sum/) (LC #1572)
2. [Flood Fill](https://leetcode.com/problems/flood-fill/) (LC #733)
3. [Transpose Matrix](https://leetcode.com/problems/transpose-matrix/) (LC #867)

### Medium
1. [Rotate Image](https://leetcode.com/problems/rotate-image/) (LC #48)
2. [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/) (LC #54)
3. [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/) (LC #73)
4. [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/) (LC #74)

### Hard
1. [Word Search II](https://leetcode.com/problems/word-search-ii/) (LC #212)
2. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/) (LC #37)
3. [Maximum Rectangle](https://leetcode.com/problems/maximal-rectangle/) (LC #85)

## Real-World Applications
1. **Image Processing**:
   - Pixel manipulation
   - Filters and transformations
2. **Computer Graphics**:
   - 2D transformations
   - Game board representations
3. **Scientific Computing**:
   - Linear algebra operations
   - Numerical methods
4. **Machine Learning**:
   - Feature matrices
   - Convolutional operations
5. **Game Development**:
   - Game boards
   - Collision detection
6. **Data Analysis**:
   - Correlation matrices
   - Heat maps

## Advanced Topics
1. **Matrix Optimization**:
   - Cache-friendly access
   - Memory-efficient storage
2. **Sparse Matrix Techniques**:
   - Compressed storage
   - Efficient operations
3. **Special Matrices**:
   - Toeplitz matrix
   - Circulant matrix
4. **Matrix Decomposition**:
   - LU decomposition
   - QR decomposition

## Important Resources
1. [Matrix Operations Tutorial](https://www.geeksforgeeks.org/matrix/)
2. [Efficient Matrix Manipulation](https://www.cs.cmu.edu/~scandal/cacm/node9.html)
3. [Sparse Matrix Algorithms](https://www.geeksforgeeks.org/sparse-matrix-representations/)
4. [Matrix Transformations](https://www.cs.cmu.edu/~462/projects/assn2/assn2/matrixfaq.html)
5. [NumPy Matrix Operations](https://numpy.org/doc/stable/reference/routines.array-manipulation.html)

## ❓ FAQ Section

**Q: How do I traverse a matrix in spiral order?**
A: Maintain four boundaries (top, bottom, left, right). Go right along top row (increment top), down along right column (decrement right), left along bottom row (decrement bottom), up along left column (increment left). Repeat until boundaries cross.

**Q: How do I rotate a matrix 90 degrees clockwise in place?**
A: Transpose the matrix (swap matrix[i][j] with matrix[j][i]), then reverse each row. For counter-clockwise: transpose then reverse each column (or reverse rows first, then transpose).

**Q: How do I search in a row-wise and column-wise sorted matrix?**
A: Start from top-right (or bottom-left) corner. If target < current, move left. If target > current, move down. This eliminates one row or column each step: O(m+n) time.

**Q: What are direction arrays and how do I use them?**
A: Direction arrays encode movement: `dx = [-1,0,1,0]`, `dy = [0,1,0,-1]` for 4-directional (up, right, down, left). Loop through directions: `newX = x + dx[i]`, `newY = y + dy[i]`. Add diagonals for 8-directional.

**Q: How do I handle island/flood fill problems?**
A: Use DFS or BFS from each unvisited land cell. Mark cells as visited (modify in place or use visited set). Count connected components for number of islands. Track component size for island area.

## Interview Tips
1. Clarify matrix properties
2. Consider space optimization
3. Handle edge cases explicitly
4. Use direction arrays
5. Consider in-place operations
6. Draw examples
7. Test boundary conditions
8. Optimize traversal patterns
9. Consider symmetry properties
10. Discuss trade-offs