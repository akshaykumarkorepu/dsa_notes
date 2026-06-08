
## PROBLEM:

Given the head of a singly linked list and a value `x`, insert a new node containing `x` at the **beginning** of the linked list and return the new head.

Example:

```text
Input:
2 -> 10
x = 1

Output:
1 -> 2 -> 10
```

---

# PATTERN:

## Linked List Head Manipulation

This pattern involves:

* changing the head node
* inserting/deleting at the beginning
* reconnecting pointers carefully

---

# WHY THIS PATTERN:

In linked lists:

* insertion at the beginning is the fastest operation
* we only need pointer updates
* no traversal is required

Since the new node becomes the first node:

```text
newNode -> oldHead
```

Then:

```text
head = newNode
```

This is a classic **head modification** problem.

---

# CORE IDEA:

The idea is extremely simple:

1. Create a new node
2. Point it to current head
3. Return it as the new head

Visual:

```text
Before:
2 -> 3 -> 4

After inserting 1:

1 -> 2 -> 3 -> 4
```

---

# BRUTE FORCE:

No brute force is needed here because:

* the optimal solution is already straightforward
* there is no meaningful optimization progression
* insertion at head is naturally O(1)

---

# OPTIMAL APPROACH:

## Steps

1. Create a new node with value `x`
2. Set:

```cpp
newNode->next = head;
```

3. Return `newNode`

That’s it.

---

# ALGORITHM:

## Step 1

Create new node:

```cpp
Node* newNode = new Node(x);
```

---

## Step 2

Connect new node to existing list:

```cpp
newNode->next = head;
```

This preserves the old linked list.

---

## Step 3

Return new node as head:

```cpp
return newNode;
```

Because the first node has changed.

---

# DRY RUN:

## Input

```text
head = 2 -> 3 -> 4
x = 1
```

---

## Initial State

```text
head
 ↓
2 -> 3 -> 4
```

---

## Create new node

```text
newNode
  ↓
  1
```

---

## Connect new node to old head

```text
1 -> 2 -> 3 -> 4
```

Because:

```cpp
newNode->next = head;
```

---

## Return new node

```text
head
 ↓
1 -> 2 -> 3 -> 4
```

Final answer:

```text
1 -> 2 -> 3 -> 4
```

---

# IMPORTANT CODE SNIPPETS:

## Create node

```cpp
Node* newNode = new Node(x);
```

---

## Attach old list

```cpp
newNode->next = head;
```

---

## Update head

```cpp
return newNode;
```

---

# COMMON MISTAKES:

## Mistake 1: Forgetting to connect old head

Wrong:

```cpp
Node* newNode = new Node(x);
return newNode;
```

This loses the entire old linked list.

---

## Mistake 2: Returning old head

Wrong:

```cpp
return head;
```

Correct:

```cpp
return newNode;
```

Because the head changes after insertion.

---

## Mistake 3: Confusing insertion at beginning with insertion at end

Beginning insertion never requires traversal.

---

# WHY I MIGHT FORGET THIS:

People usually forget:

```cpp
newNode->next = head;
```

because they focus only on creating the node.

Remember:

A linked list is connected using pointers.

If you do not connect the new node to the old head:

```text
old list becomes disconnected
```

Another common confusion:

```text
Why return newNode?
```

Because:

```text
the first node changed
```

so the head must also change.

---

# INTERVIEW FLOW:

## Step 1 — Explain intuition

“I need to insert a node at the beginning, so the new node becomes the new head.”

---

## Step 2 — Explain pointer connection

“I first connect the new node to the current head.”

```cpp
newNode->next = head;
```

---

## Step 3 — Update head

“Now the new node becomes the first node, so I return it.”

---

## Step 4 — Complexity

“This works in O(1) time because no traversal is required.”

---

# TIME COMPLEXITY:

## O(1)

Reason:

* no traversal
* only constant pointer updates
* only one node creation

Operations performed:

```text
create node
connect pointer
return node
```

All constant time.

---

# SPACE COMPLEXITY:

## O(1)

Reason:

Only one extra node is created.

No extra data structure is used.

---

# EDGE CASES:

## Case 1: Empty linked list

Input:

```text
head = NULL
x = 5
```

Output:

```text
5
```

Works because:

```cpp
newNode->next = NULL;
```

---

## Case 2: Single node list

Input:

```text
2
x = 1
```

Output:

```text
1 -> 2
```

---

## Case 3: Large linked list

Still works efficiently because no traversal occurs.

---

# PATTERN RECOGNITION:

You should think of this pattern whenever you see:

## Keywords

* insert at beginning
* add first node
* prepend node
* update head
* insert before current head

---

## Signals

If:

```text
head itself changes
```

then it is likely a:

```text
head manipulation problem
```

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
  
    Node* insertAtBegining(Node* head, int x) {
        
        // Create new node
        Node* newNode = new Node(x);
        
        // Connect new node to current head
        newNode->next = head;
        
        // Return new node as new head
        return newNode;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## Line 1

```cpp
Node* newNode = new Node(x);
```

Creates the new node that we want to insert.

---

## Line 2

```cpp
newNode->next = head;
```

Connects the new node to the old linked list.

Without this line:

```text
old list gets disconnected
```

---

## Line 3

```cpp
return newNode;
```

The first node changed.

So the head must also change.

---

# EASY-TO-REMEMBER SUMMARY

## Insertion at beginning = 3 steps

```text
Create
Connect
Return
```

or:

```text
newNode
   ↓
points to old head
   ↓
becomes new head
```

Always remember:

```cpp
newNode->next = head;
return newNode;
```
