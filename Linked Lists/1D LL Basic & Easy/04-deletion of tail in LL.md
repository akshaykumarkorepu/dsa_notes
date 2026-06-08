

# PROBLEM:

Delete the last node (tail node) of a singly linked list and return the updated head.

Example:

```text
Input:
1 -> 2 -> 3 -> 4 -> 5

Output:
1 -> 2 -> 3 -> 4
```

---

# PATTERN:

## Linked List Traversal + Pointer Manipulation

We traverse the linked list until we reach the **second last node**.

Then we:

```cpp
temp->next = NULL;
```

to disconnect the last node.

If we want proper memory cleanup in C++:

```cpp
delete temp->next;
```

---

# WHY THIS PATTERN:

In a singly linked list:

```text
node -> next
```

A node only knows about the next node.

It does NOT know about the previous node.

So to delete the last node:

- we must first reach the node before it
- then modify its `next`

That is why traversal is necessary.

---

# CORE IDEA:

## Main Goal

Reach the second last node.

Example:

```text
1 -> 2 -> 3 -> 4 -> 5
               ^
          second last
```

Then:

```cpp
delete temp->next;
temp->next = NULL;
```

This:

- deletes node `5`
- disconnects it from the list

---

# BRUTE FORCE:

No separate brute force is needed.

The optimal solution itself is already straightforward and expected in interviews.

There is no meaningful optimization transition here.

---

# OPTIMAL APPROACH:

## Steps

### Step 1:
Handle edge cases.

- empty list
- single node list

---

### Step 2:
Traverse until second last node.

Condition:

```cpp
while(temp->next->next != NULL)
```

---

### Step 3:
Delete last node.

```cpp
delete temp->next;
```

---

### Step 4:
Remove dangling pointer.

```cpp
temp->next = NULL;
```

---

# ALGORITHM:

## Step-by-step

### Step 1:
If list is empty:

```cpp
if(head == NULL)
```

return `NULL`.

---

### Step 2:
If only one node exists:

```cpp
if(head->next == NULL)
```

after deletion list becomes empty.

---

### Step 3:
Create traversal pointer.

```cpp
Node* temp = head;
```

---

### Step 4:
Move until second last node.

```cpp
while(temp->next->next != NULL)
```

---

### Step 5:
Delete last node.

```cpp
delete temp->next;
```

---

### Step 6:
Disconnect pointer.

```cpp
temp->next = NULL;
```

---

### Step 7:
Return head.

```cpp
return head;
```

---

# DRY RUN:

## Example

```text
1 -> 2 -> 3 -> 4 -> 5
```

---

## Initial

```text
temp = 1
```

---

## Move temp

### Iteration 1

```text
temp = 2
```

---

### Iteration 2

```text
temp = 3
```

---

### Iteration 3

```text
temp = 4
```

Now:

```text
temp->next->next == NULL
```

because:

```text
4 -> 5 -> NULL
```

So stop.

---

## Delete Last Node

```cpp
delete temp->next;
```

Node `5` memory is deleted.

---

## Remove Dangling Pointer

```cpp
temp->next = NULL;
```

Final list:

```text
1 -> 2 -> 3 -> 4
```

---

# IMPORTANT CODE SNIPPETS:

## Traversing to second last node

```cpp
while(temp->next->next != NULL)
```

---

## Proper deletion

```cpp
delete temp->next;
temp->next = NULL;
```

---

## Single node handling

```cpp
if(head == NULL || head->next == NULL)
    return NULL;
```

---

# COMMON MISTAKES:

## Mistake 1

Using:

```cpp
while(temp->next != NULL)
```

This moves temp to the last node.

Then:

```cpp
temp->next = NULL;
```

does nothing useful.

---

## Mistake 2

Forgetting single node case.

Then:

```cpp
temp->next->next
```

causes runtime error.

---

## Mistake 3

Confusing:

```cpp
temp->next = NULL;
```

with actual deletion.

This only disconnects.

Memory still exists unless:

```cpp
delete
```

is used.

---

# WHY I MIGHT FORGET THIS:

Because students often confuse:

```text
last node
```

with

```text
second last node
```

The key realization is:

```text
To delete the last node,
you must stand on the previous node.
```

---

# INTERVIEW FLOW:

## Step 1

Start with intuition:

> “Since this is a singly linked list, we cannot move backward. So to delete the tail node, we first need to reach the second last node.”

---

## Step 2

Explain traversal:

> “I will traverse until temp->next->next becomes NULL.”

---

## Step 3

Explain deletion:

> “Then I’ll delete temp->next and set temp->next to NULL.”

---

## Step 4

Mention edge cases:

- empty list
- single node list

---

# TIME COMPLEXITY:

```text
O(n)
```

Reason:

In the worst case we traverse the entire linked list once.

For a list of `n` nodes:

- traversal takes linear time

---

# SPACE COMPLEXITY:

```text
O(1)
```

Reason:

Only one traversal pointer is used.

No extra data structures are created.

---

# EDGE CASES:

## Case 1: Empty list

```text
NULL
```

Return `NULL`.

---

## Case 2: Single node list

```text
1
```

After deletion:

```text
NULL
```

---

## Case 3: Two node list

```text
1 -> 2
```

After deletion:

```text
1
```

---

# PATTERN RECOGNITION:

You should think of this pattern whenever:

- you need to delete a node in a singly linked list
- modification depends on previous node
- question asks:
  - delete nth node
  - delete tail
  - remove duplicates
  - remove node after X
  - reverse links

Core clue:

```text
Need to modify next pointer
→ reach previous node first
```

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
    Node* removeLastNode(Node* head) {
        
        // Empty list or single node list
        if(head == NULL || head->next == NULL) {
            return NULL;
        }
        
        Node* temp = head;
        
        // Move to second last node
        while(temp->next->next != NULL) {
            temp = temp->next;
        }
        
        // Delete last node
        delete temp->next;
        
        // Remove dangling pointer
        temp->next = NULL;
        
        return head;
    }
};
```

---

# WELL-COMMENTED CODE

```cpp
class Solution {
  public:
    Node* removeLastNode(Node* head) {
        
        // If list is empty OR has only one node
        if(head == NULL || head->next == NULL) {
            return NULL;
        }
        
        // Pointer used for traversal
        Node* temp = head;
        
        // Move until second last node
        while(temp->next->next != NULL) {
            temp = temp->next;
        }
        
        // Delete memory of last node
        delete temp->next;
        
        // Disconnect deleted node
        temp->next = NULL;
        
        // Return updated head
        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## Edge case

```cpp
if(head == NULL || head->next == NULL)
```

If there is:

- no node
- only one node

then after deletion list becomes empty.

---

## Traversal

```cpp
while(temp->next->next != NULL)
```

We stop at second last node because only that node can disconnect the tail.

---

## Deletion

```cpp
delete temp->next;
```

Frees memory of last node.

---

## Disconnect

```cpp
temp->next = NULL;
```

Removes dangling pointer.

---

# EASY-TO-REMEMBER SUMMARY

```text
To delete the last node:

1. Reach second last node
2. Delete next node
3. Set next = NULL
```

Shortcut memory line:

```text
"Stand before the node you want to remove."
```
````
