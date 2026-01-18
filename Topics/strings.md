# String Algorithms and Techniques 🧵

## Introduction
String manipulation and pattern matching are fundamental in computer science. Understanding string algorithms is crucial for text processing, pattern matching, and many real-world applications like text editors, compilers, and search engines.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (strings are character arrays)
- Basic understanding of ASCII/Unicode
- [Hashing](hashing.md) (for rolling hash, pattern matching)

### Related Topics
- **Builds on**: [Arrays](arrays.md) (character array operations)
- **Often uses**: [Hashing](hashing.md) (anagrams, Rabin-Karp), [Two Pointers](arrays.md), Sliding Window
- **Advanced structures**: [Trie](trie.md) (prefix-based operations)
- **See also**: [Dynamic Programming](dynamic-programming.md) (LCS, edit distance), [Stack](stack.md) (parsing)

## Pattern Recognition Guide

### 🎯 When to Use String Algorithms
**Keywords in problem**: "string", "substring", "pattern", "anagram", "palindrome", "parse"

**Use String techniques when you see**:
- Pattern matching or searching
- Anagram or permutation detection
- Palindrome problems
- String transformation
- Parsing or tokenization

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Sliding Window | "substring", "longest without repeating", "anagram in" | Longest Substring, Find Anagrams |
| Two Pointers | "palindrome", "reverse", "compare from ends" | Valid Palindrome, Reverse String |
| Hashing | "anagram", "frequency", "first unique" | Valid Anagram, Group Anagrams |
| Pattern Matching | "find pattern", "strStr", "needle in haystack" | Implement strStr, KMP problems |
| DP on Strings | "edit distance", "longest common", "regex" | Edit Distance, LCS, Regex Match |
| Trie | "prefix", "autocomplete", "word search" | Implement Trie, Word Search II |

### ❌ When NOT to Use
- Problem is purely numeric → use [Math](math.md) or [Arrays](arrays.md)
- Need complex pattern logic → consider [Trie](trie.md) or regex
- Character-level operations not needed → might be simpler

### 💡 Common String Techniques
| Technique | When to Use | Complexity |
|-----------|-------------|------------|
| Sorting chars | Anagram comparison | O(n log n) |
| Hash/Counter | Frequency comparison | O(n) |
| KMP | Multiple pattern searches | O(n + m) |
| Rabin-Karp | Rolling hash, multiple patterns | O(n + m) avg |

## Core Concepts

### Important Terminologies
- **String**: Sequence of characters
- **Substring**: Contiguous sequence within string
- **Subsequence**: Characters in order (not necessarily contiguous)
- **Prefix**: Beginning part of string
- **Suffix**: Ending part of string
- **Pattern**: String to search for
- **Text**: String to search in
- **Palindrome**: Reads same forward and backward
- **Anagram**: Same characters in different order
- **Rolling Hash**: Hash that updates efficiently
- **Edit Distance**: Minimum operations to transform string

### String Properties
1. **Immutability**: Strings may be immutable (language dependent)
2. **Indexing**: Zero-based access to characters
3. **Length**: Number of characters
4. **Character Set**: ASCII, Unicode, etc.
5. **Case Sensitivity**: Upper vs lowercase

### Time Complexity Analysis
| Operation               | Average Case | Worst Case |
|------------------------|--------------|------------|
| Access                 | O(1)         | O(1)       |
| Search                 | O(n)         | O(n)       |
| Insert/Delete         | O(n)         | O(n)       |
| Concatenate           | O(n+m)       | O(n+m)     |
| Pattern Match (Naive) | O(nm)        | O(nm)      |
| Pattern Match (KMP)   | O(n+m)       | O(n+m)     |
| Rabin-Karp           | O(n+m)       | O(nm)      |

Where:
- n = length of text
- m = length of pattern

## Implementation Patterns

### 1. String Matching (KMP Algorithm)
```python
def compute_lps(pattern):
    m = len(pattern)
    lps = [0] * m
    length = 0
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    n, m = len(text), len(pattern)
    lps = compute_lps(pattern)
    
    i = j = 0
    indices = []
    
    while i < n:
        if pattern[j] == text[i]:
            i += 1
            j += 1
        
        if j == m:
            indices.append(i - j)
            j = lps[j - 1]
        elif i < n and pattern[j] != text[i]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    
    return indices
```

```java
public int strStr(String haystack, String needle) {
    int[] lps = computeLPS(needle);
    int i = 0, j = 0;
    while (i < haystack.length()) {
        if (haystack.charAt(i) == needle.charAt(j)) { i++; j++; }
        if (j == needle.length()) return i - j;
        else if (i < haystack.length() && haystack.charAt(i) != needle.charAt(j)) {
            if (j != 0) j = lps[j - 1];
            else i++;
        }
    }
    return -1;
}
private int[] computeLPS(String pattern) {
    int[] lps = new int[pattern.length()];
    int len = 0, i = 1;
    while (i < pattern.length()) {
        if (pattern.charAt(i) == pattern.charAt(len)) lps[i++] = ++len;
        else if (len != 0) len = lps[len - 1];
        else lps[i++] = 0;
    }
    return lps;
}
```

```cpp
int strStr(string haystack, string needle) {
    vector<int> lps(needle.size(), 0);
    for (int i = 1, len = 0; i < needle.size();) {
        if (needle[i] == needle[len]) lps[i++] = ++len;
        else if (len) len = lps[len - 1];
        else lps[i++] = 0;
    }
    for (int i = 0, j = 0; i < haystack.size();) {
        if (haystack[i] == needle[j]) { i++; j++; }
        if (j == needle.size()) return i - j;
        else if (i < haystack.size() && haystack[i] != needle[j]) {
            if (j) j = lps[j - 1];
            else i++;
        }
    }
    return -1;
}
```

### 2. Rabin-Karp Algorithm
```python
def rabin_karp(text, pattern):
    n, m = len(text), len(pattern)
    if m > n:
        return []
    
    # Prime number for hash
    prime = 101
    # Number of characters in input alphabet
    d = 256
    
    # Calculate pattern hash
    pattern_hash = 0
    window_hash = 0
    h = pow(d, m-1) % prime
    
    # Calculate initial hash values
    for i in range(m):
        pattern_hash = (d * pattern_hash + ord(pattern[i])) % prime
        window_hash = (d * window_hash + ord(text[i])) % prime
    
    result = []
    
    # Slide pattern over text
    for i in range(n - m + 1):
        if pattern_hash == window_hash:
            if text[i:i+m] == pattern:
                result.append(i)
        
        if i < n - m:
            window_hash = (d * (window_hash - ord(text[i]) * h) + 
                         ord(text[i + m])) % prime
            if window_hash < 0:
                window_hash += prime
    
    return result
```

```java
public List<Integer> rabinKarp(String text, String pattern) {
    List<Integer> result = new ArrayList<>();
    int n = text.length(), m = pattern.length();
    if (m > n) return result;
    long prime = 101, d = 256, h = 1;
    for (int i = 0; i < m - 1; i++) h = (h * d) % prime;
    long patHash = 0, winHash = 0;
    for (int i = 0; i < m; i++) {
        patHash = (d * patHash + pattern.charAt(i)) % prime;
        winHash = (d * winHash + text.charAt(i)) % prime;
    }
    for (int i = 0; i <= n - m; i++) {
        if (patHash == winHash && text.substring(i, i + m).equals(pattern)) result.add(i);
        if (i < n - m) {
            winHash = (d * (winHash - text.charAt(i) * h) + text.charAt(i + m)) % prime;
            if (winHash < 0) winHash += prime;
        }
    }
    return result;
}
```

```cpp
vector<int> rabinKarp(string text, string pattern) {
    vector<int> result;
    int n = text.size(), m = pattern.size();
    if (m > n) return result;
    long long prime = 101, d = 256, h = 1;
    for (int i = 0; i < m - 1; i++) h = (h * d) % prime;
    long long patHash = 0, winHash = 0;
    for (int i = 0; i < m; i++) {
        patHash = (d * patHash + pattern[i]) % prime;
        winHash = (d * winHash + text[i]) % prime;
    }
    for (int i = 0; i <= n - m; i++) {
        if (patHash == winHash && text.substr(i, m) == pattern) result.push_back(i);
        if (i < n - m) {
            winHash = (d * (winHash - text[i] * h) + text[i + m]) % prime;
            if (winHash < 0) winHash += prime;
        }
    }
    return result;
}
```

### 3. Palindrome Check
```python
def is_palindrome(s: str) -> bool:
    # Remove non-alphanumeric and convert to lowercase
    s = ''.join(c.lower() for c in s if c.isalnum())
    return s == s[::-1]

def longest_palindrome_substring(s: str) -> str:
    if not s:
        return ""
    
    def expand_around_center(left: int, right: int) -> int:
        while left >= 0 and right < len(s) and s[left] == s[right]:
            left -= 1
            right += 1
        return right - left - 1
    
    start = end = 0
    for i in range(len(s)):
        len1 = expand_around_center(i, i)
        len2 = expand_around_center(i, i + 1)
        length = max(len1, len2)
        if length > end - start:
            start = i - (length - 1) // 2
            end = i + length // 2
    
    return s[start:end + 1]
```

```java
public boolean isPalindrome(String s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) left++;
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) right--;
        if (Character.toLowerCase(s.charAt(left)) != Character.toLowerCase(s.charAt(right)))
            return false;
        left++; right--;
    }
    return true;
}
```

```cpp
bool isPalindrome(string s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        while (left < right && !isalnum(s[left])) left++;
        while (left < right && !isalnum(s[right])) right--;
        if (tolower(s[left]) != tolower(s[right])) return false;
        left++; right--;
    }
    return true;
}
```

### 4. Anagram Detection
```python
from collections import Counter

def are_anagrams(s1: str, s2: str) -> bool:
    return Counter(s1) == Counter(s2)

def find_all_anagrams(s: str, p: str) -> List[int]:
    result = []
    p_count = Counter(p)
    window_count = Counter()
    
    for i in range(len(s)):
        # Add new character
        window_count[s[i]] += 1
        
        # Remove character from window if window size > pattern
        if i >= len(p):
            window_count[s[i - len(p)]] -= 1
            if window_count[s[i - len(p)]] == 0:
                del window_count[s[i - len(p)]]
        
        # Check if current window is anagram
        if window_count == p_count:
            result.append(i - len(p) + 1)
    
    return result
```

```java
public List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();
    if (s.length() < p.length()) return result;
    int[] pCount = new int[26], sCount = new int[26];
    for (char c : p.toCharArray()) pCount[c - 'a']++;
    for (int i = 0; i < s.length(); i++) {
        sCount[s.charAt(i) - 'a']++;
        if (i >= p.length()) sCount[s.charAt(i - p.length()) - 'a']--;
        if (Arrays.equals(pCount, sCount)) result.add(i - p.length() + 1);
    }
    return result;
}
```

```cpp
vector<int> findAnagrams(string s, string p) {
    vector<int> result, pCount(26, 0), sCount(26, 0);
    if (s.size() < p.size()) return result;
    for (char c : p) pCount[c - 'a']++;
    for (int i = 0; i < s.size(); i++) {
        sCount[s[i] - 'a']++;
        if (i >= p.size()) sCount[s[i - p.size()] - 'a']--;
        if (pCount == sCount) result.push_back(i - p.size() + 1);
    }
    return result;
}
```

### 5. String Compression
```python
def compress(chars: List[str]) -> int:
    write = anchor = 0
    for read, char in enumerate(chars):
        if read + 1 == len(chars) or chars[read + 1] != char:
            chars[write] = chars[anchor]
            write += 1
            
            if read > anchor:
                count = read - anchor + 1
                for digit in str(count):
                    chars[write] = digit
                    write += 1
            
            anchor = read + 1
    
    return write
```

```java
public int compress(char[] chars) {
    int write = 0, anchor = 0;
    for (int read = 0; read < chars.length; read++) {
        if (read + 1 == chars.length || chars[read + 1] != chars[read]) {
            chars[write++] = chars[anchor];
            if (read > anchor) {
                for (char c : String.valueOf(read - anchor + 1).toCharArray())
                    chars[write++] = c;
            }
            anchor = read + 1;
        }
    }
    return write;
}
```

```cpp
int compress(vector<char>& chars) {
    int write = 0, anchor = 0;
    for (int read = 0; read < chars.size(); read++) {
        if (read + 1 == chars.size() || chars[read + 1] != chars[read]) {
            chars[write++] = chars[anchor];
            if (read > anchor) {
                string cnt = to_string(read - anchor + 1);
                for (char c : cnt) chars[write++] = c;
            }
            anchor = read + 1;
        }
    }
    return write;
}
```

## Common Techniques

### 1. Sliding Window
```python
def longest_substring_without_repeating(s: str) -> int:
    char_index = {}
    max_length = start = 0
    
    for i, char in enumerate(s):
        if char in char_index and char_index[char] >= start:
            start = char_index[char] + 1
        else:
            max_length = max(max_length, i - start + 1)
        char_index[char] = i
    
    return max_length
```

```java
public int lengthOfLongestSubstring(String s) {
    Map<Character, Integer> charIndex = new HashMap<>();
    int maxLen = 0, start = 0;
    for (int i = 0; i < s.length(); i++) {
        char c = s.charAt(i);
        if (charIndex.containsKey(c) && charIndex.get(c) >= start)
            start = charIndex.get(c) + 1;
        else
            maxLen = Math.max(maxLen, i - start + 1);
        charIndex.put(c, i);
    }
    return maxLen;
}
```

```cpp
int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> charIndex;
    int maxLen = 0, start = 0;
    for (int i = 0; i < s.size(); i++) {
        if (charIndex.count(s[i]) && charIndex[s[i]] >= start)
            start = charIndex[s[i]] + 1;
        else
            maxLen = max(maxLen, i - start + 1);
        charIndex[s[i]] = i;
    }
    return maxLen;
}
```

### 2. Two Pointers
```python
def reverse_string(s: List[str]) -> None:
    left, right = 0, len(s) - 1
    while left < right:
        s[left], s[right] = s[right], s[left]
        left += 1
        right -= 1
```

```java
public void reverseString(char[] s) {
    int left = 0, right = s.length - 1;
    while (left < right) {
        char temp = s[left];
        s[left++] = s[right];
        s[right--] = temp;
    }
}
```

```cpp
void reverseString(vector<char>& s) {
    int left = 0, right = s.size() - 1;
    while (left < right) {
        swap(s[left++], s[right--]);
    }
}
```

## Edge Cases to Consider
1. Empty string
2. Single character string
3. All same characters
4. Special characters
5. Unicode characters
6. Whitespace
7. Case sensitivity
8. Very long strings
9. Pattern longer than text
10. Overlapping patterns

## Common Pitfalls
1. Not handling empty strings
2. Incorrect case handling
3. Buffer overflow
4. Unicode issues
5. Inefficient concatenation
6. Memory leaks
7. Off-by-one errors
8. Incorrect boundary checks

## Practice Problems by Difficulty

### Easy
1. [Valid Anagram](https://leetcode.com/problems/valid-anagram/) (LC #242)
2. [Reverse String](https://leetcode.com/problems/reverse-string/) (LC #344)
3. [First Unique Character](https://leetcode.com/problems/first-unique-character-in-a-string/) (LC #387)

### Medium
1. [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) (LC #3)
2. [Group Anagrams](https://leetcode.com/problems/group-anagrams/) (LC #49)
3. [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) (LC #5)
4. [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/) (LC #8)

### Hard
1. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/) (LC #10)
2. [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) (LC #76)
3. [Text Justification](https://leetcode.com/problems/text-justification/) (LC #68)

## Real-World Applications
1. **Text Editors**:
   - Search and replace
   - Syntax highlighting
2. **Compilers**:
   - Lexical analysis
   - Pattern matching
3. **Search Engines**:
   - Pattern matching
   - Relevance ranking
4. **Bioinformatics**:
   - DNA sequence matching
   - Protein analysis
5. **Data Validation**:
   - Input sanitization
   - Format checking
6. **Natural Language Processing**:
   - Text analysis
   - Language detection

## Advanced Topics
1. **String Matching Algorithms**:
   - Boyer-Moore
   - Aho-Corasick
2. **Suffix Arrays/Trees**:
   - Construction
   - Applications
3. **Regular Expressions**:
   - Pattern compilation
   - Efficient matching
4. **Unicode Handling**:
   - Normalization
   - Collation
5. **String Distance Metrics**:
   - Levenshtein
   - Hamming

## Important Resources
1. [String Algorithms](https://www.geeksforgeeks.org/string-algorithms/)
2. [KMP Algorithm](https://www.geeksforgeeks.org/kmp-algorithm-for-pattern-searching/)
3. [Rabin-Karp Algorithm](https://www.geeksforgeeks.org/rabin-karp-algorithm-for-pattern-searching/)
4. [String Matching Patterns](https://www.cs.princeton.edu/~rs/AlgsDS07/21PatternMatching.pdf)
5. [Unicode Standards](https://www.unicode.org/standard/standard.html)

## ❓ FAQ Section

**Q: When should I use KMP vs Rabin-Karp for pattern matching?**
A: KMP is better for single pattern search with guaranteed O(n+m). Rabin-Karp is better for multiple pattern search or when you want simpler code (rolling hash). Rabin-Karp has O(nm) worst case due to hash collisions but O(n+m) average.

**Q: How do I efficiently check if two strings are anagrams?**
A: (1) Sort both and compare - O(n log n), (2) Use character frequency count/hash map - O(n), (3) Use a single array of size 26 for lowercase letters. Method 2/3 is preferred for efficiency.

**Q: How do I handle case sensitivity in string problems?**
A: Always clarify with interviewer. Common approaches: (1) Convert to lowercase at start, (2) Use case-insensitive comparison, (3) Treat as separate characters. Document your assumption.

**Q: What's the best way to build strings in a loop?**
A: Use StringBuilder (Java) or list with join (Python) instead of string concatenation. String concatenation creates new string objects each time: O(n²) total. StringBuilder/list.join: O(n) total.

**Q: When should I use Trie vs HashMap for string problems?**
A: Use Trie for prefix-based operations (autocomplete, prefix search, word validation with wildcards). Use HashMap for exact lookups, frequency counting, or when memory is constrained. Trie has O(m) operations where m is string length.

## Interview Tips
1. Clarify string properties
2. Consider case sensitivity
3. Handle special characters
4. Check for constraints
5. Consider space optimization
6. Use built-in functions wisely
7. Test with edge cases
8. Consider performance
9. Handle invalid input
10. Think about scalability