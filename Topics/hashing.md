# Hashing and Hash Tables 🗺️

## Introduction
Hashing is a technique that maps data of arbitrary size to fixed-size values. Hash tables use hashing to implement associative arrays, providing fast data access. Understanding hashing is crucial for efficient data storage and retrieval.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (hash tables are built on arrays)
- Basic understanding of modular arithmetic

### Related Topics
- **Builds on**: [Arrays](arrays.md), [Linked List](linked-list.md) (for chaining)
- **Used in**: [Strings](strings.md) (anagram detection, pattern matching), [Graph](graph.md) (adjacency representation)
- **Alternative to**: [Cyclic Sort](cyclic-sort.md) (for finding missing/duplicates in ranges)
- **See also**: [Two Pointers](arrays.md) (sometimes used together for optimization)

## Pattern Recognition Guide

### 🎯 When to Use Hashing
**Keywords in problem**: "find", "lookup", "duplicate", "frequency", "count", "unique", "anagram"

**Use Hashing when you see**:
- Need O(1) average lookup/insert/delete
- Counting frequencies of elements
- Finding duplicates or unique elements
- Checking if element exists
- Grouping elements by some property

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Two Sum Pattern | "pair with sum", "complement exists" | Two Sum, Four Sum |
| Frequency Count | "most frequent", "top k", "count occurrences" | Top K Frequent, First Unique Character |
| Grouping | "group by", "anagrams", "same pattern" | Group Anagrams, Isomorphic Strings |
| Duplicate Detection | "contains duplicate", "find duplicate" | Contains Duplicate, Find Duplicate |
| Subarray Sum | "subarray with sum k", "continuous sum" | Subarray Sum Equals K |
| Sliding Window + Hash | "distinct in window", "anagram in string" | Longest Substring Without Repeating |

### ❌ When NOT to Use
- Need sorted order → use [Sorting](sorting.md) or [Tree](tree.md)-based structures
- Range queries needed → use [Prefix Sum](arrays.md) or Segment Trees
- Memory is extremely limited → consider [Bit Manipulation](bit-manipulation.md)
- Numbers are in range [1, n] → consider [Cyclic Sort](cyclic-sort.md)

## Core Concepts

### Important Terminologies
- **Hash Function**: Maps data to fixed-size value
- **Hash Table**: Data structure using hash function
- **Hash Map**: Key-value pair storage
- **Hash Set**: Unique element storage
- **Collision**: Multiple keys hash to same index
- **Load Factor**: Ratio of entries to table size
- **Chaining**: Collision resolution using linked lists
- **Open Addressing**: Collision resolution by probing
- **Rehashing**: Resizing hash table
- **Bucket**: Storage unit in hash table

### Types of Hash Tables
1. **Separate Chaining**:
   - Uses linked lists for collisions
   - No size limit per bucket
   - Extra space for links

2. **Open Addressing**:
   - Linear Probing
   - Quadratic Probing
   - Double Hashing

### Time Complexity Analysis
| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Insert    | O(1)         | O(n)       |
| Delete    | O(1)         | O(n)       |
| Search    | O(1)         | O(n)       |
| Space     | O(n)         | O(n)       |

Factors affecting performance:
- Load factor
- Hash function quality
- Collision resolution method
- Initial capacity

## Implementation Patterns

### 1. Basic Hash Map Implementation

**Pseudocode:**
```
Hash Map with Chaining:
- _hash(key): return hash(key) mod table_size
- put(key, value): 
  Find bucket using _hash(key)
  If key exists in bucket: update value
  Else: append (key, value) to bucket
- get(key):
  Find bucket, search for key, return value or default
- remove(key):
  Find bucket, remove (key, value) pair if exists
```

```python
class HashMap:
    def __init__(self, size=1000):
        self.size = size
        self.table = [[] for _ in range(self.size)]
    
    def _hash(self, key):
        return hash(key) % self.size
    
    def put(self, key, value):
        hash_key = self._hash(key)
        bucket = self.table[hash_key]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        bucket.append((key, value))
    
    def get(self, key, default=None):
        hash_key = self._hash(key)
        bucket = self.table[hash_key]
        
        for k, v in bucket:
            if k == key:
                return v
        return default
    
    def remove(self, key):
        hash_key = self._hash(key)
        bucket = self.table[hash_key]
        
        for i, (k, v) in enumerate(bucket):
            if k == key:
                del bucket[i]
                return
```

```java
class HashMap<K, V> {
    private List<List<Map.Entry<K, V>>> table;
    private int size;
    
    HashMap(int size) {
        this.size = size;
        table = new ArrayList<>(size);
        for (int i = 0; i < size; i++) table.add(new ArrayList<>());
    }
    
    private int hash(K key) { return Math.abs(key.hashCode()) % size; }
    
    void put(K key, V value) {
        int idx = hash(key);
        List<Map.Entry<K, V>> bucket = table.get(idx);
        for (int i = 0; i < bucket.size(); i++) {
            if (bucket.get(i).getKey().equals(key)) {
                bucket.set(i, Map.entry(key, value));
                return;
            }
        }
        bucket.add(Map.entry(key, value));
    }
    
    V get(K key, V defaultVal) {
        int idx = hash(key);
        for (Map.Entry<K, V> entry : table.get(idx)) {
            if (entry.getKey().equals(key)) return entry.getValue();
        }
        return defaultVal;
    }
    
    void remove(K key) {
        int idx = hash(key);
        table.get(idx).removeIf(e -> e.getKey().equals(key));
    }
}
```

```cpp
template<typename K, typename V>
class HashMap {
    vector<list<pair<K, V>>> table;
    int size;
    
    int hash(const K& key) { return std::hash<K>{}(key) % size; }
public:
    HashMap(int size = 1000) : size(size), table(size) {}
    
    void put(const K& key, const V& value) {
        int idx = hash(key);
        for (auto& [k, v] : table[idx]) {
            if (k == key) { v = value; return; }
        }
        table[idx].push_back({key, value});
    }
    
    V get(const K& key, const V& defaultVal = V{}) {
        int idx = hash(key);
        for (auto& [k, v] : table[idx]) {
            if (k == key) return v;
        }
        return defaultVal;
    }
    
    void remove(const K& key) {
        int idx = hash(key);
        table[idx].remove_if([&](auto& p) { return p.first == key; });
    }
};
```

### 2. Custom Hash Function

**Pseudocode:**
```
Polynomial rolling hash for strings:
1. Initialize hash = 0
2. For each character c in string:
   a. hash = (hash × multiplier + char_code(c)) mod modulus
3. Return hash
(Common values: multiplier = 31, modulus = 10^9 + 7)
```

```python
class CustomHash:
    def __init__(self):
        self.multiplier = 31
        self.modulus = 10**9 + 7
    
    def hash_string(self, s: str) -> int:
        hash_value = 0
        for char in s:
            hash_value = (hash_value * self.multiplier + ord(char)) % self.modulus
        return hash_value
    
    def hash_list(self, lst: list) -> int:
        hash_value = 0
        for item in lst:
            hash_value = (hash_value * self.multiplier + hash(item)) % self.modulus
        return hash_value
```

```java
class CustomHash {
    private static final int MULTIPLIER = 31;
    private static final long MOD = (long) 1e9 + 7;
    
    long hashString(String s) {
        long hash = 0;
        for (char c : s.toCharArray()) {
            hash = (hash * MULTIPLIER + c) % MOD;
        }
        return hash;
    }
    
    long hashList(List<?> lst) {
        long hash = 0;
        for (Object item : lst) {
            hash = (hash * MULTIPLIER + item.hashCode()) % MOD;
        }
        return hash;
    }
}
```

```cpp
class CustomHash {
    static const int MULTIPLIER = 31;
    static const long long MOD = 1e9 + 7;
public:
    long long hashString(const string& s) {
        long long hash = 0;
        for (char c : s) {
            hash = (hash * MULTIPLIER + c) % MOD;
        }
        return hash;
    }
    
    template<typename T>
    long long hashVector(const vector<T>& v) {
        long long hash = 0;
        for (const T& item : v) {
            hash = (hash * MULTIPLIER + std::hash<T>{}(item)) % MOD;
        }
        return hash;
    }
};
```

### 3. Rolling Hash

**Pseudocode:**
```
Rolling hash for sliding window:
1. Compute hash of first window (pattern length)
2. Store highest_power = base^(pattern_length - 1) mod modulus
3. For each position i:
   a. Record current window_hash
   b. To slide window:
      - Remove leftmost char: hash -= char × highest_power
      - Add rightmost char: hash = hash × base + new_char
      - Apply modulus
```

```python
def rolling_hash(text: str, pattern_length: int) -> List[int]:
    if not text or pattern_length <= 0:
        return []
    
    base = 26
    modulus = 10**9 + 7
    
    # Calculate the hash of first window
    window_hash = 0
    highest_power = pow(base, pattern_length - 1, modulus)
    
    for i in range(pattern_length):
        window_hash = (window_hash * base + ord(text[i])) % modulus
    
    result = [window_hash]
    
    # Calculate hash for remaining windows
    for i in range(len(text) - pattern_length):
        # Remove leading digit
        window_hash = window_hash - (ord(text[i]) * highest_power) % modulus
        if window_hash < 0:
            window_hash += modulus
        
        # Add trailing digit
        window_hash = (window_hash * base + ord(text[i + pattern_length])) % modulus
        result.append(window_hash)
    
    return result
```

```java
public long[] rollingHash(String text, int patternLen) {
    long base = 26, mod = (long)1e9 + 7;
    long hash = 0, power = 1;
    for (int i = 0; i < patternLen - 1; i++) power = (power * base) % mod;
    long[] result = new long[text.length() - patternLen + 1];
    for (int i = 0; i < text.length(); i++) {
        hash = (hash * base + text.charAt(i)) % mod;
        if (i >= patternLen) hash = (hash - text.charAt(i - patternLen) * power % mod + mod) % mod;
        if (i >= patternLen - 1) result[i - patternLen + 1] = hash;
    }
    return result;
}
```

```cpp
vector<long long> rollingHash(string text, int patternLen) {
    long long base = 26, mod = 1e9 + 7, hash = 0, power = 1;
    for (int i = 0; i < patternLen - 1; i++) power = (power * base) % mod;
    vector<long long> result;
    for (int i = 0; i < text.size(); i++) {
        hash = (hash * base + text[i]) % mod;
        if (i >= patternLen) hash = (hash - text[i - patternLen] * power % mod + mod) % mod;
        if (i >= patternLen - 1) result.push_back(hash);
    }
    return result;
}
```

### 4. Frequency Counter

**Pseudocode:**
```
Frequency analysis:
1. Create frequency map: for each item, freq[item]++
2. Most common: sort by frequency descending, take top k
3. Unique elements: items where freq[item] == 1
4. Duplicates: items where freq[item] > 1
```

```python
from collections import Counter

def frequency_analysis(items):
    # Count frequencies
    freq = Counter(items)
    
    # Find most common
    most_common = freq.most_common(3)
    
    # Find unique elements
    unique = [item for item, count in freq.items() if count == 1]
    
    # Find duplicates
    duplicates = [item for item, count in freq.items() if count > 1]
    
    return {
        'frequencies': dict(freq),
        'most_common': most_common,
        'unique': unique,
        'duplicates': duplicates
    }
```

```java
Map<String, Object> frequencyAnalysis(int[] items) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int item : items) freq.merge(item, 1, Integer::sum);
    
    // Find most common
    List<int[]> sorted = new ArrayList<>();
    for (var e : freq.entrySet()) sorted.add(new int[]{e.getKey(), e.getValue()});
    sorted.sort((a, b) -> b[1] - a[1]);
    
    // Find unique and duplicates
    List<Integer> unique = new ArrayList<>(), duplicates = new ArrayList<>();
    for (var e : freq.entrySet()) {
        if (e.getValue() == 1) unique.add(e.getKey());
        else duplicates.add(e.getKey());
    }
    
    return Map.of("frequencies", freq, "mostCommon", sorted.subList(0, Math.min(3, sorted.size())),
                  "unique", unique, "duplicates", duplicates);
}
```

```cpp
struct FrequencyResult {
    unordered_map<int, int> frequencies;
    vector<pair<int, int>> mostCommon;
    vector<int> unique, duplicates;
};

FrequencyResult frequencyAnalysis(vector<int>& items) {
    FrequencyResult result;
    for (int item : items) result.frequencies[item]++;
    
    // Find most common
    for (auto& [k, v] : result.frequencies) result.mostCommon.push_back({k, v});
    sort(result.mostCommon.begin(), result.mostCommon.end(), [](auto& a, auto& b) { return b.second < a.second; });
    if (result.mostCommon.size() > 3) result.mostCommon.resize(3);
    
    // Find unique and duplicates
    for (auto& [k, v] : result.frequencies) {
        if (v == 1) result.unique.push_back(k);
        else result.duplicates.push_back(k);
    }
    return result;
}
```

### 5. Two Sum Pattern

**Pseudocode:**
```
1. Create empty hash map (seen)
2. For each (index, num) in array:
   a. complement = target - num
   b. If complement in seen: return [seen[complement], index]
   c. Add num -> index to seen
3. Return empty (no solution found)
```

```python
def two_sum(nums: List[int], target: int) -> List[int]:
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```

```java
public int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> seen = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (seen.containsKey(complement))
            return new int[]{seen.get(complement), i};
        seen.put(nums[i], i);
    }
    return new int[]{};
}
```

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map<int, int> seen;
    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if (seen.count(complement))
            return {seen[complement], i};
        seen[nums[i]] = i;
    }
    return {};
}
```

## Common Techniques

### 1. Collision Resolution

**Pseudocode:**
```
Separate Chaining:
1. Each bucket is a linked list/array
2. On collision: append to bucket's list
3. Search: hash to find bucket, linear search within bucket

Open Addressing (Linear Probing):
1. On collision: try next slot (index + 1) mod size
2. Continue until empty slot found
3. Search: probe starting from hash until found or empty
```

```python
class HashTableWithChaining:
    def __init__(self):
        self.size = 1000
        self.table = [[] for _ in range(self.size)]
    
    def _hash(self, key):
        return sum(ord(c) for c in str(key)) % self.size
    
    def insert(self, key, value):
        index = self._hash(key)
        chain = self.table[index]
        
        # Linear search in chain
        for i, (k, v) in enumerate(chain):
            if k == key:
                chain[i] = (key, value)
                return
        
        chain.append((key, value))
```

### 2. Load Factor Management

**Pseudocode:**
```
Dynamic resizing:
1. load_factor = count / table_size
2. If load_factor >= threshold (e.g., 0.75):
   a. Create new table with 2× size
   b. Rehash all existing entries into new table
   c. Replace old table with new table
```

```python
class DynamicHashTable:
    def __init__(self, initial_size=8):
        self.size = initial_size
        self.count = 0
        self.table = [[] for _ in range(self.size)]
    
    @property
    def load_factor(self):
        return self.count / self.size
    
    def resize(self):
        if self.load_factor >= 0.75:
            old_table = self.table
            self.size *= 2
            self.table = [[] for _ in range(self.size)]
            self.count = 0
            
            for chain in old_table:
                for key, value in chain:
                    self.insert(key, value)
```

## Edge Cases to Consider
1. Empty hash table
2. Single entry
3. Multiple collisions
4. Duplicate keys
5. Non-existent keys
6. Null keys/values
7. Mutable keys
8. Integer overflow
9. High load factor
10. Concurrent access

## Common Pitfalls
1. Poor hash function
2. Not handling collisions
3. Ignoring load factor
4. Mutable key usage
5. Integer overflow
6. Memory leaks
7. Incorrect equality comparison
8. Thread safety issues

## Practice Problems by Difficulty

### Easy
1. [Two Sum](https://leetcode.com/problems/two-sum/) (LC #1)
2. [Valid Anagram](https://leetcode.com/problems/valid-anagram/) (LC #242)
3. [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/) (LC #217)

### Medium
1. [Group Anagrams](https://leetcode.com/problems/group-anagrams/) (LC #49)
2. [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) (LC #347)
3. [Design HashMap](https://leetcode.com/problems/design-hashmap/) (LC #706)
4. [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) (LC #560)

### Hard
1. [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/) (LC #128)
2. [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) (LC #41)
3. [LFU Cache](https://leetcode.com/problems/lfu-cache/) (LC #460)

## Real-World Applications
1. **Database Indexing**:
   - Fast record lookup
   - Unique key constraints
2. **Caching Systems**:
   - Memory caches
   - Web caches
3. **Cryptography**:
   - Password hashing
   - Digital signatures
4. **Data Deduplication**:
   - File systems
   - Data compression
5. **Symbol Tables**:
   - Compiler design
   - Language processing
6. **Load Balancing**:
   - Consistent hashing
   - Server selection

## Advanced Topics
1. **Perfect Hashing**:
   - Static dictionary
   - Zero collision
2. **Consistent Hashing**:
   - Distributed systems
   - Load balancing
3. **Bloom Filters**:
   - Probabilistic data structure
   - Space efficiency
4. **Cuckoo Hashing**:
   - Multiple hash functions
   - Worst-case O(1)
5. **Cryptographic Hashing**:
   - SHA family
   - MD5, bcrypt

## Important Resources
1. [Hash Table Implementation](https://www.geeksforgeeks.org/implementing-hash-table-open-addressing-linear-probing-cpp/)
2. [Collision Resolution Techniques](https://www.cs.cmu.edu/~ab/15-121N11/lectures/lecture16.pdf)
3. [Consistent Hashing](https://www.toptal.com/big-data/consistent-hashing)
4. [Perfect Hashing](https://www.geeksforgeeks.org/perfect-hashing/)
5. [Bloom Filters](https://llimllib.github.io/bloomfilter-tutorial/)

## ❓ FAQ Section

**Q: When should I use a hash map vs other data structures?**
A: Use hash map when you need O(1) average lookup/insert/delete by key: counting frequencies, checking existence, grouping by key. Use tree map when you need sorted order. Use array when keys are small integers.

**Q: How do I handle hash collisions?**
A: Two main approaches: (1) Chaining - each bucket holds a linked list of entries, (2) Open addressing - probe for next empty slot (linear, quadratic, or double hashing). Built-in implementations handle this for you.

**Q: What makes a good hash function?**
A: (1) Deterministic - same input always gives same output, (2) Uniform distribution - spreads keys evenly across buckets, (3) Fast to compute, (4) Minimizes collisions. For interviews, use built-in hash functions.

**Q: What is the Two Sum pattern?**
A: Store complement (target - current) in hash map as you iterate. For each element, check if it exists in map. This converts O(n²) brute force to O(n). Applicable to many "find pair/triplet with sum" problems.

**Q: When should I use a Set vs a Map?**
A: Use Set when you only need to track existence (no associated values): checking for duplicates, membership testing. Use Map when you need to associate values with keys: counting, storing indices, grouping.

## Interview Tips
1. Choose appropriate hash function
2. Consider collision handling
3. Analyze space-time trade-offs
4. Handle edge cases
5. Consider thread safety
6. Use built-in implementations
7. Test with various inputs
8. Consider load factor
9. Discuss scalability
10. Think about security