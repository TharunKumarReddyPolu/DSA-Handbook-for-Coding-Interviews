# Greedy Algorithms 💰

## Introduction
A greedy algorithm makes the locally optimal choice at each step, hoping to find a global optimum. While not always yielding the optimal solution, greedy algorithms are often used for optimization problems and can be both efficient and effective when the problem has optimal substructure and the greedy choice property.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (for storing and processing data)
- [Sorting](sorting.md) (many greedy algorithms require sorted input)
- [Heap/Priority Queue](heap-pq.md) (for efficient selection of optimal choices)

### Related Topics
- **Compare with**: [Dynamic Programming](dynamic-programming.md) (use DP when greedy doesn't work)
- **Often used in**: [Intervals](intervals.md) (scheduling), [Graph](graph.md) (MST, shortest paths)
- **Builds on**: [Sorting](sorting.md) (sort-first strategy)
- **See also**: [Backtracking](backtracking.md) (when greedy fails, explore all possibilities)

## Pattern Recognition Guide

### 🎯 When to Use Greedy
**Keywords in problem**: "minimum/maximum", "optimal", "most/least", "earliest/latest", "schedule"

**Use Greedy when you see**:
- Local optimal choice leads to global optimum
- Problem has optimal substructure
- Making irrevocable decisions is acceptable
- Interval scheduling or activity selection
- Huffman-style encoding problems

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Interval Scheduling | "maximum meetings", "non-overlapping", "minimum rooms" | Meeting Rooms, Non-overlapping Intervals |
| Activity Selection | "select maximum activities", "earliest finish" | Activity Selection, Minimum Platforms |
| Fractional Selection | "maximize value", "can take fractions" | Fractional Knapsack |
| Two Pointers Greedy | "container", "maximize area", "two ends" | Container With Most Water |
| Huffman-style | "encoding", "merge cost", "minimum total" | Minimum Cost to Connect Sticks |
| Jump/Reach | "can reach end", "minimum jumps", "farthest" | Jump Game, Jump Game II |

### ❌ When NOT to Use
- Local optimal doesn't guarantee global optimal → use [DP](dynamic-programming.md)
- 0/1 decisions without fractional option (often) → consider [DP](dynamic-programming.md)
- Need to explore all possibilities → use [Backtracking](backtracking.md)
- Problem requires revisiting decisions

### 💡 Greedy vs DP Decision
Ask: "If I make the locally best choice now, will I always get the globally best answer?"
- **Yes** → Greedy works
- **No/Unsure** → Use DP or prove greedy property first

## Core Concepts

### Important Terminologies
- **Greedy Choice Property**: Local optimal choice leads to global optimum
- **Optimal Substructure**: Problem's optimal solution contains optimal sub-solutions
- **Local Optimum**: Best choice at current step
- **Global Optimum**: Best overall solution
- **Feasible Solution**: Solution satisfying all constraints
- **Candidate Solution**: Potential solution being considered
- **Selection Function**: Chooses best candidate
- **Feasibility Function**: Checks if choice is valid
- **Objective Function**: Value to be optimized
- **Solution Space**: Set of all possible solutions

### Properties of Greedy Algorithms
1. **Makes Irrevocable Choices**: Decisions once made are final
2. **No Backtracking**: Never reconsiders choices
3. **Locally Optimal**: Best choice at each step
4. **Hope for Global Optimum**: May or may not achieve it
5. **Polynomial Time**: Usually efficient

### Time Complexity Analysis
| Problem Type           | Time Complexity | Space Complexity |
|-----------------------|-----------------|------------------|
| Activity Selection    | O(n log n)      | O(1)            |
| Huffman Coding       | O(n log n)      | O(n)            |
| Fractional Knapsack  | O(n log n)      | O(1)            |
| Minimum Spanning Tree| O(E log V)      | O(V)            |
| Dijkstra's Algorithm | O(V² + E)       | O(V)            |
| Job Scheduling       | O(n log n)      | O(1)            |

## Implementation Patterns

### 1. Activity Selection

**Pseudocode:**
```
1. Sort activities by finish time (ascending)
2. Select first activity, set last_finish = its finish time
3. For each remaining activity:
   a. If activity's start >= last_finish:
      - Select this activity
      - Update last_finish to this activity's finish time
4. Return selected activities (maximum non-overlapping)
```

```python
def activity_selection(start, finish):
    # Sort by finish time
    activities = sorted(zip(start, finish), key=lambda x: x[1])
    selected = [activities[0]]
    last_finish = activities[0][1]
    
    for i in range(1, len(activities)):
        if activities[i][0] >= last_finish:
            selected.append(activities[i])
            last_finish = activities[i][1]
    
    return selected
```

```java
public int activitySelection(int[][] activities) {
    Arrays.sort(activities, (a, b) -> a[1] - b[1]);
    int count = 1, lastFinish = activities[0][1];
    for (int i = 1; i < activities.length; i++) {
        if (activities[i][0] >= lastFinish) {
            count++;
            lastFinish = activities[i][1];
        }
    }
    return count;
}
```

```cpp
int activitySelection(vector<pair<int,int>>& activities) {
    sort(activities.begin(), activities.end(), 
         [](auto& a, auto& b) { return a.second < b.second; });
    int count = 1, lastFinish = activities[0].second;
    for (int i = 1; i < activities.size(); i++) {
        if (activities[i].first >= lastFinish) {
            count++;
            lastFinish = activities[i].second;
        }
    }
    return count;
}
```

### 2. Fractional Knapsack

**Pseudocode:**
```
1. Calculate value-to-weight ratio for each item
2. Sort items by ratio in descending order
3. For each item (in sorted order):
   a. If remaining capacity >= item weight:
      - Take entire item, reduce capacity
   b. Else:
      - Take fraction: add (ratio × remaining_capacity)
      - Break (knapsack full)
4. Return total value
```

```python
def fractional_knapsack(values, weights, capacity):
    # Calculate value per unit weight
    ratios = [(v/w, v, w) for v, w in zip(values, weights)]
    ratios.sort(reverse=True)
    
    total_value = 0
    remaining_capacity = capacity
    
    for ratio, value, weight in ratios:
        if remaining_capacity >= weight:
            total_value += value
            remaining_capacity -= weight
        else:
            total_value += ratio * remaining_capacity
            break
    
    return total_value
```

```java
public double fractionalKnapsack(int[] values, int[] weights, int capacity) {
    int n = values.length;
    double[][] items = new double[n][2]; // {ratio, weight}
    for (int i = 0; i < n; i++) items[i] = new double[]{(double)values[i]/weights[i], weights[i]};
    Arrays.sort(items, (a, b) -> Double.compare(b[0], a[0]));
    double totalValue = 0, remaining = capacity;
    for (double[] item : items) {
        if (remaining >= item[1]) { totalValue += item[0] * item[1]; remaining -= item[1]; }
        else { totalValue += item[0] * remaining; break; }
    }
    return totalValue;
}
```

```cpp
double fractionalKnapsack(vector<int>& values, vector<int>& weights, int capacity) {
    int n = values.size();
    vector<pair<double, int>> items(n); // {ratio, weight}
    for (int i = 0; i < n; i++) items[i] = {(double)values[i]/weights[i], weights[i]};
    sort(items.begin(), items.end(), greater<pair<double,int>>());
    double totalValue = 0, remaining = capacity;
    for (auto& item : items) {
        if (remaining >= item.second) { totalValue += item.first * item.second; remaining -= item.second; }
        else { totalValue += item.first * remaining; break; }
    }
    return totalValue;
}
```

### 3. Huffman Coding

**Pseudocode:**
```
1. Create leaf node for each character with its frequency
2. Add all nodes to min-heap (priority = frequency)
3. While heap has more than one node:
   a. Pop two nodes with lowest frequencies (left, right)
   b. Create internal node with frequency = left.freq + right.freq
   c. Set internal node's children to left and right
   d. Push internal node back to heap
4. Return root of heap (Huffman tree root)
```

```python
import heapq

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):
        return self.freq < other.freq

def huffman_coding(chars, freqs):
    heap = []
    for char, freq in zip(chars, freqs):
        heapq.heappush(heap, Node(char, freq))
    
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        internal = Node(None, left.freq + right.freq)
        internal.left = left
        internal.right = right
        
        heapq.heappush(heap, internal)
    
    return heap[0]
```

```java
class HuffmanNode implements Comparable<HuffmanNode> {
    char ch; int freq;
    HuffmanNode left, right;
    HuffmanNode(char ch, int freq) { this.ch = ch; this.freq = freq; }
    public int compareTo(HuffmanNode o) { return this.freq - o.freq; }
}
public HuffmanNode huffmanCoding(char[] chars, int[] freqs) {
    PriorityQueue<HuffmanNode> pq = new PriorityQueue<>();
    for (int i = 0; i < chars.length; i++) pq.offer(new HuffmanNode(chars[i], freqs[i]));
    while (pq.size() > 1) {
        HuffmanNode left = pq.poll(), right = pq.poll();
        HuffmanNode internal = new HuffmanNode('\0', left.freq + right.freq);
        internal.left = left; internal.right = right;
        pq.offer(internal);
    }
    return pq.poll();
}
```

```cpp
struct HuffmanNode {
    char ch; int freq;
    HuffmanNode *left, *right;
    HuffmanNode(char c, int f) : ch(c), freq(f), left(nullptr), right(nullptr) {}
};
struct Compare { bool operator()(HuffmanNode* a, HuffmanNode* b) { return a->freq > b->freq; } };
HuffmanNode* huffmanCoding(vector<char>& chars, vector<int>& freqs) {
    priority_queue<HuffmanNode*, vector<HuffmanNode*>, Compare> pq;
    for (int i = 0; i < chars.size(); i++) pq.push(new HuffmanNode(chars[i], freqs[i]));
    while (pq.size() > 1) {
        HuffmanNode* left = pq.top(); pq.pop();
        HuffmanNode* right = pq.top(); pq.pop();
        HuffmanNode* internal = new HuffmanNode('\0', left->freq + right->freq);
        internal->left = left; internal->right = right;
        pq.push(internal);
    }
    return pq.top();
}
```

### 4. Interval Scheduling

**Pseudocode:**
```
Maximum non-overlapping intervals:
1. Sort intervals by end time
2. Select first interval, track current_end
3. Count = 1
4. For each interval:
   a. If interval start >= current_end:
      - Select it, update current_end, increment count
5. Return count (or intervals.length - count for minimum removals)
```

```python
def interval_scheduling(intervals):
    # Sort by end time
    intervals.sort(key=lambda x: x[1])
    
    selected = []
    current_end = float('-inf')
    
    for start, end in intervals:
        if start >= current_end:
            selected.append([start, end])
            current_end = end
    
    return selected
```

```java
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
    int count = 1, end = intervals[0][1];
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] >= end) { count++; end = intervals[i][1]; }
    }
    return intervals.length - count;
}
```

```cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return 0;
    sort(intervals.begin(), intervals.end(), [](auto& a, auto& b) { return a[1] < b[1]; });
    int count = 1, end = intervals[0][1];
    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] >= end) { count++; end = intervals[i][1]; }
    }
    return intervals.size() - count;
}
```

### 5. Job Sequencing with Deadlines

**Pseudocode:**
```
1. Sort jobs by profit in descending order
2. Create slot array of size max_deadline, initialized to empty
3. For each job:
   a. For slot j from (deadline-1) down to 0:
      - If slot[j] is empty:
        Assign job to slot[j], break
4. Return jobs in non-empty slots (maximizes profit)
```

```python
def job_sequencing(jobs, deadlines, profits):
    # Sort jobs by profit in descending order
    job_info = sorted(zip(jobs, deadlines, profits),
                     key=lambda x: x[2], reverse=True)
    
    max_deadline = max(deadlines)
    slot = [-1] * max_deadline
    
    for job, deadline, profit in job_info:
        for j in range(deadline-1, -1, -1):
            if slot[j] == -1:
                slot[j] = job
                break
```

```java
public int jobSequencing(int[][] jobs) { // jobs[i] = {deadline, profit}
    Arrays.sort(jobs, (a, b) -> b[1] - a[1]); // Sort by profit desc
    int maxDeadline = Arrays.stream(jobs).mapToInt(j -> j[0]).max().orElse(0);
    boolean[] slot = new boolean[maxDeadline + 1];
    int totalProfit = 0;
    for (int[] job : jobs) {
        for (int j = job[0]; j > 0; j--) {
            if (!slot[j]) { slot[j] = true; totalProfit += job[1]; break; }
        }
    }
    return totalProfit;
}
```

```cpp
int jobSequencing(vector<pair<int,int>>& jobs) { // {deadline, profit}
    sort(jobs.begin(), jobs.end(), [](auto& a, auto& b) { return a.second > b.second; });
    int maxDeadline = 0;
    for (auto& job : jobs) maxDeadline = max(maxDeadline, job.first);
    vector<bool> slot(maxDeadline + 1, false);
    int totalProfit = 0;
    for (auto& job : jobs) {
        for (int j = job.first; j > 0; j--) {
            if (!slot[j]) { slot[j] = true; totalProfit += job.second; break; }
        }
    }
    return totalProfit;
}
    
    return [x for x in slot if x != -1]
```

## Common Techniques

### 1. Sort-First Strategy

**Pseudocode:**
```
Minimum coins (greedy - works for standard denominations):
1. Sort coins in descending order
2. For each coin (largest to smallest):
   a. count += remaining / coin  (take as many as possible)
   b. remaining = remaining % coin
3. If remaining == 0: return count
   Else: return -1 (cannot make exact amount)
```

```python
def minimum_coins(coins, amount):
    coins.sort(reverse=True)
    count = 0
    remaining = amount
    
    for coin in coins:
        count += remaining // coin
        remaining %= coin
    
    return count if remaining == 0 else -1
```

```java
public int minimumCoins(int[] coins, int amount) {
    Arrays.sort(coins);
    int count = 0, remaining = amount;
    for (int i = coins.length - 1; i >= 0 && remaining > 0; i--) {
        count += remaining / coins[i];
        remaining %= coins[i];
    }
    return remaining == 0 ? count : -1;
}
```

```cpp
int minimumCoins(vector<int>& coins, int amount) {
    sort(coins.rbegin(), coins.rend());
    int count = 0, remaining = amount;
    for (int coin : coins) {
        count += remaining / coin;
        remaining %= coin;
    }
    return remaining == 0 ? count : -1;
}
```

### 2. Priority Queue Approach

**Pseudocode:**
```
Minimum cost to connect ropes:
1. Build min-heap from all rope lengths
2. total_cost = 0
3. While heap has more than one element:
   a. Pop two smallest ropes
   b. cost = sum of two ropes
   c. total_cost += cost
   d. Push combined rope back to heap
4. Return total_cost
```

```python
def minimum_cost_ropes(ropes):
    heapq.heapify(ropes)
    total_cost = 0
    
    while len(ropes) > 1:
        first = heapq.heappop(ropes)
        second = heapq.heappop(ropes)
        cost = first + second
        total_cost += cost
        heapq.heappush(ropes, cost)
    
    return total_cost
```

```java
public int minimumCostRopes(int[] ropes) {
    PriorityQueue<Integer> pq = new PriorityQueue<>();
    for (int rope : ropes) pq.offer(rope);
    int totalCost = 0;
    while (pq.size() > 1) {
        int cost = pq.poll() + pq.poll();
        totalCost += cost;
        pq.offer(cost);
    }
    return totalCost;
}
```

```cpp
int minimumCostRopes(vector<int>& ropes) {
    priority_queue<int, vector<int>, greater<int>> pq(ropes.begin(), ropes.end());
    int totalCost = 0;
    while (pq.size() > 1) {
        int first = pq.top(); pq.pop();
        int second = pq.top(); pq.pop();
        int cost = first + second;
        totalCost += cost;
        pq.push(cost);
    }
    return totalCost;
}
```

## Edge Cases to Consider
1. Empty input
2. Single element
3. All elements same
4. Extreme values
5. Negative numbers
6. Overlapping intervals
7. Conflicting priorities
8. Ties in selection
9. Maximum capacity
10. Invalid input

## Common Pitfalls
1. Assuming greedy always works
2. Not proving greedy property
3. Incorrect sorting criteria
4. Missing edge cases
5. Not handling ties properly
6. Overflow/underflow
7. Incorrect priority assignment
8. Not considering constraints

## Practice Problems by Difficulty

### Easy
1. [Assign Cookies](https://leetcode.com/problems/assign-cookies/) (LC #455)
2. [Lemonade Change](https://leetcode.com/problems/lemonade-change/) (LC #860)
3. [Maximum Units on a Truck](https://leetcode.com/problems/maximum-units-on-a-truck/) (LC #1710)

### Medium
1. [Jump Game](https://leetcode.com/problems/jump-game/) (LC #55)
2. [Partition Labels](https://leetcode.com/problems/partition-labels/) (LC #763)
3. [Task Scheduler](https://leetcode.com/problems/task-scheduler/) (LC #621)
4. [Gas Station](https://leetcode.com/problems/gas-station/) (LC #134)

### Hard
1. [Candy](https://leetcode.com/problems/candy/) (LC #135)
2. [Course Schedule III](https://leetcode.com/problems/course-schedule-iii/) (LC #630)
3. [Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/) (LC #659)

## Real-World Applications
1. **Networking**:
   - Routing protocols
   - Channel allocation
2. **Operating Systems**:
   - Process scheduling
   - Memory allocation
3. **Data Compression**:
   - Huffman coding
   - Run-length encoding
4. **Financial Markets**:
   - Trading strategies
   - Portfolio optimization
5. **Resource Management**:
   - Task scheduling
   - Load balancing
6. **Logistics**:
   - Vehicle routing
   - Warehouse optimization

## Advanced Topics
1. **Approximation Algorithms**:
   - Set cover
   - Traveling salesman
2. **Online Algorithms**:
   - Competitive analysis
   - Stream processing
3. **Randomized Greedy**:
   - Probabilistic choices
   - Monte Carlo methods
4. **Multi-objective Greedy**:
   - Pareto optimality
   - Trade-off analysis

## Important Resources
1. [Introduction to Greedy Algorithms](https://www.geeksforgeeks.org/greedy-algorithms/)
2. [Activity Selection Problem](https://www.geeksforgeeks.org/activity-selection-problem-greedy-algo-1/)
3. [Huffman Coding Tutorial](https://www.geeksforgeeks.org/huffman-coding-greedy-algo-3/)
4. [Interval Scheduling](https://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf)
5. [Minimum Spanning Trees](https://www.geeksforgeeks.org/kruskals-minimum-spanning-tree-algorithm-greedy-algo-2/)

## ❓ FAQ Section

**Q: How do I know if a greedy approach will work?**
A: Check for (1) Greedy choice property: locally optimal choice leads to globally optimal solution, (2) Optimal substructure: optimal solution contains optimal sub-solutions. Try to find a counter-example; if you can't and it feels intuitive, greedy likely works.

**Q: What's the difference between greedy and dynamic programming?**
A: Greedy makes one choice and never reconsiders (faster, simpler). DP considers all choices and picks the best (always correct when applicable). If greedy works, prefer it. Use DP when greedy fails or you're unsure.

**Q: How do I prove a greedy algorithm is correct?**
A: Common proof techniques: (1) Greedy stays ahead - show greedy solution is at least as good at each step, (2) Exchange argument - show you can transform any optimal solution to greedy without losing quality, (3) Contradiction.

**Q: Why do many greedy problems require sorting first?**
A: Sorting reveals structure that enables greedy choices. For intervals, sorting by end time ensures we always pick the activity that leaves maximum room. For fractional knapsack, sorting by value/weight ratio ensures we pick highest value items first.

**Q: What are common greedy problem patterns?**
A: (1) Interval scheduling - sort by end time, pick non-overlapping, (2) Fractional selection - sort by ratio, take greedily, (3) Huffman-style - always merge smallest, (4) Activity selection - earliest finish first, (5) Jump game - track farthest reachable.

## Interview Tips
1. Prove greedy choice property
2. Consider counter-examples
3. Compare with dynamic programming
4. Analyze time complexity
5. Handle edge cases
6. Consider sorting first
7. Use priority queues when needed
8. Test with small examples
9. Optimize space usage
10. Discuss trade-offs