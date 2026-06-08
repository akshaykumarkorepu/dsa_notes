

# PROBLEM:

We are given the head of a singly linked list and a value `x`.

We need to insert a new node containing `x` at the end of the linked list and return the updated head.

### Example

```text
1 -> 2 -> 3
```

Insert `4`

Final list:

```text
1 -> 2 -> 3 -> 4
```

The challenge is that in a singly linked list, we cannot directly access the last node.

So we must traverse node by node until we reach the end.

---

# PATTERN:

Linked List Traversal + Tail Attachment

---

# WHY THIS PATTERN:

A singly linked list only gives access to:

```text
current node
next node
```

There is:

- no indexing
- no backward traversal
- no direct tail access

So to insert at the end:

1. traverse till the last node
2. attach the new node there

This is the standard linked list insertion-at-tail pattern.

---

# CORE IDEA:

Move through the linked list until:

```cpp
temp->next == NULL
```

which means `temp` is the last node.

Then connect:

```cpp
temp->next = newNode;
```

---

# BRUTE FORCE:

A separate brute force solution is not really needed here because traversal itself is the direct and optimal approach.

There is no better general solution unless a tail pointer is already maintained separately.

So interviewers usually expect the traversal solution immediately.

---

# OPTIMAL APPROACH:

1. If linked list is empty:
   - return a new node as head
2. Create a new node with value `x`
3. Traverse till the last node
4. Connect last node to the new node
5. Return head

---

# ALGORITHM:

### Step 1:

Handle empty linked list.

```cpp
if(head == NULL)
    return new Node(x);
```

If no nodes exist, the new node itself becomes the head.

---

### Step 2:

Create traversal pointer.

```cpp
Node* temp = head;
```

We use `temp` because we should not move the original `head`.

---

### Step 3:

Traverse till last node.

```cpp
while(temp->next != NULL)
```

This loop stops exactly at the last node.

---

### Step 4:

Create new node.

```cpp
Node* newNode = new Node(x);
```

---

### Step 5:

Attach new node at end.

```cpp
temp->next = newNode;
```

---

### Step 6:

Return head.

```cpp
return head;
```

Head remains unchanged because insertion is at the end.

---

# DRY RUN:

### Example:

```text
head = 1 -> 2 -> 3
x = 4
```

---

### Initial State:

```text
temp = 1
```

---

### Traversal:

Move temp forward.

```text
temp = 2
temp = 3
```

Now:

```text
temp->next == NULL
```

So `temp` is the last node.

---

### Create New Node:

```text
newNode = 4
```

---

### Attach:

```text
3 -> 4
```

Final list:

```text
1 -> 2 -> 3 -> 4
```

---

# IMPORTANT CODE SNIPPETS:

### Empty list handling:

```cpp
if(head == NULL)
    return new Node(x);
```

---

### Traversal:

```cpp
while(temp->next != NULL)
{
    temp = temp->next;
}
```

---

### Attach node:

```cpp
temp->next = newNode;
```

---

# COMMON MISTAKES:

## Mistake 1 — Forgetting Empty List Case

Wrong:

```cpp
while(temp->next != NULL)
```

If `head == NULL`, this crashes.

---

## Mistake 2 — Traversing Using `temp != NULL`

Wrong:

```cpp
while(temp != NULL)
```

After loop, `temp` becomes NULL.

You lose access to the last node.

Correct:

```cpp
while(temp->next != NULL)
```

---

## Mistake 3 — Moving Head Directly

Wrong:

```cpp
head = head->next;
```

This may lose the original head.

Always use a traversal pointer.

---

## Mistake 4 — Forgetting to Connect Node

Creating node alone is not enough.

This line is necessary:

```cpp
temp->next = newNode;
```

---

# WHY I MIGHT FORGET THIS:

People often confuse:

```text
temp != NULL
```

with:

```text
temp->next != NULL
```

Remember:

- `temp != NULL` goes one step too far
- `temp->next != NULL` stops at the last node

Another common confusion:

```text
Why use temp instead of head?
```

Because moving head may lose the starting node of the linked list.

---

# INTERVIEW FLOW:

1. Mention singly linked list has no direct tail access
2. Say we must traverse till the last node
3. Handle empty list separately
4. Use traversal pointer
5. Stop at last node using:

```cpp
while(temp->next != NULL)
```

6. Attach new node
7. Return head
8. Mention complexity:
   - Time: O(N)
   - Space: O(1)

---

# TIME COMPLEXITY:

```text
O(N)
```

### Reason:

In worst case, we traverse all nodes once to reach the end.

If there are `N` nodes:

```text
1 traversal = N steps
```

---

# SPACE COMPLEXITY:

```text
O(1)
```

### Reason:

Only a few pointers are used.

No extra data structure is created.

---

# EDGE CASES:

## Case 1 — Empty List

Input:

```text
NULL
```

Insert `5`

Output:

```text
5
```

---

## Case 2 — Single Node

Input:

```text
1
```

Insert `2`

Output:

```text
1 -> 2
```

---

## Case 3 — Large List

Traversal still works correctly.

---

# PATTERN RECOGNITION:

You should think of this pattern whenever:

- question says “insert at end”
- question involves “last node”
- linked list traversal is required
- no tail pointer is given

### Key signal:

```text
Traverse until next becomes NULL
```

That usually means:

```text
You are trying to reach the tail node
```

---

# CLEAN C++ CODE

```cpp
/*
class Node {
  public:
    int data;
    Node* next;

    Node(int data) {
        this->data = data;
        this->next = nullptr;
    }
};
*/

class Solution {
  public:
  
    Node *insertAtEnd(Node *head, int x) {
        
        // If list is empty
        if(head == NULL)
            return new Node(x);
        
        // Traversal pointer
        Node* temp = head;
        
        // Move till last node
        while(temp->next != NULL) {
            temp = temp->next;
        }
        
        // Create new node
        Node* newNode = new Node(x);
        
        // Attach new node
        temp->next = newNode;
        
        // Return original head
        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Empty list handling

```cpp
if(head == NULL)
    return new Node(x);
```

If no nodes exist, new node itself becomes the head.

---

### Traversal pointer

```cpp
Node* temp = head;
```

Used to move through the list without changing head.

---

### Traversing till end

```cpp
while(temp->next != NULL)
```

Stops exactly at the last node.

---

### Creating new node

```cpp
Node* newNode = new Node(x);
```

Creates node that must be inserted.

---

### Linking node

```cpp
temp->next = newNode;
```

Connects old tail to new node.

---

### Returning head

```cpp
return head;
```

Starting node remains unchanged.

---

# EASY-TO-REMEMBER SUMMARY

```text
1. Empty list?
   → return new node

2. Otherwise:
   → move till last node

3. Create new node

4. Connect last node to new node

5. Return head
```

### Shortcut Memory Line

```text
Traverse → Reach Tail → Attach Node
```
````
