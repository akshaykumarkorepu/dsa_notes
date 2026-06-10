

## PROBLEM:
Given the head of a singly linked list, check whether the linked list is a palindrome or not.

A palindrome means:
- reads same from left to right
- and right to left

Examples:
- `1 -> 2 -> 3 -> 2 -> 1` → Palindrome
- `1 -> 2 -> 3 -> 4` → Not Palindrome

---

# PATTERN:
1. Fast & Slow Pointer
2. Reverse Linked List
3. Two Pointer Comparison

---

# WHY THIS PATTERN:
In a singly linked list:
- we can move only forward
- we cannot traverse backward

But palindrome checking needs:
- comparison from both ends

So we:
1. Find middle
2. Reverse second half
3. Compare first half with reversed second half

This gives:
- `O(N)` time
- `O(1)` extra space

---

# CORE IDEA:
Find the middle of the linked list.

Then:
- reverse the second half
- compare both halves node by node

If every node matches:
- palindrome

Else:
- not palindrome

---

# BRUTE FORCE:

## INTUITION:
A stack stores elements in reverse order because of LIFO.

So:
1. Push all nodes into stack
2. Traverse linked list again
3. Compare current node with stack top

This simulates:
- forward traversal
- reverse traversal

without needing backward pointers.

---

# BRUTE FORCE CODE:

```cpp
class Solution {
  public:
    bool isPalindrome(Node *head) {

        stack<int> st;

        Node* temp = head;

        // Push all nodes into stack
        while(temp) {
            st.push(temp->data);
            temp = temp->next;
        }

        temp = head;

        // Compare values
        while(temp) {

            if(temp->data != st.top())
                return false;

            st.pop();
            temp = temp->next;
        }

        return true;
    }
};
```

---

# BRUTE FORCE DRY RUN:

Input:
```text
1 -> 2 -> 3 -> 2 -> 1
```

Stack after insertion:

```text
Top
1
2
3
2
1
```

Now compare:
- `1` with `1`
- `2` with `2`
- `3` with `3`
- `2` with `2`
- `1` with `1`

All matched.

Answer = `true`

---

# BRUTE FORCE COMPLEXITY:

## Time Complexity:
`O(N)`
- one traversal for stack insertion
- one traversal for comparison

## Space Complexity:
`O(N)`
- stack stores all nodes

---

# OPTIMAL APPROACH:

## INTUITION:
We cannot move backward in a singly linked list.

So instead:
1. Find middle
2. Reverse second half
3. Compare both halves normally

---

# VERY IMPORTANT OBSERVATION:

To find FIRST middle in even length list:

We use:

```cpp
while(fast->next && fast->next->next)
```

Example:

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

slow stops at:

```text
3 ← first middle
```

---

If we used:

```cpp
while(fast && fast->next)
```

Then slow stops at:

```text
4 ← second middle
```

---

## Why first middle is needed?

Because:
`slow->next` should become start of second half.

Example:

```text
1 -> 2 -> 3 -> 4 -> 5 -> 6
```

slow = `3`

`slow->next`:

```text
4 -> 5 -> 6
```

Perfect second half.

---

# ALGORITHM:

## STEP 1:
Handle edge cases:
- empty list
- single node

Both are palindrome.

---

## STEP 2:
Find middle using:
- slow pointer
- fast pointer

slow moves 1 step  
fast moves 2 steps

---

## STEP 3:
Reverse second half starting from:
```cpp
slow->next
```

---

## STEP 4:
Compare:
- first half
- reversed second half

If mismatch occurs:
```cpp
return false;
```

Else:
```cpp
return true;
```

---

# FULL EXPLANATION OF REVERSE FUNCTION:

Suppose second half is:

```text
4 -> 5 -> 6
```

We want:

```text
6 -> 5 -> 4
```

---

# REVERSE CODE:

```cpp
Node* reverseList(Node* head) {

    Node* prev = NULL;
    Node* curr = head;

    while(curr) {

        Node* nextNode = curr->next;

        curr->next = prev;

        prev = curr;

        curr = nextNode;
    }

    return prev;
}
```

---

# UNDERSTANDING EACH POINTER:

## curr:
- current node being processed

## prev:
- head of already reversed part

## nextNode:
- stores future node
- prevents losing remaining list

---

# MOST IMPORTANT INTUITION:

At every iteration:
1. Save next node
2. Reverse current arrow
3. Move prev ahead
4. Move curr ahead

---

# REVERSAL VISUALIZATION:

Original:
```text
4 -> 5 -> 6
```

Reversed:
```text
NULL
```

Take `4`:

Original:
```text
5 -> 6
```

Reversed:
```text
4
```

Take `5`:

Original:
```text
6
```

Reversed:
```text
5 -> 4
```

Take `6`:

Original:
```text
NULL
```

Reversed:
```text
6 -> 5 -> 4
```

---

# WHY RETURN PREV?

At the end:
```cpp
curr = NULL
```

But `prev` stands at:
- new head of reversed list

Example:

```text
6 -> 5 -> 4
^
prev
```

So:
```cpp
return prev;
```

---

# DRY RUN:

## INPUT:
```text
1 -> 2 -> 3 -> 2 -> 1
```

---

# STEP 1: FIND MIDDLE

Initial:

```text
slow = 1
fast = 1
```

---

## Iteration 1:

```text
slow = 2
fast = 3
```

---

## Iteration 2:

```text
slow = 3
fast = 1
```

Loop stops.

Middle:
```text
3
```

---

# STEP 2: SECOND HALF

```cpp
slow->next
```

becomes:

```text
2 -> 1
```

---

# STEP 3: REVERSE SECOND HALF

Before reverse:

```text
2 -> 1
```

After reverse:

```text
1 -> 2
```

---

# STEP 4: COMPARE BOTH HALVES

First half:
```text
1 -> 2
```

Second half:
```text
1 -> 2
```

Compare:
- `1 == 1`
- `2 == 2`

All matched.

Answer = `true`

---

# IMPORTANT CODE SNIPPETS:

## FIND FIRST MIDDLE:

```cpp
while(fast->next && fast->next->next)
```

---

## START OF SECOND HALF:

```cpp
slow->next
```

---

## SAVE NEXT NODE BEFORE BREAKING LINK:

```cpp
Node* nextNode = curr->next;
```

---

## REVERSE LINK:

```cpp
curr->next = prev;
```

---

## NEW HEAD OF REVERSED LIST:

```cpp
return prev;
```

---

# COMMON MISTAKES:

## 1. Using wrong middle condition

Using:

```cpp
while(fast && fast->next)
```

returns second middle.

---

## 2. Forgetting to save next node

Without:

```cpp
nextNode = curr->next;
```

remaining list gets lost.

---

## 3. Comparing entire first half

We only compare till:

```cpp
secondHalf
```

---

## 4. Forgetting edge cases

- empty list
- single node

---

# WHY I MIGHT FORGET THIS:

Because this problem combines:
- fast & slow pointers
- reversal
- comparison

Students usually:
- understand concept
- but forget pointer movement during reversal

Main confusion:
- why save next node
- why return prev
- why `slow->next` is used

---

# INTERVIEW FLOW:

1. Explain palindrome requirement
2. Mention singly linked list cannot move backward
3. Give brute force stack approach
4. Mention optimization:
   “Can reduce `O(N)` space to `O(1)`”
5. Explain:
   - find middle
   - reverse second half
   - compare halves
6. Explain why first middle is needed
7. Dry run reversal carefully
8. State complexities

---

# TIME COMPLEXITY:

## Finding middle:
`O(N)`

## Reversing second half:
`O(N)`

## Comparing halves:
`O(N)`

## Total:
`O(N)`

Reason:
Each node is processed constant number of times.

---

# SPACE COMPLEXITY:

`O(1)`

Reason:
Only pointers are used:
- slow
- fast
- prev
- curr
- nextNode

No extra data structure used.

---

# EDGE CASES:

## 1. Empty List

```text
NULL
```

Palindrome

---

## 2. Single Node

```text
1
```

Palindrome

---

## 3. Two Same Nodes

```text
1 -> 1
```

Palindrome

---

## 4. Two Different Nodes

```text
1 -> 2
```

Not palindrome

---

## 5. Even Length Palindrome

```text
1 -> 2 -> 2 -> 1
```

---

## 6. Odd Length Palindrome

```text
1 -> 2 -> 3 -> 2 -> 1
```

---

# PATTERN RECOGNITION:

Use this pattern when:
- linked list requires comparison from both ends
- `O(1)` space optimization is expected
- reversal of second half can help
- singly linked list prevents backward traversal

Common questions using same pattern:
- Palindrome Linked List
- Reorder List
- Twin Sum of Linked List
- Reverse Second Half problems

---

# CLEAN C++ CODE:

```cpp
class Solution {
  public:

    Node* reverseList(Node* head) {

        Node* prev = NULL;
        Node* curr = head;

        while(curr) {

            Node* nextNode = curr->next;

            curr->next = prev;

            prev = curr;

            curr = nextNode;
        }

        return prev;
    }

    bool isPalindrome(Node *head) {

        // Empty list or single node
        if(head == NULL || head->next == NULL)
            return true;

        // Find first middle
        Node* slow = head;
        Node* fast = head;

        while(fast->next && fast->next->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Reverse second half
        Node* secondHalf = reverseList(slow->next);

        // Compare both halves
        Node* firstHalf = head;

        while(secondHalf) {

            if(firstHalf->data != secondHalf->data)
                return false;

            firstHalf = firstHalf->next;
            secondHalf = secondHalf->next;
        }

        return true;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES:

```cpp
while(fast->next && fast->next->next)
```

Finds FIRST middle.

---

```cpp
slow->next
```

Start of second half.

---

```cpp
Node* nextNode = curr->next;
```

Prevents losing remaining list.

---

```cpp
curr->next = prev;
```

Reverses arrow direction.

---

```cpp
return prev;
```

Returns new head of reversed list.

---

# EASY TO REMEMBER SUMMARY:

Palindrome in linked list:

1. Find first middle
2. Reverse second half
3. Compare both halves

Reverse logic:

1. Save next
2. Reverse link
3. Move prev
4. Move curr

Most important memory trick:

`prev` always represents:
“head of reversed part”

At end:
`prev` becomes new head of reversed list.
````
