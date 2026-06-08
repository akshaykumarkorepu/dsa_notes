

# PROBLEM:
Delete the head node of a singly linked list and return the new head.

Example:

```text
1 -> 2 -> 3 -> 4

After deleting head:

2 -> 3 -> 4
```

---

# PATTERN:
Linked List Pointer Manipulation

More specifically:

- Head modification
- Node deletion using pointer updates

---

# WHY THIS PATTERN:
In linked lists:

- nodes are connected using pointers
- deletion is done by changing links
- elements are NOT shifted like arrays

Since we need to remove the first node:

- we simply move the `head` pointer forward

This is a direct pointer manipulation problem.

---

# CORE IDEA:
The node after the current head becomes the new head.

```cpp
head = head->next;
```

That is the entire core logic.

Before deleting:

```text
10 -> 20 -> 30
^
head
```

After moving head:

```text
10 -> 20 -> 30
      ^
     head
```

Now delete old node.

---

# BRUTE FORCE:
❌ No brute force needed.

Reason:

- optimal solution is already straightforward
- no better transition exists
- interviewer expects direct pointer handling

---

# OPTIMAL APPROACH:

## Idea:

1. Store old head temporarily
2. Move head to next node
3. Delete old head
4. Return new head

---

# ALGORITHM:

## Step 1:
Check if linked list is empty.

```cpp
if(head == NULL)
    return NULL;
```

---

## Step 2:
Store current head.

```cpp
Node* temp = head;
```

Why?

Because after moving head forward,
we will lose access to old head.

---

## Step 3:
Move head forward.

```cpp
head = head->next;
```

Now second node becomes the new head.

---

## Step 4:
Delete old head.

```cpp
delete temp;
```

Frees memory.

---

## Step 5:
Return updated head.

```cpp
return head;
```

---

# DRY RUN:

Input:

```text
10 -> 20 -> 30 -> 40
```

---

## Initial State

```text
head = 10
```

---

## Store old head

```cpp
Node* temp = head;
```

```text
temp = 10
```

---

## Move head

```cpp
head = head->next;
```

Now:

```text
head = 20
```

List becomes effectively:

```text
20 -> 30 -> 40
```

---

## Delete old head

```cpp
delete temp;
```

Node `10` removed from memory.

---

## Return head

Final list:

```text
20 -> 30 -> 40
```

---

# IMPORTANT CODE SNIPPETS:

## Empty List Check

```cpp
if(head == NULL)
    return NULL;
```

---

## Store Old Head

```cpp
Node* temp = head;
```

---

## Move Head

```cpp
head = head->next;
```

---

## Delete Old Node

```cpp
delete temp;
```

---

# COMMON MISTAKES:

## Mistake 1: Forgetting NULL Check

Wrong:

```cpp
head = head->next;
```

If head is NULL,
this crashes.

---

## Mistake 2: Losing Old Head

Wrong:

```cpp
head = head->next;
delete head;
```

Now original head is lost forever.

---

## Mistake 3: Returning Old Head

Always return updated head.

---

## Mistake 4: Forgetting Memory Cleanup

In C++:

```cpp
delete temp;
```

should be used.

---

# WHY I MIGHT FORGET THIS:

People usually overcomplicate linked lists.

But this problem is actually just:

```cpp
head = head->next;
```

The only tricky part is:

- not losing old head before deleting it

So remember:

```text
Save -> Move -> Delete
```

---

# INTERVIEW FLOW:

You can explain like this:

> Since the head node is the first node, deleting it means shifting the head pointer to the next node.
>
> I’ll first store the old head temporarily so I don’t lose access to it.
>
> Then I’ll move head forward, delete the old node, and return the updated head.

---

# TIME COMPLEXITY:

## O(1)

Reason:

- only constant number of operations
- no traversal needed

Operations:

- pointer update
- delete operation

All constant time.

---

# SPACE COMPLEXITY:

## O(1)

Reason:

Only one extra pointer variable is used.

```cpp
Node* temp
```

No extra data structures used.

---

# EDGE CASES:

## Edge Case 1: Empty List

Input:

```text
NULL
```

Output:

```text
NULL
```

Handled using:

```cpp
if(head == NULL)
```

---

## Edge Case 2: Single Node

Input:

```text
10
```

After deletion:

```text
NULL
```

Works correctly.

---

## Edge Case 3: Multiple Nodes

Input:

```text
1 -> 2 -> 3
```

Output:

```text
2 -> 3
```

---

# PATTERN RECOGNITION:

You should think of this pattern when:

- question says "delete head"
- "remove first node"
- "move head"
- linked list modification starts at beginning
- only head pointer changes

Key clue:

```text
No traversal needed
```

because operation is directly at the head.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    
    Node* deleteHead(Node* head) {

        // If list is empty
        if(head == NULL)
            return NULL;

        // Store old head
        Node* temp = head;

        // Move head to next node
        head = head->next;

        // Delete old head node
        delete temp;

        // Return updated head
        return head;
    }
};
```

---

# WELL-COMMENTED CODE

```cpp
class Solution {
public:

    Node* deleteHead(Node* head) {

        // Case: Empty linked list
        if(head == NULL)
            return NULL;

        // Save current head
        // Otherwise we lose access after moving head
        Node* temp = head;

        // Move head forward
        // Second node becomes new head
        head = head->next;

        // Delete old head node from memory
        delete temp;

        // Return updated linked list
        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## NULL Check

```cpp
if(head == NULL)
```

Avoids accessing:

```cpp
head->next
```

on an empty list.

---

## Store Head

```cpp
Node* temp = head;
```

Needed because after:

```cpp
head = head->next;
```

old node becomes unreachable.

---

## Move Head

```cpp
head = head->next;
```

This is the REAL deletion step logically.

---

## Delete Temp

```cpp
delete temp;
```

Removes old node from memory.

---

# EASY-TO-REMEMBER SUMMARY

## Golden Rule:

```text
Save -> Move -> Delete
```

Code flow:

```cpp
temp = head;
head = head->next;
delete temp;
```

Core idea:

```text
New head is simply old head's next node.
```
````
