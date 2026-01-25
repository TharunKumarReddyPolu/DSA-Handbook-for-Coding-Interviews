# Sorting Algorithms 🔃

## Introduction
Sorting is the process of arranging elements in a specific order, typically in ascending or descending order. Understanding sorting algorithms is crucial as they form the basis for many other algorithms and are frequently used in real-world applications.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (in-place sorting, array manipulation)
- Basic comparison operations
- [Recursion](recursion.md) (for divide-and-conquer algorithms like merge sort, quick sort)

### Related Topics
- **Builds on**: [Arrays](arrays.md), [Recursion](recursion.md) (divide and conquer)
- **Used by**: [Heap/Priority Queue](heap-pq.md) (heap sort), [Intervals](intervals.md), [Greedy](greedy.md)
- **Enables**: Binary search, Two Pointers technique
- **See also**: [Cyclic Sort](cyclic-sort.md) (O(n) for specific cases), [Counting/Radix sort](bit-manipulation.md)

## Pattern Recognition Guide

### 🎯 When to Use Sorting
**Keywords in problem**: "sorted order", "arrange", "organize", "k-th largest/smallest", "merge"

**Use Sorting when you see**:
- Need to process elements in order
- Finding k-th element problems
- Merge-related problems
- Interval problems (sort by start/end)
- Problems simplified by ordering

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Algorithm Choice |
|---------|---------------|------------------|
| General sorting | "sort array", "arrange in order" | Quick Sort (avg), Merge Sort (stable) |
| K-th element | "k-th largest", "k-th smallest" | Quick Select O(n), or Heap O(n log k) |
| Nearly sorted | "k positions away", "almost sorted" | Insertion Sort O(nk) |
| Limited range | "values 0 to k", "age/grade sorting" | Counting Sort O(n+k) |
| Stability needed | "preserve original order for ties" | Merge Sort, Counting Sort |
| Custom ordering | "sort by multiple keys", "special order" | Custom comparator |

### Common Sorting Applications
| Problem Type | Why Sort? | Example |
|--------------|-----------|---------|
| Two Pointers | Enables O(n) two-pointer technique | Two Sum II (sorted) |
| Intervals | Process in order by start/end | Merge Intervals |
| Greedy | Sort to make greedy choices | Activity Selection |
| Binary Search | Enables O(log n) search | Search in Sorted Array |

### ❌ When NOT to Use
- Already sorted or order doesn't matter
- Need to maintain insertion order → preserve original
- Hash-based solution is O(n) vs sort O(n log n)
- Streaming data that can't be fully loaded

## Core Concepts

### Important Terminologies
- **In-place Sorting**: Algorithm uses O(1) extra space
- **Stable Sorting**: Preserves relative order of equal elements
- **Comparison Sort**: Uses element comparisons to sort
- **Non-comparison Sort**: Uses element properties to sort
- **Adaptive Sort**: Performance improves with presorted data
- **Internal Sort**: All data fits in main memory
- **External Sort**: Data needs to be retrieved from external storage
- **Natural Sort**: Exploits existing order in the data
- **Hybrid Sort**: Combines multiple sorting algorithms

### Time and Space Complexity Analysis
| Algorithm      | Best       | Average   | Worst      | Space     | Stable | In-place |
|---------------|------------|-----------|------------|-----------|---------|----------|
| Bubble Sort   | O(n)       | O(n²)     | O(n²)      | O(1)      | Yes     | Yes      |
| Selection Sort| O(n²)      | O(n²)     | O(n²)      | O(1)      | No      | Yes      |
| Insertion Sort| O(n)       | O(n²)     | O(n²)      | O(1)      | Yes     | Yes      |
| Merge Sort    | O(n log n) | O(n log n)| O(n log n) | O(n)      | Yes     | No       |
| Quick Sort    | O(n log n) | O(n log n)| O(n²)      | O(log n)  | No      | Yes      |
| Heap Sort     | O(n log n) | O(n log n)| O(n log n) | O(1)      | No      | Yes      |
| Counting Sort | O(n + k)   | O(n + k)  | O(n + k)   | O(k)      | Yes     | No       |
| Radix Sort    | O(d(n + k))| O(d(n + k))| O(d(n + k))| O(n + k) | Yes     | No       |
| Shell Sort    | O(n log n) | O(n^1.3)  | O(n²)      | O(1)      | No      | Yes      |
| Tim Sort      | O(n)       | O(n log n)| O(n log n) | O(n)      | Yes     | No       |

Where:
- n = number of elements
- k = range of elements
- d = number of digits

## Implementation

### 1. Quick Sort

**Pseudocode:**
```
1. If low >= high, return (base case)
2. Partition: choose pivot (e.g., last element)
   a. i = low - 1
   b. For j from low to high-1:
      If arr[j] <= pivot: i++, swap arr[i] and arr[j]
   c. Swap arr[i+1] and pivot
   d. Return i+1 (pivot's final position)
3. Recursively sort left of pivot, then right of pivot
```

```python
def quick_sort(arr, low, high):
    def partition(low, high):
        pivot = arr[high]
        i = low - 1
        
        for j in range(low, high):
            if arr[j] <= pivot:
                i += 1
                arr[i], arr[j] = arr[j], arr[i]
        
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        return i + 1
    
    if low < high:
        pi = partition(low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
```

```java
public void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
private int partition(int[] arr, int low, int high) {
    int pivot = arr[high], i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) { i++; int t = arr[i]; arr[i] = arr[j]; arr[j] = t; }
    }
    int t = arr[i+1]; arr[i+1] = arr[high]; arr[high] = t;
    return i + 1;
}
```

```cpp
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pivot = arr[high], i = low - 1;
        for (int j = low; j < high; j++)
            if (arr[j] <= pivot) swap(arr[++i], arr[j]);
        swap(arr[i + 1], arr[high]);
        int pi = i + 1;
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

### 2. Merge Sort

**Pseudocode:**
```
1. If array length <= 1, return array
2. Split into left and right halves
3. Recursively sort each half
4. Merge sorted halves:
   a. Compare front elements of both halves
   b. Take smaller element, add to result
   c. Repeat until one half empty
   d. Append remaining elements
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
public void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
private void merge(int[] arr, int left, int mid, int right) {
    int[] temp = new int[right - left + 1];
    int i = left, j = mid + 1, k = 0;
    while (i <= mid && j <= right)
        temp[k++] = arr[i] <= arr[j] ? arr[i++] : arr[j++];
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    System.arraycopy(temp, 0, arr, left, temp.length);
}
```

```cpp
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp;
    int i = left, j = mid + 1;
    while (i <= mid && j <= right)
        temp.push_back(arr[i] <= arr[j] ? arr[i++] : arr[j++]);
    while (i <= mid) temp.push_back(arr[i++]);
    while (j <= right) temp.push_back(arr[j++]);
    for (int k = 0; k < temp.size(); k++) arr[left + k] = temp[k];
}
```

### 3. Heap Sort

**Pseudocode:**
```
1. Build max-heap: for i from n/2-1 down to 0, heapify(n, i)
2. Extract elements: for i from n-1 down to 1:
   a. Swap arr[0] with arr[i] (move max to end)
   b. Heapify(i, 0) on reduced heap
Heapify: ensure parent >= children, recursively fix violations
```

```python
def heap_sort(arr):
    def heapify(n, i):
        largest = i
        left = 2 * i + 1
        right = 2 * i + 2
        
        if left < n and arr[left] > arr[largest]:
            largest = left
        if right < n and arr[right] > arr[largest]:
            largest = right
        
        if largest != i:
            arr[i], arr[largest] = arr[largest], arr[i]
            heapify(n, largest)
    
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(n, i)
    
    # Extract elements from heap
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify(i, 0)
```

```java
public void heapSort(int[] arr) {
    int n = arr.length;
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    for (int i = n - 1; i > 0; i--) {
        int temp = arr[0]; arr[0] = arr[i]; arr[i] = temp;
        heapify(arr, i, 0);
    }
}
private void heapify(int[] arr, int n, int i) {
    int largest = i, left = 2 * i + 1, right = 2 * i + 2;
    if (left < n && arr[left] > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;
    if (largest != i) {
        int temp = arr[i]; arr[i] = arr[largest]; arr[largest] = temp;
        heapify(arr, n, largest);
    }
}
```

```cpp
void heapSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
void heapify(vector<int>& arr, int n, int i) {
    int largest = i, left = 2 * i + 1, right = 2 * i + 2;
    if (left < n && arr[left] > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;
    if (largest != i) { swap(arr[i], arr[largest]); heapify(arr, n, largest); }
}
```

### 4. Counting Sort

**Pseudocode:**
```
1. Find min and max values, calculate range
2. Create count array of size range
3. Count occurrences: count[num - min]++
4. Build cumulative count: count[i] += count[i-1]
5. Build output: for each element from end to start:
   a. Place in output at position count[num - min] - 1
   b. Decrement count
(Stable sort, O(n + k) where k is range)
```

```python
def counting_sort(arr):
    if not arr:
        return arr
    
    # Find range of array elements
    max_val = max(arr)
    min_val = min(arr)
    range_val = max_val - min_val + 1
    
    # Initialize counting array and output array
    count = [0] * range_val
    output = [0] * len(arr)
    
    # Store count of each element
    for num in arr:
        count[num - min_val] += 1
    
    # Modify count array to store actual positions
    for i in range(1, len(count)):
        count[i] += count[i - 1]
    
    # Build output array
    for num in reversed(arr):
        index = count[num - min_val] - 1
        output[index] = num
        count[num - min_val] -= 1
    
    return output
```

```java
public int[] countingSort(int[] arr) {
    if (arr.length == 0) return arr;
    int maxVal = Arrays.stream(arr).max().getAsInt();
    int minVal = Arrays.stream(arr).min().getAsInt();
    int range = maxVal - minVal + 1;
    int[] count = new int[range], output = new int[arr.length];
    for (int num : arr) count[num - minVal]++;
    for (int i = 1; i < range; i++) count[i] += count[i - 1];
    for (int i = arr.length - 1; i >= 0; i--) {
        output[count[arr[i] - minVal] - 1] = arr[i];
        count[arr[i] - minVal]--;
    }
    return output;
}
```

```cpp
vector<int> countingSort(vector<int>& arr) {
    if (arr.empty()) return arr;
    int maxVal = *max_element(arr.begin(), arr.end());
    int minVal = *min_element(arr.begin(), arr.end());
    int range = maxVal - minVal + 1;
    vector<int> count(range, 0), output(arr.size());
    for (int num : arr) count[num - minVal]++;
    for (int i = 1; i < range; i++) count[i] += count[i - 1];
    for (int i = arr.size() - 1; i >= 0; i--) {
        output[count[arr[i] - minVal] - 1] = arr[i];
        count[arr[i] - minVal]--;
    }
    return output;
}
```

## Common Techniques

### 1. Three-Way Partitioning (Dutch National Flag)

**Pseudocode:**
```
Sort array with values 0, 1, 2:
1. Initialize low = 0, mid = 0, high = n-1
2. While mid <= high:
   a. If nums[mid] == 0: swap(low, mid), low++, mid++
   b. If nums[mid] == 1: mid++
   c. If nums[mid] == 2: swap(mid, high), high--
(Partitions into: [0s | 1s | 2s])
```

```python
def sort_colors(nums):
    low = mid = 0
    high = len(nums) - 1
    
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:  # nums[mid] == 2
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
```

```java
public void sortColors(int[] nums) {
    int low = 0, mid = 0, high = nums.length - 1;
    while (mid <= high) {
        if (nums[mid] == 0) { swap(nums, low++, mid++); }
        else if (nums[mid] == 1) { mid++; }
        else { swap(nums, mid, high--); }
    }
}
private void swap(int[] nums, int i, int j) { int t = nums[i]; nums[i] = nums[j]; nums[j] = t; }
```

```cpp
void sortColors(vector<int>& nums) {
    int low = 0, mid = 0, high = nums.size() - 1;
    while (mid <= high) {
        if (nums[mid] == 0) swap(nums[low++], nums[mid++]);
        else if (nums[mid] == 1) mid++;
        else swap(nums[mid], nums[high--]);
    }
}
```

### 2. Custom Comparator

**Pseudocode:**
```
Sort with custom ordering:
1. Define comparison function: compare(a, b) returns negative/zero/positive
2. Or define key function: key(x) returns value to sort by
3. For multi-key sort: return tuple (primary_key, secondary_key, ...)
Example: sort strings by length, then alphabetically
key = lambda x: (len(x), x)
```

```python
def custom_sort(arr):
    return sorted(arr, key=lambda x: (priority(x), x))

# Example: Sort strings by length then lexicographically
strings = ["banana", "apple", "cherry"]
sorted_strings = sorted(strings, key=lambda x: (len(x), x))
```

```java
// Example: Sort strings by length then lexicographically
String[] strings = {"banana", "apple", "cherry"};
Arrays.sort(strings, (a, b) -> {
    if (a.length() != b.length()) return a.length() - b.length();
    return a.compareTo(b);
});
```

```cpp
// Example: Sort strings by length then lexicographically
vector<string> strings = {"banana", "apple", "cherry"};
sort(strings.begin(), strings.end(), [](const string& a, const string& b) {
    if (a.size() != b.size()) return a.size() < b.size();
    return a < b;
});
```

## Edge Cases to Consider
1. Empty array
2. Single element array
3. Array with all identical elements
4. Array already sorted
5. Array sorted in reverse
6. Array with negative numbers
7. Array with floating point numbers
8. Very large arrays
9. Arrays with duplicates
10. Arrays with special characters

## Common Pitfalls
1. Not handling empty or single-element arrays
2. Incorrect pivot selection in QuickSort
3. Stack overflow in recursive implementations
4. Not considering stability requirements
5. Inefficient handling of duplicates
6. Memory leaks in implementations
7. Incorrect boundary conditions

## Practice Problems by Difficulty

### Easy
1. [Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity/) (LC #905)
2. [Height Checker](https://leetcode.com/problems/height-checker/) (LC #1051)
3. [Sort Array by Increasing Frequency](https://leetcode.com/problems/sort-array-by-increasing-frequency/) (LC #1636)

### Medium
1. [Sort Colors](https://leetcode.com/problems/sort-colors/) (LC #75)
2. [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/) (LC #451)
3. [Custom Sort String](https://leetcode.com/problems/custom-sort-string/) (LC #791)
4. [Sort the Matrix Diagonally](https://leetcode.com/problems/sort-the-matrix-diagonally/) (LC #1329)

### Hard
1. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) (LC #41)
2. [Maximum Gap](https://leetcode.com/problems/maximum-gap/) (LC #164)
3. [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) (LC #315)

## Real-World Applications
1. **Database Systems**: Indexing and query optimization
2. **File Systems**: File organization
3. **Operating Systems**: Process scheduling
4. **Graphics**: Rendering order
5. **Text Processing**: Dictionary ordering
6. **Network Routing**: Packet scheduling
7. **Computational Biology**: Genome sequencing

## Advanced Topics
1. **External Sorting**:
   - Multi-way merge
   - Replacement selection
2. **Parallel Sorting**:
   - Parallel merge sort
   - Bitonic sort
3. **Specialized Sorting**:
   - Network sorting
   - Pancake sorting
4. **String Sorting**:
   - Radix sort variations
   - Suffix arrays
5. **Online Sorting**:
   - Insertion streams
   - Priority queues

## Important Resources
1. [Sorting Algorithms Visualizations](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)
2. [Python's Timsort Implementation](https://github.com/python/cpython/blob/main/Objects/listsort.txt)
3. [Comparison of Sorting Algorithms](https://www.geeksforgeeks.org/comparison-of-different-sorting-algorithms/)
4. [External Sorting Tutorial](https://www.geeksforgeeks.org/external-sorting/)
5. [Parallel Sorting Algorithms](https://www.geeksforgeeks.org/parallel-sorting-algorithms/)

## ❓ FAQ Section

**Q: Which sorting algorithm should I use?**
A: Quick Sort for general purpose (fast average case). Merge Sort when stability is needed or for linked lists. Heap Sort for guaranteed O(n log n) with O(1) space. Counting/Radix Sort for integers in known range. In practice, use built-in sort (Timsort).

**Q: What does "stable" sorting mean and when does it matter?**
A: Stable sort preserves the relative order of equal elements. It matters when you sort by multiple keys (sort by name, then by age - stable sort keeps name order for same ages) or when original order has meaning.

**Q: When is O(n²) sorting acceptable?**
A: For small arrays (n < 50), insertion sort is often faster than O(n log n) algorithms due to lower overhead. It's also good when array is nearly sorted. That's why Timsort uses insertion sort for small runs.

**Q: How do I sort custom objects?**
A: Define a comparison function or key function. In Python: `sorted(arr, key=lambda x: x.attr)`. In Java: implement Comparable or pass Comparator. Sort by tuple for multiple criteria: `key=lambda x: (x.age, x.name)`.

**Q: What's the lower bound for comparison-based sorting?**
A: Ω(n log n). You can't do better with comparisons alone. Non-comparison sorts (counting, radix, bucket) can achieve O(n) for restricted inputs (integers in range, uniform distribution).

## Interview Tips
1. Clarify input constraints
2. Consider stability requirements
3. Analyze space complexity needs
4. Choose appropriate algorithm
5. Handle edge cases explicitly
6. Consider optimization opportunities
7. Test with small examples
8. Discuss trade-offs between algorithms