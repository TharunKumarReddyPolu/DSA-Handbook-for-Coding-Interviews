# Tree Data Structure 🌲

## Introduction
A tree is a hierarchical data structure composed of nodes connected by edges. Each node contains a value and references to its child nodes. Trees are widely used for representing hierarchical relationships and optimizing search operations.

## Prerequisites & Related Topics

### Prerequisites
- [Recursion](recursion.md) (essential for tree traversals and operations)
- [Linked List](linked-list.md) (similar node-pointer concepts)
- [Queue](queue.md) (for level-order/BFS traversal)
- [Stack](stack.md) (for iterative DFS traversals)

### Related Topics
- **Special case of**: [Graph](graph.md) (connected acyclic graph)
- **Implementations**: [Heap/Priority Queue](heap-pq.md) (complete binary tree), [Trie](trie.md) (prefix tree)
- **Traversal uses**: [Recursion](recursion.md), [Stack](stack.md), [Queue](queue.md)
- **See also**: [Dynamic Programming](dynamic-programming.md) (tree DP), [Backtracking](backtracking.md) (tree exploration)

## Pattern Recognition Guide

### 🎯 When to Use Tree Data Structures
**Keywords in problem**: "tree", "root", "node", "children", "ancestor", "BST", "binary tree"

**Use Trees when you see**:
- Hierarchical data representation
- Binary Search Tree operations
- Tree traversal problems
- Lowest Common Ancestor
- Path-based problems

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| DFS Traversal | "inorder/preorder/postorder", "serialize tree" | Tree Traversals, Serialize Tree |
| BFS Traversal | "level order", "right side view", "zigzag" | Level Order, Right Side View |
| BST Operations | "search BST", "insert BST", "validate BST" | Validate BST, Kth Smallest in BST |
| LCA | "lowest common ancestor", "distance between nodes" | LCA of Binary Tree |
| Path Problems | "path sum", "max path", "root to leaf" | Path Sum, Binary Tree Max Path Sum |
| Tree Construction | "build tree from traversals", "construct BST" | Build Tree from Preorder/Inorder |
| Tree Modification | "invert", "flatten", "prune" | Invert Binary Tree, Flatten to List |

### ❌ When NOT to Use
- Data has multiple parents → use [Graph](graph.md)
- Need fast lookup by value → use [Hashing](hashing.md) (unless BST property needed)
- Simple linear data → use [Arrays](arrays.md) or [Linked List](linked-list.md)
- Priority-based access → use [Heap](heap-pq.md) (simpler for just min/max)

### 💡 Traversal Choice Guide
| Need | Traversal | Use Case |
|------|-----------|----------|
| Sorted order (BST) | Inorder | Validate BST, Kth smallest |
| Copy/Serialize | Preorder | Clone tree, serialize |
| Delete tree | Postorder | Free memory, evaluate expression |
| Level by level | BFS | Level order, right view |

## Core Concepts

### Important Terminologies
- **Node**: Basic unit containing data and references to children
- **Root**: Top node of the tree
- **Leaf**: Node with no children
- **Internal Node**: Node with at least one child
- **Parent/Child**: Relationship between connected nodes
- **Ancestor/Descendant**: Nodes in path to root/leaves
- **Height**: Length of path from node to deepest leaf
- **Depth**: Length of path from node to root
- **Level**: Number of edges from root to node
- **Balanced Tree**: Height difference of subtrees ≤ 1
- **Binary Tree**: Each node has at most 2 children
- **Binary Search Tree (BST)**: Left subtree < node < right subtree
- **Complete Tree**: All levels filled except possibly last
- **Perfect Tree**: All levels completely filled

### Time Complexity Analysis
| Operation     | Average (BST) | Worst (BST) | Balanced BST |
|--------------|---------------|-------------|--------------|
| Search       | O(log n)      | O(n)        | O(log n)     |
| Insert       | O(log n)      | O(n)        | O(log n)     |
| Delete       | O(log n)      | O(n)        | O(log n)     |
| Traversal    | O(n)          | O(n)        | O(n)         |
| Height       | O(log n)      | O(n)        | O(log n)     |

### Space Complexity
- **Perfect Binary Tree**: O(2^h) where h is height
- **Balanced Binary Tree**: O(n) where n is number of nodes
- **Skewed Tree**: O(n) but behaves like linked list

## Implementation

### Basic Tree Node

**Pseudocode:**
```
Binary tree node structure:
- value: data stored in node
- left: reference to left child (or null)
- right: reference to right child (or null)
```

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

### Binary Search Tree Implementation

**Pseudocode:**
```
BST Insert:
1. If tree empty: create root node with value
2. Start at root, traverse:
   a. If value < current: go left (create node if null)
   b. If value >= current: go right (create node if null)

BST Search:
1. Start at root
2. While current node exists AND value not found:
   a. If value < current: go left
   b. Else: go right
3. Return current node (null if not found)
```

```python
class BST:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        if not self.root:
            self.root = TreeNode(val)
            return
        
        curr = self.root
        while True:
            if val < curr.val:
                if not curr.left:
                    curr.left = TreeNode(val)
                    break
                curr = curr.left
            else:
                if not curr.right:
                    curr.right = TreeNode(val)
                    break
                curr = curr.right
    
    def search(self, val):
        curr = self.root
        while curr and curr.val != val:
            curr = curr.left if val < curr.val else curr.right
        return curr
```

```java
class BST {
    TreeNode root;
    
    void insert(int val) {
        if (root == null) { root = new TreeNode(val); return; }
        TreeNode curr = root;
        while (true) {
            if (val < curr.val) {
                if (curr.left == null) { curr.left = new TreeNode(val); break; }
                curr = curr.left;
            } else {
                if (curr.right == null) { curr.right = new TreeNode(val); break; }
                curr = curr.right;
            }
        }
    }
    
    TreeNode search(int val) {
        TreeNode curr = root;
        while (curr != null && curr.val != val) {
            curr = val < curr.val ? curr.left : curr.right;
        }
        return curr;
    }
}
```

```cpp
class BST {
    TreeNode* root = nullptr;
public:
    void insert(int val) {
        if (!root) { root = new TreeNode(val); return; }
        TreeNode* curr = root;
        while (true) {
            if (val < curr->val) {
                if (!curr->left) { curr->left = new TreeNode(val); break; }
                curr = curr->left;
            } else {
                if (!curr->right) { curr->right = new TreeNode(val); break; }
                curr = curr->right;
            }
        }
    }
    
    TreeNode* search(int val) {
        TreeNode* curr = root;
        while (curr && curr->val != val) {
            curr = val < curr->val ? curr->left : curr->right;
        }
        return curr;
    }
};
```

## Tree Traversal Techniques

### 1. Depth-First Search (DFS)

**Pseudocode:**
```
Inorder (Left, Root, Right):
1. If node null, return
2. Recurse on left, process node, recurse on right

Preorder (Root, Left, Right):
1. If node null, return
2. Process node, recurse on left, recurse on right

Postorder (Left, Right, Root):
1. If node null, return
2. Recurse on left, recurse on right, process node
```

```python
def inorder(root):  # Left -> Root -> Right
    if not root:
        return []
    return inorder(root.left) + [root.val] + inorder(root.right)

def preorder(root):  # Root -> Left -> Right
    if not root:
        return []
    return [root.val] + preorder(root.left) + preorder(root.right)

def postorder(root):  # Left -> Right -> Root
    if not root:
        return []
    return postorder(root.left) + postorder(root.right) + [root.val]
```

```java
// Inorder Traversal
public List<Integer> inorder(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    inorderHelper(root, result);
    return result;
}
private void inorderHelper(TreeNode node, List<Integer> result) {
    if (node == null) return;
    inorderHelper(node.left, result);
    result.add(node.val);
    inorderHelper(node.right, result);
}
```

```cpp
// Inorder Traversal
vector<int> inorder(TreeNode* root) {
    vector<int> result;
    inorderHelper(root, result);
    return result;
}
void inorderHelper(TreeNode* node, vector<int>& result) {
    if (!node) return;
    inorderHelper(node->left, result);
    result.push_back(node->val);
    inorderHelper(node->right, result);
}
```

### 2. Breadth-First Search (BFS)

**Pseudocode:**
```
Level order traversal:
1. If root is null, return empty list
2. Create queue, add root
3. While queue not empty:
   a. Get level_size = queue size (nodes at current level)
   b. Process level_size nodes:
      - Dequeue node, add value to current level list
      - Enqueue left and right children if they exist
   c. Add level list to result
4. Return result (list of lists, one per level)
```

```python
from collections import deque

def level_order(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level = []
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

```java
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        result.add(level);
    }
    return result;
}
```

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    queue<TreeNode*> q;
    q.push(root);
    while (!q.empty()) {
        int levelSize = q.size();
        vector<int> level;
        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        result.push_back(level);
    }
    return result;
}
```

## Common Techniques

### 1. Lowest Common Ancestor (LCA)

**Pseudocode:**
```
1. If root is null or equals p or q, return root
2. Recursively search left and right subtrees
3. If both searches return non-null: root is LCA
4. Else: return whichever is non-null (or null if both null)
```

```python
def lowest_common_ancestor(root, p, q):
    if not root or root == p or root == q:
        return root
    
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    
    if left and right:
        return root
    return left if left else right
```

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) return root;
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) return root;
    return left != null ? left : right;
}
```

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    if (left && right) return root;
    return left ? left : right;
}
```

### 2. Path Sum

**Pseudocode:**
```
Check if path exists from root to leaf with given sum:
1. If root is null: return false
2. If root is leaf (no children):
   a. Return true if root.val equals remaining target_sum
3. Recursively check left OR right subtree with reduced target:
   a. Return has_path_sum(left, target - root.val) OR
          has_path_sum(right, target - root.val)
```

```python
def has_path_sum(root, target_sum):
    if not root:
        return False
    
    if not root.left and not root.right:
        return root.val == target_sum
    
    return (has_path_sum(root.left, target_sum - root.val) or
            has_path_sum(root.right, target_sum - root.val))
```

```java
public boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;
    if (root.left == null && root.right == null) return root.val == targetSum;
    return hasPathSum(root.left, targetSum - root.val) || hasPathSum(root.right, targetSum - root.val);
}
```

```cpp
bool hasPathSum(TreeNode* root, int targetSum) {
    if (!root) return false;
    if (!root->left && !root->right) return root->val == targetSum;
    return hasPathSum(root->left, targetSum - root->val) || hasPathSum(root->right, targetSum - root->val);
}
```

### 3. Tree Validation

**Pseudocode:**
```
Validate BST:
1. Pass valid range (min_val, max_val) to each node
2. If root null, return true
3. If root.val <= min_val OR root.val >= max_val, return false
4. Recursively validate:
   - Left subtree with (min_val, root.val)
   - Right subtree with (root.val, max_val)
```

```python
def is_valid_bst(root, min_val=float('-inf'), max_val=float('inf')):
    if not root:
        return True
    
    if root.val <= min_val or root.val >= max_val:
        return False
    
    return (is_valid_bst(root.left, min_val, root.val) and
            is_valid_bst(root.right, root.val, max_val))
```

```java
public boolean isValidBST(TreeNode root) {
    return isValid(root, Long.MIN_VALUE, Long.MAX_VALUE);
}
private boolean isValid(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return isValid(node.left, min, node.val) && isValid(node.right, node.val, max);
}
```

```cpp
bool isValidBST(TreeNode* root, long minVal = LONG_MIN, long maxVal = LONG_MAX) {
    if (!root) return true;
    if (root->val <= minVal || root->val >= maxVal) return false;
    return isValidBST(root->left, minVal, root->val) && isValidBST(root->right, root->val, maxVal);
}
```

### 4. Range Query Trees (Fenwick Tree & Segment Tree)

When an array is updated dynamically and you need fast prefix or range queries (such as *Range Sum Query - Mutable*, LC #307), a **Fenwick Tree (Binary Indexed Tree)** supports point updates and prefix sums in $O(\log n)$ time with $O(n)$ space, while a **Segment Tree** generalizes to any associative range operation (sum, min, max, GCD) in $O(\log n)$ time.

**Pseudocode:**
```
Fenwick Tree (1-indexed internal array bit[1..n]):
- add(i, delta): while i <= n: bit[i] += delta; i += i & -i
- prefix_sum(i): sum = 0; while i > 0: sum += bit[i]; i -= i & -i; return sum
- range_sum(left, right): return prefix_sum(right + 1) - prefix_sum(left)

Iterative Segment Tree (size 2n, leaves at [n..2n-1]):
- build: copy nums to tree[n..2n-1]; for i = n-1 down to 1: tree[i] = tree[2i] + tree[2i+1]
- update(i, val): set i += n, tree[i] = val; while i > 1: i //= 2, tree[i] = tree[2i] + tree[2i+1]
- query(left, right) inclusive:
  Shift left += n, right += n; accumulate tree[left++] when left is right child,
  and tree[right--] when right is left child; halve both each step
```

```python
class FenwickTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.bit = [0] * (self.n + 1)
        for i, x in enumerate(nums, 1):
            self.bit[i] += x
            parent = i + (i & -i)
            if parent <= self.n:
                self.bit[parent] += self.bit[i]

    def add(self, index, delta):
        i = index + 1
        while i <= self.n:
            self.bit[i] += delta
            i += i & -i

    def prefix_sum(self, index):
        total = 0
        i = index + 1
        while i > 0:
            total += self.bit[i]
            i -= i & -i
        return total

    def range_sum(self, left, right):
        return self.prefix_sum(right) - (self.prefix_sum(left - 1) if left > 0 else 0)


class SegmentTree:
    def __init__(self, nums):
        self.n = len(nums)
        self.tree = [0] * (2 * self.n)
        self.tree[self.n:2 * self.n] = nums
        for i in range(self.n - 1, 0, -1):
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def update(self, index, val):
        i = index + self.n
        self.tree[i] = val
        while i > 1:
            i //= 2
            self.tree[i] = self.tree[2 * i] + self.tree[2 * i + 1]

    def range_sum(self, left, right):
        total = 0
        l, r = left + self.n, right + self.n
        while l <= r:
            if l & 1:
                total += self.tree[l]
                l += 1
            if not (r & 1):
                total += self.tree[r]
                r -= 1
            l //= 2
            r //= 2
        return total
```

```java
class FenwickTree {
    private final long[] bit;

    public FenwickTree(int[] nums) {
        bit = new long[nums.length + 1];
        for (int i = 1; i <= nums.length; i++) {
            bit[i] += nums[i - 1];
            int parent = i + (i & -i);
            if (parent <= nums.length) bit[parent] += bit[i];
        }
    }

    public void add(int index, long delta) {
        for (int i = index + 1; i < bit.length; i += i & -i) bit[i] += delta;
    }

    public long prefixSum(int index) {
        long sum = 0;
        for (int i = index + 1; i > 0; i -= i & -i) sum += bit[i];
        return sum;
    }

    public long rangeSum(int left, int right) {
        return prefixSum(right) - (left > 0 ? prefixSum(left - 1) : 0);
    }
}

class SegmentTree {
    private final int n;
    private final long[] tree;

    public SegmentTree(int[] nums) {
        n = nums.length;
        tree = new long[2 * n];
        for (int i = 0; i < n; i++) tree[n + i] = nums[i];
        for (int i = n - 1; i > 0; i--) tree[i] = tree[2 * i] + tree[2 * i + 1];
    }

    public void update(int index, long val) {
        int i = index + n;
        tree[i] = val;
        while (i > 1) {
            i /= 2;
            tree[i] = tree[2 * i] + tree[2 * i + 1];
        }
    }

    public long rangeSum(int left, int right) {
        long sum = 0;
        for (int l = left + n, r = right + n; l <= r; l /= 2, r /= 2) {
            if ((l & 1) == 1) sum += tree[l++];
            if ((r & 1) == 0) sum += tree[r--];
        }
        return sum;
    }
}
```

```cpp
class FenwickTree {
    vector<long long> bit;
public:
    explicit FenwickTree(const vector<int>& nums) : bit(nums.size() + 1, 0) {
        for (size_t i = 1; i <= nums.size(); i++) {
            bit[i] += nums[i - 1];
            size_t parent = i + (i & -i);
            if (parent <= nums.size()) bit[parent] += bit[i];
        }
    }

    void add(int index, long long delta) {
        for (size_t i = index + 1; i < bit.size(); i += i & -i) bit[i] += delta;
    }

    long long prefixSum(int index) const {
        long long sum = 0;
        for (int i = index + 1; i > 0; i -= i & -i) sum += bit[i];
        return sum;
    }

    long long rangeSum(int left, int right) const {
        return prefixSum(right) - (left > 0 ? prefixSum(left - 1) : 0);
    }
};

class SegmentTree {
    int n;
    vector<long long> tree;
public:
    explicit SegmentTree(const vector<int>& nums) : n(nums.size()), tree(2 * nums.size(), 0) {
        for (int i = 0; i < n; i++) tree[n + i] = nums[i];
        for (int i = n - 1; i > 0; i--) tree[i] = tree[2 * i] + tree[2 * i + 1];
    }

    void update(int index, long long val) {
        int i = index + n;
        tree[i] = val;
        while (i > 1) {
            i /= 2;
            tree[i] = tree[2 * i] + tree[2 * i + 1];
        }
    }

    long long rangeSum(int left, int right) const {
        long long sum = 0;
        for (int l = left + n, r = right + n; l <= r; l /= 2, r /= 2) {
            if (l & 1) sum += tree[l++];
            if (!(r & 1)) sum += tree[r--];
        }
        return sum;
    }
};
```

## Edge Cases to Consider
1. Empty tree
2. Single node tree
3. Complete binary tree
4. Perfect binary tree
5. Skewed tree (left/right)
6. Duplicate values
7. Negative values
8. Maximum/minimum values
9. Unbalanced tree
10. Tree with cycles (invalid)

## Common Pitfalls
1. Not handling null nodes
2. Incorrect recursion base cases
3. Stack overflow in deep trees
4. Not considering duplicates
5. Assuming balanced tree
6. Incorrect BST validation
7. Memory leaks in deletion

## Practice Problems by Difficulty

### Easy
1. [Same Tree](https://leetcode.com/problems/same-tree/) (LC #100)
2. [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) (LC #104)
3. [Path Sum](https://leetcode.com/problems/path-sum/) (LC #112)
4. [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) (LC #226)

### Medium
1. [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) (LC #102)
2. [Construct Binary Tree from Preorder and Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/) (LC #105)
3. [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/) (LC #103)
4. [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/) (LC #98)

### Hard
1. [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/) (LC #124)
2. [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) (LC #297)
3. [Binary Tree Cameras](https://leetcode.com/problems/binary-tree-cameras/) (LC #968)

## Real-World Applications
1. **File Systems**: Directory structure
2. **HTML DOM**: Web page structure
3. **Database Indexing**: B-trees and variants
4. **Compiler Design**: Abstract syntax trees
5. **Network Routing**: Routing tables
6. **AI/Game Theory**: Decision trees
7. **Compression**: Huffman coding

## Advanced Topics
1. **Self-Balancing Trees**:
   - AVL Trees
   - Red-Black Trees
   - B-Trees
2. **Segment Trees**: Range queries
3. **Fenwick Trees**: Prefix sums
4. **Threaded Binary Trees**
5. **Splay Trees**
6. **Trie (Prefix Trees)**

## Important Resources
1. [Tree Traversals](https://www.geeksforgeeks.org/tree-traversals-inorder-preorder-and-postorder/)
2. [Binary Search Tree](https://www.programiz.com/dsa/binary-search-tree)
3. [AVL Tree Tutorial](https://www.geeksforgeeks.org/avl-tree-set-1-insertion/)
4. [Red-Black Tree Guide](https://www.geeksforgeeks.org/red-black-tree-set-1-introduction-2/)
5. [B-Tree and B+ Tree](https://www.geeksforgeeks.org/introduction-of-b-tree-2/)

## ❓ FAQ Section

**Q: When should I use DFS vs BFS for tree problems?**
A: Use DFS (recursion/stack) for problems involving paths, depth, or when you need to process nodes before their children (preorder) or after (postorder). Use BFS (queue) for level-by-level processing, shortest path in unweighted trees, or when you need to process nodes at the same level together.

**Q: How do I choose between recursive and iterative tree traversal?**
A: Recursive is cleaner and easier to write for most tree problems. Use iterative when you're worried about stack overflow (very deep trees) or need more control over the traversal order. Morris traversal is O(1) space but modifies the tree temporarily.

**Q: What's the difference between a complete, full, and perfect binary tree?**
A: Complete: all levels filled except possibly the last, which fills left-to-right. Full: every node has 0 or 2 children. Perfect: all internal nodes have 2 children and all leaves at same level. A perfect tree is both complete and full.

**Q: How do I handle null nodes in tree problems?**
A: Always check for null at the start of recursive functions (`if not root: return`). This is your base case. For iterative solutions, check before adding children to queue/stack.

**Q: When should I use a parent pointer?**
A: Use parent pointers when you need to traverse upward (e.g., finding ancestors, LCA with given node references) or when the problem requires bidirectional movement. Otherwise, avoid them to save space.

## Interview Tips
1. Clarify tree type and properties
2. Consider recursive vs iterative approaches
3. Analyze space complexity (recursion stack)
4. Handle edge cases explicitly
5. Use helper functions for complex logic
6. Consider in-place vs new tree solutions
7. Test with small examples first
8. Draw the tree for visualization