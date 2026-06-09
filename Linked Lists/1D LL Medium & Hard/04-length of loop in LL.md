
# PROBLEM:

Given the head of a linked list, determine whether the linked list contains a loop.

- If no loop exists → return `0`
- If loop exists → return the number of nodes present inside the loop

Example:

```text
1 → 2 → 3 → 4 → 5
          ↑     ↓
          ← ← ←
```

Loop:

```text
3 → 4 → 5 → 3
```

Loop length = `3`

---

# PATTERN:

```text
Fast & Slow Pointer Pattern (Floyd’s Cycle Detection Algorithm)
```

---

# WHY THIS PATTERN:

This problem involves:

- cycle detection
- repeated traversal inside linked list
- loop identification without extra space

Fast & Slow pointers are ideal because:

- slow moves 1 step
- fast moves 2 steps

If a loop exists:
- fast eventually catches slow

If no loop exists:
- fast reaches NULL

This gives:
- O(N) time
- O(1) space

---

# CORE IDEA:

The problem has 2 parts:

## Part 1 — Detect Loop

Use:
- slow pointer
- fast pointer

If they meet:

```text
loop exists
```

---

## Part 2 — Count Loop Length

Once both meet:

- keep one pointer fixed
- move another pointer one step at a time
- count nodes until we return to same node

That count = loop length

---

# BRUTE FORCE:

## Idea

Use a Hash Set to store visited node addresses.

While traversing:
- if node is already present in set:
  - loop detected
- then start counting loop nodes

---

## Why This Helps

This helps understand:
- how loops repeat nodes
- why revisiting same node means cycle
- transition toward O(1) space solution

---

## Brute Force Code

```cpp
class Solution {
  public:

    int countNodesinLoop(Node *head) {

        unordered_set<Node*> visited;

        Node* temp = head;

        while(temp != NULL) {

            // Loop detected
            if(visited.find(temp) != visited.end()) {

                Node* start = temp;

                int count = 1;

                temp = temp->next;

                while(temp != start) {
                    count++;
                    temp = temp->next;
                }

                return count;
            }

            visited.insert(temp);

            temp = temp->next;
        }

        return 0;
    }
};
```

---

## Brute Force Dry Run

Linked List:

```text
1 → 2 → 3 → 4 → 5
          ↑     ↓
          ← ← ←
```

Visited traversal:

```text
{1}
{1,2}
{1,2,3}
{1,2,3,4}
{1,2,3,4,5}
```

After node 5:

```text
5 → 3
```

Node 3 already exists in set.

Loop detected.

Now count:

```text
3 → 4 → 5 → 3
```

Nodes counted:
- 3
- 4
- 5

Answer:

```text
3
```

---

## Brute Force Complexity

### Time Complexity

```text
O(N)
```

### Space Complexity

```text
O(N)
```

because hash set stores visited nodes.

---

# OPTIMAL APPROACH:

Use Floyd’s Cycle Detection Algorithm.

Steps:

## Step 1
Detect cycle using:
- slow pointer
- fast pointer

## Step 2
Once they meet:
- start from meeting point
- move until same node appears again
- count nodes

---

# ALGORITHM:

## Detect Loop

```cpp
while(fast != NULL && fast->next != NULL)
```

Move:
- slow by 1
- fast by 2

If:

```cpp
slow == fast
```

loop exists.

---

## Count Loop Length

Suppose meeting point is node X.

Start:

```text
count = 1
temp = temp->next
```

Then:

```cpp
while(temp != meetingPoint)
```

Keep:
- incrementing count
- moving temp

When temp again becomes meetingPoint:
- one full cycle completed

Return count.

---

# DRY RUN:

Linked List:

```text
1 → 2 → 3 → 4 → 5
          ↑     ↓
          ← ← ←
```

Loop:

```text
3 → 4 → 5 → 3
```

---

## Initial State

```text
slow = 1
fast = 1
```

---

## Iteration 1

Slow:

```text
1 → 2
```

Fast:

```text
1 → 2 → 3
```

Now:

```text
slow = 2
fast = 3
```

---

## Iteration 2

Slow:

```text
2 → 3
```

Fast:

```text
3 → 4 → 5
```

Now:

```text
slow = 3
fast = 5
```

---

## Iteration 3

Slow:

```text
3 → 4
```

Fast:

```text
5 → 3 → 4
```

Now:

```text
slow = 4
fast = 4
```

Loop detected.

---

## Count Loop Length

Meeting point:

```text
4
```

Traversal:

```text
4 → 5 → 3 → 4
```

Count:
- 4
- 5
- 3

Answer:

```text
3
```

---

# IMPORTANT CODE SNIPPETS:

## Floyd Detection

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

## Count Loop

```cpp
int count = 1;

temp = temp->next;

while(temp != meetingPoint) {
    count++;
    temp = temp->next;
}
```

---

## Function Call

```cpp
return countLoopLength(slow);
```

Meaning:

```text
Start counting loop nodes from where slow is standing.
```

---

# COMMON MISTAKES:

## 1. Forgetting NULL checks

Wrong:

```cpp
fast = fast->next->next;
```

without checking:

```cpp
fast != NULL && fast->next != NULL
```

Causes segmentation fault.

---

## 2. Using node values instead of node addresses

Wrong:

```cpp
unordered_set<int>
```

Values can repeat normally.

Always store:

```cpp
Node*
```

---

## 3. Starting count from 0

Loop counting should start from:

```cpp
count = 1
```

because meeting node itself belongs to loop.

---

## 4. Forgetting to move temp before while loop

Wrong:

```cpp
while(temp != meetingPoint)
```

immediately fails if temp already equals meetingPoint.

Correct:

```cpp
temp = temp->next;
```

before loop.

---

# WHY I MIGHT FORGET THIS:

Because there are 2 separate concepts:

## Concept 1
Detecting loop

## Concept 2
Counting loop length

Many people understand:

```text
slow == fast
```

but forget:

```text
how to count nodes after detection
```

Key memory trick:

```text
Start from meeting point and count how many moves are needed to return back again.
```

---

# INTERVIEW FLOW:

## Step 1
Explain brute force using hashing.

Mention:
- revisiting same node means loop
- uses extra space

---

## Step 2
Optimize using Floyd’s Algorithm.

Explain:
- slow moves 1 step
- fast moves 2 steps
- fast eventually catches slow inside cycle

---

## Step 3
After meeting:
- traverse cycle once
- count nodes

---

## Step 4
Discuss complexity.

---

# TIME COMPLEXITY:

## Detecting Loop

```text
O(N)
```

because pointers traverse linked list.

---

## Counting Loop

```text
O(L)
```

where:

```text
L = loop size
```

Overall:

```text
O(N)
```

because:

```text
L ≤ N
```

---

# SPACE COMPLEXITY:

## Brute Force

```text
O(N)
```

due to hash set.

---

## Optimal

```text
O(1)
```

No extra data structure used.

---

# EDGE CASES:

## 1. Empty Linked List

```text
head = NULL
```

Return:

```text
0
```

---

## 2. Single Node Without Loop

```text
1 → NULL
```

Return:

```text
0
```

---

## 3. Single Node With Self Loop

```text
1 → 1
```

Return:

```text
1
```

---

## 4. Entire Linked List Is Loop

```text
1 → 2 → 3 → 1
```

Return:

```text
3
```

---

# PATTERN RECOGNITION:

You should think of Floyd’s Algorithm when:

- linked list mentions:
  - cycle
  - loop
  - repeated traversal
  - circular structure

OR asks:
- detect cycle
- find cycle length
- find cycle starting node

Keywords:

```text
loop
cycle
circular
revisiting node
```

Usually indicates:

```text
Fast & Slow Pointer Pattern
```

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
  
    int countLoopLength(Node* meetingPoint) {
        
        Node* temp = meetingPoint;
        
        int count = 1;
        
        temp = temp->next;
        
        while(temp != meetingPoint) {
            count++;
            temp = temp->next;
        }
        
        return count;
    }
  
    int countNodesinLoop(Node *head) {
        
        Node* slow = head;
        Node* fast = head;
        
        while(fast != NULL && fast->next != NULL) {
            
            slow = slow->next;
            
            fast = fast->next->next;
            
            // Loop detected
            if(slow == fast) {
                return countLoopLength(slow);
            }
        }
        
        // No loop
        return 0;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES

## Initialize Two Pointers

```cpp
Node* slow = head;
Node* fast = head;
```

Both start from beginning.

---

## Move Slow Pointer

```cpp
slow = slow->next;
```

Moves 1 step.

---

## Move Fast Pointer

```cpp
fast = fast->next->next;
```

Moves 2 steps.

---

## Detect Loop

```cpp
if(slow == fast)
```

Same node reached again inside cycle.

---

## Count Loop Nodes

```cpp
while(temp != meetingPoint)
```

Move until we return to starting point again.

---

## Return Count

```cpp
return count;
```

Total nodes inside cycle.

---

# EASY-TO-REMEMBER SUMMARY

```text
1. Use slow-fast pointers to detect cycle.
2. If both meet, loop exists.
3. Start from meeting point.
4. Move until same node appears again.
5. Count moves.
6. That count = loop length.
```
````
