
# PROBLEM:

Given the head of a singly linked list, return the first node of the loop if a cycle exists, otherwise return `-1`.

---

# PATTERN:

# Fast & Slow Pointer (Floyd’s Cycle Detection Algorithm)

Also known as:

```text
Tortoise and Hare Algorithm
```

---

# WHY THIS PATTERN:

This pattern is used because:

- Linked lists do not allow backward traversal
- We need cycle detection without extra space
- Fast and slow pointers help detect loops efficiently
- The same pattern can:
  - detect cycle
  - find cycle length
  - find starting node of cycle

This gives:

```text
O(N) time
O(1) space
```

which is optimal.

---

# CORE IDEA:

There are TWO phases.

---

## Phase 1 → Detect Cycle

- slow moves 1 step
- fast moves 2 steps

If a loop exists:

```text
fast will eventually catch slow
```

like runners on a circular track.

If they meet:

```text
cycle exists
```

---

## Phase 2 → Find First Node of Loop

After first meeting:

- move slow back to head
- keep fast at meeting point

Now move BOTH one step at a time.

The node where they meet again is:

```text
starting node of loop
```

---

# INTUITION OF WHY SECOND MEETING GIVES LOOP START

This is the MOST important understanding.

Suppose:

```text
1 -> 2 -> 3 -> 4 -> 5
          ^         |
          |_________|
```

Loop starts at `3`.

Assume slow and fast first meet at `4`.

Now:

```text
slow = head = 1
fast = 4
```

Move both one step at a time.

---

## Move 1

```text
slow = 2
fast = 5
```

---

## Move 2

```text
slow = 3
fast = 3
```

They meet at loop start.

---

## WHY DOES THIS HAPPEN?

The first collision creates a very special alignment.

At the moment of first meeting:

```text
distance(head → loop start)
=
distance(meeting point → loop start)
```

So when:

- one pointer starts from head
- one starts from meeting point

and both move equally,

they MUST collide at loop start.

---

## Simple Real-Life Intuition

Imagine:

- one person starts outside a circular track
- another starts somewhere inside the track

Because fast pointer already completed extra laps earlier,

both people become perfectly aligned with respect to the loop entrance.

So walking together makes them meet exactly at the entrance.

That entrance is:

```text
starting node of loop
```

---

# BRUTE FORCE:

## Idea

Store all visited nodes inside a HashSet.

If a node is visited again:

```text
that repeated node is the starting point of loop
```

---

# WHY DOES THIS WORK?

Traversal order inside loop becomes:

```text
3 -> 4 -> 5 -> 3 -> 4 -> 5 ...
```

The FIRST repeated node is always:

```text
starting node of cycle
```

---

# Brute Force Code

```cpp
class Solution {
  public:
    int findFirstNode(Node* head) {

        unordered_set<Node*> visited;

        Node* temp = head;

        while(temp != NULL){

            // Node already visited
            if(visited.find(temp) != visited.end()){
                return temp->data;
            }

            visited.insert(temp);

            temp = temp->next;
        }

        return -1;
    }
};
```

---

# Brute Force Dry Run

Linked list:

```text
1 -> 2 -> 3 -> 4 -> 5
          ^         |
          |_________|
```

Visited order:

```text
1, 2, 3, 4, 5, 3
```

First repeated node:

```text
3
```

So loop starts at `3`.

---

# OPTIMAL APPROACH:

Use Floyd’s Cycle Detection Algorithm.

---

# ALGORITHM:

## Step 1 → Initialize

```cpp
slow = head
fast = head
```

---

## Step 2 → Detect Loop

Move:

```text
slow -> 1 step
fast -> 2 steps
```

If:

```cpp
slow == fast
```

then cycle exists.

---

## Step 3 → Reset Slow

Move:

```cpp
slow = head
```

Keep fast at meeting point.

---

## Step 4 → Move Both Equally

Move:

```cpp
slow = slow->next
fast = fast->next
```

The node where they meet again is:

```text
first node of loop
```

---

# DRY RUN:

## Example

```text
1 -> 2 -> 3 -> 4 -> 5
          ^         |
          |_________|
```

Loop starts at `3`.

---

# Phase 1 → Detect Cycle

## Initial

```text
slow = 1
fast = 1
```

---

## Move 1

```text
slow = 2
fast = 3
```

---

## Move 2

```text
slow = 3
fast = 5
```

---

## Move 3

```text
slow = 4
fast = 4
```

They meet.

Cycle exists.

---

# Phase 2 → Find Loop Start

Reset:

```text
slow = 1
fast = 4
```

---

## Move 1

```text
slow = 2
fast = 5
```

---

## Move 2

```text
slow = 3
fast = 3
```

They meet at `3`.

Answer:

```text
3
```

---

# IMPORTANT OBSERVATIONS:

1. First meeting point is NOT necessarily loop start.

2. Resetting one pointer to head is the key trick.

3. Both pointers must move ONE step during second phase.

4. This works ONLY because fast moved twice as fast earlier.

5. We compare node addresses, NOT node values.

---

# IMPORTANT CODE SNIPPETS:

## Detect Cycle

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## Move Pointers

```cpp
slow = slow->next;
fast = fast->next->next;
```

---

## Detect Meeting

```cpp
if(slow == fast)
```

---

## Find Loop Start

```cpp
slow = head;

while(slow != fast){
    slow = slow->next;
    fast = fast->next;
}
```

---

# COMMON MISTAKES:

## 1. Forgetting NULL Checks

Wrong:

```cpp
while(fast)
```

Correct:

```cpp
while(fast != NULL && fast->next != NULL)
```

---

## 2. Returning temp Instead of temp->data

If function return type is:

```cpp
int
```

return:

```cpp
temp->data
```

NOT:

```cpp
temp
```

---

## 3. Comparing Values Instead of Nodes

Wrong:

```cpp
slow->data == fast->data
```

Correct:

```cpp
slow == fast
```

Because different nodes can contain same value.

---

## 4. Forgetting Second Phase

Some students stop after first collision.

But first collision only confirms cycle.

It does NOT give loop start directly.

---

## 5. Moving One Pointer Faster in Second Phase

During second phase:

```text
BOTH must move one step
```

---

# WHY I MIGHT FORGET THIS:

Because students usually memorize:

```cpp
slow = head;
```

without understanding WHY.

The real understanding is:

```text
meeting point and head become equally far from loop start
```

That is the hidden intuition.

---

# INTERVIEW FLOW:

## Step 1

Explain brute force using HashSet.

---

## Step 2

Mention optimization:

```text
Can we solve without extra space?
```

---

## Step 3

Introduce Floyd’s Cycle Detection.

---

## Step 4

Explain first meeting intuition:

```text
fast eventually catches slow inside cycle
```

---

## Step 5

Explain second meeting intuition:

```text
head → loop start
=
meeting point → loop start
```

---

## Step 6

Write clean code.

---

## Step 7

Dry run clearly.

---

# TIME COMPLEXITY:

# Brute Force

## Time Complexity

```text
O(N)
```

Reason:

Every node visited once.

HashSet operations are average:

```text
O(1)
```

---

## Space Complexity

```text
O(N)
```

Reason:

HashSet stores visited nodes.

---

# Optimal Approach

## Time Complexity

```text
O(N)
```

Reason:

- First phase traverses list once
- Second phase traverses at most one more pass

Still linear overall.

---

## Space Complexity

```text
O(1)
```

Reason:

Only pointers used.

No extra data structures.

---

# EDGE CASES:

## 1. No Loop

```text
1 -> 2 -> 3 -> NULL
```

Return:

```text
-1
```

---

## 2. Single Node Loop

```text
1 -> 1
```

Works correctly.

---

## 3. Loop Starts At Head

```text
1 -> 2 -> 3
^         |
|_________|
```

Still works.

---

## 4. Two Node Cycle

```text
1 -> 2
     ^ |
     |_|
```

Works correctly.

---

# PATTERN RECOGNITION:

Use Fast & Slow Pointer when:

- linked list has cycle/loop
- repeated traversal pattern exists
- question asks:
  - detect loop
  - find loop length
  - find loop start
  - middle node
  - happy number
  - duplicate number using cycle logic

Key signals:

```text
“without extra space”
“cycle”
“loop”
“circular traversal”
```

usually indicate Floyd’s Algorithm.

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
    int findFirstNode(Node* head) {

        Node* slow = head;
        Node* fast = head;

        // Step 1: Detect Cycle
        while(fast != NULL && fast->next != NULL){

            slow = slow->next;
            fast = fast->next->next;

            // Cycle detected
            if(slow == fast){

                // Step 2: Find starting node
                slow = head;

                while(slow != fast){
                    slow = slow->next;
                    fast = fast->next;
                }

                return slow->data;
            }
        }

        // No cycle
        return -1;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES

## Fast Pointer Moves Two Steps

```cpp
fast = fast->next->next;
```

Purpose:

```text
make fast catch slow inside cycle
```

---

## Collision Check

```cpp
if(slow == fast)
```

Purpose:

```text
detect cycle existence
```

---

## Reset Slow

```cpp
slow = head;
```

Purpose:

```text
align both pointers toward loop start
```

---

## Move Equally

```cpp
slow = slow->next;
fast = fast->next;
```

Purpose:

```text
both pointers now become synchronized toward cycle start
```

---

# EASY-TO-REMEMBER SUMMARY

## Floyd Cycle Detection

### Phase 1

```text
slow = 1 step
fast = 2 steps
```

If they meet:

```text
cycle exists
```

---

### Phase 2

```text
move slow to head
move both one step
```

Where they meet again:

```text
starting node of loop
```

---

# ONE-LINE MEMORY TRICK

```text
First meeting detects cycle.
Second meeting finds cycle start.
```
````
