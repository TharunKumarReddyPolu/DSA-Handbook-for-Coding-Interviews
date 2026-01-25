# Heap and Priority Queue Data Structures 🔺

## Introduction
A heap is a specialized tree-based data structure that satisfies the heap property. A priority queue is an abstract data type that operates similarly to a regular queue but with each element having a priority. Heaps are commonly used to implement priority queues.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (binary heaps are implemented using arrays)
- [Tree](tree.md) (conceptual understanding of complete binary trees)
- Basic understanding of logarithmic operations

### Related Topics
- **Builds on**: [Arrays](arrays.md) (array-based implementation), [Tree](tree.md) (heap is a tree structure)
- **Used in**: [Sorting](sorting.md) (heap sort), [Graph](graph.md) (Dijkstra's, Prim's algorithms)
- **Optimizes**: [Greedy](greedy.md) algorithms (efficient selection), [Intervals](intervals.md) (meeting rooms)
- **See also**: [Queue](queue.md) (regular vs priority queue comparison)

## Pattern Recognition Guide

### 🎯 When to Use Heap/Priority Queue
**Keywords in problem**: "k largest", "k smallest", "top k", "median", "schedule", "merge sorted"

**Use Heap when you see**:
- Need to repeatedly find min/max element
- K-th largest/smallest element problems
- Merging K sorted lists/arrays
- Stream of data with running statistics
- Priority-based scheduling

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Top K | "k largest", "k smallest", "k most frequent" | Kth Largest, Top K Frequent Elements |
| Merge K Sorted | "merge k lists", "k sorted arrays" | Merge K Sorted Lists |
| Running Median | "median from stream", "sliding window median" | Find Median from Data Stream |
| Scheduling | "minimize wait time", "task scheduling", "CPU" | Task Scheduler, Meeting Rooms II |
| Greedy + Heap | "minimum cost", "optimal selection" | Minimum Cost to Hire K Workers |
| Two Heaps | "balance", "median", "smaller/larger half" | Find Median, Sliding Window Median |

### ❌ When NOT to Use
- Need to access all elements (not just min/max) → use [Arrays](arrays.md)
- Need sorted iteration → use sorted [Arrays](arrays.md) or BST
- Simple FIFO/LIFO operations → use [Queue](queue.md)/[Stack](stack.md)
- K is very small → simple iteration might suffice

## Core Concepts

### Important Terminologies
- **Heap**: Complete binary tree with heap property
- **Min Heap**: Parent nodes are smaller than or equal to children
- **Max Heap**: Parent nodes are larger than or equal to children
- **Priority Queue**: Queue where elements have priorities
- **Heapify**: Process of creating a heap from an array
- **Heap Property**: Relationship between parent and child nodes
- **Complete Binary Tree**: All levels filled except possibly last
- **Binary Heap**: Implementation using array
- **d-ary Heap**: Each node has d children

### Time Complexity Analysis
| Operation        | Average Case | Worst Case |
|-----------------|--------------|------------|
| Insert          | O(log n)     | O(log n)   |
| Delete Min/Max  | O(log n)     | O(log n)   |
| Get Min/Max     | O(1)         | O(1)       |
| Build Heap      | O(n)         | O(n)       |
| Heapify         | O(log n)     | O(log n)   |
| Decrease Key    | O(log n)     | O(log n)   |
| Increase Key    | O(log n)     | O(log n)   |

### Space Complexity
- **Binary Heap**: O(n) where n is number of elements
- **d-ary Heap**: O(n) but with different constant factors
- **Priority Queue**: O(n) based on heap implementation

## Implementation

### Basic Binary Heap

**Pseudocode:**
```
Array-based heap (0-indexed):
- parent(i) = (i - 1) / 2
- left_child(i) = 2*i + 1
- right_child(i) = 2*i + 2

Insert: append to end, sift_up to restore heap property
Extract: swap root with last, remove last, sift_down from root
Sift_up: while parent violates heap property, swap with parent
Sift_down: while children violate heap property, swap with best child
```

```python
class BinaryHeap:
    def __init__(self, max_heap=False):
        self.heap = []
        self.max_heap = max_heap
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    def insert(self, key):
        self.heap.append(key)
        self._sift_up(len(self.heap) - 1)
    
    def extract_top(self):
        if not self.heap:
            return None
        
        top = self.heap[0]
        last_elem = self.heap.pop()
        
        if self.heap:
            self.heap[0] = last_elem
            self._sift_down(0)
        
        return top
    
    def _sift_up(self, i):
        parent = self.parent(i)
        if self.max_heap:
            while i > 0 and self.heap[parent] < self.heap[i]:
                self.swap(i, parent)
                i = parent
                parent = self.parent(i)
        else:
            while i > 0 and self.heap[parent] > self.heap[i]:
                self.swap(i, parent)
                i = parent
                parent = self.parent(i)
    
    def _sift_down(self, i):
        size = len(self.heap)
        while True:
            largest = i if self.max_heap else i
            left = self.left_child(i)
            right = self.right_child(i)
            
            if self.max_heap:
                if left < size and self.heap[left] > self.heap[largest]:
                    largest = left
                if right < size and self.heap[right] > self.heap[largest]:
                    largest = right
            else:
                if left < size and self.heap[left] < self.heap[largest]:
                    largest = left
                if right < size and self.heap[right] < self.heap[largest]:
                    largest = right
            
            if largest == i:
                break
            
            self.swap(i, largest)
            i = largest
```

```java
class BinaryHeap {
    private List<Integer> heap = new ArrayList<>();
    private boolean maxHeap;
    
    BinaryHeap(boolean maxHeap) { this.maxHeap = maxHeap; }
    
    void insert(int key) {
        heap.add(key);
        siftUp(heap.size() - 1);
    }
    
    int extractTop() {
        if (heap.isEmpty()) throw new RuntimeException("Empty heap");
        int top = heap.get(0);
        int last = heap.remove(heap.size() - 1);
        if (!heap.isEmpty()) { heap.set(0, last); siftDown(0); }
        return top;
    }
    
    private void siftUp(int i) {
        while (i > 0) {
            int parent = (i - 1) / 2;
            if (maxHeap ? heap.get(parent) < heap.get(i) : heap.get(parent) > heap.get(i)) {
                Collections.swap(heap, i, parent);
                i = parent;
            } else break;
        }
    }
    
    private void siftDown(int i) {
        int size = heap.size();
        while (true) {
            int best = i, left = 2*i+1, right = 2*i+2;
            if (left < size && (maxHeap ? heap.get(left) > heap.get(best) : heap.get(left) < heap.get(best))) best = left;
            if (right < size && (maxHeap ? heap.get(right) > heap.get(best) : heap.get(right) < heap.get(best))) best = right;
            if (best == i) break;
            Collections.swap(heap, i, best);
            i = best;
        }
    }
}
```

```cpp
class BinaryHeap {
    vector<int> heap;
    bool maxHeap;
public:
    BinaryHeap(bool maxHeap = false) : maxHeap(maxHeap) {}
    
    void insert(int key) {
        heap.push_back(key);
        siftUp(heap.size() - 1);
    }
    
    int extractTop() {
        if (heap.empty()) throw runtime_error("Empty heap");
        int top = heap[0];
        heap[0] = heap.back();
        heap.pop_back();
        if (!heap.empty()) siftDown(0);
        return top;
    }
    
private:
    void siftUp(int i) {
        while (i > 0) {
            int parent = (i - 1) / 2;
            if (maxHeap ? heap[parent] < heap[i] : heap[parent] > heap[i]) {
                swap(heap[i], heap[parent]);
                i = parent;
            } else break;
        }
    }
    
    void siftDown(int i) {
        int size = heap.size();
        while (true) {
            int best = i, left = 2*i+1, right = 2*i+2;
            if (left < size && (maxHeap ? heap[left] > heap[best] : heap[left] < heap[best])) best = left;
            if (right < size && (maxHeap ? heap[right] > heap[best] : heap[right] < heap[best])) best = right;
            if (best == i) break;
            swap(heap[i], heap[best]);
            i = best;
        }
    }
};
```

### Priority Queue using heapq

**Pseudocode:**
```
Priority Queue operations:
- push(item, priority): add (priority, index, item) to heap
  (index breaks ties, ensures FIFO for same priority)
- pop(): remove and return item with highest priority (lowest value for min-heap)
- peek(): return top item without removing
- is_empty(): check if heap is empty
```

```python
import heapq

class PriorityQueue:
    def __init__(self):
        self._queue = []
        self._index = 0
    
    def push(self, item, priority):
        # Use negative priority for max heap
        heapq.heappush(self._queue, (priority, self._index, item))
        self._index += 1
    
    def pop(self):
        return heapq.heappop(self._queue)[-1]
    
    def peek(self):
        return self._queue[0][-1] if self._queue else None
    
    def is_empty(self):
        return len(self._queue) == 0
```

```java
class PriorityQueueWrapper<T> {
    private PriorityQueue<int[]> queue = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    private Map<Integer, T> items = new HashMap<>();
    private int index = 0;
    
    void push(T item, int priority) {
        queue.offer(new int[]{priority, index});
        items.put(index++, item);
    }
    
    T pop() {
        int[] top = queue.poll();
        return items.remove(top[1]);
    }
    
    boolean isEmpty() { return queue.isEmpty(); }
}
// Note: Java's PriorityQueue can be used directly: new PriorityQueue<>() for min-heap
// or new PriorityQueue<>(Collections.reverseOrder()) for max-heap
```

```cpp
// C++ has built-in priority_queue in <queue>
// Min-heap: priority_queue<int, vector<int>, greater<int>> minHeap;
// Max-heap: priority_queue<int> maxHeap;

// Custom priority queue with item and priority
template<typename T>
class PriorityQueueWrapper {
    priority_queue<pair<int, pair<int, T>>, vector<pair<int, pair<int, T>>>, 
                   greater<pair<int, pair<int, T>>>> pq;
    int index = 0;
public:
    void push(T item, int priority) {
        pq.push({priority, {index++, item}});
    }
    T pop() {
        T item = pq.top().second.second;
        pq.pop();
        return item;
    }
    bool isEmpty() { return pq.empty(); }
};
```

## Common Applications

### 1. K-th Largest Element

**Pseudocode:**
```
Using min-heap of size k:
1. For each number in array:
   a. Push to heap
   b. If heap size > k: pop smallest
2. After processing all: heap top is kth largest
(Heap maintains k largest elements, smallest of those is kth largest)
```

```python
def find_kth_largest(nums, k):
    heap = []
    for num in nums:
        heapq.heappush(heap, num)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap[0]
```

```java
public int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> heap = new PriorityQueue<>();
    for (int num : nums) {
        heap.offer(num);
        if (heap.size() > k) heap.poll();
    }
    return heap.peek();
}
```

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> heap;
    for (int num : nums) {
        heap.push(num);
        if (heap.size() > k) heap.pop();
    }
    return heap.top();
}
```

### 2. Merge K Sorted Lists

**Pseudocode:**
```
1. Create min-heap with first element from each list: (value, list_index, element_index)
2. While heap not empty:
   a. Pop smallest (val, list_idx, elem_idx)
   b. Add val to result
   c. If list has next element: push (next_val, list_idx, elem_idx+1) to heap
3. Return result
```

```python
def merge_k_sorted_lists(lists):
    heap = []
    result = []
    
    # Initialize heap with first element from each list
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst[0], i, 0))
    
    while heap:
        val, list_idx, elem_idx = heapq.heappop(heap)
        result.append(val)
        
        if elem_idx + 1 < len(lists[list_idx]):
            next_elem = lists[list_idx][elem_idx + 1]
            heapq.heappush(heap, (next_elem, list_idx, elem_idx + 1))
    
    return result
```

```java
public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);
    for (ListNode node : lists) if (node != null) pq.offer(node);
    ListNode dummy = new ListNode(0), curr = dummy;
    while (!pq.isEmpty()) {
        ListNode node = pq.poll();
        curr.next = node;
        curr = curr.next;
        if (node.next != null) pq.offer(node.next);
    }
    return dummy.next;
}
```

```cpp
ListNode* mergeKLists(vector<ListNode*>& lists) {
    auto cmp = [](ListNode* a, ListNode* b) { return a->val > b->val; };
    priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> pq(cmp);
    for (auto node : lists) if (node) pq.push(node);
    ListNode dummy(0), *curr = &dummy;
    while (!pq.empty()) {
        ListNode* node = pq.top(); pq.pop();
        curr->next = node;
        curr = curr->next;
        if (node->next) pq.push(node->next);
    }
    return dummy.next;
}
```

### 3. Running Median

**Pseudocode:**
```
Two heaps: max-heap for smaller half, min-heap for larger half
Add number:
1. Add to max-heap (smaller half)
2. Move max of smaller to larger (balance values)
3. If larger heap bigger: move min of larger to smaller (balance sizes)
Find median:
- If heaps equal size: average of both tops
- Else: top of larger heap
```

```python
class MedianFinder:
    def __init__(self):
        self.small = []  # max heap
        self.large = []  # min heap
    
    def addNum(self, num):
        # Add to max heap
        heapq.heappush(self.small, -num)
        
        # Balance heaps
        if self.small and self.large and -self.small[0] > self.large[0]:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        # Handle size difference
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        if len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)
    
    def findMedian(self):
        if len(self.small) > len(self.large):
            return -self.small[0]
        if len(self.large) > len(self.small):
            return self.large[0]
        return (-self.small[0] + self.large[0]) / 2
```

```java
class MedianFinder {
    PriorityQueue<Integer> small = new PriorityQueue<>(Collections.reverseOrder());
    PriorityQueue<Integer> large = new PriorityQueue<>();
    
    public void addNum(int num) {
        small.offer(num);
        large.offer(small.poll());
        if (large.size() > small.size()) small.offer(large.poll());
    }
    
    public double findMedian() {
        return small.size() > large.size() ? small.peek() : (small.peek() + large.peek()) / 2.0;
    }
}
```

```cpp
class MedianFinder {
    priority_queue<int> small;  // max heap
    priority_queue<int, vector<int>, greater<int>> large;  // min heap
public:
    void addNum(int num) {
        small.push(num);
        large.push(small.top()); small.pop();
        if (large.size() > small.size()) { small.push(large.top()); large.pop(); }
    }
    
    double findMedian() {
        return small.size() > large.size() ? small.top() : (small.top() + large.top()) / 2.0;
    }
};
```

## Edge Cases to Consider
1. Empty heap operations
2. Single element heap
3. Duplicate priorities
4. Equal elements
5. Negative priorities
6. Maximum capacity
7. Priority overflow/underflow
8. Concurrent access
9. Memory constraints
10. Large number of operations

## Common Pitfalls
1. Not maintaining heap property
2. Incorrect parent-child relationships
3. Array index errors
4. Not handling duplicates properly
5. Inefficient heapify implementation
6. Memory leaks
7. Priority inversion

## Practice Problems by Difficulty

### Easy
1. [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) (LC #1046)
2. [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) (LC #703)
3. [Relative Ranks](https://leetcode.com/problems/relative-ranks/) (LC #506)

### Medium
1. [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) (LC #215)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) (LC #347)
3. [Task Scheduler](https://leetcode.com/problems/task-scheduler/) (LC #621)
4. [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/) (LC #264)

### Hard
1. [Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) (LC #295)
2. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) (LC #23)
3. [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) (LC #239)

## Real-World Applications
1. **Operating Systems**: Process scheduling
2. **Graph Algorithms**: Dijkstra's shortest path
3. **Data Compression**: Huffman coding
4. **Event-Driven Simulation**: Event scheduling
5. **Network Management**: Packet scheduling
6. **Database Systems**: Query optimization
7. **Load Balancing**: Task prioritization

## Advanced Topics
1. **Fibonacci Heap**:
   - Amortized complexity
   - Decrease key operation
2. **Binomial Heap**:
   - Multiple trees
   - Merge operation
3. **Pairing Heap**:
   - Simple implementation
   - Good practical performance
4. **Double-Ended Priority Queue**:
   - Min-max heap
   - Deap
5. **Concurrent Priority Queue**:
   - Lock-free implementation
   - Skip list based

## Important Resources
1. [Python heapq Documentation](https://docs.python.org/3/library/heapq.html)
2. [Binary Heap Implementation](https://www.geeksforgeeks.org/binary-heap/)
3. [Priority Queue Tutorial](https://www.programiz.com/dsa/priority-queue)
4. [Advanced Heap Structures](https://en.wikipedia.org/wiki/Heap_(data_structure))
5. [Fibonacci Heap Guide](https://www.geeksforgeeks.org/fibonacci-heap-set-1-introduction/)

## ❓ FAQ Section

**Q: When should I use a heap vs sorting?**
A: Use heap when you need the k smallest/largest elements (O(n log k) vs O(n log n)), streaming data, or repeated min/max extraction. Sort when you need all elements in order or will access by index.

**Q: How do I implement a max heap when the library only provides min heap?**
A: Negate the values: push `-value` and negate when popping. In Python: `heapq.heappush(heap, -value)`. Alternatively, use tuples with negated priority: `(-priority, value)`.

**Q: What's the difference between heap and priority queue?**
A: Priority queue is the abstract data type (ADT) - elements with priorities, highest priority served first. Heap is the most common implementation of priority queue. Other implementations: sorted array, unsorted array, BST.

**Q: Why is building a heap O(n) instead of O(n log n)?**
A: Using heapify (sift-down from bottom), most nodes are near the bottom and need few swaps. Mathematical analysis shows total work is O(n). Inserting n elements one by one is O(n log n).

**Q: How do I find the kth largest element efficiently?**
A: Use a min-heap of size k. For each element: if heap size < k, push it; else if element > heap top, pop and push. After processing all elements, heap top is kth largest. Time: O(n log k).

## Interview Tips
1. Clarify heap type requirements
2. Consider built-in implementations
3. Handle edge cases explicitly
4. Analyze space-time tradeoffs
5. Consider custom comparators
6. Test with small examples
7. Optimize for specific operations
8. Consider thread safety needs