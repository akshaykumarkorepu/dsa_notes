
# PROBLEM:

Given a singly linked list, delete the middle node and return the modified head.

Important conditions:

- If the list has even number of nodes:
  - delete the **second middle**
- If only one node exists:
  - return `NULL`

Examples:

```text
1 -> 2 -> 3 -> 4 -> 5
Output:
1 -> 2 -> 4 -> 5
```

```text
2 -> 4 -> 6 -> 7 -> 5 -> 1
Output:
2 -> 4 -> 6 -> 5 -> 1
```

---

# PATTERN:

## Fast and Slow Pointer Pattern (Tortoise & Hare)

Pointers used:

- `slow` → moves 1 step
- `fast` → moves 2 steps
- `prev` → stores node before `slow`

---

# WHY THIS PATTERN:

This pattern helps us:

- Find middle node in one traversal
- Avoid counting nodes separately
- Achieve:
  - `O(N)` time
  - `O(1)` space

Also:

Using:

```cpp
while(fast != NULL && fast->next != NULL)
```

automatically places `slow` on:

```text
SECOND middle node
```

for even-sized linked lists.

Exactly what the question requires.

---

# CORE IDEA:

Move:

- `slow` by 1
- `fast` by 2

When `fast` reaches the end:

```text
slow reaches middle
```

Store previous node using:

```cpp
prev = slow;
```

before moving `slow`.

Then delete middle node using:

```cpp
prev->next = slow->next;
```

---

# BRUTE FORCE:

## Why Learn Brute Force?

Useful because:

- interviewer may expect progression
- helps understand optimization
- transition to fast/slow becomes easier

---

## Brute Force Idea

1. Count total nodes
2. Find middle index using:
   ```cpp
   count / 2
   ```
3. Traverse again to node before middle
4. Delete middle node

---

## Brute Force Code

```cpp
class Solution {
  public:
    Node* deleteMid(Node* head) {
        
        if(head == NULL || head->next == NULL){
            return NULL;
        }
        
        int count = 0;
        Node* temp = head;
        
        // Count nodes
        while(temp != NULL){
            count++;
            temp = temp->next;
        }
        
        int mid = count / 2;
        
        temp = head;
        
        // Move to node before middle
        while(mid > 1){
            temp = temp->next;
            mid--;
        }
        
        Node* nodeToDelete = temp->next;
        
        temp->next = temp->next->next;
        
        delete nodeToDelete;
        
        return head;
    }
};
```

---

## Brute Force Dry Run

Input:

```text
1 -> 2 -> 3 -> 4 -> 5
```

Count:

```text
5
```

Middle:

```text
5 / 2 = 2
```

Move to node before middle:

```text
temp reaches 2
```

Delete:

```text
3
```

Final:

```text
1 -> 2 -> 4 -> 5
```

---

# OPTIMAL APPROACH:

Use:

- slow pointer
- fast pointer
- previous pointer

in a single traversal.

---

# ALGORITHM:

## Step 1

Handle edge case:

```cpp
if(head == NULL || head->next == NULL)
    return NULL;
```

---

## Step 2

Initialize pointers:

```cpp
Node* slow = head;
Node* fast = head;
Node* prev = NULL;
```

---

## Step 3

Move pointers:

```cpp
while(fast != NULL && fast->next != NULL)
```

Inside loop:

```cpp
prev = slow;
slow = slow->next;
fast = fast->next->next;
```

---

## Step 4

Delete middle node:

```cpp
prev->next = slow->next;
delete slow;
```

---

## Step 5

Return head.

---

# DRY RUN:

# Odd Length Example

Input:

```text
1 -> 2 -> 3 -> 4 -> 5
```

---

## Initial State

```text
prev = NULL
slow = 1
fast = 1
```

---

## Iteration 1

```text
prev = 1
slow = 2
fast = 3
```

---

## Iteration 2

```text
prev = 2
slow = 3
fast = 5
```

Next move impossible because:

```text
fast->next == NULL
```

Loop stops.

---

## Current Position

```text
prev = 2
slow = 3
```

Delete:

```text
3
```

Final list:

```text
1 -> 2 -> 4 -> 5
```

---

# Even Length Example

Input:

```text
2 -> 4 -> 6 -> 7 -> 5 -> 1
```

---

## Initial

```text
slow = 2
fast = 2
```

---

## Iteration 1

```text
prev = 2
slow = 4
fast = 6
```

---

## Iteration 2

```text
prev = 4
slow = 6
fast = 5
```

---

## Iteration 3

```text
prev = 6
slow = 7
fast = NULL
```

Loop stops.

Delete:

```text
7
```

Final list:

```text
2 -> 4 -> 6 -> 5 -> 1
```

---

# IMPORTANT CODE SNIPPETS:

## Finding Middle

```cpp
while(fast != NULL && fast->next != NULL){
    
    prev = slow;
    slow = slow->next;
    fast = fast->next->next;
}
```

---

## Deleting Middle Node

```cpp
prev->next = slow->next;
delete slow;
```

---

## Single Node Edge Case

```cpp
if(head == NULL || head->next == NULL){
    return NULL;
}
```

---

# COMMON MISTAKES:

## 1. Forgetting previous pointer

Without `prev`, we cannot delete middle node in singly linked list.

---

## 2. Wrong loop condition

Wrong:

```cpp
while(fast->next != NULL)
```

Correct:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## 3. Forgetting single node case

This causes runtime errors.

---

## 4. Deleting wrong middle in even length

Some solutions accidentally delete first middle.

Question specifically asks for:

```text
SECOND middle
```

---

## 5. Doing:

```cpp
delete slow;
```

before reconnecting links.

Always reconnect first.

---

# WHY I MIGHT FORGET THIS:

Because:

- deleting needs previous node
- slow/fast logic feels tricky initially
- even-length behavior is confusing
- pointer movement order matters

Easy memory trick:

```text
prev follows slow
slow follows middle
fast follows end
```

---

# INTERVIEW FLOW:

## Step 1

State brute force:

```text
Count nodes, traverse again, delete middle.
```

---

## Step 2

Mention optimization:

```text
Can we find middle in one traversal?
```

---

## Step 3

Introduce fast & slow pointers.

Explain:

```text
fast moves 2 steps
slow moves 1 step
```

---

## Step 4

Mention need for previous pointer.

---

## Step 5

Explain even-length handling:

```text
This condition automatically gives second middle.
```

---

## Step 6

Write clean code carefully.

---

# TIME COMPLEXITY:

# Brute Force

## Time Complexity

```text
O(N) + O(N) = O(N)
```

Two traversals.

---

## Space Complexity

```text
O(1)
```

No extra space.

---

# Optimal Approach

## Time Complexity

```text
O(N)
```

Single traversal.

Each node visited at most once.

---

## Space Complexity

```text
O(1)
```

Only pointers used.

---

# SPACE COMPLEXITY:

## Brute Force

```text
O(1)
```

---

## Optimal

```text
O(1)
```

No additional data structures used.

---

# EDGE CASES:

## 1. Single Node

```text
7
```

Return:

```text
NULL
```

---

## 2. Two Nodes

```text
1 -> 2
```

Delete second middle:

```text
2
```

Result:

```text
1
```

---

## 3. Odd Length

```text
1 -> 2 -> 3 -> 4 -> 5
```

Delete:

```text
3
```

---

## 4. Even Length

```text
1 -> 2 -> 3 -> 4
```

Middle nodes:

```text
2 and 3
```

Delete:

```text
3
```

(second middle)

---

# PATTERN RECOGNITION:

Use Fast & Slow Pointer when problem says:

- middle of linked list
- detect cycle
- nth node from end
- split linked list
- palindrome linked list
- cycle start
- remove middle
- find midpoint

Recognition clue:

```text
If one pointer can move faster than another,
the slower pointer often reaches an important position.
```

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
    Node* deleteMid(Node* head) {
        
        // Edge case: empty or single node
        if(head == NULL || head->next == NULL){
            return NULL;
        }
        
        Node* slow = head;
        Node* fast = head;
        Node* prev = NULL;
        
        // Find middle node
        while(fast != NULL && fast->next != NULL){
            
            prev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }
        
        // Delete middle node
        prev->next = slow->next;
        
        delete slow;
        
        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## Edge Case

```cpp
if(head == NULL || head->next == NULL)
```

If only one node exists, deleting middle means deleting whole list.

---

## Slow Pointer

```cpp
slow = slow->next;
```

Moves normally.

---

## Fast Pointer

```cpp
fast = fast->next->next;
```

Moves twice as fast so slow reaches middle.

---

## Previous Pointer

```cpp
prev = slow;
```

Stores node before middle.

Needed because singly linked list cannot move backward.

---

## Delete Logic

```cpp
prev->next = slow->next;
```

Skips middle node.

---

## Free Memory

```cpp
delete slow;
```

Removes node properly.

---

# EASY-TO-REMEMBER SUMMARY

```text
1. Use slow and fast pointers
2. fast moves 2 steps
3. slow moves 1 step
4. prev tracks node before slow
5. when fast reaches end:
      slow = middle
6. delete slow using prev
```

Memory shortcut:

```text
Fast reaches END
Slow reaches MIDDLE
Prev helps DELETE
```
````
