
## PROBLEM:

Given the head of a **sorted singly linked list**, remove all duplicate nodes such that every value appears exactly once.

Example:

```text
2 -> 2 -> 4 -> 5

Output:

2 -> 4 -> 5
```

Constraint:

- List is already sorted.
- Solve without extra space if possible.

---

## PATTERN:

**Linked List Traversal + In-Place Pointer Modification**

---

## WHY THIS PATTERN:

Since the linked list is sorted:

```text
1 -> 1 -> 2 -> 2 -> 2 -> 5
```

all duplicates appear consecutively.

This means we do not need a HashSet to remember previously seen values.

We can detect duplicates simply by comparing:

```cpp
curr->data
```

with

```cpp
curr->next->data
```

and modify pointers in-place.

---

## CORE IDEA:

If two adjacent nodes contain the same value:

```text
2 -> 2 -> 4
```

remove the second node by doing:

```cpp
curr->next = curr->next->next;
```

which becomes:

```text
2 -----> 4
```

If values are different:

```cpp
curr = curr->next;
```

Move forward.

---

## BRUTE FORCE:

### Idea

Store all previously seen values in a HashSet.

If the current value already exists in the set:

- Remove the node.

Otherwise:

- Insert value into the set.

### Code

```cpp
class Solution {
public:
    Node* removeDuplicates(Node* head) {

        if(head == NULL)
            return head;

        unordered_set<int> seen;

        Node* curr = head;
        Node* prev = NULL;

        while(curr != NULL) {

            if(seen.find(curr->data) != seen.end()) {
                prev->next = curr->next;
            }
            else {
                seen.insert(curr->data);
                prev = curr;
            }

            curr = curr->next;
        }

        return head;
    }
};
```

### Dry Run

Input:

```text
2 -> 2 -> 4 -> 5
```

Initially:

```text
seen = {}
```

Visit first 2:

```text
seen = {2}
```

Visit second 2:

```text
2 already exists
```

Remove it:

```text
2 -> 4 -> 5
```

Visit 4:

```text
seen = {2,4}
```

Visit 5:

```text
seen = {2,4,5}
```

Output:

```text
2 -> 4 -> 5
```

### Time Complexity

```text
O(N)
```

### Space Complexity

```text
O(N)
```

### Transition to Optimal

The HashSet is only needed when duplicates can occur anywhere.

But because the list is sorted:

```text
2 -> 2 -> 2 -> 4 -> 5 -> 5
```

duplicates are always adjacent.

Therefore we can remove duplicates by comparing neighboring nodes and eliminate the HashSet entirely.

---

## OPTIMAL APPROACH:

Use a single pointer:

```cpp
curr
```

Compare current node with next node.

If values are equal:

```cpp
curr->next = curr->next->next;
```

Otherwise:

```cpp
curr = curr->next;
```

Continue until the end.

---

## ALGORITHM:

1. Initialize:

```cpp
curr = head
```

2. Traverse while:

```cpp
curr != NULL && curr->next != NULL
```

3. If duplicate found:

```cpp
curr->data == curr->next->data
```

remove next node:

```cpp
curr->next = curr->next->next
```

4. Otherwise move ahead:

```cpp
curr = curr->next
```

5. Return head.

---

## DRY RUN:

Input:

```text
2 -> 2 -> 2 -> 4 -> 5
```

### Step 1

```text
curr
 ↓
2 -> 2 -> 2 -> 4 -> 5
```

Compare:

```text
2 == 2
```

Remove next node:

```text
2 -> 2 -> 4 -> 5
```

Keep curr at first 2.

---

### Step 2

```text
curr
 ↓
2 -> 2 -> 4 -> 5
```

Compare:

```text
2 == 2
```

Remove next node:

```text
2 -> 4 -> 5
```

Keep curr at first 2.

---

### Step 3

Compare:

```text
2 != 4
```

Move forward:

```text
2 -> 4 -> 5
     ^
    curr
```

---

### Step 4

Compare:

```text
4 != 5
```

Move forward:

```text
2 -> 4 -> 5
          ^
         curr
```

---

### Step 5

```text
curr->next == NULL
```

Stop.

Output:

```text
2 -> 4 -> 5
```

---

## IMPORTANT CODE SNIPPETS:

### Duplicate Detection

```cpp
if(curr->data == curr->next->data)
```

### Remove Duplicate

```cpp
curr->next = curr->next->next;
```

### Move Forward

```cpp
curr = curr->next;
```

### Traversal Condition

```cpp
while(curr != NULL && curr->next != NULL)
```

---

## COMMON MISTAKES:

### Mistake 1

Moving curr after deleting.

Wrong:

```cpp
curr->next = curr->next->next;
curr = curr->next;
```

Why wrong?

There may be more duplicates.

Example:

```text
2 -> 2 -> 2 -> 4
```

You would skip checking the third 2.

---

### Mistake 2

Using:

```cpp
while(curr != NULL)
```

Then accessing:

```cpp
curr->next->data
```

Can cause NULL pointer access.

---

### Mistake 3

Using extra HashSet even though list is sorted.

Unnecessary space usage.

---

## WHY I MIGHT FORGET THIS:

Because it looks like a duplicate-removal problem where HashSet is usually used.

The key observation is:

```text
SORTED LIST
=
DUPLICATES ARE ADJACENT
```

This removes the need for extra memory.

---

## INTERVIEW FLOW:

1. Since the list is sorted, duplicates occur consecutively.
2. Brute force would use a HashSet to track seen values.
3. But sorted property makes HashSet unnecessary.
4. Compare current node with next node.
5. If equal, remove next node.
6. Otherwise move forward.
7. Single traversal.
8. O(N) time and O(1) extra space.

---

## TIME COMPLEXITY:

### O(N)

Reason:

Each node is visited at most once during traversal.

Even when deleting nodes, we are only updating pointers.

No nested traversal.

---

## SPACE COMPLEXITY:

### O(1)

Reason:

Only one pointer:

```cpp
curr
```

is used.

No HashSet, array, recursion stack, or extra data structure.

---

## EDGE CASES:

### Single Node

```text
5
```

Output:

```text
5
```

---

### All Nodes Same

```text
2 -> 2 -> 2 -> 2
```

Output:

```text
2
```

---

### No Duplicates

```text
1 -> 2 -> 3 -> 4
```

Output:

```text
1 -> 2 -> 3 -> 4
```

---

### Duplicate At End

```text
1 -> 2 -> 3 -> 3
```

Output:

```text
1 -> 2 -> 3
```

---

## PATTERN RECOGNITION:

Look for this pattern when:

### Clue 1

Problem contains:

```text
Sorted Linked List
```

### Clue 2

Need to remove duplicates.

### Clue 3

Adjacent nodes can be compared directly.

### Clue 4

Question asks for:

```text
O(1) extra space
```

Immediate thought:

```text
Compare current node with next node
and modify pointers in-place.
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    Node* removeDuplicates(Node* head) {

        Node* curr = head;

        while (curr != NULL && curr->next != NULL) {

            if (curr->data == curr->next->data) {
                curr->next = curr->next->next;
            }
            else {
                curr = curr->next;
            }
        }

        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
Node* curr = head;
```

Start traversing from the first node.

---

```cpp
while (curr != NULL && curr->next != NULL)
```

We need both current and next node because we're comparing adjacent nodes.

---

```cpp
if (curr->data == curr->next->data)
```

Check if the next node is a duplicate.

---

```cpp
curr->next = curr->next->next;
```

Remove the duplicate node by skipping it.

---

```cpp
curr = curr->next;
```

Move ahead only when current and next values are different.

---

```cpp
return head;
```

Head remains unchanged, so return it.

---

# EASY-TO-REMEMBER SUMMARY

```text
Sorted Linked List
        ↓
Duplicates are adjacent
        ↓
Compare current with next
        ↓
Same?
    Yes → Remove next node
    No  → Move current
        ↓
One traversal
        ↓
O(N) Time, O(1) Space
```

### Memory Trick

> Sorted list means no HashSet. Just compare neighbors and skip duplicates.
````
