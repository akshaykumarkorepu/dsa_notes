
## PROBLEM:

Given the head of a singly linked list, find the total number of nodes present in the linked list.

Example:

```txt
1 -> 2 -> 3 -> 4 -> 5
```

Output:

```txt
5
```

---

# PATTERN:

Traversal Pattern / Iterative Linked List Traversal

---

# WHY THIS PATTERN:

A linked list does not support direct indexing like arrays.

To access nodes:

* we must start from the head
* move node by node using `next`

So whenever a question asks:

* count nodes
* search element
* print list
* sum values
* find max/min

we use traversal.

This is the MOST BASIC linked list pattern.

---

# CORE IDEA:

Traverse the linked list from head till `NULL`.

For every node visited:

* increment count
* move forward

At the end:

* count = length of linked list

---

# BRUTE FORCE:

Not needed.

Reason:

* traversal itself is already optimal
* there is no better than visiting all nodes
* interviewer does not expect optimization progression here

So directly explain the optimal traversal approach.

---

# OPTIMAL APPROACH:

Use a temporary pointer.

Steps:

1. Create `temp = head`
2. Initialize `count = 0`
3. Traverse while `temp != NULL`
4. Increment count
5. Move temp forward
6. Return count

---

# ALGORITHM:

## Step 1:

Initialize count.

```cpp
int count = 0;
```

---

## Step 2:

Create traversal pointer.

```cpp
Node* temp = head;
```

We use temp so original head is not lost.

---

## Step 3:

Traverse till end.

```cpp
while(temp != NULL)
```

This means:
continue until list ends.

---

## Step 4:

Increase count.

```cpp
count++;
```

Every iteration means:
one node visited.

---

## Step 5:

Move to next node.

```cpp
temp = temp->next;
```

Without this:
loop becomes infinite.

---

## Step 6:

Return answer.

```cpp
return count;
```

---

# DRY RUN:

Input:

```txt
2 -> 4 -> 6 -> 7 -> 5 -> 1 -> 0
```

Initial State:

```txt
count = 0
temp = head (2)
```

---

## Iteration 1

Current Node = 2

```txt
count = 1
temp moves to 4
```

---

## Iteration 2

Current Node = 4

```txt
count = 2
temp moves to 6
```

---

## Iteration 3

Current Node = 6

```txt
count = 3
temp moves to 7
```

---

## Iteration 4

Current Node = 7

```txt
count = 4
temp moves to 5
```

---

## Iteration 5

Current Node = 5

```txt
count = 5
temp moves to 1
```

---

## Iteration 6

Current Node = 1

```txt
count = 6
temp moves to 0
```

---

## Iteration 7

Current Node = 0

```txt
count = 7
temp becomes NULL
```

Loop stops.

Return:

```txt
7
```

---

# IMPORTANT CODE SNIPPETS:

## Basic Traversal Pattern

```cpp
Node* temp = head;

while(temp != NULL) {
    
    // work
    
    temp = temp->next;
}
```

This is the FOUNDATION of linked lists.

---

## Counting Pattern

```cpp
int count = 0;

while(temp != NULL) {
    
    count++;
    temp = temp->next;
}
```

---

# COMMON MISTAKES:

## 1. Forgetting to move pointer

Wrong:

```cpp
while(temp != NULL) {
    count++;
}
```

This causes infinite loop.

Always:

```cpp
temp = temp->next;
```

---

## 2. Modifying head directly

Wrong:

```cpp
head = head->next;
```

This loses original head.

Use temp pointer instead.

---

## 3. Using wrong loop condition

Wrong:

```cpp
while(temp->next != NULL)
```

This skips last node.

Correct:

```cpp
while(temp != NULL)
```

---

# WHY I MIGHT FORGET THIS:

Because the problem feels TOO easy.

But interviewers use this question to check:

* pointer handling
* traversal understanding
* linked list basics

Main thing to remember:

```txt
Linked List = Move using next pointer
```

Unlike arrays:
you cannot jump using index.

---

# INTERVIEW FLOW:

## Step 1: State intuition

“We need to count how many nodes exist in the linked list.”

---

## Step 2: Mention linked list property

“Since linked lists do not support indexing, we must traverse node by node.”

---

## Step 3: Explain approach

“I’ll use a temporary pointer starting from head and move till NULL.
For every node visited, I increment the counter.”

---

## Step 4: Mention stopping condition

“When temp becomes NULL, it means we reached end of list.”

---

## Step 5: Complexity

“We visit each node once, so time complexity is O(N) and space complexity is O(1).”

---

# TIME COMPLEXITY:

O(N)

Reason:

* we visit every node exactly once
* if there are N nodes, loop runs N times

---

# SPACE COMPLEXITY:

O(1)

Reason:

* only one temporary pointer and one counter used
* no extra data structure used

---

# EDGE CASES:

## 1. Single Node

```txt
1
```

Output:

```txt
1
```

---

## 2. Empty List (if allowed)

```txt
head = NULL
```

Output:

```txt
0
```

---

## 3. Large Linked List

Still works because traversal is linear.

---

# PATTERN RECOGNITION:

Use this pattern whenever:

* traversal is needed
* question says “visit every node”
* counting/searching/printing/summing/max/min

Keywords:

* linked list
* traverse
* iterate
* count nodes
* search element

Immediate thought:

```cpp
Node* temp = head;

while(temp != NULL)
```

That is the core linked list traversal template.

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
  
    int getCount(Node* head) {
        
        // stores total number of nodes
        int count = 0;
        
        // temporary pointer for traversal
        Node* temp = head;
        
        // traverse till end of linked list
        while(temp != NULL) {
            
            // one node visited
            count++;
            
            // move to next node
            temp = temp->next;
        }
        
        // return total nodes
        return count;
    }
};
```

---

# WELL-COMMENTED CODE

```cpp
class Solution {
  public:
  
    int getCount(Node* head) {
        
        // Counter to store length
        int count = 0;
        
        // Temp pointer used for traversal
        // We do NOT move head directly
        Node* temp = head;
        
        // Continue until end of list
        while(temp != NULL) {
            
            // Current node counted
            count++;
            
            // Move forward in linked list
            temp = temp->next;
        }
        
        // Final answer
        return count;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES

## This line:

```cpp
Node* temp = head;
```

Why?

Because:

* head should remain unchanged
* temp is used for movement

---

## This line:

```cpp
while(temp != NULL)
```

Why?

Because:

* NULL means end of linked list
* loop continues while nodes exist

---

## This line:

```cpp
temp = temp->next;
```

Why?

Because linked list traversal happens using pointers.

This is equivalent to:

```cpp
i++
```

in arrays.

---

# EASY-TO-REMEMBER SUMMARY

```txt
Linked List Length = Simple Traversal

1. Start temp from head
2. While temp exists:
       count++
       temp = temp->next
3. Return count

MOST IMPORTANT TEMPLATE:

Node* temp = head;

while(temp != NULL) {
    temp = temp->next;
}
```
