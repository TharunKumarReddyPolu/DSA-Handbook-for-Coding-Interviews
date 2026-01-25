# Queue Data Structure ⏩

## Introduction
A queue is a fundamental data structure that follows the First-In-First-Out (FIFO) principle. Think of it like a line of people waiting - the first person to join the line is the first one to be served.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (array-based queue implementation)
- [Linked List](linked-list.md) (linked list-based implementation)
- Basic understanding of FIFO concept

### Related Topics
- **Compare with**: [Stack](stack.md) (LIFO vs FIFO)
- **Essential for**: [Graph](graph.md) (BFS traversal), [Tree](tree.md) (level-order traversal)
- **Extensions**: [Heap/Priority Queue](heap-pq.md) (priority-based ordering)
- **See also**: [Sliding Window](arrays.md) (deque for window problems)

## Pattern Recognition Guide

### 🎯 When to Use Queue
**Keywords in problem**: "FIFO", "first-come-first-serve", "level order", "BFS", "process in order"

**Use Queue when you see**:
- Breadth-First Search (BFS) traversal
- Level-order tree traversal
- Processing in arrival order
- Sliding window maximum/minimum (with deque)
- Task scheduling with order preservation

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| BFS Traversal | "shortest path (unweighted)", "minimum moves" | Word Ladder, Open the Lock |
| Level Order | "level by level", "print levels", "zigzag" | Binary Tree Level Order, Zigzag Traversal |
| Sliding Window Max/Min | "maximum in window", "sliding maximum" | Sliding Window Maximum |
| Simulation | "queue of tasks", "process in order" | Number of Recent Calls |
| Multi-source BFS | "rotting oranges", "walls and gates", "infection" | Rotting Oranges, 01 Matrix |
| Circular Queue | "ring buffer", "fixed size queue" | Design Circular Queue |

### ❌ When NOT to Use
- Need LIFO (last-in-first-out) → use [Stack](stack.md)
- Need priority-based processing → use [Heap](heap-pq.md)
- Need random access → use [Arrays](arrays.md)
- DFS traversal → use [Stack](stack.md) or [Recursion](recursion.md)

## Core Concepts

### Important Terminologies
- **Queue**: A FIFO (First-In-First-Out) data structure
- **Enqueue**: Operation to add an element to the back of queue
- **Dequeue**: Operation to remove an element from the front of queue
- **Front/Head**: First element in the queue
- **Rear/Tail**: Last element in the queue
- **Deque**: Double-ended queue allowing operations at both ends
- **Priority Queue**: Queue where elements have priorities
- **Circular Queue**: Fixed-size queue implemented circularly

### Time Complexity Analysis
| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Enqueue   | O(1)         | O(1)       |
| Dequeue   | O(1)         | O(1)       |
| Peek      | O(1)         | O(1)       |
| Search    | O(n)         | O(n)       |
| isEmpty   | O(1)         | O(1)       |

### Space Complexity
- **Basic Queue**: O(n) where n is number of elements
- **Circular Queue**: O(n) where n is fixed size
- **Priority Queue**: O(n) for binary heap implementation

## Implementation

### Basic Queue Implementation

**Pseudocode:**
```
Queue operations (FIFO):
- enqueue(item): add item to back of queue
- dequeue(): remove and return item from front
- front(): return front item without removing
- is_empty(): check if queue has no elements
- size(): return number of elements
```

```python
class Queue:
    def __init__(self):
        self.items = []
    
    def enqueue(self, item):
        self.items.append(item)
    
    def dequeue(self):
        if not self.is_empty():
            return self.items.pop(0)
        raise IndexError("Dequeue from empty queue")
    
    def front(self):
        if not self.is_empty():
            return self.items[0]
        raise IndexError("Front from empty queue")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

```java
class Queue<T> {
    private LinkedList<T> items = new LinkedList<>();
    
    public void enqueue(T item) { items.addLast(item); }
    
    public T dequeue() {
        if (isEmpty()) throw new RuntimeException("Dequeue from empty queue");
        return items.removeFirst();
    }
    
    public T front() {
        if (isEmpty()) throw new RuntimeException("Front from empty queue");
        return items.getFirst();
    }
    
    public boolean isEmpty() { return items.isEmpty(); }
    public int size() { return items.size(); }
}
```

```cpp
template<typename T>
class Queue {
    deque<T> items;
public:
    void enqueue(T item) { items.push_back(item); }
    
    T dequeue() {
        if (isEmpty()) throw runtime_error("Dequeue from empty queue");
        T item = items.front();
        items.pop_front();
        return item;
    }
    
    T front() {
        if (isEmpty()) throw runtime_error("Front from empty queue");
        return items.front();
    }
    
    bool isEmpty() { return items.empty(); }
    int size() { return items.size(); }
};
```

### Circular Queue Implementation

**Pseudocode:**
```
Fixed-size circular queue:
- Use front and rear pointers, both start at -1
- enqueue: if not full, rear = (rear + 1) % size, add item
- dequeue: if not empty, get item at front, front = (front + 1) % size
- is_full: (rear + 1) % size == front
- is_empty: front == -1
Wrapping allows reuse of space when elements are dequeued
```

```python
class CircularQueue:
    def __init__(self, size):
        self.size = size
        self.queue = [None] * size
        self.front = self.rear = -1
    
    def enqueue(self, item):
        if self.is_full():
            raise Exception("Queue is full")
        
        if self.front == -1:
            self.front = 0
        self.rear = (self.rear + 1) % self.size
        self.queue[self.rear] = item
    
    def dequeue(self):
        if self.is_empty():
            raise Exception("Queue is empty")
        
        item = self.queue[self.front]
        if self.front == self.rear:
            self.front = self.rear = -1
        else:
            self.front = (self.front + 1) % self.size
        return item
    
    def is_empty(self):
        return self.front == -1
    
    def is_full(self):
        return (self.rear + 1) % self.size == self.front
```

```java
class CircularQueue {
    int[] queue;
    int front = -1, rear = -1, size;
    
    CircularQueue(int size) {
        this.size = size;
        queue = new int[size];
    }
    
    void enqueue(int item) {
        if (isFull()) throw new RuntimeException("Queue is full");
        if (front == -1) front = 0;
        rear = (rear + 1) % size;
        queue[rear] = item;
    }
    
    int dequeue() {
        if (isEmpty()) throw new RuntimeException("Queue is empty");
        int item = queue[front];
        if (front == rear) { front = rear = -1; }
        else { front = (front + 1) % size; }
        return item;
    }
    
    boolean isEmpty() { return front == -1; }
    boolean isFull() { return (rear + 1) % size == front; }
}
```

```cpp
class CircularQueue {
    vector<int> queue;
    int front = -1, rear = -1, size;
public:
    CircularQueue(int size) : size(size), queue(size) {}
    
    void enqueue(int item) {
        if (isFull()) throw runtime_error("Queue is full");
        if (front == -1) front = 0;
        rear = (rear + 1) % size;
        queue[rear] = item;
    }
    
    int dequeue() {
        if (isEmpty()) throw runtime_error("Queue is empty");
        int item = queue[front];
        if (front == rear) { front = rear = -1; }
        else { front = (front + 1) % size; }
        return item;
    }
    
    bool isEmpty() { return front == -1; }
    bool isFull() { return (rear + 1) % size == front; }
};
```

### Deque Implementation

**Pseudocode:**
```
Double-ended queue operations:
- add_front(item): insert item at front of deque
- add_rear(item): insert item at back of deque
- remove_front(): remove and return item from front
- remove_rear(): remove and return item from back
- is_empty(): check if deque has no elements
```

```python
from collections import deque

class Deque:
    def __init__(self):
        self.items = deque()
    
    def add_front(self, item):
        self.items.appendleft(item)
    
    def add_rear(self, item):
        self.items.append(item)
    
    def remove_front(self):
        return self.items.popleft()
    
    def remove_rear(self):
        return self.items.pop()
    
    def is_empty(self):
        return len(self.items) == 0
```

```java
class Deque<T> {
    private LinkedList<T> items = new LinkedList<>();
    
    void addFront(T item) { items.addFirst(item); }
    void addRear(T item) { items.addLast(item); }
    T removeFront() { return items.removeFirst(); }
    T removeRear() { return items.removeLast(); }
    boolean isEmpty() { return items.isEmpty(); }
}
```

```cpp
template<typename T>
class Deque {
    deque<T> items;
public:
    void addFront(T item) { items.push_front(item); }
    void addRear(T item) { items.push_back(item); }
    T removeFront() { T item = items.front(); items.pop_front(); return item; }
    T removeRear() { T item = items.back(); items.pop_back(); return item; }
    bool isEmpty() { return items.empty(); }
};
```

## Common Applications

### 1. BFS Implementation

**Pseudocode:**
```
1. Create visited set, add start vertex
2. Create queue, enqueue start
3. While queue not empty:
   a. Dequeue vertex, process it
   b. For each neighbor:
      - If not visited: mark visited, enqueue
```

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=' ')
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

```java
public void bfs(Map<Integer, List<Integer>> graph, int start) {
    Set<Integer> visited = new HashSet<>();
    Queue<Integer> queue = new LinkedList<>();
    queue.offer(start);
    visited.add(start);
    while (!queue.isEmpty()) {
        int vertex = queue.poll();
        System.out.print(vertex + " ");
        for (int neighbor : graph.getOrDefault(vertex, new ArrayList<>())) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.offer(neighbor);
            }
        }
    }
}
```

```cpp
void bfs(unordered_map<int, vector<int>>& graph, int start) {
    unordered_set<int> visited;
    queue<int> q;
    q.push(start);
    visited.insert(start);
    while (!q.empty()) {
        int vertex = q.front(); q.pop();
        cout << vertex << " ";
        for (int neighbor : graph[vertex]) {
            if (visited.find(neighbor) == visited.end()) {
                visited.insert(neighbor);
                q.push(neighbor);
            }
        }
    }
}
```

### 2. Sliding Window Maximum

**Pseudocode:**
```
Using monotonic deque (store indices):
1. For each element at index i:
   a. Remove indices outside window: while front < i - k + 1, pop_front
   b. Remove smaller elements: while back element < current, pop_back
   c. Add current index to back
   d. If i >= k - 1: add nums[front] to result (front is max)
```

```python
from collections import deque

def max_sliding_window(nums, k):
    result = []
    window = deque()
    
    for i in range(len(nums)):
        # Remove elements outside current window
        while window and window[0] < i - k + 1:
            window.popleft()
        
        # Remove smaller elements
        while window and nums[window[-1]] < nums[i]:
            window.pop()
        
        window.append(i)
        
        if i >= k - 1:
            result.append(nums[window[0]])
    
    return result
```

```java
public int[] maxSlidingWindow(int[] nums, int k) {
    int[] result = new int[nums.length - k + 1];
    Deque<Integer> deque = new ArrayDeque<>();
    for (int i = 0; i < nums.length; i++) {
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) deque.pollFirst();
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) deque.pollLast();
        deque.offerLast(i);
        if (i >= k - 1) result[i - k + 1] = nums[deque.peekFirst()];
    }
    return result;
}
```

```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> result;
    deque<int> dq;
    for (int i = 0; i < nums.size(); i++) {
        while (!dq.empty() && dq.front() < i - k + 1) dq.pop_front();
        while (!dq.empty() && nums[dq.back()] < nums[i]) dq.pop_back();
        dq.push_back(i);
        if (i >= k - 1) result.push_back(nums[dq.front()]);
    }
    return result;
}
```

## Common Patterns

### 1. Level Order Traversal

**Pseudocode:**
```
1. If root is null, return empty list
2. Create queue, enqueue root
3. While queue not empty:
   a. level_size = queue size (nodes at current level)
   b. Create level list
   c. For i from 0 to level_size:
      - Dequeue node, add value to level list
      - Enqueue left and right children if exist
   d. Add level list to result
```

```python
def level_order_traversal(root):
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
        int size = queue.size();
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
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
        int size = q.size();
        vector<int> level;
        for (int i = 0; i < size; i++) {
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

### 2. Task Scheduling

**Pseudocode:**
```
Minimum intervals with cooldown:
1. Count frequency of each task
2. Find max_freq (most frequent task count)
3. Count tasks with max_freq
4. Minimum time = max(total_tasks, (max_freq - 1) × (n + 1) + max_freq_count)
```

```python
def task_scheduling(tasks, n):
    from collections import Counter
    counter = Counter(tasks)
    max_freq = max(counter.values())
    max_freq_tasks = sum(1 for v in counter.values() if v == max_freq)
```

```java
public int leastInterval(char[] tasks, int n) {
    int[] count = new int[26];
    for (char c : tasks) count[c - 'A']++;
    int maxFreq = Arrays.stream(count).max().getAsInt();
    int maxCount = (int) Arrays.stream(count).filter(c -> c == maxFreq).count();
    return Math.max(tasks.length, (maxFreq - 1) * (n + 1) + maxCount);
}
```

```cpp
int leastInterval(vector<char>& tasks, int n) {
    vector<int> count(26, 0);
    for (char c : tasks) count[c - 'A']++;
    int maxFreq = *max_element(count.begin(), count.end());
    int maxCount = count_if(count.begin(), count.end(), [maxFreq](int c) { return c == maxFreq; });
    return max((int)tasks.size(), (maxFreq - 1) * (n + 1) + maxCount);
}
```

```python
def task_scheduling_continued(tasks, n):
    
    return max(len(tasks), (max_freq - 1) * (n + 1) + max_freq_tasks)
```

## Edge Cases to Consider
1. Empty queue operations
2. Queue with single element
3. Queue reaching maximum capacity
4. Circular queue wrap-around
5. Priority conflicts in priority queue
6. Deque operations at both ends
7. Multiple elements with same priority

## Common Pitfalls
1. Not handling empty queue
2. Incorrect circular queue indexing
3. Memory leaks in custom implementations
4. Not maintaining FIFO order
5. Inefficient array-based implementation

## Practice Problems by Difficulty

### Easy
1. [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) (LC #232)
2. [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/) (LC #933)
3. [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/) (LC #387)

### Medium
1. [Design Circular Queue](https://leetcode.com/problems/design-circular-queue/) (LC #622)
2. [Perfect Squares](https://leetcode.com/problems/perfect-squares/) (LC #279)
3. [Number of Islands](https://leetcode.com/problems/number-of-islands/) (LC #200)
4. [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) (LC #994)

### Hard
1. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) (LC #239)
2. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) (LC #295)
3. [Maximum Performance of a Team](https://leetcode.com/problems/maximum-performance-of-a-team/) (LC #1383)

## Real-World Applications
1. **Job Scheduling**: Process and task management
2. **Printer Queue**: Document printing order
3. **Web Servers**: Request handling
4. **BFS Applications**: GPS navigation, social networks
5. **Cache Implementation**: LRU cache

## Advanced Topics
1. **Priority Queue Implementations**:
   - Binary Heap
   - Fibonacci Heap
   - Binomial Heap
2. **Specialized Queues**:
   - Double-ended Priority Queue
   - Multi-level Queue
   - Blocking Queue
3. **Concurrent Queue Implementation**

## Important Resources
1. [Python collections.deque](https://docs.python.org/3/library/collections.html#collections.deque)
2. [Queue Data Structure](https://www.geeksforgeeks.org/queue-data-structure/)
3. [Priority Queue Tutorial](https://www.programiz.com/dsa/priority-queue)
4. [Circular Queue Guide](https://www.programiz.com/dsa/circular-queue)
5. [Queue in Standard Libraries](https://docs.python.org/3/library/queue.html)

## ❓ FAQ Section

**Q: When should I use a queue vs a stack?**
A: Use a queue (FIFO) when order of processing matters and you need to process elements in the order they arrived: BFS traversal, task scheduling, buffering. Use a stack (LIFO) when you need reverse order or nested structure handling.

**Q: What's the difference between Queue and Deque?**
A: Queue allows insertion at rear and removal from front only. Deque (Double-Ended Queue) allows insertion and removal at both ends. Deque is more versatile and can simulate both stack and queue.

**Q: When should I use a Priority Queue?**
A: Use Priority Queue when elements have different priorities and you need to always process the highest (or lowest) priority element first: Dijkstra's algorithm, task scheduling with priorities, finding kth largest/smallest elements.

**Q: How do I implement a queue using two stacks?**
A: Use one stack for enqueue (push) and one for dequeue. When dequeuing, if the dequeue stack is empty, pop all elements from enqueue stack and push to dequeue stack, then pop from dequeue stack.

**Q: What's a circular queue and when do I use it?**
A: Circular queue reuses empty spaces by wrapping around. Use it when you have a fixed-size buffer and want efficient memory usage: CPU scheduling, traffic management, buffer in streaming data.

## Interview Tips
1. Clarify queue type requirements
2. Consider space/time complexity trade-offs
3. Handle edge cases explicitly
4. Use built-in implementations when allowed
5. Consider thread-safety requirements
6. Test boundary conditions thoroughly