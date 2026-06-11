

## PROBLEM:

You are given two linked lists where each node contains a digit.

For LeetCode Add Two Numbers:

```text
2 -> 4 -> 3
5 -> 6 -> 4
```

represents:

```text
342
465
```

because digits are stored in reverse order.

Return:

```text
7 -> 0 -> 8
```

which represents:

```text
807
```

---

## PATTERN:

**Linked List + Carry Propagation + Dummy Node**

---

## WHY THIS PATTERN:

Whenever a problem involves:

```text
Digit-by-digit addition
Carry handling
Two linked lists traversed together
```

we naturally process digits one position at a time while carrying overflow to the next position.

A dummy node helps build the answer list without special handling for the first node.

---

## CORE IDEA:

Treat each node as a digit.

At every step:

```text
sum = digit1 + digit2 + carry
```

Store:

```text
digit = sum % 10
```

Carry forward:

```text
carry = sum / 10
```

Continue until:

```text
Both lists end
AND
No carry remains
```

---

## BRUTE FORCE:

### Idea

Convert both linked lists into numbers.

Example:

```text
2 -> 4 -> 3
```

becomes:

```text
342
```

Then:

```text
342 + 465 = 807
```

Convert answer back into linked list:

```text
7 -> 0 -> 8
```

### Why It Fails

If the linked list contains many digits:

```text
9 -> 9 -> 9 -> 9 -> ...
```

the number may exceed integer limits.

The optimal solution avoids constructing the entire number.

### Time Complexity

```text
O(n + m)
```

### Space Complexity

```text
O(max(n,m))
```

for answer list.

---

## OPTIMAL APPROACH:

Traverse both linked lists simultaneously.

For every position:

```text
Add digit from l1
Add digit from l2
Add previous carry
```

Create a new node containing:

```text
sum % 10
```

Store next carry:

```text
sum / 10
```

Build answer using a dummy node.

---

## ALGORITHM:

### Step 1

Create:

```cpp
dummy
curr
carry = 0
```

### Step 2

Traverse while:

```cpp
while(l1 || l2 || carry)
```

### Step 3

Initialize:

```cpp
sum = carry
```

### Step 4

If l1 exists:

```cpp
sum += l1->val
```

Move l1.

### Step 5

If l2 exists:

```cpp
sum += l2->val
```

Move l2.

### Step 6

Update carry:

```cpp
carry = sum / 10
```

### Step 7

Create digit:

```cpp
sum % 10
```

Append to answer.

### Step 8

Move curr.

### Step 9

Return:

```cpp
dummy->next
```

---

## DRY RUN:

Input:

```text
l1 = 2 -> 4 -> 3
l2 = 5 -> 6 -> 4
```

### Iteration 1

```text
2 + 5 + 0 = 7

digit = 7
carry = 0
```

Answer:

```text
7
```

### Iteration 2

```text
4 + 6 + 0 = 10

digit = 0
carry = 1
```

Answer:

```text
7 -> 0
```

### Iteration 3

```text
3 + 4 + 1 = 8

digit = 8
carry = 0
```

Answer:

```text
7 -> 0 -> 8
```

Loop ends.

Return:

```text
7 -> 0 -> 8
```

---

## IMPORTANT CODE SNIPPETS:

### Loop Condition

```cpp
while(l1 || l2 || carry)
```

Handles:

```text
Different lengths
Final carry
```

### Carry Calculation

```cpp
carry = sum / 10;
```

Example:

```text
15 / 10 = 1
```

### Current Digit

```cpp
sum % 10
```

Example:

```text
15 % 10 = 5
```

### Building Answer

```cpp
curr->next = new ListNode(sum % 10);
curr = curr->next;
```

---

## COMMON MISTAKES:

### Forgetting carry in loop condition

Wrong:

```cpp
while(l1 || l2)
```

Correct:

```cpp
while(l1 || l2 || carry)
```

### Forgetting to move curr

Wrong:

```cpp
curr->next = node;
```

Correct:

```cpp
curr->next = node;
curr = curr->next;
```

### Returning dummy

Wrong:

```cpp
return dummy;
```

Correct:

```cpp
return dummy->next;
```

### Ignoring unequal lengths

One list may end earlier.

Always check:

```cpp
if(l1)
if(l2)
```

before using values.

---

## WHY I MIGHT FORGET THIS:

Most people focus on linked lists and forget:

```text
This is fundamentally a carry propagation problem.
```

Think:

```text
Elementary school addition
+
Linked List traversal
```

and the solution becomes obvious.

---

## INTERVIEW FLOW:

### Observation

Digits are stored in reverse order.

So:

```text
Units digit comes first.
```

Perfect for addition.

### Idea

Traverse both lists simultaneously.

Maintain carry.

For every position:

```text
digit1 + digit2 + carry
```

Store digit.

Pass carry forward.

### Data Structure

Use:

```text
Dummy Node
```

to build answer cleanly.

### Finish

Continue until:

```text
Both lists exhausted
AND
carry becomes 0
```

Return:

```cpp
dummy->next
```

---

## TIME COMPLEXITY:

```text
O(max(n,m))
```

### Reason

Every node from both linked lists is visited exactly once.

No nested traversal.

---

## SPACE COMPLEXITY:

```text
O(max(n,m))
```

### Reason

The answer linked list itself may contain:

```text
max(n,m)+1
```

nodes.

Ignoring output list:

```text
Auxiliary Space = O(1)
```

---

## EDGE CASES:

### Different Lengths

```text
2 -> 4 -> 3
5 -> 6
```

### Final Carry

```text
9
+
1
=
10
```

Need an extra node.

### One List Longer

```text
9 -> 9 -> 9
1
```

### Zero Values

```text
0
+
0
=
0
```

---

## PATTERN RECOGNITION:

Look for:

```text
Two linked lists
Digit operations
Addition/Subtraction
Carry/Borrow
Numbers represented as lists
```

Keywords:

```text
Add Two Numbers
Sum of Linked Lists
Huge Numbers
Digit-wise arithmetic
```

Immediate thought should be:

```text
Dummy Node
+
Carry Propagation
+
Simultaneous Traversal
```

---

# Clean C++ Code

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        ListNode* dummy = new ListNode(0);
        ListNode* curr = dummy;

        int carry = 0;

        while (l1 || l2 || carry) {

            int sum = carry;

            if (l1) {
                sum += l1->val;
                l1 = l1->next;
            }

            if (l2) {
                sum += l2->val;
                l2 = l2->next;
            }

            carry = sum / 10;

            curr->next = new ListNode(sum % 10);
            curr = curr->next;
        }

        return dummy->next;
    }
};
```

# Intuition Behind Every Important Line

### Dummy Node

```cpp
ListNode* dummy = new ListNode(0);
```

Avoids special handling for first answer node.

### Current Pointer

```cpp
ListNode* curr = dummy;
```

Always points to last node of answer list.

### Carry

```cpp
int carry = 0;
```

Stores overflow from previous digit.

### Loop

```cpp
while(l1 || l2 || carry)
```

Continue while work remains.

### Sum

```cpp
int sum = carry;
```

Always start with previous carry.

### Carry Update

```cpp
carry = sum / 10;
```

Extract overflow.

### Current Digit

```cpp
sum % 10
```

Extract digit to store.

### Move Current

```cpp
curr = curr->next;
```

Prepare for next insertion.

### Return Answer

```cpp
return dummy->next;
```

Skip dummy node.

---

# Easy-to-Remember Summary

```text
Digits are already reversed.
↓
Create dummy node.
↓
Traverse both lists together.
↓
sum = digit1 + digit2 + carry
↓
Store sum % 10
↓
carry = sum / 10
↓
Move pointers
↓
Repeat until l1, l2 and carry are exhausted.
↓
Return dummy->next
```
````
