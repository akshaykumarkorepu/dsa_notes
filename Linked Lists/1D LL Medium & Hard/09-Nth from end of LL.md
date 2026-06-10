
# PROBLEM:
Find the kth node from the end of a Linked List.

Example:

```txt
1 -> 2 -> 3 -> 4 -> 5
k = 2
```

Answer:

```txt
4
```

because 4 is the 2nd node from the end.

---

# PATTERN:
Fast and Slow Pointer Pattern (Two Pointer Technique)

---

# WHY THIS PATTERN:
We need information relative to the end of the linked list.

Linked Lists do NOT allow backward traversal.

So instead of finding length first every time, we maintain a fixed gap between two pointers.

This allows us to directly reach the kth node from the end in one traversal.

---

# CORE IDEA:

## Brute Force Idea
1. Count total nodes
2. Convert kth-from-end position into kth-from-start position
3. Traverse again to that node

Formula:

```txt
Position from start = count - k
```

---

## Optimal Idea
Keep a gap of k nodes between fast and slow pointers.

When fast reaches NULL:

```txt
slow automatically reaches kth node from end
```

---

# BRUTE FORCE:

## Step 1: Count nodes

```cpp
while(temp != NULL){
    count++;
    temp = temp->next;
}
```

---

## Step 2: Invalid case

```cpp
if(k > count){
    return -1;
}
```

---

## Step 3: Find position from start

```cpp
int res = count - k;
```

---

## Step 4: Move to required node

```cpp
temp = head;

while(temp != NULL){

    if(res == 0) break;

    res--;

    temp = temp->next;
}
```

---

## Step 5: Return answer

```cpp
return temp->data;
```

---

## Brute Force Dry Run

Example:

```txt
1 -> 2 -> 3 -> 4 -> 5
k = 2
```

Length:

```txt
count = 5
```

Position from start:

```txt
res = count - k
    = 5 - 2
    = 3
```

Move 3 steps:

```txt
1 -> 2 -> 3 -> 4
               ^
```

Return:

```txt
4
```

---

## Brute Force Complexity

### Time Complexity

```txt
O(2N)
```

because:
- One traversal for counting
- One traversal for finding node

### Space Complexity

```txt
O(1)
```

---

# OPTIMAL APPROACH:

Use Fast and Slow pointers.

Move fast pointer k steps ahead first.

Then move both together.

When fast reaches NULL:

```txt
slow will be at kth node from end
```

---

# ALGORITHM:

## Step 1:
Initialize:

```cpp
Node* slow = head;
Node* fast = head;
```

---

## Step 2:
Move fast k steps ahead.

```cpp
for(int i=0;i<k;i++){

    if(fast==NULL){
        return -1;
    }

    fast = fast->next;
}
```

---

## Step 3:
Move both pointers together.

```cpp
while(fast!=NULL){

    slow = slow->next;
    fast = fast->next;
}
```

---

## Step 4:
Return answer.

```cpp
return slow->data;
```

---

# DRY RUN:

Example:

```txt
1 -> 2 -> 3 -> 4 -> 5
k = 2
```

---

## Step 1:
Initially:

```txt
slow = 1
fast = 1
```

---

## Step 2:
Move fast 2 steps ahead:

```txt
slow = 1
fast = 3
```

Gap between them:

```txt
2 nodes
```

---

## Step 3:
Move both together.

Iteration 1:

```txt
slow = 2
fast = 4
```

Iteration 2:

```txt
slow = 3
fast = 5
```

Iteration 3:

```txt
slow = 4
fast = NULL
```

Loop stops.

Answer:

```txt
slow = 4
```

Return:

```txt
4
```

---

# IMPORTANT CODE SNIPPETS:

## 1. Invalid Case

```cpp
if(fast == NULL){
    return -1;
}
```

Used when:

```txt
k > length
```

---

## 2. Maintaining k Distance

```cpp
fast = fast->next;
```

Move fast k steps first.

This creates the fixed gap.

---

## 3. Main Logic

```cpp
while(fast != NULL){

    slow = slow->next;
    fast = fast->next;
}
```

When fast reaches end:

```txt
slow reaches kth node from end
```

---

# DIFFERENCE BETWEEN:
# RETURNING KTH NODE
VS
# DELETING KTH NODE

THIS IS EXTREMELY IMPORTANT.

Most people confuse these two.

---

# DIFFERENCE 1:
# WHAT WE NEED

## Returning Problem

We need:

```txt
actual kth node
```

---

## Deleting Problem

We need:

```txt
previous node of kth node
```

because deletion requires:

```cpp
prev->next = target->next;
```

---

# DIFFERENCE 2:
# BRUTE FORCE DIFFERENCE

---

## RETURNING VALUE

Code:

```cpp
int res = count - k;

temp = head;

while(temp != NULL){

    if(res == 0) break;

    res--;

    temp = temp->next;
}
```

This stops ON the actual node.

---

## DELETING NODE

Code:

```cpp
int res = count - n;

temp = head;

while(temp != NULL){

    res--;

    if(res == 0){
        break;
    }

    temp = temp->next;
}
```

This stops on PREVIOUS node.

---

# WHY THIS DIFFERENCE HAPPENS

---

## Returning

We want:

```txt
1 -> 2 -> 3 -> 4 -> 5
               ^
```

So we stop directly on:

```txt
4
```

---

## Deleting

To delete 4:

```txt
1 -> 2 -> 3 -> 4 -> 5
          ^
```

We must stop on:

```txt
3
```

because we need:

```cpp
3->next = 5
```

---

# DIFFERENCE 3:
# HEAD EDGE CASE

---

## Returning Problem

NO special head case needed.

Why?

Because even if:

```txt
k == count
```

slow naturally reaches head.

No deletion happens.

No pointer modification happens.

---

## Deleting Problem

HEAD CASE IS MANDATORY.

Because:

```txt
head has NO previous node
```

---

# IMPORTANT HEAD DELETION CODE

```cpp
if(fast == NULL){

    Node* newHead = head->next;

    delete head;

    return newHead;
}
```

OR in brute force:

```cpp
if(n == count){

    Node* newHead = head->next;

    delete head;

    return newHead;
}
```

---

# WHY THIS IS NEEDED

Example:

```txt
1 -> 2 -> 3
n = 3
```

We must delete:

```txt
1
```

But:

```txt
1 has no previous node
```

So normal deletion logic fails.

That is why special handling is required.

---

# DIFFERENCE 4:
# OPTIMAL APPROACH DIFFERENCE

---

## RETURNING VALUE

Loop:

```cpp
while(fast != NULL)
```

Why?

We want slow to land ON target node.

---

## DELETING NODE

Loop:

```cpp
while(fast->next != NULL)
```

Why?

We want slow to stop ONE STEP BEFORE target node.

---

# VISUAL DIFFERENCE

---

## RETURNING

```txt
slow = target
```

---

## DELETING

```txt
slow = previous node
```

---

# COMMON MISTAKES:

## 1. Forgetting invalid case

```cpp
if(k > count)
```

or

```cpp
if(fast == NULL)
```

Without this:

```txt
NULL pointer access
```

can happen.

---

## 2. Confusing delete logic with return logic

People accidentally write:

```cpp
while(fast->next != NULL)
```

inside returning problem.

This gives previous node instead of actual node.

---

## 3. Forgetting head deletion case

Very common mistake.

---

## 4. Wrong placement of res--

Small placement change changes answer completely.

---

# WHY I MIGHT FORGET THIS:

Because both questions look almost identical.

But internally:

```txt
Returning:
Need target node

Deleting:
Need previous node
```

This single difference changes:
- loop condition
- stopping point
- edge cases

---

# INTERVIEW FLOW:

## Step 1:
Explain brute force.

```txt
Count length first
Convert kth-from-end into kth-from-start
Traverse again
```

---

## Step 2:
Mention optimization.

```txt
Can be done in one traversal using two pointers
```

---

## Step 3:
Explain fixed gap concept.

```txt
Maintain distance of k nodes
```

---

## Step 4:
Explain why slow reaches answer.

```txt
When fast reaches end,
slow automatically reaches kth node from end
```

---

## Step 5:
Mention edge cases.

```txt
k > length
Deleting head node
```

---

# TIME COMPLEXITY:

## Brute Force

```txt
O(2N)
```

Reason:
- First traversal for count
- Second traversal for answer

---

## Optimal

```txt
O(N)
```

Reason:
- Only one traversal

---

# SPACE COMPLEXITY:

Both:

```txt
O(1)
```

Reason:
Only pointers used.

No extra data structure.

---

# EDGE CASES:

## 1. k > length

Return:

```cpp
-1
```

or

```cpp
nullptr
```

depending on return type.

---

## 2. k == length

Returning:
- answer is head

Deleting:
- delete head specially

---

## 3. Single node list

```txt
1
k = 1
```

Returning:
- return 1

Deleting:
- return NULL

---

# PATTERN RECOGNITION:

Use Fast & Slow Pointer Pattern when:

## 1. Problem involves:
- kth from end
- middle node
- cycle detection
- relative positions

---

## 2. You see:

```txt
from end
```

Usually means:
- fixed distance pointers

---

## 3. Need one-pass solution

Two pointers are usually optimal.

---

# CLEAN C++ CODE

## Optimal Returning kth Element From End

```cpp
class Solution {
public:

    int getKthFromLast(Node* head, int k) {

        Node* slow = head;
        Node* fast = head;

        // Move fast k steps ahead
        for(int i = 0; i < k; i++) {

            // Invalid case
            if(fast == NULL) {
                return -1;
            }

            fast = fast->next;
        }

        // Move both pointers together
        while(fast != NULL) {

            slow = slow->next;
            fast = fast->next;
        }

        // slow reaches kth node from end
        return slow->data;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES

## This creates k distance

```cpp
fast = fast->next;
```

---

## This maintains same distance

```cpp
slow = slow->next;
fast = fast->next;
```

---

## This makes slow land exactly at answer

```cpp
while(fast != NULL)
```

---

# EASY-TO-REMEMBER SUMMARY

## Returning kth node

```txt
Need actual node
```

So:

```cpp
while(fast != NULL)
```

---

## Deleting kth node

```txt
Need previous node
```

So:

```cpp
while(fast->next != NULL)
```

---

# GOLDEN MEMORY TRICK

## RETURNING

```txt
slow stops ON target
```

---

## DELETING

```txt
slow stops BEFORE target
```

THAT is the entire difference.
````
