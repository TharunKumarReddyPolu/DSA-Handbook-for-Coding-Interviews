# Trie (Prefix Tree) Data Structure 🔤

## Introduction
A trie (pronounced "try") is a tree-like data structure used to store and retrieve strings. It's particularly efficient for prefix-based operations and is commonly used in applications like autocomplete, spell checkers, and IP routing tables.

## Prerequisites & Related Topics

### Prerequisites
- [Tree](tree.md) (trie is a specialized tree structure)
- [Strings](strings.md) (operations on character sequences)
- [Recursion](recursion.md) (for traversal and search operations)

### Related Topics
- **Builds on**: [Tree](tree.md) (tree traversal concepts), [Strings](strings.md) (character processing)
- **Alternative to**: [Hashing](hashing.md) (for string lookups, with different trade-offs)
- **Used with**: [Backtracking](backtracking.md) (word search problems), [Graph](graph.md) (word ladder)
- **See also**: Suffix Trees, Radix Trees (compressed tries)

## Pattern Recognition Guide

### 🎯 When to Use Trie
**Keywords in problem**: "prefix", "dictionary", "autocomplete", "word search", "starts with"

**Use Trie when you see**:
- Prefix-based searching or matching
- Autocomplete/typeahead functionality
- Word dictionary with frequent lookups
- Finding words with common prefixes
- Word search in a grid with dictionary

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Prefix Search | "starts with", "words with prefix", "autocomplete" | Implement Trie, Search Suggestions |
| Word Dictionary | "add word", "search word", "dictionary" | Add and Search Word |
| Word Search | "find words in grid", "word search with list" | Word Search II |
| Longest Prefix | "longest common prefix", "longest matching" | Longest Common Prefix |
| XOR Problems | "maximum XOR", "XOR queries" | Maximum XOR of Two Numbers |
| Stream Matching | "stream of characters", "match suffix" | Stream of Characters |

### ❌ When NOT to Use
- Exact word lookup only → use [Hashing](hashing.md) (simpler, O(1))
- Small dictionary → simple array/set might suffice
- No prefix-based operations needed
- Memory is constrained (tries can use significant space)

### 💡 Trie vs Hash Map
| Operation | Trie | Hash Map |
|-----------|------|----------|
| Exact lookup | O(m) | O(m) avg |
| Prefix search | O(p + results) | O(n) scan needed |
| Autocomplete | ✅ Natural fit | ❌ Requires extra work |
| Space | O(ALPHABET × m × n) | O(total chars) |
| Memory locality | Poor | Better |

*m = word length, n = number of words, p = prefix length*

## Core Concepts

### Important Terminologies
- **Trie**: Tree-like data structure for storing strings
- **Node**: Contains children references and flags
- **Root**: Starting node of the trie
- **Prefix**: Common starting characters of strings
- **Terminal Node**: Marks end of a complete word
- **Children**: Next characters in sequences
- **Branch**: Path from root to any node
- **Leaf**: Node with no children
- **Pattern**: Sequence of characters to match

### Time Complexity Analysis
| Operation          | Average Case | Worst Case |
|-------------------|--------------|------------|
| Insert            | O(m)         | O(m)       |
| Search            | O(m)         | O(m)       |
| Delete            | O(m)         | O(m)       |
| Prefix Search     | O(p + n)     | O(p + n)   |
| Space Complexity  | O(ALPHABET_SIZE * m * n) | O(ALPHABET_SIZE * m * n) |

Where:
- m = length of word
- p = length of prefix
- n = number of matches
- ALPHABET_SIZE = size of character set

## Implementation

### Basic Trie Node

**Pseudocode:**
```
TrieNode structure:
- children: map/array of child nodes (26 for lowercase letters)
- is_end_of_word: boolean flag marking complete words
```

```python
class TrieNode:
    def __init__(self):
        self.children = {}  # or [None] * ALPHABET_SIZE
        self.is_end_of_word = False
```

```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}
```

```cpp
struct TrieNode {
    TrieNode* children[26] = {nullptr};
    bool isEndOfWord = false;
};
```

### Complete Trie Implementation

**Pseudocode:**
```
Insert(word):
1. Start at root
2. For each char in word:
   a. If char not in children: create new node
   b. Move to child node
3. Mark current node as end of word

Search(word):
1. Traverse trie following word characters
2. If any char missing: return false
3. Return node.is_end_of_word

StartsWith(prefix):
1. Traverse trie following prefix characters
2. If any char missing: return false
3. Return true (prefix exists)
```

```python
class Trie:
    def __init__(self):
        self.root = TrieNode()
    
    def insert(self, word: str) -> None:
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True
    
    def search(self, word: str) -> bool:
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word
    
    def starts_with(self, prefix: str) -> bool:
        node = self.root
        for char in prefix:
            if char not in node.children:
                return False
            node = node.children[char]
        return True
```

```java
class Trie {
    private TrieNode root = new TrieNode();
    
    public void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            if (node.children[c - 'a'] == null)
                node.children[c - 'a'] = new TrieNode();
            node = node.children[c - 'a'];
        }
        node.isEndOfWord = true;
    }
    
    public boolean search(String word) {
        TrieNode node = searchPrefix(word);
        return node != null && node.isEndOfWord;
    }
    
    public boolean startsWith(String prefix) {
        return searchPrefix(prefix) != null;
    }
    
    private TrieNode searchPrefix(String prefix) {
        TrieNode node = root;
        for (char c : prefix.toCharArray()) {
            if (node.children[c - 'a'] == null) return null;
            node = node.children[c - 'a'];
        }
        return node;
    }
}
```

```cpp
class Trie {
    TrieNode* root = new TrieNode();
public:
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c - 'a'])
                node->children[c - 'a'] = new TrieNode();
            node = node->children[c - 'a'];
        }
        node->isEndOfWord = true;
    }
    
    bool search(string word) {
        TrieNode* node = searchPrefix(word);
        return node && node->isEndOfWord;
    }
    
    bool startsWith(string prefix) {
        return searchPrefix(prefix) != nullptr;
    }
    
private:
    TrieNode* searchPrefix(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            if (!node->children[c - 'a']) return nullptr;
            node = node->children[c - 'a'];
        }
        return node;
    }
};
```

**Pseudocode (Get Words with Prefix):**
```
1. Navigate to prefix node (return empty if prefix doesn't exist)
2. DFS from prefix node to find all complete words:
   a. If current node is end of word: add current path to results
   b. For each child: recurse with extended path
3. Return all words found
```

```python
    def get_words_with_prefix(self, prefix: str) -> List[str]:
        words = []
        node = self.root
        
        # Traverse to prefix node
        for char in prefix:
            if char not in node.children:
                return words
            node = node.children[char]
        
        # DFS to find all words
        def dfs(node, curr_word):
            if node.is_end_of_word:
                words.append(curr_word)
            
            for char, child in node.children.items():
                dfs(child, curr_word + char)
        
        dfs(node, prefix)
        return words
```

```java
public List<String> getWordsWithPrefix(String prefix) {
    List<String> words = new ArrayList<>();
    TrieNode node = root;
    for (char c : prefix.toCharArray()) {
        if (node.children[c - 'a'] == null) return words;
        node = node.children[c - 'a'];
    }
    dfs(node, new StringBuilder(prefix), words);
    return words;
}
private void dfs(TrieNode node, StringBuilder curr, List<String> words) {
    if (node.isEndOfWord) words.add(curr.toString());
    for (char c = 'a'; c <= 'z'; c++) {
        if (node.children[c - 'a'] != null) {
            curr.append(c);
            dfs(node.children[c - 'a'], curr, words);
            curr.deleteCharAt(curr.length() - 1);
        }
    }
}
```

```cpp
vector<string> getWordsWithPrefix(string prefix) {
    vector<string> words;
    TrieNode* node = root;
    for (char c : prefix) {
        if (!node->children[c - 'a']) return words;
        node = node->children[c - 'a'];
    }
    dfs(node, prefix, words);
    return words;
}
void dfs(TrieNode* node, string curr, vector<string>& words) {
    if (node->isEndOfWord) words.push_back(curr);
    for (char c = 'a'; c <= 'z'; c++) {
        if (node->children[c - 'a']) {
            dfs(node->children[c - 'a'], curr + c, words);
        }
    }
}
```

## Common Applications

### 1. Autocomplete System

**Pseudocode:**
```
Autocomplete with frequency ranking:
1. Build trie from dictionary words with frequency counts
2. On input prefix:
   a. Get all words matching prefix using trie traversal
   b. Sort by frequency (descending), then alphabetically
   c. Return top k suggestions (typically 3)
3. On complete (#): add/update word in dictionary
```

```python
class AutocompleteSystem:
    def __init__(self, words: List[str], times: List[int]):
        self.trie = Trie()
        self.word_count = {}
        for word, count in zip(words, times):
            self.word_count[word] = count
            self.trie.insert(word)
    
    def input(self, c: str) -> List[str]:
        if c == '#':
            # Complete current word
            return []
        
        # Get suggestions
        suggestions = self.trie.get_words_with_prefix(c)
        # Sort by frequency and lexicographically
        suggestions.sort(key=lambda x: (-self.word_count.get(x, 0), x))
        return suggestions[:3]  # Return top 3
```

```java
class AutocompleteSystem {
    Trie trie = new Trie();
    Map<String, Integer> wordCount = new HashMap<>();
    
    AutocompleteSystem(String[] words, int[] times) {
        for (int i = 0; i < words.length; i++) {
            wordCount.put(words[i], times[i]);
            trie.insert(words[i]);
        }
    }
    
    List<String> input(String prefix) {
        List<String> suggestions = trie.getWordsWithPrefix(prefix);
        suggestions.sort((a, b) -> {
            int cntA = wordCount.getOrDefault(a, 0);
            int cntB = wordCount.getOrDefault(b, 0);
            return cntA != cntB ? cntB - cntA : a.compareTo(b);
        });
        return suggestions.subList(0, Math.min(3, suggestions.size()));
    }
}
```

```cpp
class AutocompleteSystem {
    Trie trie;
    unordered_map<string, int> wordCount;
public:
    AutocompleteSystem(vector<string>& words, vector<int>& times) {
        for (int i = 0; i < words.size(); i++) {
            wordCount[words[i]] = times[i];
            trie.insert(words[i]);
        }
    }
    
    vector<string> input(string prefix) {
        vector<string> suggestions = trie.getWordsWithPrefix(prefix);
        sort(suggestions.begin(), suggestions.end(), [&](auto& a, auto& b) {
            return wordCount[a] != wordCount[b] ? wordCount[a] > wordCount[b] : a < b;
        });
        if (suggestions.size() > 3) suggestions.resize(3);
        return suggestions;
    }
};
```

### 2. Spell Checker

**Pseudocode:**
```
Suggest corrections within 1 edit distance:
1. Build trie from dictionary
2. DFS with edit tracking:
   a. If edits > max allowed: return
   b. If at end and edits valid: add to suggestions
   c. For each child char: recurse with or without counting edit
```

```python
class SpellChecker:
    def __init__(self, dictionary: List[str]):
        self.trie = Trie()
        for word in dictionary:
            self.trie.insert(word)
    
    def suggest_corrections(self, word: str) -> List[str]:
        suggestions = []
        
        def dfs(node, curr_word, edits):
            if edits > 1:  # Allow max 1 edit
                return
            
            if node.is_end_of_word and edits <= 1:
                suggestions.append(curr_word)
            
            for char in 'abcdefghijklmnopqrstuvwxyz':
                if char in node.children:
                    # No edit
                    dfs(node.children[char], curr_word + char, edits)
                else:
                    # Try with edit
                    dfs(node.children.get(char, TrieNode()), 
                        curr_word + char, edits + 1)
        
        dfs(self.trie.root, "", 0)
        return suggestions
```

```java
class SpellChecker {
    Trie trie = new Trie();
    
    SpellChecker(String[] dictionary) {
        for (String word : dictionary) trie.insert(word);
    }
    
    List<String> suggestCorrections(String word) {
        List<String> suggestions = new ArrayList<>();
        dfs(trie.root, new StringBuilder(), word, 0, 0, suggestions);
        return suggestions;
    }
    
    void dfs(TrieNode node, StringBuilder curr, String target, int idx, int edits, List<String> suggestions) {
        if (edits > 1) return;
        if (node.isEndOfWord && idx == target.length()) suggestions.add(curr.toString());
        if (idx >= target.length()) return;
        for (char c = 'a'; c <= 'z'; c++) {
            if (node.children[c - 'a'] != null) {
                curr.append(c);
                int newEdits = (c == target.charAt(idx)) ? edits : edits + 1;
                dfs(node.children[c - 'a'], curr, target, idx + 1, newEdits, suggestions);
                curr.deleteCharAt(curr.length() - 1);
            }
        }
    }
}
```

```cpp
class SpellChecker {
    Trie trie;
public:
    SpellChecker(vector<string>& dictionary) {
        for (auto& word : dictionary) trie.insert(word);
    }
    
    vector<string> suggestCorrections(string word) {
        vector<string> suggestions;
        dfs(trie.root, "", word, 0, 0, suggestions);
        return suggestions;
    }
    
    void dfs(TrieNode* node, string curr, string& target, int idx, int edits, vector<string>& suggestions) {
        if (edits > 1) return;
        if (node->isEndOfWord && idx == target.size()) suggestions.push_back(curr);
        if (idx >= target.size()) return;
        for (char c = 'a'; c <= 'z'; c++) {
            if (node->children[c - 'a']) {
                int newEdits = (c == target[idx]) ? edits : edits + 1;
                dfs(node->children[c - 'a'], curr + c, target, idx + 1, newEdits, suggestions);
            }
        }
    }
};
```

## Common Patterns

### 1. Word Dictionary with Wildcards

**Pseudocode:**
```
Search with wildcard '.' matching any character:
1. Use DFS with current node, word, and index
2. Base case: if index == word length, return node.is_end_of_word
3. If current char is '.':
   a. Try ALL children recursively
   b. Return true if any child path matches
4. Else: if char not in children return false
   Otherwise: continue DFS with matching child
```

```python
def search_with_dots(self, word: str) -> bool:
    def dfs(node, word, index):
        if index == len(word):
            return node.is_end_of_word
        
        if word[index] == '.':
            for child in node.children.values():
                if dfs(child, word, index + 1):
                    return True
            return False
        
        if word[index] not in node.children:
            return False
        
        return dfs(node.children[word[index]], word, index + 1)
    
    return dfs(self.root, word, 0)
```

```java
public boolean searchWithDots(String word) {
    return dfs(root, word, 0);
}
private boolean dfs(TrieNode node, String word, int index) {
    if (index == word.length()) return node.isEndOfWord;
    char c = word.charAt(index);
    if (c == '.') {
        for (TrieNode child : node.children) {
            if (child != null && dfs(child, word, index + 1)) return true;
        }
        return false;
    }
    if (node.children[c - 'a'] == null) return false;
    return dfs(node.children[c - 'a'], word, index + 1);
}
```

```cpp
bool searchWithDots(string word) {
    return dfs(root, word, 0);
}
bool dfs(TrieNode* node, string& word, int index) {
    if (index == word.size()) return node->isEndOfWord;
    char c = word[index];
    if (c == '.') {
        for (int i = 0; i < 26; i++) {
            if (node->children[i] && dfs(node->children[i], word, index + 1)) return true;
        }
        return false;
    }
    if (!node->children[c - 'a']) return false;
    return dfs(node->children[c - 'a'], word, index + 1);
}
```

### 2. Replace Words with Roots

**Pseudocode:**
```
1. Build trie from all root words
2. For each word in sentence:
   a. Traverse trie character by character
   b. If reach end-of-word marker: replace with root (prefix)
   c. If can't continue: keep original word
3. Return modified sentence
```

```python
def replace_with_root(self, sentence: str, roots: List[str]) -> str:
    # Build trie with roots
    trie = Trie()
    for root in roots:
        trie.insert(root)
    
    words = sentence.split()
    for i, word in enumerate(words):
        node = trie.root
        for j, char in enumerate(word):
            if char not in node.children:
                break
            node = node.children[char]
            if node.is_end_of_word:
                words[i] = word[:j+1]
                break
    
    return ' '.join(words)
```

```java
public String replaceWithRoot(String sentence, List<String> roots) {
    Trie trie = new Trie();
    for (String root : roots) trie.insert(root);
    String[] words = sentence.split(" ");
    for (int i = 0; i < words.length; i++) {
        TrieNode node = trie.root;
        for (int j = 0; j < words[i].length(); j++) {
            char c = words[i].charAt(j);
            if (node.children[c - 'a'] == null) break;
            node = node.children[c - 'a'];
            if (node.isEndOfWord) { words[i] = words[i].substring(0, j + 1); break; }
        }
    }
    return String.join(" ", words);
}
```

```cpp
string replaceWithRoot(string sentence, vector<string>& roots) {
    Trie trie;
    for (auto& root : roots) trie.insert(root);
    stringstream ss(sentence);
    string word, result;
    while (ss >> word) {
        TrieNode* node = trie.root;
        for (int j = 0; j < word.size(); j++) {
            if (!node->children[word[j] - 'a']) break;
            node = node->children[word[j] - 'a'];
            if (node->isEndOfWord) { word = word.substr(0, j + 1); break; }
        }
        if (!result.empty()) result += " ";
        result += word;
    }
    return result;
}
```

## Edge Cases to Consider
1. Empty string operations
2. Single character words
3. Overlapping prefixes
4. Case sensitivity
5. Special characters
6. Very long words
7. Words that are prefixes of other words
8. Maximum word length
9. Memory constraints
10. Concurrent access

## Common Pitfalls
1. Not handling memory efficiently
2. Incorrect end-of-word marking
3. Case sensitivity issues
4. Not cleaning up deleted words
5. Inefficient children implementation
6. Not considering space complexity
7. Poor handling of special characters

## Practice Problems by Difficulty

### Easy
1. [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) (LC #208)
2. [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) (LC #14)
3. [Map Sum Pairs](https://leetcode.com/problems/map-sum-pairs/) (LC #677)

### Medium
1. [Add and Search Word](https://leetcode.com/problems/add-and-search-word-data-structure-design/) (LC #211)
2. [Replace Words](https://leetcode.com/problems/replace-words/) (LC #648)
3. [Design Search Autocomplete System](https://leetcode.com/problems/design-search-autocomplete-system/) (LC #642)
4. [Word Search II](https://leetcode.com/problems/word-search-ii/) (LC #212)

### Hard
1. [Stream of Characters](https://leetcode.com/problems/stream-of-characters/) (LC #1032)
2. [Word Squares](https://leetcode.com/problems/word-squares/) (LC #425)
3. [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/) (LC #336)

## Real-World Applications
1. **Autocomplete Systems**: Search suggestions
2. **Spell Checkers**: Word validation and correction
3. **IP Routing Tables**: Network routing
4. **Phone Directories**: Contact search
5. **Text Editors**: Word completion
6. **DNA Analysis**: Pattern matching
7. **Natural Language Processing**: Word prediction

## Advanced Topics
1. **Compressed Trie (Radix Tree)**:
   - Path compression
   - Space optimization
2. **Ternary Search Tree**:
   - Space-efficient alternative
3. **Suffix Tree/Array**:
   - Pattern matching
   - String operations
4. **Patricia Trie**:
   - Compact representation
5. **Concurrent Trie**:
   - Thread-safe operations

## Important Resources
1. [Trie Data Structure](https://www.geeksforgeeks.org/trie-insert-and-search/)
2. [Advanced Trie Concepts](https://www.toptal.com/java/the-trie-a-neglected-data-structure)
3. [Compressed Trie Tutorial](https://www.geeksforgeeks.org/pattern-searching-using-compressed-trie/)
4. [Suffix Tree Applications](https://www.geeksforgeeks.org/pattern-searching-using-suffix-tree/)
5. [Trie vs Hash Table](https://www.geeksforgeeks.org/advantages-trie-data-structure/)

## ❓ FAQ Section

**Q: When should I use a Trie vs HashMap for string problems?**
A: Use Trie for prefix-based operations (autocomplete, spell check, prefix search), when you need to find all words with a prefix, or for word games. Use HashMap for exact string lookups, frequency counting, or when memory is limited.

**Q: How much space does a Trie use?**
A: O(ALPHABET_SIZE × total_characters × average_word_length) in worst case. For 26 lowercase letters, this can be significant. Optimization: use HashMap for children instead of array, or use compressed trie (radix tree).

**Q: How do I implement wildcard search in a Trie?**
A: When you encounter a wildcard (like '.'), recursively search ALL children instead of just one specific child. Return true if any child path matches the rest of the pattern.

**Q: Can I use Trie for problems other than strings?**
A: Yes! Tries work for any sequence: binary numbers (bitwise trie for XOR problems), arrays of integers, or any data that can be represented as a sequence of discrete symbols.

**Q: How do I delete a word from a Trie?**
A: Mark the end-of-word flag as false. Optionally, remove nodes that are no longer needed (have no children and are not end of another word). Deletion is O(m) where m is word length.

## Interview Tips
1. Clarify character set and constraints
2. Consider memory-space tradeoffs
3. Discuss optimization possibilities
4. Handle edge cases explicitly
5. Consider case sensitivity
6. Implement helper functions
7. Test with small examples
8. Consider deletion requirements