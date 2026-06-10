
# PROBLEM:

Given the head of a singly linked list and an integer `n`, delete the nth node from the end of the linked list and return the updated head.

If `n` is greater than the length of the linked list, return `nullptr`.

---

## Example 1

```text
Input:
1 -> 2 -> 3 -> 4 -> 5
n = 2

Output:
1 -> 2 -> 3 -> 5
```

---

## Example 2

```text
Input:
7 -> 8 -> 4 -> 3 -> 2
n = 1

Output:
7 -> 8 -> 4 -> 3
```

---

## Example 3

```text
Input:
1 -> 2 -> 3
n = 5

Output:
nullptr
```

---

# PATTERN:

Fast & Slow Pointer Pattern

---

# WHY THIS PATTERN:

This pattern is used when:

- we need relative positioning inside a linked list
- one node depends on another node’s distance
- we need kth element from end
- we want one-pass optimization

Here:

- fast pointer moves `n` nodes ahead
- slow pointer follows behind
- when fast reaches end,
  slow automatically reaches the previous node of target

This avoids calculating length separately.

---

# CORE IDEA:

Maintain a gap of `n` nodes between:

```text
fast and slow
```

When fast reaches the last node:

```text
slow reaches the previous node of the node to delete
```

Then:

```cpp
slow->next = slow->next->next;
```

removes the target node.

---

# BRUTE FORCE:

## Intuition

Nth node from end can be converted into:

```text
(length - n + 1)th node from beginning
```

For deletion:

we need the previous node.

So previous node position becomes:

```text
length - n
```

---

## Steps

1. Count total nodes.
2. If `n > length`, return `nullptr`.
3. If `n == length`, delete head.
4. Find previous node.
5. Delete target node.

---

## Brute Force Code

```cpp
class Solution {
public:

    Node* removeNthFromEnd(Node* head, int n) {

        int count = 0;
        Node* temp = head;

        // Count total nodes
        while(temp != NULL) {
            count++;
            temp = temp->next;
        }

        // Invalid case
        if(n > count) {
            return nullptr;
        }

        // Delete head node
        if(n == count) {

            Node* newHead = head->next;

            delete head;

            return newHead;
        }

        // Previous node position
        int res = count - n;

        temp = head;

        // Move to previous node
        while(temp != NULL) {

            res--;

            if(res == 0) {
                break;
            }

            temp = temp->next;
        }

        // Delete target node
        Node* delNode = temp->next;

        temp->next = delNode->next;

        delete delNode;

        return head;
    }
};
```

---

## Brute Force Dry Run

Example:

```text
1 -> 2 -> 3 -> 4 -> 5
n = 2
```

### Count nodes

```text
count = 5
```

### Previous node position

```text
count - n = 5 - 2 = 3
```

Move temp to node 3.

Delete:

```text
temp->next = 4
```

After deletion:

```text
1 -> 2 -> 3 -> 5
```

---

# OPTIMAL APPROACH:

Use Fast & Slow pointers.

Move fast pointer `n` steps ahead.

Then move both together.

When fast reaches the last node:

```text
slow reaches previous node of target
```

Delete target node directly.

This avoids counting length separately.

---

# ALGORITHM:

## Step 1

Initialize:

```cpp
fast = head
slow = head
```

---

## Step 2

Move fast pointer `n` steps ahead.

---

## Step 3

If fast becomes NULL before completing `n` moves:

```text
n > length
```

Return:

```cpp
nullptr
```

---

## Step 4

If fast becomes NULL exactly after `n` moves:

```text
n == length
```

Delete head node.

---

## Step 5

Move both pointers together until:

```cpp
fast->next == NULL
```

Now:

```text
slow = previous node of target
```

---

## Step 6

Delete target node.

---

# DRY RUN:

## Example

```text
1 -> 2 -> 3 -> 4 -> 5
n = 2
```

---

## Initial State

```text
slow = 1
fast = 1
```

---

## Move Fast 2 Steps

### Move 1

```text
fast = 2
```

### Move 2

```text
fast = 3
```

Now:

```text
slow = 1
fast = 3
```

Gap:

```text
2 nodes
```

---

## Move Both Together

| slow | fast |
|------|------|
|1|3|
|2|4|
|3|5|

Stop because:

```cpp
fast->next == NULL
```

Now:

```text
slow = 3
```

Target node:

```text
slow->next = 4
```

Delete node 4.

Final list:

```text
1 -> 2 -> 3 -> 5
```

---

# IMPORTANT CODE SNIPPETS:

## Move fast n steps ahead

```cpp
for(int i = 0; i < n; i++) {

    if(fast == NULL) {
        return nullptr;
    }

    fast = fast->next;
}
```

---

## Delete head node

```cpp
if(fast == NULL) {

    Node* newHead = head->next;

    delete head;

    return newHead;
}
```

---

## Move both pointers together

```cpp
while(fast->next != NULL) {

    fast = fast->next;
    slow = slow->next;
}
```

---

## Delete target node safely

```cpp
Node* delNode = slow->next;

slow->next = delNode->next;

delete delNode;
```

---

# COMMON MISTAKES:

## 1. Mixing up problems

Students confuse:

- kth node from end
- remove nth node from end

---

## 2. Deleting before reconnecting

Wrong:

```cpp
delete delNode;
temp->next = temp->next->next;
```

Correct:

```cpp
temp->next = delNode->next;
delete delNode;
```

---

## 3. Forgetting head deletion case

When:

```text
n == length
```

head itself must be deleted.

---

## 4. Using wrong while condition

Wrong:

```cpp
while(fast != NULL)
```

Correct:

```cpp
while(fast->next != NULL)
```

because slow must stop at previous node.

---

## 5. Returning -1 in Node* function

Function returns:

```cpp
Node*
```

So invalid case should return:

```cpp
nullptr
```

NOT:

```cpp
-1
```

---

# WHY I MIGHT FORGET THIS:

Because this problem has multiple tricky cases:

- deleting head
- previous node logic
- maintaining pointer gap
- pointer movement synchronization
- deletion order

Most confusion happens because people focus on deletion instead of understanding:

```text
relative positioning between pointers
```

---

# INTERVIEW FLOW:

## Step 1

Explain brute force first.

```text
Count nodes
Find previous node
Delete target
```

This shows basic understanding.

---

## Step 2

Mention optimization.

```text
We can avoid counting separately using two pointers.
```

---

## Step 3

Explain fixed gap idea.

```text
Keep fast pointer n nodes ahead of slow.
```

---

## Step 4

Explain why slow reaches previous node automatically.

---

## Step 5

Discuss edge cases:

- n > length
- deleting head
- single node list

---

# TIME COMPLEXITY:

## Brute Force

### Counting nodes

```text
O(N)
```

### Reaching previous node

```text
O(N)
```

Total:

```text
O(2N)
```

Ignoring constants:

```text
O(N)
```

---

## Optimal

Single traversal:

```text
O(N)
```

---

# SPACE COMPLEXITY:

## Brute Force

Only pointers used:

```text
O(1)
```

---

## Optimal

Only pointers used:

```text
O(1)
```

---

# EDGE CASES:

## Case 1 — n > length

```text
1 -> 2 -> 3
n = 5
```

Return:

```cpp
nullptr
```

---

## Case 2 — Delete head node

```text
1 -> 2 -> 3
n = 3
```

Delete node 1.

---

## Case 3 — Delete last node

```text
1 -> 2 -> 3
n = 1
```

Delete node 3.

---

## Case 4 — Single node list

```text
1
n = 1
```

Result:

```text
empty list
```

---

# PATTERN RECOGNITION:

You should think of Fast & Slow pointers when:

- question mentions:
  - kth from end
  - nth from end
  - middle node
  - relative distance
  - one-pass linked list solution

Main clue:

```text
One pointer must maintain fixed distance from another.
```

That almost always indicates:

```text
Fast & Slow Pointer Pattern
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:

    Node* removeNthFromEnd(Node* head, int n) {

        Node* fast = head;
        Node* slow = head;

        // Move fast n steps ahead
        for(int i = 0; i < n; i++) {

            // Invalid case
            if(fast == NULL) {
                return nullptr;
            }

            fast = fast->next;
        }

        // Delete head node
        if(fast == NULL) {

            Node* newHead = head->next;

            delete head;

            return newHead;
        }

        // Move both pointers together
        while(fast->next != NULL) {

            fast = fast->next;
            slow = slow->next;
        }

        // Delete target node
        Node* delNode = slow->next;

        slow->next = delNode->next;

        delete delNode;

        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

## Initialize pointers

```cpp
Node* fast = head;
Node* slow = head;
```

Both start together.

---

## Move fast ahead

```cpp
fast = fast->next;
```

Creates gap of `n` nodes.

---

## Invalid case

```cpp
if(fast == NULL)
```

means:

```text
n > length
```

---

## Move together

```cpp
fast = fast->next;
slow = slow->next;
```

Maintains constant gap.

---

## Delete node

```cpp
slow->next = delNode->next;
```

Reconnects links safely.

---

# EASY-TO-REMEMBER SUMMARY

```text
1. Move fast n steps ahead.
2. Move fast and slow together.
3. When fast reaches end:
   slow reaches previous node.
4. Delete slow->next.
```

Core memory trick:

```text
Fixed gap between pointers automatically creates correct positioning.
```
````
