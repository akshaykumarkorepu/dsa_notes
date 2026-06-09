

# PROBLEM:

Given the head of a singly linked list, determine whether the linked list contains a loop (cycle) or not.

A loop exists if some node points back to a previous node instead of pointing to `NULL`.

---

# PATTERN:

# Fast and Slow Pointer Pattern (Floyd’s Cycle Detection Algorithm)

Two pointers move at different speeds:

- `slow` → moves 1 step
- `fast` → moves 2 steps

---

# WHY THIS PATTERN:

This pattern is used because:

- if no loop exists → `fast` eventually reaches `NULL`
- if a loop exists → `fast` eventually catches `slow`

This allows cycle detection in:

- Linear Time
- Constant Space

without using extra memory.

---

# CORE IDEA:

Imagine two runners on a circular track:

- one runs slowly
- one runs faster

The faster runner keeps gaining on the slower runner.

Since the track is circular and finite:

# the distance between slow and fast keeps decreasing by 1

Eventually:

# distance becomes 0

meaning both runners land on the same position.

That is exactly why:

```cpp
slow == fast
```

guarantees a loop exists.

---

# BRUTE FORCE:

## Idea

Store every visited node in a hash set.

While traversing:

- if node already exists in set → loop exists
- otherwise insert node and continue traversal

---

## Brute Force Code

```cpp
bool detectLoop(Node* head) {

    unordered_set<Node*> visited;

    Node* temp = head;

    while(temp != NULL) {

        if(visited.find(temp) != visited.end()) {
            return true;
        }

        visited.insert(temp);

        temp = temp->next;
    }

    return false;
}
```

---

## Brute Force Dry Run

### Example

```text
1 → 2 → 3 → 4
     ↑     ↓
     ← ← ←
```

Traversal:

```text
visited = {}

1 → insert 1
2 → insert 2
3 → insert 3
4 → insert 4
2 again → already present
```

So:

```cpp
return true
```

---

## Brute Force Time Complexity

```text
O(N)
```

### Reasoning

- each node is visited at most once
- hash set lookup takes average `O(1)`
- hash set insertion takes average `O(1)`

Total:

```text
O(N × 1) = O(N)
```

---

## Brute Force Space Complexity

```text
O(N)
```

### Reasoning

- in worst case, all nodes are stored in hash set
- if linked list has `N` nodes:

```text
visited = {all N nodes}
```

So extra memory becomes:

```text
O(N)
```

---

# OPTIMAL APPROACH:

# Floyd’s Cycle Detection Algorithm

Use two pointers:

- `slow` moves one step
- `fast` moves two steps

---

## Key Observation

### If NO loop exists

`fast` reaches:

```cpp
NULL
```

---

### If loop EXISTS

Both pointers enter the cycle.

Now:

- `slow` moves 1 step
- `fast` moves 2 steps

So:

# fast gains 1 node over slow every iteration

Because:

```text
2 - 1 = 1
```

Thus:

# the distance between them keeps decreasing by 1

Eventually:

# distance becomes 0

and both pointers meet.

That guarantees cycle detection.

---

# ALGORITHM:

## Step 1

Initialize:

```cpp
slow = head
fast = head
```

---

## Step 2

Traverse while:

```cpp
fast != NULL && fast->next != NULL
```

---

## Step 3

Move pointers:

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

## Step 4

Check:

```cpp
if(slow == fast)
```

If true:

```cpp
return true
```

---

## Step 5

If traversal ends naturally:

```cpp
return false
```

---

# DRY RUN:

# Example With Loop

```text
1 → 2 → 3 → 4 → 5
         ↑     ↓
         ← ← ←
```

Loop starts at node `3`.

---

## Initial State

```text
slow = 1
fast = 1
```

Distance = 0 initially.

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

Now both are inside the loop.

---

## Iteration 3

```text
slow = 4
fast = 4
```

Distance becomes:

```text
0
```

Pointers meet.

So:

```cpp
return true
```

---

# Example Without Loop

```text
1 → 2 → 3 → 4 → NULL
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
fast = NULL
```

Loop stops.

So:

```cpp
return false
```

---

# IMPORTANT CODE SNIPPETS:

## Safe Traversal Condition

```cpp
while(fast != NULL && fast->next != NULL)
```

Always remember this.

---

## Move Pointers

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

## Cycle Detection Condition

```cpp
if(slow == fast)
```

---

# COMMON MISTAKES:

## 1. Forgetting NULL Checks

Wrong:

```cpp
while(fast != NULL)
```

Correct:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## 2. Comparing Node Values Instead of Addresses

Wrong:

```cpp
if(slow->data == fast->data)
```

Correct:

```cpp
if(slow == fast)
```

Different nodes can have same value.

---

## 3. Moving Fast by One Step

Wrong:

```cpp
fast = fast->next;
```

This destroys the whole logic.

---

## 4. Using Extra Space in Optimal Solution

Optimal solution must use:

```text
O(1) space
```

---

# WHY I MIGHT FORGET THIS:

People usually get confused because:

- it feels “magical” that fast meets slow
- they don’t think in terms of relative distance

Remember this line:

# fast gains 1 node over slow every iteration

So:

# distance decreases by 1 every iteration

Eventually:

# distance becomes 0

and they must meet.

That is the whole intuition.

---

# INTERVIEW FLOW:

## Step 1

Start with intuition:

> “If a node is revisited, a cycle exists.”

---

## Step 2

Mention brute force:

> “We can store visited nodes in a hash set.”

Complexities:

```text
TC = O(N)
SC = O(N)
```

---

## Step 3

Optimize:

> “Instead of storing nodes, we can use two pointers moving at different speeds.”

---

## Step 4

Explain key intuition:

> “Inside the loop, fast gains one node over slow every iteration, so the distance between them decreases by 1 and eventually becomes 0.”

---

## Step 5

Explain complexities:

```text
TC = O(N)
SC = O(1)
```

---

# TIME COMPLEXITY:

# Optimal Solution

```text
O(N)
```

---

## Reasoning

Each node is processed at most once.

Pointers move continuously forward.

Even in loop case:

- pointers do not restart
- traversal remains linear
- meeting occurs within at most one cycle length after entering loop

Hence:

```text
O(N)
```

---

# SPACE COMPLEXITY:

# Optimal Solution

```text
O(1)
```

---

## Reasoning

Only two pointers used:

```cpp
slow
fast
```

No extra data structure used.

Hence constant space.

---

# EDGE CASES:

## 1. Empty List

```cpp
head == NULL
```

Return:

```cpp
false
```

---

## 2. Single Node Without Loop

```text
1 → NULL
```

Return:

```cpp
false
```

---

## 3. Single Node Pointing to Itself

```text
1 ↺
```

Return:

```cpp
true
```

---

## 4. Loop Starting at Head

```text
1 → 2 → 3
↑       ↓
← ← ← ←
```

Still works perfectly.

---

## 5. Very Large Linked List

Floyd’s algorithm remains efficient because:

```text
O(1) space
```

---

# PATTERN RECOGNITION:

You should think of Fast & Slow Pointer whenever:

- linked list has cycle/loop
- question asks:
  - detect cycle
  - find middle node
  - find cycle starting point
  - happy number
  - duplicate number using cycle logic
- different traversal speeds are useful
- constant space optimization is expected

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:

    bool detectLoop(Node* head) {

        Node* slow = head;
        Node* fast = head;

        while(fast != NULL && fast->next != NULL) {

            slow = slow->next;

            fast = fast->next->next;

            if(slow == fast) {
                return true;
            }
        }

        return false;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## Initialize Two Pointers

```cpp
Node* slow = head;
Node* fast = head;
```

Both start together.

---

## Safe Traversal

```cpp
while(fast != NULL && fast->next != NULL)
```

Needed because fast moves two steps.

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

Moves faster to reduce distance.

---

## Meeting Check

```cpp
if(slow == fast)
```

If both point to same node:

- cycle exists
- fast caught slow

---

# EASY-TO-REMEMBER SUMMARY

## Brute Force

- store visited nodes
- revisit means cycle
- `O(N)` space

---

## Optimal

- slow moves 1 step
- fast moves 2 steps

Inside loop:

# fast gains 1 node every iteration

So:

# distance decreases by 1

Eventually:

# distance becomes 0

Hence:

```cpp
slow == fast
```

which guarantees a cycle.
````
