# Stack Data Structure 🧱

## Introduction
A stack is a fundamental data structure that follows the Last-In-First-Out (LIFO) principle. Think of it like a stack of plates - you can only add or remove plates from the top.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (array-based stack implementation)
- Basic understanding of LIFO concept

### Related Topics
- **Compare with**: [Queue](queue.md) (FIFO counterpart)
- **Essential for**: [Recursion](recursion.md) (call stack), [Graph](graph.md) (DFS), [Tree](tree.md) (iterative traversals)
- **Used in**: [Strings](strings.md) (parentheses matching, expression evaluation), [Backtracking](backtracking.md)
- **See also**: [Monotonic Stack](arrays.md) (next greater element problems)

## Pattern Recognition Guide

### 🎯 When to Use Stack
**Keywords in problem**: "matching", "nested", "reverse", "undo", "previous/next greater/smaller"

**Use Stack when you see**:
- Matching pairs (parentheses, tags)
- Reverse order processing
- Undo operations or history
- Next/previous greater/smaller element
- Expression evaluation or parsing

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Parentheses Matching | "valid parentheses", "balanced brackets" | Valid Parentheses, Longest Valid |
| Monotonic Stack | "next greater", "previous smaller", "span" | Daily Temperatures, Stock Span |
| Expression Evaluation | "calculator", "evaluate expression", "RPN" | Basic Calculator, Evaluate RPN |
| Nested Structures | "decode string", "flatten nested", "nested tags" | Decode String, Flatten Nested List |
| Undo/History | "backspace", "remove adjacent", "simplify path" | Backspace String Compare |
| DFS Iterative | "iterative DFS", "avoid recursion" | Tree/Graph DFS without recursion |

### ❌ When NOT to Use
- FIFO (first-in-first-out) needed → use [Queue](queue.md)
- Need random access → use [Arrays](arrays.md)
- Priority-based retrieval → use [Heap](heap-pq.md)
- BFS traversal → use [Queue](queue.md)

### 💡 Monotonic Stack Pattern
```
For "next greater element":
- Use decreasing stack
- Pop when current > stack top
- Popped elements found their next greater
```

## Core Concepts

### Important Terminologies
- **Stack**: A LIFO (Last-In-First-Out) data structure
- **Push**: Operation to add an element to the top of stack
- **Pop**: Operation to remove the top element from stack
- **Peek/Top**: Operation to view the top element without removing it
- **Stack Overflow**: When trying to push to a full stack
- **Stack Underflow**: When trying to pop from an empty stack
- **MinStack**: Stack with O(1) minimum element access
- **Monotonic Stack**: Stack maintaining increasing/decreasing order

### Time Complexity Analysis
| Operation | Average Case | Worst Case |
|-----------|--------------|------------|
| Push      | O(1)         | O(1)       |
| Pop       | O(1)         | O(1)       |
| Peek      | O(1)         | O(1)       |
| Search    | O(n)         | O(n)       |
| isEmpty   | O(1)         | O(1)       |

### Space Complexity
- **Basic Stack**: O(n) where n is number of elements
- **MinStack**: O(n) for maintaining minimum values
- **Call Stack**: O(h) where h is recursion depth

## Implementation

### Basic Stack Implementation

**Pseudocode:**
```
Stack operations (LIFO):
- push(item): add item to top
- pop(): remove and return top item
- peek(): return top item without removing
- is_empty(): check if stack is empty
- size(): return number of elements
```

```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
        raise IndexError("Pop from empty stack")
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
        raise IndexError("Peek at empty stack")
    
    def is_empty(self):
        return len(self.items) == 0
    
    def size(self):
        return len(self.items)
```

```java
class Stack<T> {
    private List<T> items = new ArrayList<>();
    
    public void push(T item) { items.add(item); }
    
    public T pop() {
        if (isEmpty()) throw new RuntimeException("Pop from empty stack");
        return items.remove(items.size() - 1);
    }
    
    public T peek() {
        if (isEmpty()) throw new RuntimeException("Peek at empty stack");
        return items.get(items.size() - 1);
    }
    
    public boolean isEmpty() { return items.isEmpty(); }
    public int size() { return items.size(); }
}
```

```cpp
template<typename T>
class Stack {
    vector<T> items;
public:
    void push(T item) { items.push_back(item); }
    
    T pop() {
        if (isEmpty()) throw runtime_error("Pop from empty stack");
        T item = items.back();
        items.pop_back();
        return item;
    }
    
    T peek() {
        if (isEmpty()) throw runtime_error("Peek at empty stack");
        return items.back();
    }
    
    bool isEmpty() { return items.empty(); }
    int size() { return items.size(); }
};
```

### MinStack Implementation

**Pseudocode:**
```
Stack with O(1) getMin:
- Use two stacks: main stack and min stack
- push(val): push to main; if val <= min_stack top, push to min_stack
- pop(): if main top == min_stack top, pop both; else pop main only
- getMin(): return min_stack top
```

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []
    
    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
    
    def pop(self):
        if self.stack:
            if self.stack[-1] == self.min_stack[-1]:
                self.min_stack.pop()
            return self.stack.pop()
    
    def get_min(self):
        return self.min_stack[-1] if self.min_stack else None
```

```java
class MinStack {
    Stack<Integer> stack = new Stack<>();
    Stack<Integer> minStack = new Stack<>();
    
    public void push(int val) {
        stack.push(val);
        if (minStack.isEmpty() || val <= minStack.peek()) minStack.push(val);
    }
    public void pop() {
        if (stack.peek().equals(minStack.peek())) minStack.pop();
        stack.pop();
    }
    public int top() { return stack.peek(); }
    public int getMin() { return minStack.peek(); }
}
```

```cpp
class MinStack {
    stack<int> stk, minStk;
public:
    void push(int val) {
        stk.push(val);
        if (minStk.empty() || val <= minStk.top()) minStk.push(val);
    }
    void pop() {
        if (stk.top() == minStk.top()) minStk.pop();
        stk.pop();
    }
    int top() { return stk.top(); }
    int getMin() { return minStk.top(); }
};
```

## Common Applications

### 1. Expression Evaluation

**Pseudocode:**
```
Evaluate Reverse Polish Notation (RPN):
1. Initialize empty stack
2. For each token:
   a. If token is operator (+, -, *, /):
      - Pop two operands (b first, then a)
      - Apply operator: result = a operator b
      - Push result to stack
   b. Else: push number to stack
3. Return top of stack (final result)
```

```python
def evaluate_rpn(tokens):
    stack = []
    operators = {'+': lambda x,y: x+y, '-': lambda x,y: x-y,
                '*': lambda x,y: x*y, '/': lambda x,y: int(x/y)}
    
    for token in tokens:
        if token in operators:
            b, a = stack.pop(), stack.pop()
            stack.append(operators[token](a, b))
        else:
            stack.append(int(token))
    
    return stack[0]
```

```java
public int evalRPN(String[] tokens) {
    Stack<Integer> stack = new Stack<>();
    for (String token : tokens) {
        if ("+-*/".contains(token)) {
            int b = stack.pop(), a = stack.pop();
            switch (token) {
                case "+": stack.push(a + b); break;
                case "-": stack.push(a - b); break;
                case "*": stack.push(a * b); break;
                case "/": stack.push(a / b); break;
            }
        } else stack.push(Integer.parseInt(token));
    }
    return stack.pop();
}
```

```cpp
int evalRPN(vector<string>& tokens) {
    stack<int> stk;
    for (string& token : tokens) {
        if (token == "+" || token == "-" || token == "*" || token == "/") {
            int b = stk.top(); stk.pop();
            int a = stk.top(); stk.pop();
            if (token == "+") stk.push(a + b);
            else if (token == "-") stk.push(a - b);
            else if (token == "*") stk.push(a * b);
            else stk.push(a / b);
        } else stk.push(stoi(token));
    }
    return stk.top();
}
```

### 2. Parentheses Matching

**Pseudocode:**
```
Valid parentheses check:
1. Create stack and mapping: ')' -> '(', ']' -> '[', '}' -> '{'
2. For each character:
   a. If opening bracket: push to stack
   b. If closing bracket:
      - If stack empty OR top doesn't match: return false
      - Else: pop stack
3. Return true if stack is empty
```

```python
def is_valid_parentheses(s):
    stack = []
    pairs = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in '({[':
            stack.append(char)
        elif char in ')}]':
            if not stack or stack.pop() != pairs[char]:
                return False
    
    return len(stack) == 0
```

```java
public boolean isValid(String s) {
    Stack<Character> stack = new Stack<>();
    Map<Character, Character> pairs = Map.of(')', '(', '}', '{', ']', '[');
    for (char c : s.toCharArray()) {
        if (c == '(' || c == '{' || c == '[') stack.push(c);
        else if (stack.isEmpty() || stack.pop() != pairs.get(c)) return false;
    }
    return stack.isEmpty();
}
```

```cpp
bool isValid(string s) {
    stack<char> stk;
    unordered_map<char, char> pairs = {{')', '('}, {'}', '{'}, {']', '['}};
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') stk.push(c);
        else {
            if (stk.empty() || stk.top() != pairs[c]) return false;
            stk.pop();
        }
    }
    return stk.empty();
}
```

## Common Patterns

### 1. Monotonic Stack Pattern
Used for finding next greater/smaller element problems.

**Pseudocode:**
```
Next greater element using monotonic decreasing stack:
1. Initialize result array with -1, empty stack (stores indices)
2. For each index i from 0 to n-1:
   a. While stack not empty AND nums[i] > nums[stack.top()]:
      - Pop index from stack
      - Set result[popped_index] = nums[i] (found next greater)
   b. Push current index i to stack
3. Return result (remaining stack indices have no greater element)
```

```python
def next_greater_element(nums):
    result = [-1] * len(nums)
    stack = []  # stores indices
    
    for i in range(len(nums)):
        while stack and nums[i] > nums[stack[-1]]:
            result[stack.pop()] = nums[i]
        stack.append(i)
    
    return result
```

```java
public int[] nextGreaterElement(int[] nums) {
    int[] result = new int[nums.length];
    Arrays.fill(result, -1);
    Stack<Integer> stack = new Stack<>();
    for (int i = 0; i < nums.length; i++) {
        while (!stack.isEmpty() && nums[i] > nums[stack.peek()])
            result[stack.pop()] = nums[i];
        stack.push(i);
    }
    return result;
}
```

```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    vector<int> result(nums.size(), -1);
    stack<int> stk;
    for (int i = 0; i < nums.size(); i++) {
        while (!stk.empty() && nums[i] > nums[stk.top()]) {
            result[stk.top()] = nums[i];
            stk.pop();
        }
        stk.push(i);
    }
    return result;
}
```

### 2. Calculator Pattern
Used for basic calculator problems.

**Pseudocode:**
```
Basic calculator (no parentheses):
1. Initialize result = 0, num = 0, sign = 1
2. For each character:
   a. If digit: num = num × 10 + digit
   b. If + or -:
      - result += sign × num
      - num = 0
      - sign = 1 if '+', else -1
3. Add final number: result += sign × num
```

```python
def calculate(s):
    stack = []
    num = 0
    sign = 1
    result = 0
    
    for char in s + '+':
        if char.isdigit():
            num = num * 10 + int(char)
        elif char in '+-':
            result += sign * num
            num = 0
            sign = 1 if char == '+' else -1
    
    return result
```

```java
public int calculate(String s) {
    int result = 0, num = 0, sign = 1;
    for (char c : (s + "+").toCharArray()) {
        if (Character.isDigit(c)) num = num * 10 + (c - '0');
        else if (c == '+' || c == '-') {
            result += sign * num;
            num = 0;
            sign = (c == '+') ? 1 : -1;
        }
    }
    return result;
}
```

```cpp
int calculate(string s) {
    int result = 0, num = 0, sign = 1;
    for (char c : s + "+") {
        if (isdigit(c)) num = num * 10 + (c - '0');
        else if (c == '+' || c == '-') {
            result += sign * num;
            num = 0;
            sign = (c == '+') ? 1 : -1;
        }
    }
    return result;
}
```

## Edge Cases to Consider
1. Empty stack operations
2. Stack with single element
3. Stack reaching maximum capacity
4. Invalid input formats
5. Nested expressions
6. Multiple valid solutions
7. Negative numbers in calculations

## Common Pitfalls
1. Forgetting to handle empty stack
2. Not considering operator precedence
3. Incorrect handling of negative numbers
4. Stack overflow in recursion
5. Not clearing stack between test cases

## Practice Problems by Difficulty

### Easy
1. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) (LC #20)
2. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/) (LC #225)
3. [Baseball Game](https://leetcode.com/problems/baseball-game/) (LC #682)

### Medium
1. [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) (LC #150)
2. [Min Stack](https://leetcode.com/problems/min-stack/) (LC #155)
3. [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) (LC #739)
4. [Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/) (LC #503)

### Hard
1. [Basic Calculator](https://leetcode.com/problems/basic-calculator/) (LC #224)
2. [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) (LC #84)
3. [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/) (LC #895)

## Real-World Applications
1. **Browser History**: Back/Forward navigation
2. **Undo/Redo**: Text editors and graphic software
3. **Expression Evaluation**: Calculators and compilers
4. **Memory Management**: Call stack in programming languages
5. **Syntax Parsing**: Compiler design and validation

## Advanced Topics
1. **Thread-Safe Stacks**: Concurrent implementations
2. **Memory Efficient Stacks**: Using linked lists
3. **Custom Stack Implementations**:
   - Bounded Stack
   - Circular Stack
   - Double Stack

## Important Resources
1. [Stack Data Structure](https://www.geeksforgeeks.org/stack-data-structure/)
2. [Monotonic Stack Pattern](https://leetcode.com/problems/daily-temperatures/solution/)
3. [Stack Implementation Guide](https://www.programiz.com/dsa/stack)
4. [Stack Applications](https://www.geeksforgeeks.org/applications-of-stack-data-structure/)
5. [Stack in Standard Libraries](https://docs.python.org/3/library/collections.html#collections.deque)

## ❓ FAQ Section

**Q: When should I use a stack vs other data structures?**
A: Use a stack when you need LIFO (Last-In-First-Out) behavior: matching parentheses, undo operations, function call tracking, DFS traversal, or when you need to process elements in reverse order of their arrival.

**Q: What is a monotonic stack and when do I use it?**
A: A monotonic stack maintains elements in sorted order (increasing or decreasing). Use it for "next greater/smaller element" problems, histogram problems, or when you need to efficiently find the nearest larger/smaller element for each position.

**Q: How do I implement a stack that supports getMin() in O(1)?**
A: Use two stacks: one for values and one for minimums. When pushing, also push to min-stack if value ≤ current minimum. When popping, also pop from min-stack if the popped value equals the current minimum.

**Q: Can I use an array as a stack?**
A: Yes! Arrays with push (append) and pop (remove last) operations work as stacks. In Python, use `list.append()` and `list.pop()`. This is often more efficient than implementing a node-based stack.

**Q: How do I convert recursive solution to iterative using a stack?**
A: Replace the function call stack with an explicit stack. Push initial state, then loop while stack is not empty: pop state, process it, and push new states for "recursive calls". This is essentially manual DFS.

## Interview Tips
1. Always validate input constraints
2. Consider time/space complexity requirements
3. Handle edge cases explicitly
4. Use built-in implementations when allowed
5. Consider stack variations (min stack, monotonic stack)
6. Test your solution with boundary conditions