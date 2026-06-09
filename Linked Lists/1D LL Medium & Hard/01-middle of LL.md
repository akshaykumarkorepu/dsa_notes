
# PROBLEM:

Given the head of a singly linked list, return the value of the middle node.

### Conditions:
- If the linked list contains an odd number of nodes → return the exact middle node.
- If the linked list contains an even number of nodes → return the second middle node.

### Example 1

```text
1 -> 2 -> 3 -> 4 -> 5
```

Middle = `3`

### Example 2

```text
2 -> 4 -> 6 -> 7 -> 5 -> 1
```

Middle nodes = `6` and `7`

Return = `7`

---

# PATTERN:

## Fast and Slow Pointer Pattern

Also called:
- Tortoise and Hare Algorithm

---

# WHY THIS PATTERN:

This problem requires finding the middle efficiently.

If:
- one pointer moves slowly
- another pointer moves twice as fast

then:
- when the fast pointer reaches the end,
- the slow pointer automatically reaches the middle.

This helps:
- avoid counting nodes separately
- solve the problem in one traversal
- reduce unnecessary traversal

---

# CORE IDEA:

Use two pointers:

- `slow` pointer → moves 1 step
- `fast` pointer → moves 2 steps

When fast reaches the end:
- slow reaches the middle.

---

# BRUTE FORCE:

## Idea:
- First count total nodes.
- Find middle position.
- Traverse again to reach that node.

This approach helps understand:
- why the optimal approach is better
- how we optimize from 2 traversals to 1 traversal

---

## Brute Force Steps:

### Step 1
Traverse the list and count total nodes.

### Step 2
Find middle position.

Using 1-based indexing:

```cpp
int midNode = count / 2 + 1;
```

### Step 3
Traverse again until reaching middle node.

---

## Brute Force Code

```cpp
class Solution {
  public:
    int getMiddle(Node* head) {
        
        int count = 0;
        Node* temp = head;
        
        // Count nodes
        while(temp != NULL){
            count++;
            temp = temp->next;
        }
        
        temp = head;
        
        // Find middle position
        int midNode = count / 2 + 1;
        
        // Reach middle node
        while(temp != NULL){
            
            midNode--;
            
            if(midNode == 0){
                break;
            }
            
            temp = temp->next;
        }
        
        return temp->data;
    }
};
```

---

## Brute Force Complexity

- Time → `O(N + N)`
- Space → `O(1)`

---

# OPTIMAL APPROACH:

Instead of counting nodes separately:

Use:
- slow pointer
- fast pointer

Move:
- slow by 1 step
- fast by 2 steps

By the time fast reaches the end:
- slow reaches the middle automatically.

This reduces:
- two traversals → one traversal

---

# ALGORITHM:

## Step 1

Initialize:

```cpp
Node* slow = head;
Node* fast = head;
```

---

## Step 2

Traverse while:

```cpp
fast != NULL && fast->next != NULL
```

### Why?
Because fast moves 2 steps.

---

## Step 3

Move pointers:

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

## Step 4

When loop ends:
- slow points to middle node.

Return:

```cpp
slow->data;
```

---

# DRY RUN:

# Example 1

```text
1 -> 2 -> 3 -> 4 -> 5
```

## Initial

```text
slow = 1
fast = 1
```

---

## Iteration 1

```text
slow = 2
fast = 3
```

---

## Iteration 2

```text
slow = 3
fast = 5
```

---

## Next

```text
fast->next = NULL
```

Loop stops.

### Answer

```text
3
```

---

# Example 2

```text
2 -> 4 -> 6 -> 7 -> 5 -> 1
```

## Initial

```text
slow = 2
fast = 2
```

---

## Iteration 1

```text
slow = 4
fast = 6
```

---

## Iteration 2

```text
slow = 6
fast = 5
```

---

## Iteration 3

```text
slow = 7
fast = NULL
```

Loop stops.

### Answer

```text
7
```

---

# IMPORTANT OBSERVATIONS:

1. Fast pointer always moves twice as fast.

2. When fast reaches end:
   - slow reaches middle.

3. This automatically gives second middle node in even-length lists.

4. No extra memory is used.

5. Only one traversal is required.

---

# IMPORTANT CODE SNIPPETS:

## Initialize pointers

```cpp
Node* slow = head;
Node* fast = head;
```

---

## Loop condition

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## Move pointers

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

## Return middle

```cpp
return slow->data;
```

---

# COMMON MISTAKES:

## 1. Using wrong loop condition

### Wrong

```cpp
while(fast != NULL)
```

### Correct

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## 2. Forgetting to check:
- `fast`
- `fast->next`

before moving 2 steps.

---

## 3. Returning first middle instead of second middle

This approach naturally returns second middle because both pointers start from head.

---

## 4. Returning node instead of node value

### Wrong

```cpp
return slow;
```

### Correct

```cpp
return slow->data;
```

---

# WHY I MIGHT FORGET THIS:

Because:
- fast pointer logic feels unintuitive initially
- students forget why fast moves 2 steps
- loop condition is easy to confuse
- first middle vs second middle creates confusion

---

## Memory Trick

```text
Fast reaches the finish line,
Slow reaches the middle.
```

---

# INTERVIEW FLOW:

## Step 1
Explain brute force first.

- Count total nodes
- Traverse again to middle

---

## Step 2
Mention inefficiency.

Observation:
We traverse the list twice.

---

## Step 3
Introduce optimization.

Can we find middle in one traversal?

---

## Step 4
Introduce fast & slow pointer pattern.

- slow moves 1 step
- fast moves 2 steps

---

## Step 5
Explain core observation.

When fast reaches end:
- slow becomes middle.

---

## Step 6
Discuss even-length case.

This approach automatically returns second middle node.

---

# TIME COMPLEXITY:

## `O(N)`

### Reason:
- only one traversal
- slow visits each node once
- fast skips nodes but overall traversal remains linear

---

# SPACE COMPLEXITY:

## `O(1)`

### Reason:
- only two pointers used
- no extra data structures used

---

# EDGE CASES:

## 1. Single node list

```text
1
```

Answer = `1`

---

## 2. Two node list

```text
1 -> 2
```

Answer = `2`

(second middle)

---

## 3. Odd length linked list

Works correctly.

---

## 4. Even length linked list

Returns second middle automatically.

---

## 5. Empty linked list

Usually not given in constraints,
but should be handled carefully in real interviews.

---

# PATTERN RECOGNITION:

Use Fast & Slow Pointer pattern when:

## 1. You need:
- middle node
- cycle detection
- nth node problems
- palindrome linked list problems

---

## 2. Question involves:
- linked list traversal
- different traversal speeds

---

## 3. You want:
- one-pass solution
- constant space optimization

---

# CLEAN C++ CODE:

```cpp
class Solution {
  public:
    int getMiddle(Node* head) {
        
        Node* slow = head;
        Node* fast = head;
        
        while(fast != NULL && fast->next != NULL){
            
            slow = slow->next;
            fast = fast->next->next;
        }
        
        return slow->data;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE:

## Initialize slow pointer

```cpp
Node* slow = head;
```

Slow pointer moves normally.

---

## Initialize fast pointer

```cpp
Node* fast = head;
```

Fast pointer moves twice as fast.

---

## Loop condition

```cpp
while(fast != NULL && fast->next != NULL)
```

Ensures fast can safely move 2 steps.

---

## Move slow pointer

```cpp
slow = slow->next;
```

Move slow by one node.

---

## Move fast pointer

```cpp
fast = fast->next->next;
```

Move fast by two nodes.

---

## Return answer

```cpp
return slow->data;
```

When fast reaches end:
- slow becomes middle.

---

# EASY-TO-REMEMBER SUMMARY:

## Brute Force
- Count nodes
- Traverse again to middle

---

## Optimal
- slow moves 1 step
- fast moves 2 steps

When fast reaches the end:
- slow reaches the middle

---

# MEMORY TRICK

```text
Fast finishes the race,
Slow reaches the center.
```
````
