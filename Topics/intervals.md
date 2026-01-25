# Interval Problems and Algorithms 📆

## Introduction
Interval problems involve ranges defined by start and end points. These problems are common in scheduling, resource allocation, and time management applications. Understanding interval manipulation and algorithms is crucial for solving real-world optimization problems.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (for storing intervals)
- [Sorting](sorting.md) (most interval algorithms require sorting first)
- Basic comparison operations (start/end point comparisons)

### Related Topics
- **Builds on**: [Arrays](arrays.md), [Sorting](sorting.md) (sort by start/end time)
- **Often uses**: [Heap/Priority Queue](heap-pq.md) (for meeting rooms, merge K sorted)
- **Algorithmic approach**: [Greedy](greedy.md) (interval scheduling maximization)
- **See also**: [Tree](tree.md) (interval trees), [Sweep line algorithms](graph.md)

## Pattern Recognition Guide

### 🎯 When to Use Interval Techniques
**Keywords in problem**: "intervals", "meetings", "schedule", "overlap", "merge", "time range"

**Use Interval techniques when you see**:
- Problems involving ranges [start, end]
- Scheduling or resource allocation
- Finding overlaps or gaps
- Merging time/space ranges
- Calendar or booking systems

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Merge Intervals | "merge overlapping", "combine ranges" | Merge Intervals, Insert Interval |
| Meeting Rooms | "minimum rooms", "conflicting meetings", "can attend all" | Meeting Rooms I & II |
| Interval Scheduling | "maximum non-overlapping", "select most" | Non-overlapping Intervals |
| Sweep Line | "number of overlaps at time t", "max concurrent" | Maximum Population Year |
| Interval Intersection | "common time", "overlap of lists" | Interval List Intersections |
| Insert/Remove | "add interval", "remove range" | Insert Interval, Remove Interval |

### ❌ When NOT to Use
- Point data, not ranges → use [Arrays](arrays.md) or [Hashing](hashing.md)
- No time/range aspect to the problem
- Simple 1D array problems → use basic [Array](arrays.md) techniques

### 💡 Sorting Strategy
- **Sort by start time**: When merging or checking overlaps sequentially
- **Sort by end time**: When maximizing number of non-overlapping intervals (greedy)

## Core Concepts

### Important Terminologies
- **Interval**: Range defined by start and end points
- **Overlapping**: Intervals that intersect
- **Disjoint**: Non-overlapping intervals
- **Merge**: Combining overlapping intervals
- **Intersection**: Common range between intervals
- **Union**: Combined range of intervals
- **Nested**: One interval completely inside another
- **Adjacent**: Intervals that touch but don't overlap
- **Coverage**: Total range covered by intervals
- **Gap**: Space between non-overlapping intervals

### Types of Interval Problems
1. **Overlap Detection**: Find if intervals overlap
2. **Merging**: Combine overlapping intervals
3. **Scheduling**: Find maximum non-overlapping intervals
4. **Resource Allocation**: Track simultaneous intervals
5. **Coverage**: Find total covered range

### Time Complexity Analysis
| Operation               | Time Complexity | Space Complexity |
|------------------------|----------------|------------------|
| Sort Intervals         | O(n log n)     | O(1) or O(n)    |
| Merge Intervals        | O(n)           | O(n)            |
| Find Overlaps         | O(n log n)     | O(n)            |
| Maximum Non-overlapping| O(n log n)     | O(1)            |
| Meeting Rooms         | O(n log n)     | O(n)            |
| Insert Interval       | O(n)           | O(n)            |

## Implementation Patterns

### 1. Basic Interval Operations

**Pseudocode:**
```
Check overlap: max(a.start, b.start) <= min(a.end, b.end)
Get intersection: if overlap, return [max(starts), min(ends)]
Merge two intervals: if overlap, return [min(starts), max(ends)]
```

```python
class Interval:
    def __init__(self, start, end):
        self.start = start
        self.end = end

def do_overlap(a: Interval, b: Interval) -> bool:
    return max(a.start, b.start) <= min(a.end, b.end)

def get_overlap(a: Interval, b: Interval) -> Interval:
    if not do_overlap(a, b):
        return None
    return Interval(max(a.start, b.start), min(a.end, b.end))

def merge_two_intervals(a: Interval, b: Interval) -> Interval:
    if not do_overlap(a, b):
        return None
    return Interval(min(a.start, b.start), max(a.end, b.end))
```

```java
class Interval {
    int start, end;
    Interval(int start, int end) { this.start = start; this.end = end; }
}

boolean doOverlap(Interval a, Interval b) {
    return Math.max(a.start, b.start) <= Math.min(a.end, b.end);
}

Interval getOverlap(Interval a, Interval b) {
    if (!doOverlap(a, b)) return null;
    return new Interval(Math.max(a.start, b.start), Math.min(a.end, b.end));
}

Interval mergeTwoIntervals(Interval a, Interval b) {
    if (!doOverlap(a, b)) return null;
    return new Interval(Math.min(a.start, b.start), Math.max(a.end, b.end));
}
```

```cpp
struct Interval {
    int start, end;
    Interval(int s, int e) : start(s), end(e) {}
};

bool doOverlap(Interval& a, Interval& b) {
    return max(a.start, b.start) <= min(a.end, b.end);
}

Interval* getOverlap(Interval& a, Interval& b) {
    if (!doOverlap(a, b)) return nullptr;
    return new Interval(max(a.start, b.start), min(a.end, b.end));
}

Interval* mergeTwoIntervals(Interval& a, Interval& b) {
    if (!doOverlap(a, b)) return nullptr;
    return new Interval(min(a.start, b.start), max(a.end, b.end));
}
```

### 2. Merge Intervals

**Pseudocode:**
```
1. Sort intervals by start time
2. Add first interval to result
3. For each interval:
   a. If current.start <= last_result.end: (overlap)
      - Update last_result.end = max(last_result.end, current.end)
   b. Else: add current to result
4. Return merged result
```

```python
def merge_intervals(intervals):
    if not intervals:
        return []
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    merged = [intervals[0]]
    
    for interval in intervals[1:]:
        if interval[0] <= merged[-1][1]:
            # Overlapping intervals, update end time
            merged[-1][1] = max(merged[-1][1], interval[1])
        else:
            # Non-overlapping interval, add to result
            merged.append(interval)
    
    return merged
```

```java
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) return intervals;
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> result = new ArrayList<>();
    int[] current = intervals[0];
    result.add(current);
    for (int[] interval : intervals) {
        if (interval[0] <= current[1])
            current[1] = Math.max(current[1], interval[1]);
        else {
            current = interval;
            result.add(current);
        }
    }
    return result.toArray(new int[result.size()][]);
}
```

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};
    sort(intervals.begin(), intervals.end());
    vector<vector<int>> result = {intervals[0]};
    for (auto& interval : intervals) {
        if (interval[0] <= result.back()[1])
            result.back()[1] = max(result.back()[1], interval[1]);
        else
            result.push_back(interval);
    }
    return result;
}
```

### 3. Meeting Rooms

**Pseudocode:**
```
Minimum rooms needed:
1. Sort meetings by start time
2. Create min-heap to track end times of ongoing meetings
3. For each meeting:
   a. If heap not empty AND heap_top <= meeting.start:
      Pop from heap (that room is now free)
   b. Push current meeting's end time to heap
4. Return heap size (number of concurrent meetings at peak)
```

```python
import heapq

def min_meeting_rooms(intervals):
    if not intervals:
        return 0
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    # Min heap to track end times
    rooms = []
    heapq.heappush(rooms, intervals[0][1])
    
    for interval in intervals[1:]:
        # If meeting can use existing room
        if rooms[0] <= interval[0]:
            heapq.heappop(rooms)
        
        # Add current meeting's end time
        heapq.heappush(rooms, interval[1])
    
    return len(rooms)
```

```java
public int minMeetingRooms(int[][] intervals) {
    if (intervals.length == 0) return 0;
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    PriorityQueue<Integer> heap = new PriorityQueue<>();
    heap.offer(intervals[0][1]);
    for (int i = 1; i < intervals.length; i++) {
        if (heap.peek() <= intervals[i][0]) heap.poll();
        heap.offer(intervals[i][1]);
    }
    return heap.size();
}
```

```cpp
int minMeetingRooms(vector<vector<int>>& intervals) {
    if (intervals.empty()) return 0;
    sort(intervals.begin(), intervals.end());
    priority_queue<int, vector<int>, greater<int>> heap;
    heap.push(intervals[0][1]);
    for (int i = 1; i < intervals.size(); i++) {
        if (heap.top() <= intervals[i][0]) heap.pop();
        heap.push(intervals[i][1]);
    }
    return heap.size();
}
```

### 4. Insert Interval

**Pseudocode:**
```
1. Add all intervals that end before newInterval starts (no overlap)
2. Merge overlapping intervals:
   a. While current.start <= newInterval.end:
      - newInterval.start = min(newInterval.start, current.start)
      - newInterval.end = max(newInterval.end, current.end)
3. Add merged newInterval
4. Add all remaining intervals
```

```python
def insert_interval(intervals, newInterval):
    result = []
    i = 0
    n = len(intervals)
    
    # Add all intervals ending before newInterval
    while i < n and intervals[i][1] < newInterval[0]:
        result.append(intervals[i])
        i += 1
    
    # Merge overlapping intervals
    while i < n and intervals[i][0] <= newInterval[1]:
        newInterval[0] = min(newInterval[0], intervals[i][0])
        newInterval[1] = max(newInterval[1], intervals[i][1])
        i += 1
    
    result.append(newInterval)
    
    # Add remaining intervals
    while i < n:
        result.append(intervals[i])
        i += 1
    
    return result
```

```java
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0;
    while (i < intervals.length && intervals[i][1] < newInterval[0])
        result.add(intervals[i++]);
    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.add(newInterval);
    while (i < intervals.length) result.add(intervals[i++]);
    return result.toArray(new int[result.size()][]);
}
```

```cpp
vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
    vector<vector<int>> result;
    int i = 0;
    while (i < intervals.size() && intervals[i][1] < newInterval[0])
        result.push_back(intervals[i++]);
    while (i < intervals.size() && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = min(newInterval[0], intervals[i][0]);
        newInterval[1] = max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.push_back(newInterval);
    while (i < intervals.size()) result.push_back(intervals[i++]);
    return result;
}
```

### 5. Non-overlapping Intervals

**Pseudocode:**
```
Minimum removals for non-overlapping:
1. Sort by end time (greedy: keep intervals that end earliest)
2. count = 1, current_end = first interval's end
3. For each interval:
   a. If start >= current_end: (no overlap)
      - Increment count, update current_end
4. Return total - count (intervals to remove)
```

```python
def erase_overlap_intervals(intervals):
    if not intervals:
        return 0
    
    # Sort by end time
    intervals.sort(key=lambda x: x[1])
    
    non_overlap = 1
    end = intervals[0][1]
    
    for i in range(1, len(intervals)):
        if intervals[i][0] >= end:
            non_overlap += 1
            end = intervals[i][1]
    
    return len(intervals) - non_overlap
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

## Common Techniques

### 1. Sweep Line Algorithm

**Pseudocode:**
```
Find maximum overlap:
1. Create events: (start, +1) and (end, -1) for each interval
2. Sort events by time (process ends before starts at same time)
3. Sweep through events:
   a. current_count += event_value
   b. max_overlap = max(max_overlap, current_count)
4. Return max_overlap
```

```python
def sweep_line(intervals):
    events = []
    # Add start and end events
    for start, end in intervals:
        events.append((start, 1))   # 1 for start
        events.append((end, -1))    # -1 for end
    
    events.sort()  # Sort by time
    
    current = 0
    max_overlap = 0
    
    for time, count in events:
        current += count
        max_overlap = max(max_overlap, current)
    
    return max_overlap
```

```java
public int sweepLine(int[][] intervals) {
    List<int[]> events = new ArrayList<>();
    for (int[] interval : intervals) {
        events.add(new int[]{interval[0], 1});  // start
        events.add(new int[]{interval[1], -1}); // end
    }
    events.sort((a, b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
    int current = 0, maxOverlap = 0;
    for (int[] event : events) {
        current += event[1];
        maxOverlap = Math.max(maxOverlap, current);
    }
    return maxOverlap;
}
```

```cpp
int sweepLine(vector<vector<int>>& intervals) {
    vector<pair<int, int>> events;
    for (auto& interval : intervals) {
        events.push_back({interval[0], 1});  // start
        events.push_back({interval[1], -1}); // end
    }
    sort(events.begin(), events.end());
    int current = 0, maxOverlap = 0;
    for (auto& event : events) {
        current += event.second;
        maxOverlap = max(maxOverlap, current);
    }
    return maxOverlap;
}
```

### 2. Interval Tree

**Pseudocode:**
```
BST where each node stores an interval:
- Node: interval, max_end (max end point in subtree), left, right
- Insert: BST insert by start time, update max_end going back up
- Search for overlap: 
  If left exists AND left.max_end >= query.start: search left
  Else: search right
```

```python
class IntervalNode:
    def __init__(self, interval):
        self.interval = interval
        self.max_end = interval[1]
        self.left = None
        self.right = None

def insert_interval_tree(root, interval):
    if not root:
        return IntervalNode(interval)
    
    if interval[0] < root.interval[0]:
        root.left = insert_interval_tree(root.left, interval)
    else:
        root.right = insert_interval_tree(root.right, interval)
    
    root.max_end = max(root.max_end, interval[1])
    return root
```

```java
class IntervalNode {
    int[] interval;
    int maxEnd;
    IntervalNode left, right;
    IntervalNode(int[] interval) { this.interval = interval; this.maxEnd = interval[1]; }
}
IntervalNode insertIntervalTree(IntervalNode root, int[] interval) {
    if (root == null) return new IntervalNode(interval);
    if (interval[0] < root.interval[0]) root.left = insertIntervalTree(root.left, interval);
    else root.right = insertIntervalTree(root.right, interval);
    root.maxEnd = Math.max(root.maxEnd, interval[1]);
    return root;
}
```

```cpp
struct IntervalNode {
    vector<int> interval;
    int maxEnd;
    IntervalNode *left = nullptr, *right = nullptr;
    IntervalNode(vector<int>& i) : interval(i), maxEnd(i[1]) {}
};
IntervalNode* insertIntervalTree(IntervalNode* root, vector<int>& interval) {
    if (!root) return new IntervalNode(interval);
    if (interval[0] < root->interval[0]) root->left = insertIntervalTree(root->left, interval);
    else root->right = insertIntervalTree(root->right, interval);
    root->maxEnd = max(root->maxEnd, interval[1]);
    return root;
}
```

## Edge Cases to Consider
1. Empty interval list
2. Single interval
3. All intervals same
4. No overlap
5. Complete overlap
6. Nested intervals
7. Adjacent intervals
8. Negative time values
9. Large time values
10. Duplicate intervals

## Common Pitfalls
1. Not sorting intervals
2. Incorrect overlap check
3. Not handling empty input
4. Modifying input array
5. Integer overflow
6. Wrong sorting criteria
7. Memory inefficiency
8. Not considering duplicates

## Practice Problems by Difficulty

### Easy
1. [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/) (LC #252)
2. [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/) (LC #986)
3. [Teemo Attacking](https://leetcode.com/problems/teemo-attacking/) (LC #495)

### Medium
1. [Merge Intervals](https://leetcode.com/problems/merge-intervals/) (LC #56)
2. [Insert Interval](https://leetcode.com/problems/insert-interval/) (LC #57)
3. [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) (LC #253)
4. [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) (LC #435)

### Hard
1. [Employee Free Time](https://leetcode.com/problems/employee-free-time/) (LC #759)
2. [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) (LC #452)
3. [Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/) (LC #352)

## Real-World Applications
1. **Calendar Systems**:
   - Meeting scheduling
   - Resource booking
2. **Resource Management**:
   - CPU scheduling
   - Memory allocation
3. **Network Management**:
   - Bandwidth allocation
   - Traffic scheduling
4. **Project Planning**:
   - Task scheduling
   - Timeline management
5. **Database Systems**:
   - Transaction scheduling
   - Lock management
6. **Transportation**:
   - Flight scheduling
   - Vehicle routing

## Advanced Topics
1. **Interval Trees**:
   - Construction
   - Query optimization
2. **Segment Trees**:
   - Range queries
   - Lazy propagation
3. **Line Sweep Algorithms**:
   - Event processing
   - Geometric applications
4. **Dynamic Intervals**:
   - Online algorithms
   - Real-time updates

## Important Resources
1. [Interval Scheduling Algorithms](https://www.cs.princeton.edu/~wayne/kleinberg-tardos/pdf/04GreedyAlgorithmsI.pdf)
2. [Sweep Line Technique](https://www.topcoder.com/thrive/articles/Line%20Sweep%20Algorithms)
3. [Interval Tree Tutorial](https://www.geeksforgeeks.org/interval-tree/)
4. [Meeting Room Problems](https://leetcode.com/problems/meeting-rooms-ii/solution/)
5. [Segment Tree Applications](https://cp-algorithms.com/data_structures/segment_tree.html)

## ❓ FAQ Section

**Q: Should I sort intervals by start time or end time?**
A: It depends on the problem. Sort by start time for merging intervals. Sort by end time for activity selection (maximum non-overlapping intervals). When in doubt, try both approaches.

**Q: How do I check if two intervals overlap?**
A: Two intervals [a,b] and [c,d] overlap if `max(a,c) <= min(b,d)`. Equivalently, they DON'T overlap if one ends before the other starts: `b < c` or `d < a`. Negate this condition.

**Q: What's the sweep line technique?**
A: Convert intervals to events (start = +1, end = -1), sort by time, and sweep through maintaining a running count. Useful for finding maximum overlap, number of concurrent events, or interval coverage.

**Q: How do I merge overlapping intervals?**
A: Sort by start time. Iterate through: if current interval overlaps with previous (current.start <= prev.end), merge them (update end to max of both ends). Otherwise, add current to result.

**Q: How do I find minimum meeting rooms needed?**
A: Use sweep line: create start (+1) and end (-1) events, sort by time (process ends before starts at same time), track max concurrent meetings. Or use min-heap of end times: for each meeting, pop if earliest end ≤ current start, then push current end.

## Interview Tips
1. Clarify interval representation
2. Consider sorting strategy
3. Handle edge cases explicitly
4. Draw timeline diagrams
5. Consider optimization opportunities
6. Test with small examples
7. Analyze time/space complexity
8. Consider follow-up questions
9. Discuss trade-offs
10. Think about scalability