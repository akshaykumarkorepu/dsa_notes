

# PROBLEM:

We are given the head of a singly linked list.

We need to reverse the linked list and return the new head.

Example:

```text
1 -> 2 -> 3 -> 4 -> NULL
```

After reversing:

```text
4 -> 3 -> 2 -> 1 -> NULL
```

---

# PATTERN:

### 1. Stack Pattern (Brute Force)

Use LIFO property of stack to reverse node values.

---

### 2. Pointer Manipulation Pattern (Optimal Iterative)

Reverse links directly using pointers.

---

### 3. Recursion + Backtracking Pattern

Go till the last node recursively and reverse links while returning back.

---

# WHY THIS PATTERN:

Linked lists are pointer-based data structures.

To reverse a linked list:

- either reverse values
- or reverse connections

The optimal solution directly manipulates pointers.

This avoids extra memory usage.

---

# CORE IDEA:

For reversal:

```text
current -> next
```

must become:

```text
next -> current
```

While changing links, we must NOT lose the remaining list.

That is why we temporarily store the next node.

---

# BRUTE FORCE:

## Stack Based Approach

### Intuition

Stack follows:

```text
LIFO → Last In First Out
```

If we push:

```text
1 2 3 4
```

Then pop order becomes:

```text
4 3 2 1
```

which is exactly the reversed order.

---

## Stack Based Code

```cpp
/*
class Node {
 public:
    int data ;
    Node *next ;

    Node(int x) {
        data = x ;
        next = nullptr ;
    }
};
*/

class Solution {
  public:
    Node* reverseList(Node* head) {
        
        stack<int> st;
        
        Node* temp = head;
        
        // Push all values into stack
        while(temp != NULL){
            st.push(temp->data);
            temp = temp->next;
        }
        
        temp = head;
        
        // Replace values in reverse order
        while(temp != NULL){
            
            temp->data = st.top();
            st.pop();
            
            temp = temp->next;
        }
        
        return head;
    }
};
```

---

# OPTIMAL APPROACH:

## Iterative Pointer Reversal

This is the most expected interview solution.

Instead of reversing values:

```text
We reverse actual links.
```

We maintain 3 pointers:

```text
prev  -> previous node
temp  -> current node
front -> next node
```

---

# ALGORITHM:

## Iterative Approach

### Step 1

Initialize:

```cpp
prev = NULL
temp = head
```

---

### Step 2

Store next node:

```cpp
front = temp->next
```

This prevents losing remaining list.

---

### Step 3

Reverse current link:

```cpp
temp->next = prev
```

---

### Step 4

Move pointers forward:

```cpp
prev = temp
temp = front
```

---

### Step 5

Repeat until temp becomes NULL.

---

### Step 6

Return prev.

prev becomes the new head.

---

# DRY RUN:

# CASE 1 — STACK BASED

Initial list:

```text
1 -> 2 -> 3 -> 4
```

Push into stack:

```text
TOP
4
3
2
1
```

Pop and overwrite values:

```text
4 -> 3 -> 2 -> 1
```

---

# CASE 2 — ITERATIVE POINTER REVERSAL

Initial:

```text
1 -> 2 -> 3 -> 4 -> NULL
```

Initially:

```text
prev = NULL
temp = 1
```

---

## Iteration 1

Store next:

```text
front = 2
```

Reverse:

```text
1 -> NULL
```

Move:

```text
prev = 1
temp = 2
```

---

## Iteration 2

Store next:

```text
front = 3
```

Reverse:

```text
2 -> 1
```

Move:

```text
prev = 2
temp = 3
```

List becomes:

```text
2 -> 1 -> NULL
```

---

## Iteration 3

Reverse:

```text
3 -> 2
```

List:

```text
3 -> 2 -> 1
```

---

## Iteration 4

Reverse:

```text
4 -> 3
```

Final:

```text
4 -> 3 -> 2 -> 1 -> NULL
```

---

# CASE 3 — RECURSIVE APPROACH

Recursive calls:

```text
reverse(1)
reverse(2)
reverse(3)
reverse(4)
```

At node 4:

```text
return 4
```

Backtracking starts.

---

## Backtrack at Node 3

```cpp
4->next = 3
3->next = NULL
```

List:

```text
4 -> 3
```

---

## Backtrack at Node 2

```cpp
3->next = 2
2->next = NULL
```

List:

```text
4 -> 3 -> 2
```

---

## Backtrack at Node 1

```cpp
2->next = 1
1->next = NULL
```

Final:

```text
4 -> 3 -> 2 -> 1
```

---

# IMPORTANT CODE SNIPPETS:

## Iterative Reversal

```cpp
Node* front = temp->next;

temp->next = prev;

prev = temp;
temp = front;
```

---

## Recursive Reversal

```cpp
head->next->next = head;

head->next = NULL;
```

---

# COMMON MISTAKES:

## 1. Forgetting to store next node

Wrong:

```cpp
temp->next = prev;
temp = temp->next;
```

Remaining list gets lost.

---

## 2. Forgetting:

```cpp
head->next = NULL;
```

in recursion.

This creates infinite cycles.

---

## 3. Returning wrong node

Correct return:

```cpp
return prev;
```

for iterative.

```cpp
return newHead;
```

for recursion.

---

## 4. Confusing pointer movement order

Always:

```text
Store next first
Then reverse
Then move pointers
```

---

# WHY I MIGHT FORGET THIS:

Because pointer directions keep changing dynamically.

The most confusing part is:

```cpp
head->next->next = head;
```

The easiest way to remember:

```text
Make next node point back to current node.
```

---

# INTERVIEW FLOW:

## Step 1

Clarify problem.

```text
We need to reverse a singly linked list.
```

---

## Step 2

Mention approaches.

```text
1. Stack based
2. Iterative pointer reversal
3. Recursive
```

---

## Step 3

Briefly explain brute force.

```text
Using stack requires O(N) extra space.
```

---

## Step 4

Move to optimal.

```text
We can reverse links directly using pointers.
```

---

## Step 5

Explain pointers.

```text
prev  -> previous node
temp  -> current node
front -> stores next node
```

---

## Step 6

Dry run.

Interviewers heavily judge linked list dry runs.

---

## Step 7

Write clean code.

---

## Step 8

Complexity analysis.

---

# TIME COMPLEXITY:

# Stack Based

```text
O(N)
```

Two traversals.

---

# Iterative

```text
O(N)
```

Each node visited once.

---

# Recursive

```text
O(N)
```

Each node visited once.

---

# SPACE COMPLEXITY:

# Stack Based

```text
O(N)
```

Extra stack used.

---

# Iterative

```text
O(1)
```

No extra space.

---

# Recursive

```text
O(N)
```

Recursive call stack.

---

# EDGE CASES:

## 1. Empty List

```text
NULL
```

Return NULL.

---

## 2. Single Node

```text
1
```

Return same node.

---

## 3. Two Nodes

```text
1 -> 2
```

Becomes:

```text
2 -> 1
```

---

## 4. Large Linked List

Ensure iterative solution for memory efficiency.

---

# PATTERN RECOGNITION:

Use this pattern when:

```text
1. Problem asks to reverse links
2. Need in-place linked list modification
3. Need O(1) space solution
4. Need pointer manipulation
```

Common questions:

```text
- Reverse Linked List
- Reverse in K Groups
- Reverse Between Positions
- Palindrome Linked List
- Reorder List
```

---

# CLEAN C++ CODE

# 1. Stack Based

```cpp
class Solution {
  public:
    Node* reverseList(Node* head) {
        
        stack<int> st;
        
        Node* temp = head;
        
        while(temp != NULL){
            st.push(temp->data);
            temp = temp->next;
        }
        
        temp = head;
        
        while(temp != NULL){
            
            temp->data = st.top();
            st.pop();
            
            temp = temp->next;
        }
        
        return head;
    }
};
```

---

# 2. Iterative Optimal

```cpp
class Solution {
  public:
    Node* reverseList(Node* head) {
        
        Node* prev = NULL;
        Node* temp = head;
        
        while(temp != NULL){
            
            Node* front = temp->next;
            
            temp->next = prev;
            
            prev = temp;
            temp = front;
        }
        
        return prev;
    }
};
```

---

# 3. Recursive

```cpp
class Solution {
  public:
    
    Node* reverseList(Node* head) {
        
        if(head == NULL || head->next == NULL){
            return head;
        }
        
        Node* newHead = reverseList(head->next);
        
        head->next->next = head;
        
        head->next = NULL;
        
        return newHead;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES

## Iterative

### Store next node

```cpp
Node* front = temp->next;
```

Prevents losing remaining list.

---

### Reverse link

```cpp
temp->next = prev;
```

Changes direction.

---

### Move pointers

```cpp
prev = temp;
temp = front;
```

Moves traversal ahead.

---

## Recursive

### Recursive call

```cpp
Node* newHead = reverseList(head->next);
```

Reverse remaining list first.

---

### Reverse current link

```cpp
head->next->next = head;
```

Makes next node point back.

---

### Break old link

```cpp
head->next = NULL;
```

Prevents cycle formation.

---

# EASY TO REMEMBER SUMMARY

# Stack

```text
Push values
Pop values
Overwrite nodes
```

---

# Iterative

```text
Store next
Reverse link
Move pointers
Repeat
```

---

# Recursive

```text
Go till end
Reverse while coming back
Break old links
```
````
