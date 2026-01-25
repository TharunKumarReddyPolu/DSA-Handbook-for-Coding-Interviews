# Linked List Data Structure 🔗

## Introduction
A linked list is a linear data structure where elements are stored in nodes, and each node points to the next node in the sequence. Unlike arrays, linked lists don't require contiguous memory allocation.

## Prerequisites & Related Topics

### Prerequisites
- Basic programming (pointers/references concept)
- Understanding of memory allocation
- [Arrays](arrays.md) (for comparison and understanding trade-offs)

### Related Topics
- **Compare with**: [Arrays](arrays.md) (trade-offs in access vs insertion)
- **Used in**: [Hashing](hashing.md) (chaining for collision resolution), [Stack](stack.md) and [Queue](queue.md) (alternative implementations)
- **Techniques**: Two Pointers (fast/slow), [Recursion](recursion.md) (recursive operations)
- **See also**: [Tree](tree.md) (linked structures), [Graph](graph.md) (adjacency lists)

## Pattern Recognition Guide

### 🎯 When to Use Linked List
**Keywords in problem**: "linked list", "node", "next pointer", "reverse", "merge lists", "cycle"

**Use Linked List when you see**:
- Frequent insertions/deletions at arbitrary positions
- Unknown size with frequent modifications
- Need to reverse or reorder nodes
- Cycle detection problems
- Merging sorted sequences

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| Fast/Slow Pointers | "cycle", "middle element", "palindrome" | Linked List Cycle, Middle of List |
| Reverse | "reverse list", "reverse between", "k-group" | Reverse Linked List, Reverse K-Group |
| Merge | "merge sorted lists", "combine lists" | Merge Two Sorted Lists, Merge K Lists |
| Two Pointers | "nth from end", "remove nth", "intersection" | Remove Nth From End, Intersection Point |
| Dummy Node | "insert/delete at head", "simplify edge cases" | Add Two Numbers, Remove Duplicates |
| In-place Modify | "reorder", "partition", "rearrange" | Reorder List, Partition List |

### ❌ When NOT to Use
- Need random access by index → use [Arrays](arrays.md)
- Frequent lookups by value → use [Hashing](hashing.md)
- Need sorted order with fast search → use BST or sorted [Arrays](arrays.md)
- Memory overhead is a concern (pointers add overhead)

## Core Concepts

### Important Terminologies
- **Node**: Basic unit containing data and pointer(s)
- **Head**: First node of the linked list
- **Tail**: Last node of the linked list
- **Singly Linked List**: Each node has one pointer to next node
- **Doubly Linked List**: Each node has pointers to next and previous nodes
- **Circular Linked List**: Last node points back to first node
- **Dummy Node**: Extra node used to handle edge cases

### Time Complexity Analysis
| Operation           | Singly Linked | Doubly Linked |
|--------------------|---------------|---------------|
| Access             | O(n)          | O(n)          |
| Search             | O(n)          | O(n)          |
| Insert at Head     | O(1)          | O(1)          |
| Insert at Tail     | O(1)*         | O(1)          |
| Delete at Head     | O(1)          | O(1)          |
| Delete at Tail     | O(n)          | O(1)          |
| Insert at Position | O(n)          | O(n)          |
| Delete at Position | O(n)          | O(n)          |

*O(1) if we maintain tail pointer

### Space Complexity
- **Singly Linked**: O(n) with 1 pointer per node
- **Doubly Linked**: O(n) with 2 pointers per node
- **Memory Overhead**: Higher than arrays due to pointers

## Implementation

### Singly Linked List

**Pseudocode:**
```
Node: value, next pointer

Insert at head:
1. Create new node with value
2. new_node.next = head
3. head = new_node

Delete at head:
1. Store head value
2. head = head.next
3. Return stored value
```

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class SinglyLinkedList:
    def __init__(self):
        self.head = None
        self.size = 0
    
    def insert_at_head(self, val):
        new_node = ListNode(val)
        new_node.next = self.head
        self.head = new_node
        self.size += 1
    
    def delete_at_head(self):
        if not self.head:
            return None
        val = self.head.val
        self.head = self.head.next
        self.size -= 1
        return val
    
    def get_size(self):
        return self.size
```

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
}

class SinglyLinkedList {
    ListNode head;
    int size = 0;
    
    void insertAtHead(int val) {
        ListNode newNode = new ListNode(val);
        newNode.next = head;
        head = newNode;
        size++;
    }
    
    Integer deleteAtHead() {
        if (head == null) return null;
        int val = head.val;
        head = head.next;
        size--;
        return val;
    }
    
    int getSize() { return size; }
}
```

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class SinglyLinkedList {
    ListNode* head = nullptr;
    int size = 0;
public:
    void insertAtHead(int val) {
        ListNode* newNode = new ListNode(val);
        newNode->next = head;
        head = newNode;
        size++;
    }
    
    int deleteAtHead() {
        if (!head) return -1;
        int val = head->val;
        ListNode* temp = head;
        head = head->next;
        delete temp;
        size--;
        return val;
    }
    
    int getSize() { return size; }
};
```

### Doubly Linked List

**Pseudocode:**
```
Node: value, next pointer, prev pointer

Insert at head:
1. Create new node
2. new_node.next = head
3. If head exists: head.prev = new_node
4. head = new_node, update tail if needed

Insert at tail:
1. Create new node
2. new_node.prev = tail
3. If tail exists: tail.next = new_node
4. tail = new_node, update head if needed
```

```python
class DoublyListNode:
    def __init__(self, val=0, next=None, prev=None):
        self.val = val
        self.next = next
        self.prev = prev

class DoublyLinkedList:
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0
    
    def insert_at_head(self, val):
        new_node = DoublyListNode(val)
        if not self.head:
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node
        self.size += 1
    
    def insert_at_tail(self, val):
        new_node = DoublyListNode(val)
        if not self.tail:
            self.head = self.tail = new_node
        else:
            new_node.prev = self.tail
            self.tail.next = new_node
            self.tail = new_node
        self.size += 1
```

```java
class DoublyListNode {
    int val;
    DoublyListNode next, prev;
    DoublyListNode(int val) { this.val = val; }
}

class DoublyLinkedList {
    DoublyListNode head, tail;
    int size = 0;
    
    void insertAtHead(int val) {
        DoublyListNode newNode = new DoublyListNode(val);
        if (head == null) { head = tail = newNode; }
        else { newNode.next = head; head.prev = newNode; head = newNode; }
        size++;
    }
    
    void insertAtTail(int val) {
        DoublyListNode newNode = new DoublyListNode(val);
        if (tail == null) { head = tail = newNode; }
        else { newNode.prev = tail; tail.next = newNode; tail = newNode; }
        size++;
    }
}
```

```cpp
struct DoublyListNode {
    int val;
    DoublyListNode *next, *prev;
    DoublyListNode(int x) : val(x), next(nullptr), prev(nullptr) {}
};

class DoublyLinkedList {
    DoublyListNode *head = nullptr, *tail = nullptr;
    int size = 0;
public:
    void insertAtHead(int val) {
        DoublyListNode* newNode = new DoublyListNode(val);
        if (!head) { head = tail = newNode; }
        else { newNode->next = head; head->prev = newNode; head = newNode; }
        size++;
    }
    
    void insertAtTail(int val) {
        DoublyListNode* newNode = new DoublyListNode(val);
        if (!tail) { head = tail = newNode; }
        else { newNode->prev = tail; tail->next = newNode; tail = newNode; }
        size++;
    }
};
```

## Common Techniques

### 1. Floyd's Cycle Detection (Tortoise and Hare)

**Pseudocode:**
```
1. Initialize slow = head, fast = head
2. While fast and fast.next exist:
   a. slow = slow.next (move 1 step)
   b. fast = fast.next.next (move 2 steps)
   c. If slow == fast: cycle detected, return true
3. Return false (no cycle - fast reached end)
```

```python
def has_cycle(head):
    if not head or not head.next:
        return False
    
    slow = head
    fast = head.next
    
    while slow != fast:
        if not fast or not fast.next:
            return False
        slow = slow.next
        fast = fast.next.next
    
    return True
```

```java
public boolean hasCycle(ListNode head) {
    if (head == null) return false;
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

```cpp
bool hasCycle(ListNode* head) {
    if (!head) return false;
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

### 2. Reverse Linked List

**Pseudocode:**
```
1. Initialize prev = null, curr = head
2. While curr is not null:
   a. Store next = curr.next
   b. Reverse pointer: curr.next = prev
   c. Move prev = curr
   d. Move curr = next
3. Return prev (new head)
```

```python
def reverse_list(head):
    prev = None
    curr = head
    
    while curr:
        next_temp = curr.next
        curr.next = prev
        prev = curr
        curr = next_temp
    
    return prev
```

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### 3. Merge Two Sorted Lists

**Pseudocode:**
```
1. Create dummy node, set curr = dummy
2. While both lists have nodes:
   a. If l1.val <= l2.val: curr.next = l1, l1 = l1.next
   b. Else: curr.next = l2, l2 = l2.next
   c. curr = curr.next
3. Attach remaining list: curr.next = l1 or l2
4. Return dummy.next
```

```python
def merge_two_lists(l1, l2):
    dummy = ListNode(0)
    curr = dummy
    
    while l1 and l2:
        if l1.val <= l2.val:
            curr.next = l1
            l1 = l1.next
        else:
            curr.next = l2
            l2 = l2.next
        curr = curr.next
    
    curr.next = l1 if l1 else l2
    return dummy.next
```

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0), curr = dummy;
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
        else { curr.next = l2; l2 = l2.next; }
        curr = curr.next;
    }
    curr.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0), *curr = &dummy;
    while (l1 && l2) {
        if (l1->val <= l2->val) { curr->next = l1; l1 = l1->next; }
        else { curr->next = l2; l2 = l2->next; }
        curr = curr->next;
    }
    curr->next = l1 ? l1 : l2;
    return dummy.next;
}
```

## Common Patterns

### 1. Two Pointer Technique
- Fast & Slow pointers (Floyd's algorithm)
- Multiple passes with offset
- Parallel traversal

### 2. Dummy Node Pattern

**Pseudocode:**
```
Remove nth from end:
1. Create dummy node, dummy.next = head
2. Set first = dummy, second = dummy
3. Advance first by n+1 steps
4. Move both until first reaches end
5. second.next = second.next.next (skip nth node)
6. Return dummy.next
```

```python
def remove_nth_from_end(head, n):
    dummy = ListNode(0)
    dummy.next = head
    first = dummy
    second = dummy
    
    # Advance first pointer by n+1 steps
    for i in range(n + 1):
        first = first.next
    
    # Move both pointers until first reaches end
    while first:
        first = first.next
        second = second.next
    
    # Remove nth node
    second.next = second.next.next
    return dummy.next
```

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy, second = dummy;
    for (int i = 0; i <= n; i++) first = first.next;
    while (first != null) { first = first.next; second = second.next; }
    second.next = second.next.next;
    return dummy.next;
}
```

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode *first = &dummy, *second = &dummy;
    for (int i = 0; i <= n; i++) first = first->next;
    while (first) { first = first->next; second = second->next; }
    second->next = second->next->next;
    return dummy.next;
}
```

## Edge Cases to Consider
1. Empty list
2. Single node list
3. Two node list
4. Circular list
5. List with cycles
6. Operations at head/tail
7. Duplicate values
8. Multiple cycles

## Common Pitfalls
1. Not handling null pointers
2. Memory leaks in deletion
3. Losing the head reference
4. Incorrect cycle detection
5. Off-by-one errors in counting
6. Not updating tail pointer

## Practice Problems by Difficulty

### Easy
1. [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) (LC #206)
2. [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) (LC #141)
3. [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) (LC #234)
4. [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) (LC #876)

### Medium
1. [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/) (LC #2)
2. [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) (LC #19)
3. [Rotate List](https://leetcode.com/problems/rotate-list/) (LC #61)
4. [Sort List](https://leetcode.com/problems/sort-list/) (LC #148)

### Hard
1. [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) (LC #23)
2. [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/) (LC #25)
3. [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/) (LC #138)

## Real-World Applications
1. **Memory Management**: Implementation of free lists
2. **Undo Systems**: History tracking in editors
3. **Hash Tables**: Collision resolution using chaining
4. **File Systems**: Directory implementations
5. **Music Player**: Playlist implementation

## Advanced Topics
1. **Skip Lists**: Probabilistic alternative to balanced trees
2. **XOR Linked Lists**: Memory-efficient doubly linked lists
3. **Self-Organizing Lists**: 
   - Move to Front
   - Transpose
   - Count
4. **Concurrent Linked Lists**:
   - Lock-free implementations
   - Wait-free implementations

## Important Resources
1. [Floyd's Cycle Detection Algorithm](https://www.geeksforgeeks.org/floyds-cycle-finding-algorithm/)
2. [Linked List Data Structure](https://www.geeksforgeeks.org/data-structures/linked-list/)
3. [Skip Lists Tutorial](https://www.geeksforgeeks.org/skip-list/)
4. [XOR Linked List](https://www.geeksforgeeks.org/xor-linked-list-a-memory-efficient-doubly-linked-list-set-1/)
5. [Self-Organizing Lists](https://www.geeksforgeeks.org/self-organizing-list-set-1-introduction/)

## ❓ FAQ Section

**Q: When should I use linked list vs array?**
A: Use linked list when you need frequent insertions/deletions at arbitrary positions (O(1) with pointer vs O(n) for array), or when size is unknown and varies significantly. Use arrays when you need random access or memory efficiency.

**Q: What is a dummy/sentinel node and when should I use it?**
A: A dummy node is a placeholder node at the beginning of the list. Use it when operations might modify the head (deletion, insertion at head) to avoid special cases. Return `dummy.next` as the new head.

**Q: How do I detect a cycle in a linked list?**
A: Use Floyd's Tortoise and Hare algorithm: two pointers, slow moves 1 step, fast moves 2 steps. If they meet, there's a cycle. To find cycle start, reset one pointer to head and move both at same speed until they meet.

**Q: How do I find the middle of a linked list?**
A: Use slow and fast pointers. Slow moves 1 step, fast moves 2 steps. When fast reaches end, slow is at the middle. For even-length lists, this gives you the second middle node.

**Q: Should I use singly or doubly linked list?**
A: Use singly linked list for simpler memory usage and when you only traverse forward. Use doubly linked list when you need to traverse both directions or delete a node given only that node's reference (O(1) deletion).

## Interview Tips
1. Always validate input parameters
2. Consider memory management
3. Handle edge cases explicitly
4. Use dummy nodes when helpful
5. Draw pointer manipulations
6. Test with small examples first
7. Consider maintaining tail pointer