

## PROBLEM:
Search for a given key in a singly linked list.

Return:
- `true` → if key exists
- `false` → if key does not exist

Example:

```text
1 -> 2 -> 3 -> 4 -> NULL

key = 3
```

Output:

```text
true
```

---

# PATTERN:
Linear Traversal / Linked List Traversal

---

# WHY THIS PATTERN:
A linked list does NOT support direct indexing like arrays.

In arrays:
```cpp
arr[i]
```

is possible because memory is contiguous.

But in linked lists:
- nodes are scattered in memory
- each node only knows the address of the next node

So the ONLY way to search is:

```text
Start from head
Visit nodes one by one
Move using next pointers
```

That is exactly what traversal means.

---

# CORE IDEA:

We create a temporary pointer:

```cpp
Node* temp = head;
```

Then keep moving forward:

```cpp
temp = temp->next;
```

At every node:
- compare current node data with key
- if equal → return true
- if list ends → return false

---

# BRUTE FORCE:

There is NO separate brute force here.

The traversal itself is already the optimal solution.

Why?

Because in worst case:
- we may need to inspect every node
- linked lists do not allow binary search
- no random access exists

So:
```text
O(N)
```

is unavoidable.

---

# OPTIMAL APPROACH:

Use iterative traversal.

Steps:
1. Create temporary pointer
2. Traverse till NULL
3. Compare node data with key
4. Return true if found
5. Return false otherwise

---

# ALGORITHM:

## Step 1:
Create traversal pointer

```cpp
Node* temp = head;
```

---

## Step 2:
Traverse linked list

```cpp
while(temp != NULL)
```

Meaning:
keep visiting nodes until list ends.

---

## Step 3:
Check current node

```cpp
if(temp->data == key)
```

If found:

```cpp
return true;
```

---

## Step 4:
Move ahead

```cpp
temp = temp->next;
```

This is the MOST IMPORTANT line.

Without this:
- pointer never moves
- infinite loop occurs

---

## Step 5:
If traversal finishes

That means key was never found.

So:

```cpp
return false;
```

---

# DRY RUN:

Linked List:

```text
1 -> 2 -> 3 -> 4 -> NULL
```

Key = 3

---

## INITIAL STATE

```text
temp = 1
```

---

## ITERATION 1

Current node:

```text
1
```

Check:

```text
1 == 3 ?
```

NO

Move:

```cpp
temp = temp->next;
```

Now:

```text
temp = 2
```

---

## ITERATION 2

Current node:

```text
2
```

Check:

```text
2 == 3 ?
```

NO

Move ahead.

Now:

```text
temp = 3
```

---

## ITERATION 3

Current node:

```text
3
```

Check:

```text
3 == 3 ?
```

YES

Return:

```cpp
true
```

DONE.

---

# IMPORTANT CODE SNIPPETS:

## 1. Basic Traversal Pattern

```cpp
Node* temp = head;

while(temp != NULL) {

    temp = temp->next;
}
```

This pattern is used in:
- searching
- counting nodes
- finding max/min
- printing list
- deleting nodes
- reversing list

Master this properly.

---

## 2. Searching Pattern

```cpp
while(temp != NULL) {

    if(temp->data == key) {
        return true;
    }

    temp = temp->next;
}
```

---

## 3. Safe Traversal Pointer

```cpp
Node* temp = head;
```

Never move head directly unless required.

---

# COMMON MISTAKES:

## 1. Forgetting to move pointer

Wrong:

```cpp
while(temp != NULL) {

    if(temp->data == key)
        return true;
}
```

This causes infinite loop.

Always:

```cpp
temp = temp->next;
```

---

## 2. Using `temp->next != NULL`

Wrong:

```cpp
while(temp->next != NULL)
```

Why wrong?

Last node never gets checked.

Correct:

```cpp
while(temp != NULL)
```

---

## 3. Moving head directly

Wrong:

```cpp
head = head->next;
```

This can lose original head pointer.

Better:

```cpp
Node* temp = head;
```

---

## 4. Forgetting empty list case

If:

```text
head == NULL
```

Loop should naturally stop.

---

# WHY I MIGHT FORGET THIS:

Because it looks “too easy”.

But beginners commonly forget:
- pointer movement
- traversal condition
- NULL handling

The key memory trick:

```text
Linked list = walk node by node
```

You CANNOT jump.

You MUST traverse.

---

# INTERVIEW FLOW:

If interviewer asks:

> “How will you search in a linked list?”

You can explain like this:

---

“Since linked lists do not support indexing or random access, we must traverse sequentially from the head node.

I create a temporary pointer starting at head.

Then I iterate while the pointer is not NULL.

At every node:
- compare current node’s data with the key
- if equal, return true immediately

Otherwise move to the next node using:
`temp = temp->next`

If traversal completes, that means key does not exist, so return false.”

---

# TIME COMPLEXITY:

## Worst Case:

```text
O(N)
```

Reason:
- key may be at last node
- or may not exist
- so all nodes might need checking

---

# SPACE COMPLEXITY:

```text
O(1)
```

Reason:
- only one pointer variable is used
- no extra data structures

---

# EDGE CASES:

## 1. Empty Linked List

```text
head = NULL
```

Return:

```text
false
```

---

## 2. Single Node Found

```text
5 -> NULL
key = 5
```

Return:

```text
true
```

---

## 3. Single Node Not Found

```text
5 -> NULL
key = 10
```

Return:

```text
false
```

---

## 4. Key at Head

```text
3 -> 5 -> 7
```

Key found immediately.

---

## 5. Key at Last Node

Need full traversal.

---

## 6. Key Not Present

Entire list gets traversed.

---

# PATTERN RECOGNITION:

You should immediately think of this traversal pattern when:
- question mentions linked list
- need to inspect every node
- searching/checking/counting is involved
- no random access exists

Typical keywords:
- search element
- find value
- count nodes
- print list
- traverse list

Whenever you see:

```text
Visit every node once
```

Think:

```cpp
Node* temp = head;

while(temp != NULL)
```

This is THE foundational linked list pattern.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:

    bool searchKey(Node* head, int key) {

        // Start traversal from head
        Node* temp = head;

        // Traverse entire linked list
        while(temp != NULL) {

            // Check current node
            if(temp->data == key) {
                return true;
            }

            // Move to next node
            temp = temp->next;
        }

        // Key not found
        return false;
    }
};
```

---

# WELL-COMMENTED CODE WITH INTUITION

```cpp
class Solution {
public:

    bool searchKey(Node* head, int key) {

        // temp is used to travel through the list
        // We do not move head directly
        Node* temp = head;

        // Continue until list ends
        while(temp != NULL) {

            // If current node contains key
            // immediately return true
            if(temp->data == key) {
                return true;
            }

            // Move forward in linked list
            // VERY IMPORTANT STEP
            temp = temp->next;
        }

        // If loop finished,
        // key does not exist
        return false;
    }
};
```

---

# EASY-TO-REMEMBER SUMMARY:

```text
Linked List Search =
Start from head
Check current node
Move to next node
Repeat until NULL
```

Golden pattern:

```cpp
Node* temp = head;

while(temp != NULL) {

    // process node

    temp = temp->next;
}
```

Master this once.

It appears in almost EVERY linked list problem.
````
