

# PROBLEM:

Given the head of a singly linked list and an integer `x`, delete the `xth` node (1-based indexing) from the linked list and return the updated linked list.

Example:

```cpp
Input:
1 -> 2 -> 3 -> 4
x = 3

Output:
1 -> 2 -> 4
```

---

# PATTERN:

## Linked List Traversal + Pointer Manipulation

---

# WHY THIS PATTERN:

In a singly linked list:

- each node stores only the address of the next node
- nodes are connected through pointers
- deletion cannot be done using indexing like arrays

To delete a node:

```text
prev -> current -> next
```

we must reconnect:

```text
prev ---------> next
```

This is why pointer manipulation is required.

---

# CORE IDEA:

To delete the `xth` node:

1. Reach the target node
2. Keep track of the previous node
3. Change previous node’s next pointer
4. Delete target node

Special handling is needed when deleting the head node.

---

# BRUTE FORCE:

No separate brute force approach is needed because:

- linked list deletion naturally requires traversal
- optimal solution itself is already straightforward

So directly writing the optimal solution is acceptable.

---

# OPTIMAL APPROACH:

## Case 1 → Delete Head Node

If:

```cpp
x == 1
```

Move head to next node and delete old head.

---

## Case 2 → Delete Any Other Node

Traverse linked list while maintaining:

```cpp
prev
temp
```

When target node is reached:

```cpp
prev->next = temp->next;
```

Delete temp node.

---

# ALGORITHM:

## Step 1

Check if linked list is empty.

```cpp
if(head == NULL)
```

Return head.

---

## Step 2

Handle head deletion separately.

```cpp
if(x == 1)
```

- store old head
- move head forward
- delete old head
- return updated head

---

## Step 3

Initialize traversal pointers.

```cpp
temp = head
prev = NULL
count = 0
```

---

## Step 4

Traverse linked list.

```cpp
while(temp != NULL)
```

Increment count.

---

## Step 5

When:

```cpp
count == x
```

target node found.

Reconnect links:

```cpp
prev->next = temp->next;
```

Delete node:

```cpp
delete temp;
```

Break loop.

---

## Step 6

Return updated head.

---

# DRY RUN:

## Input

```text
1 -> 2 -> 3 -> 4
x = 3
```

---

## Initial State

```text
temp = 1
prev = NULL
count = 0
```

---

## Iteration 1

```text
count = 1
```

Move pointers:

```text
prev = 1
temp = 2
```

---

## Iteration 2

```text
count = 2
```

Move pointers:

```text
prev = 2
temp = 3
```

---

## Iteration 3

```text
count = 3
```

Target node found.

---

## Before Deletion

```text
2 -> 3 -> 4
```

Execute:

```cpp
prev->next = temp->next;
```

becomes:

```text
2 -> 4
```

Entire list:

```text
1 -> 2 -> 4
```

Delete node `3`.

Final answer:

```text
1 -> 2 -> 4
```

---

# IMPORTANT CODE SNIPPETS:

## Delete Head Node

```cpp
Node* temp = head;
head = head->next;
delete temp;
```

---

## Core Deletion Logic

```cpp
prev->next = temp->next;
delete temp;
```

---

## Traversal

```cpp
prev = temp;
temp = temp->next;
```

---

# COMMON MISTAKES:

## Mistake 1

Writing:

```cpp
temp->next = temp->next->next;
```

instead of:

```cpp
prev->next = temp->next;
```

Wrong pointer gets modified.

---

## Mistake 2

Not handling head deletion separately.

Head node has no previous node.

---

## Mistake 3

Deleting node before reconnecting pointers.

Can lose access to remaining list.

---

## Mistake 4

Forgetting:

```cpp
return head;
```

at end.

---

## Mistake 5

Confusing current node and previous node during traversal.

---

# WHY I MIGHT FORGET THIS:

Because linked list problems are pointer-heavy.

Most confusion happens due to:

- pointer directions
- reconnecting nodes
- deleting correct node
- tracking previous node

Main thing to remember:

```text
Previous node reconnects the chain.
```

---

# INTERVIEW FLOW:

## Step 1

Explain special case:

```text
If x == 1, delete head separately.
```

---

## Step 2

Explain traversal using:

```text
prev and temp pointers
```

---

## Step 3

Explain deletion visually:

```text
prev -> temp -> next
```

becomes:

```text
prev ---------> next
```

---

## Step 4

Explain complexity.

---

# TIME COMPLEXITY:

## O(N)

Reason:

In worst case:

- we may traverse entire linked list once

Example:

```text
deleting last node
```

requires full traversal.

---

# SPACE COMPLEXITY:

## O(1)

Reason:

Only a few pointers are used.

No extra data structure needed.

---

# EDGE CASES:

## Case 1

Empty linked list

```text
NULL
```

---

## Case 2

Single node list

```text
1
x = 1
```

Result:

```text
NULL
```

---

## Case 3

Delete head node

```text
1 -> 2 -> 3
x = 1
```

---

## Case 4

Delete last node

```text
1 -> 2 -> 3
x = 3
```

---

## Case 5

Two-node linked list

```text
1 -> 2
x = 2
```

---

# PATTERN RECOGNITION:

Think of this pattern when:

- deleting nodes from linked list
- modifying connections
- removing nth node
- deleting by position
- reconnecting linked list structure

Key signal:

```text
Delete node from linked list
```

Usually means:

```text
Pointer manipulation + previous node tracking
```

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
    
    Node* deleteNode(Node* head, int x) {
        
        // Empty linked list
        if(head == NULL)
            return head;
        
        // Delete head node
        if(x == 1) {
            
            Node* temp = head;
            
            // Move head forward
            head = head->next;
            
            // Delete old head
            delete temp;
            
            return head;
        }
        
        Node* temp = head;
        Node* prev = NULL;
        int count = 0;
        
        // Traverse linked list
        while(temp != NULL) {
            
            count++;
            
            // Target node found
            if(count == x) {
                
                // Skip current node
                prev->next = temp->next;
                
                // Delete current node
                delete temp;
                
                break;
            }
            
            // Move pointers
            prev = temp;
            temp = temp->next;
        }
        
        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## Line

```cpp
if(x == 1)
```

Why?

Head node has no previous node.

Deletion logic changes.

---

## Line

```cpp
prev->next = temp->next;
```

Why?

Previous node skips current node.

---

## Line

```cpp
delete temp;
```

Why?

Frees memory of removed node.

---

## Line

```cpp
prev = temp;
temp = temp->next;
```

Why?

Moves traversal forward while maintaining previous node.

---

# EASY-TO-REMEMBER SUMMARY

## Linked List Deletion Formula

```text
prev -> temp -> next
```

Convert into:

```text
prev ---------> next
```

using:

```cpp
prev->next = temp->next;
```

Then:

```cpp
delete temp;
```

MOST IMPORTANT RULE:

```text
In singly linked list deletion,
previous node does the real work.
```
````
