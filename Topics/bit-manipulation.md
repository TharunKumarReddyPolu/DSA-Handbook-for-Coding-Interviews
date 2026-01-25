# Bit Manipulation Techniques 🧮

## Introduction
Bit manipulation involves working with individual bits in numbers. It's essential for optimizing space usage, performing fast arithmetic operations, and solving various algorithmic problems efficiently.

## Prerequisites & Related Topics

### Prerequisites
- Binary number system (base-2 representation)
- Basic programming concepts (integers, operators)
- Understanding of signed vs unsigned integers

### Related Topics
- **Builds on**: [Math](math.md) (number theory basics)
- **Used in**: [Dynamic Programming](dynamic-programming.md) (bitmask DP), [Backtracking](backtracking.md) (subset generation)
- **Optimizes**: [Arrays](arrays.md) operations, space-efficient data storage
- **See also**: [Hashing](hashing.md) (hash functions use bit operations)

## Pattern Recognition Guide

### 🎯 When to Use Bit Manipulation
**Keywords in problem**: "binary", "bits", "XOR", "single number", "power of two", "bitmask"

**Use Bit Manipulation when you see**:
- Problems involving binary representations
- Finding unique elements (XOR properties)
- Subset generation with bitmasks
- Space optimization (flags, states)
- Problems with exactly two states per element

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| XOR Properties | "single number", "appears once", "missing number" | Single Number, Missing Number |
| Power of Two | "power of 2", "binary representation", "count 1s" | Power of Two, Number of 1 Bits |
| Bitmask DP | "subset states", "visit states", "all combinations" | Traveling Salesman, Subsets |
| Bit Flags | "toggle", "on/off", "binary states" | Counting Bits, Reverse Bits |
| Fast Operations | "multiply by 2", "divide by 2", "modulo 2" | Any arithmetic optimization |

### ❌ When NOT to Use
- Working with floating-point numbers
- Problem doesn't involve discrete/binary states
- Simpler arithmetic solution exists
- Language doesn't handle bit operations well for large integers

## Core Concepts

### Important Terminologies
- **Bit**: Binary digit (0 or 1)
- **Bitwise AND (&)**: 1 if both bits are 1
- **Bitwise OR (|)**: 1 if either bit is 1
- **Bitwise XOR (^)**: 1 if bits are different
- **Bitwise NOT (~)**: Inverts all bits
- **Left Shift (<<)**: Shifts bits left
- **Right Shift (>>)**: Shifts bits right
- **Bit Mask**: Pattern for selecting bits
- **Set Bit**: Bit with value 1
- **Clear Bit**: Bit with value 0

### Basic Operations
| Operation | Description | Example |
|-----------|-------------|---------|
| AND (&) | Both 1 | 1100 & 1010 = 1000 |
| OR (\|) | Either 1 | 1100 \| 1010 = 1110 |
| XOR (^) | Different | 1100 ^ 1010 = 0110 |
| NOT (~) | Invert | ~1100 = 0011 |
| Left Shift | Multiply by 2 | 0011 << 1 = 0110 |
| Right Shift | Divide by 2 | 0110 >> 1 = 0011 |

### Time Complexity Analysis
| Operation | Time Complexity |
|-----------|----------------|
| Basic Operations | O(1) |
| Counting Bits | O(1) or O(k)* |
| Bit Traversal | O(log n) |
| Power of 2 Check | O(1) |
| Subset Generation | O(2^n) |

*k = number of set bits (for Brian Kernighan's algorithm)

## Implementation Patterns

### 1. Basic Bit Operations

**Pseudocode:**
```
1. Get bit at position i: shift num right by i, AND with 1
2. Set bit at position i: OR num with (1 shifted left by i)
3. Clear bit at position i: AND num with NOT(1 shifted left by i)
4. Update bit at position i: clear bit first, then OR with (value shifted left by i)
```

```python
def bit_operations(a: int, b: int) -> dict:
    return {
        'AND': a & b,
        'OR': a | b,
        'XOR': a ^ b,
        'NOT_A': ~a,
        'LEFT_SHIFT': a << 1,
        'RIGHT_SHIFT': a >> 1
    }

def get_bit(num: int, i: int) -> bool:
    return bool(num & (1 << i))

def set_bit(num: int, i: int) -> int:
    return num | (1 << i)

def clear_bit(num: int, i: int) -> int:
    mask = ~(1 << i)
    return num & mask

def update_bit(num: int, i: int, value: bool) -> int:
    mask = ~(1 << i)
    return (num & mask) | (value << i)
```

```java
// Basic Bit Operations
public int getBit(int num, int i) { return (num >> i) & 1; }
public int setBit(int num, int i) { return num | (1 << i); }
public int clearBit(int num, int i) { return num & ~(1 << i); }
```

```cpp
// Basic Bit Operations
int getBit(int num, int i) { return (num >> i) & 1; }
int setBit(int num, int i) { return num | (1 << i); }
int clearBit(int num, int i) { return num & ~(1 << i); }
```

### 2. Counting Set Bits

**Pseudocode:**
```
Brian Kernighan's Algorithm:
1. Initialize count = 0
2. While n is not zero:
   a. n = n AND (n - 1)  // This clears the rightmost set bit
   b. Increment count
3. Return count
(Each iteration removes exactly one set bit)
```

```python
def count_set_bits(n: int) -> int:
    """Brian Kernighan's Algorithm"""
    count = 0
    while n:
        n &= (n - 1)  # Clear rightmost set bit
        count += 1
    return count

def count_bits_lookup(n: int) -> int:
    """Using lookup table"""
    table = [0] * 256
    for i in range(256):
        table[i] = (i & 1) + table[i >> 1]
    
    return (table[n & 0xff] +
            table[(n >> 8) & 0xff] +
            table[(n >> 16) & 0xff] +
            table[n >> 24])
```

```java
public int countSetBits(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1);
        count++;
    }
    return count;
}
```

```cpp
int countSetBits(int n) {
    int count = 0;
    while (n) {
        n &= (n - 1);
        count++;
    }
    return count;
}
```

### 3. Power of Two

**Pseudocode:**
```
Check if power of 2:
1. A power of 2 has exactly one bit set (e.g., 8 = 1000)
2. n - 1 flips all bits after that single bit (e.g., 7 = 0111)
3. n AND (n - 1) will be 0 only for powers of 2
4. Return: n > 0 AND (n AND (n - 1)) == 0
```

```python
def is_power_of_two(n: int) -> bool:
    """Check if number is power of 2"""
    return n > 0 and (n & (n - 1)) == 0

def next_power_of_two(n: int) -> int:
    """Find next power of 2"""
    if n <= 0:
        return 1
    
    n -= 1
    n |= n >> 1
    n |= n >> 2
    n |= n >> 4
    n |= n >> 8
    n |= n >> 16
    return n + 1
```

```java
public boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

```cpp
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

### 4. Subset Generation

**Pseudocode:**
```
1. For i from 0 to 2^n - 1 (each i represents a subset):
   a. Create empty subset
   b. For j from 0 to n-1:
      - If bit j is set in i (i AND (1 << j) is non-zero):
        Add nums[j] to subset
   c. Add subset to results
2. Return all subsets
(Each bit position decides include/exclude for that element)
```

```python
def generate_subsets(nums: List[int]) -> List[List[int]]:
    n = len(nums)
    subsets = []
    
    for i in range(1 << n):  # 2^n
        subset = []
        for j in range(n):
            if i & (1 << j):
                subset.append(nums[j])
        subsets.append(subset)
    
    return subsets
```

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    for (int i = 0; i < (1 << n); i++) {
        List<Integer> subset = new ArrayList<>();
        for (int j = 0; j < n; j++)
            if ((i & (1 << j)) != 0) subset.add(nums[j]);
        result.add(subset);
    }
    return result;
}
```

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    vector<vector<int>> result;
    int n = nums.size();
    for (int i = 0; i < (1 << n); i++) {
        vector<int> subset;
        for (int j = 0; j < n; j++)
            if (i & (1 << j)) subset.push_back(nums[j]);
        result.push_back(subset);
    }
    return result;
}
```

### 5. Bit Manipulation Tricks

**Pseudocode:**
```
Common tricks:
- Check if even: (n AND 1) == 0
- Multiply by 2: n << 1 (left shift)
- Divide by 2: n >> 1 (right shift)
- Clear rightmost set bit: n AND (n - 1)
- Get rightmost set bit: n AND (-n)
- Swap without temp: a ^= b; b ^= a; a ^= b
```

```python
def bit_tricks(n: int) -> dict:
    return {
        'is_even': (n & 1) == 0,
        'multiply_by_2': n << 1,
        'divide_by_2': n >> 1,
        'clear_rightmost_set_bit': n & (n - 1),
        'get_rightmost_set_bit': n & (-n),
        'clear_all_bits_except_rightmost': n & (-n),
        'clear_rightmost_bits': n & (n + 1),
        'swap_values': lambda x, y: (x := x ^ y, y := x ^ y, x := x ^ y)
    }
```

```java
// Bit Manipulation Tricks
boolean isEven(int n) { return (n & 1) == 0; }
int multiplyBy2(int n) { return n << 1; }
int divideBy2(int n) { return n >> 1; }
int clearRightmostSetBit(int n) { return n & (n - 1); }
int getRightmostSetBit(int n) { return n & (-n); }
void swap(int[] arr, int i, int j) { arr[i] ^= arr[j]; arr[j] ^= arr[i]; arr[i] ^= arr[j]; }
```

```cpp
// Bit Manipulation Tricks
bool isEven(int n) { return (n & 1) == 0; }
int multiplyBy2(int n) { return n << 1; }
int divideBy2(int n) { return n >> 1; }
int clearRightmostSetBit(int n) { return n & (n - 1); }
int getRightmostSetBit(int n) { return n & (-n); }
void swap(int& a, int& b) { a ^= b; b ^= a; a ^= b; }
```

## Common Techniques

### 1. XOR Properties

**Pseudocode:**
```
Find single number (all others appear twice):
1. Initialize result = 0
2. For each number in array:
   a. result = result XOR number
3. Return result
(XOR properties: a^a=0, a^0=a, commutative - pairs cancel out)
```

```python
def xor_techniques(nums: List[int]) -> dict:
    # Properties:
    # 1. a ^ a = 0
    # 2. a ^ 0 = a
    # 3. a ^ b = b ^ a
    # 4. (a ^ b) ^ c = a ^ (b ^ c)
    
    def find_single_number():
        result = 0
        for num in nums:
            result ^= num
        return result
    
    def swap_numbers(a, b):
        a ^= b
        b ^= a
        a ^= b
        return a, b
    
    return {
        'single_number': find_single_number(),
        'swap_example': swap_numbers(5, 10)
    }
```

```java
public int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) result ^= num;
    return result;
}
```

```cpp
int singleNumber(vector<int>& nums) {
    int result = 0;
    for (int num : nums) result ^= num;
    return result;
}
```

### 2. Bit Masking

**Pseudocode:**
```
- Create mask with n rightmost 1s: (1 << n) - 1
- Clear rightmost n bits: x AND (-1 << n)
- Clear bits from MSB to position i: x AND ((1 << i) - 1)
- Extract bits: x AND mask
```

```python
def bit_masking_examples() -> dict:
    # Create mask with n rightmost 1's
    def create_mask(n):
        return (1 << n) - 1
    
    # Clear rightmost n bits
    def clear_rightmost_bits(x, n):
        return x & (-1 << n)
    
    # Clear bits from MSB to i
    def clear_msb_to_i(x, i):
        return x & ((1 << i) - 1)
    
    return {
        'mask_3_bits': create_mask(3),          # 000...0111
        'clear_right_3': clear_rightmost_bits(0b1111, 3),  # 1000
        'clear_msb_to_2': clear_msb_to_i(0b1111, 2)       # 0011
    }
```

```java
// Bit Masking Operations
int createMask(int n) { return (1 << n) - 1; }
int clearRightmostBits(int x, int n) { return x & (-1 << n); }
int clearMsbToI(int x, int i) { return x & ((1 << i) - 1); }
```

```cpp
// Bit Masking Operations
int createMask(int n) { return (1 << n) - 1; }
int clearRightmostBits(int x, int n) { return x & (-1 << n); }
int clearMsbToI(int x, int i) { return x & ((1 << i) - 1); }
```

## Edge Cases to Consider
1. Zero input
2. Negative numbers
3. Integer overflow
4. All bits set
5. All bits clear
6. Alternating bits
7. Single bit set
8. Maximum integer
9. Minimum integer
10. Power of two values

## Common Pitfalls
1. Sign bit handling
2. Integer overflow
3. Negative shifts
4. Language-specific behavior
5. Operator precedence
6. Unsigned vs signed
7. Off-by-one errors
8. Assuming 32/64 bits

## Practice Problems by Difficulty

### Easy
1. [Single Number](https://leetcode.com/problems/single-number/) (LC #136)
2. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) (LC #191)
3. [Power of Two](https://leetcode.com/problems/power-of-two/) (LC #231)

### Medium
1. [Single Number III](https://leetcode.com/problems/single-number-iii/) (LC #260)
2. [Counting Bits](https://leetcode.com/problems/counting-bits/) (LC #338)
3. [Subsets](https://leetcode.com/problems/subsets/) (LC #78)
4. [Reverse Bits](https://leetcode.com/problems/reverse-bits/) (LC #190)

### Hard
1. [Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) (LC #421)
2. [Minimum Number of Flips to Convert Binary Matrix to Zero Matrix](https://leetcode.com/problems/minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix/) (LC #1284)
3. [Find XOR Sum of All Pairs Bitwise AND](https://leetcode.com/problems/find-xor-sum-of-all-pairs-bitwise-and/) (LC #1835)

## Real-World Applications
1. **Low-Level Programming**:
   - Device drivers
   - Hardware interfaces
2. **Network Programming**:
   - IP address manipulation
   - Network masks
3. **Graphics Programming**:
   - Color manipulation
   - Pixel operations
4. **Cryptography**:
   - Hash functions
   - Encryption
5. **Data Compression**:
   - Run-length encoding
   - Huffman coding
6. **Performance Optimization**:
   - Fast arithmetic
   - Memory efficiency

## Advanced Topics
1. **Advanced Bit Techniques**:
   - Gray code
   - Hamming distance
2. **Bit-Level Parallelism**:
   - SIMD operations
   - Parallel bit counting
3. **Error Detection/Correction**:
   - Parity bits
   - Hamming codes
4. **Specialized Algorithms**:
   - Population count
   - Bit reversal
5. **Hardware Aspects**:
   - CPU flags
   - Cache alignment

## Important Resources
1. [Bit Manipulation Tricks](https://graphics.stanford.edu/~seander/bithacks.html)
2. [Bit Twiddling Hacks](https://bits.stephan-brumme.com/)
3. [Advanced Bit Manipulation](https://www.geeksforgeeks.org/advanced-bit-manipulation/)
4. [Bitwise Operators in Detail](https://www.hackerearth.com/practice/basic-programming/bit-manipulation/basics-of-bit-manipulation/tutorial/)
5. [Bit Manipulation Patterns](https://leetcode.com/discuss/general-discussion/1073221/bit-manipulation-patterns)

## ❓ FAQ Section

**Q: How do I check if a number is a power of 2?**
A: Use `n > 0 && (n & (n-1)) == 0`. Powers of 2 have exactly one bit set. Subtracting 1 flips all bits after that bit, so AND gives 0. Example: 8 (1000) & 7 (0111) = 0.

**Q: What does n & (n-1) do?**
A: It clears the rightmost set bit. This is useful for counting set bits (Brian Kernighan's algorithm: count how many times you can do this before n becomes 0).

**Q: How do I get/set/clear a specific bit?**
A: Get bit i: `(n >> i) & 1`. Set bit i: `n | (1 << i)`. Clear bit i: `n & ~(1 << i)`. Toggle bit i: `n ^ (1 << i)`. These are the fundamental operations for bit manipulation.

**Q: When should I use bit manipulation in interviews?**
A: For problems involving: subsets (2^n possibilities), single number among duplicates (XOR), checking powers of 2, compact state representation, or when the problem explicitly involves binary/bits.

**Q: How does XOR help find the single number?**
A: XOR properties: a^a=0, a^0=a, XOR is commutative and associative. If all numbers appear twice except one, XOR all of them - pairs cancel out (a^a=0), leaving the single number.

## Interview Tips
1. Draw bit patterns
2. Test with small numbers
3. Consider edge cases
4. Use built-in functions
5. Explain bit operations
6. Consider optimization
7. Handle negative numbers
8. Test boundary conditions
9. Consider portability
10. Think about scalability