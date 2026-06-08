# LINKED LIST — COMPLETE BEGINNER NOTES

---

# 1. WHAT IS A LINKED LIST?

A **Linked List** is a linear data structure where elements are connected using pointers.

Unlike arrays:

- elements are **not stored continuously in memory**
- each element points to the next element

Each element is called a **Node**.

A node contains:

1. **Data**
2. **Pointer/Reference** to next node

Example:

```text
[10 | * ] -> [20 | * ] -> [30 | NULL]
```

- `10`, `20`, `30` are data
- arrows represent pointers
- last node points to `NULL`

---

# 2. WHY DO WE NEED LINKED LISTS?

Arrays have limitations:

- fixed size
- insertion/deletion costly
- continuous memory needed

Linked Lists solve these issues.

---

# 3. ARRAY VS LINKED LIST

| Feature | Array | Linked List |
|---|---|
| Memory | Continuous | Non-continuous |
| Size | Fixed | Dynamic |
| Access | O(1) random access | O(n) traversal |
| Insert/Delete at beginning | Costly | Easy |
| Cache Friendly | Yes | No |
| Extra Memory | No | Pointer needed |

---

# 4. REAL LIFE USES OF LINKED LIST

Linked Lists are used in:

- Browser history
- Music playlists
- Undo/Redo systems
- LRU Cache
- HashMaps
- Graphs
- Memory management
- Implementing stacks/queues

---

# 5. BASIC STRUCTURE OF A NODE

## C++ Code

```cpp
class Node {
public:
    int data;
    Node* next;

    Node(int val) {
        data = val;
        next = NULL;
    }
};
```

---

# 6. UNDERSTANDING THIS CODE

## `int data`

Stores the value.

---

## `Node* next`

Stores address of next node.

---

## Constructor

```cpp
Node(int val)
```

Called automatically when object is created.

---

# 7. HOW TO CREATE A NODE

```cpp
Node* node1 = new Node(10);
```

Memory created dynamically.

Now:

```text
data = 10
next = NULL
```

---

# 8. HOW TO CONNECT NODES

```cpp
Node* node1 = new Node(10);
Node* node2 = new Node(20);

node1->next = node2;
```

Now:

```text
10 -> 20 -> NULL
```

---

# 9. WHAT IS HEAD?

`head` stores address of first node.

```cpp
Node* head = node1;
```

Visualization:

```text
head -> 10 -> 20 -> NULL
```

---

# 10. HOW TO PRINT A LINKED LIST

```cpp
void printList(Node* head) {

    Node* temp = head;

    while(temp != NULL) {
        cout << temp->data << " ";
        temp = temp->next;
    }
}
```

---

# 11. DRY RUN OF PRINT

Suppose:

```text
10 -> 20 -> 30 -> NULL
```

Initially:

```cpp
temp = head
```

### Iteration 1

```text
temp = 10
print 10
move to 20
```

### Iteration 2

```text
temp = 20
print 20
move to 30
```

### Iteration 3

```text
temp = 30
print 30
move to NULL
```

Loop stops.

Output:

```text
10 20 30
```

---

# 12. COMPLETE BASIC LINKED LIST PROGRAM

```cpp
#include<bits/stdc++.h>
using namespace std;

class Node {

public:
    int data;
    Node* next;

    Node(int val) {
        data = val;
        next = NULL;
    }
};

void printList(Node* head) {

    Node* temp = head;

    while(temp != NULL) {
        cout << temp->data << " ";
        temp = temp->next;
    }
}

int main() {

    Node* node1 = new Node(10);
    Node* node2 = new Node(20);
    Node* node3 = new Node(30);

    node1->next = node2;
    node2->next = node3;

    Node* head = node1;

    printList(head);
}
```

---

# 13. IMPORTANT TERMINOLOGIES

| Term | Meaning |
|---|---|
| Node | Single element |
| Head | First node |
| Tail | Last node |
| Pointer | Stores address |
| NULL | End of list |

---

# 14. TYPES OF LINKED LISTS

## (A) Singly Linked List

```text
10 -> 20 -> 30
```

Only next pointer.

---

## (B) Doubly Linked List

```text
NULL <- 10 <-> 20 <-> 30 -> NULL
```

Has:

- next pointer
- previous pointer

---

## (C) Circular Linked List

Last node points back to head.

```text
10 -> 20 -> 30
^            |
|____________|
```

---

# 15. MOST IMPORTANT OPERATIONS

You must master:

1. Traversal
2. Insertion
3. Deletion
4. Reversal
5. Detect cycle
6. Find middle
7. Merge lists

These form almost entire interview preparation.

---

# 16. TIME COMPLEXITIES

| Operation | Complexity |
|---|---|
| Access | O(n) |
| Insert at beginning | O(1) |
| Insert at end | O(n) |
| Delete at beginning | O(1) |
| Search | O(n) |

---

# 17. MOST IMPORTANT INTERVIEW CONCEPT

## ARRAYS

Use indexing:

```cpp
arr[i]
```

---

## LINKED LIST

No indexing.

You must move node by node.

This is THE biggest mindset shift.

---

# 18. MOST COMMON INTERVIEW PATTERN

Almost every Linked List question uses:

```cpp
Node* temp = head;
```

AND

```cpp
while(temp != NULL)
```

You traverse node-by-node.

---

# 19. VERY IMPORTANT POINTER SYNTAX

## Access data

```cpp
temp->data
```

---

## Access next

```cpp
temp->next
```

---

# 20. WHY `->` ?

Because `temp` is a pointer.

Equivalent:

```cpp
(*temp).data
```

But:

```cpp
temp->data
```

is shorter.

---

# 21. MOST COMMON BEGINNER MISTAKES

## Mistake 1

```cpp
temp.next
```

Wrong because temp is pointer.

Correct:

```cpp
temp->next
```

---

## Mistake 2

Forgetting:

```cpp
temp = temp->next;
```

Leads to infinite loop.

---

## Mistake 3

Losing head pointer accidentally.

---

# 22. INTERVIEW FLOW TO EXPLAIN LINKED LIST

If interviewer asks:

## “What is a Linked List?”

You can say:

> A linked list is a dynamic linear data structure where nodes are connected using pointers instead of continuous memory locations. Each node contains data and a pointer to the next node. It allows efficient insertions and deletions compared to arrays.

---

# 23. WHEN TO USE LINKED LIST

Use Linked List when:

- many insertions/deletions
- dynamic size needed
- memory allocation flexible

Use Array when:

- random access needed
- cache performance important

---

# 24. CORE THING YOU MUST REMEMBER

Linked List problems are mostly about:

```text
Pointer manipulation
```

That is the entire topic.

If you master pointers:

Linked Lists become easy.

---

# 25. NEXT TOPICS YOU SHOULD LEARN

After this learn in order:

1. Insert at beginning
2. Insert at end
3. Delete node
4. Reverse linked list
5. Middle of linked list
6. Cycle detection
7. Merge two sorted lists
8. Remove nth node
9. Palindrome linked list
10. LRU cache

These are the most important interview problems.

---

# 26. ULTRA SHORT REVISION

## Linked List

- dynamic structure
- nodes connected by pointers
- no continuous memory
- efficient insertion/deletion

---

## Node Contains

```text
data + next pointer
```

---

## Main Pointer

```cpp
Node* head
```

---

## Traversal

```cpp
while(temp != NULL)
```

---

## Biggest Difference From Arrays

No indexing.

Move node-by-node.

---

# 27. MEMORY VISUALIZATION

```text
head
  |
  v

+------+------+
|  10  |  *---|---->
+------+------+

                +------+------+
                |  20  |  *---|---->
                +------+------+

                                +------+------+
                                |  30  | NULL |
                                +------+------+
```

---

# 28. ONE FINAL THING

Linked Lists feel hard initially because:

- pointers
- addresses
- memory visualization

But after 8–10 problems:

patterns repeat heavily.

Most interview problems are variations of:

```text
move pointers carefully
```

That’s the core of Linked Lists.
