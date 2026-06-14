
## PROBLEM:

Given the head of a singly linked list and an integer `k`, reverse every group of `k` nodes and return the modified list.

There are **two common versions** of this problem:

### GFG Version

Reverse every group, including the final group even if its size is less than `k`.

Example:

```text
1 → 2 → 3 → 4 → 5
k = 3

Output:
3 → 2 → 1 → 5 → 4
```

---

### LeetCode Version

Reverse only complete groups of size `k`.

If the final group contains fewer than `k` nodes, leave it unchanged.

Example:

```text
1 → 2 → 3 → 4 → 5
k = 3

Output:
3 → 2 → 1 → 4 → 5
```

---

## PATTERN:

**Linked List Reversal + Recursion**

---

## WHY THIS PATTERN:

The problem naturally breaks into smaller identical subproblems.

For any group:

1. Reverse first `k` nodes.
2. Remaining list is again:
   "Reverse nodes in groups of `k`."

Whenever we see:

```text
Solve first part
+
Apply same logic on remaining part
```

Recursion becomes a natural fit.

The reversal itself uses the standard linked list reversal pattern:

```text
prev
curr
next
```

---

## CORE IDEA:

Each recursive call handles exactly **ONE group**.

For every call:

1. Reverse first `k` nodes.
2. Recursively process remaining nodes.
3. Connect current group to recursive result.
4. Return new head of current group.

Think:

```text
Reverse Current Group
+
Solve Remaining Groups
```

---

## BRUTE FORCE:

### Idea

1. Store all nodes in a vector.
2. Reverse vector elements in groups of size `k`.
3. Reconnect pointers.

Example:

```text
1 → 2 → 3 → 4 → 5 → 6
```

Store:

```text
[1,2,3,4,5,6]
```

Groups:

```text
[1,2]
[3,4]
[5,6]
```

Reverse:

```text
[2,1]
[4,3]
[6,5]
```

Reconnect all nodes.

### Code

```cpp
Node* reverseKGroup(Node* head, int k) {

    vector<Node*> nodes;

    Node* temp = head;

    while(temp){
        nodes.push_back(temp);
        temp = temp->next;
    }

    for(int i=0;i<nodes.size();i+=k){

        int l=i;
        int r=min(i+k-1,(int)nodes.size()-1);

        while(l<r){
            swap(nodes[l],nodes[r]);
            l++;
            r--;
        }
    }

    for(int i=0;i<nodes.size()-1;i++){
        nodes[i]->next = nodes[i+1];
    }

    nodes.back()->next = NULL;

    return nodes[0];
}
```

### Time Complexity

```text
O(N)
```

Why?

- One traversal to store nodes → O(N)
- One traversal to reverse groups → O(N)
- One traversal to reconnect → O(N)

Total:

```text
O(N + N + N)
= O(N)
```

### Space Complexity

```text
O(N)
```

Why?

Vector stores all nodes.

```text
N nodes stored
⇒ O(N) extra space
```

---

## OPTIMAL APPROACH:

Use standard linked list reversal on exactly `k` nodes.

After reversal:

```text
prev = new head

head = new tail

curr = start of next group
```

Recursively solve the remaining list and connect it.

---

## ALGORITHM:

### Step 1

Handle empty list.

```cpp
if(head == NULL)
    return NULL;
```

---

### Step 2

Initialize reversal pointers.

```cpp
curr = head
prev = NULL
next = NULL
```

---

### Step 3

Reverse exactly `k` nodes.

```cpp
while(curr != NULL && count < k)
```

Standard reversal:

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

---

### Step 4

After reversal:

```text
prev = head of reversed group

head = tail of reversed group

curr = start of next group
```

---

### Step 5

Recursively solve remaining list.

```cpp
head->next = reverseKGroup(next,k);
```

---

### Step 6

Return new head.

```cpp
return prev;
```

---

### LeetCode Variation

Before reversing, check whether `k` nodes exist.

```cpp
Node* temp = head;

for(int i=0;i<k;i++){

    if(temp == NULL)
        return head;

    temp = temp->next;
}
```

If fewer than `k` nodes remain:

```cpp
return head;
```

Leave group unchanged.

---

## DRY RUN:

### Input

```text
1 → 2 → 3 → 4 → 5 → 6
k = 2
```

---

### First Call

Reverse:

```text
1 → 2
```

After reversal:

```text
2 → 1
```

Remaining:

```text
3 → 4 → 5 → 6
```

Call:

```cpp
1->next = reverseKGroup(3,2);
```

---

### Second Call

Reverse:

```text
3 → 4
```

After reversal:

```text
4 → 3
```

Remaining:

```text
5 → 6
```

Call:

```cpp
3->next = reverseKGroup(5,2);
```

---

### Third Call

Reverse:

```text
5 → 6
```

After reversal:

```text
6 → 5
```

Remaining:

```text
NULL
```

Return:

```text
6
```

---

### Backtracking

Third call returns:

```text
6 → 5
```

Second call becomes:

```text
4 → 3 → 6 → 5
```

Returns:

```text
4
```

First call becomes:

```text
2 → 1 → 4 → 3 → 6 → 5
```

Final Answer:

```text
2 → 1 → 4 → 3 → 6 → 5
```

---

## IMPORTANT OBSERVATIONS:

### Observation 1

After reversal:

Before:

```text
1 → 2
```

After:

```text
2 → 1
```

`head` still points to `1`.

Now `1` is the tail.

Therefore:

```cpp
head->next = ...
```

works perfectly.

---

### Observation 2

After reversal:

```text
prev
 ↓
2 → 1
```

`prev` always points to the new head.

Therefore:

```cpp
return prev;
```

---

### Observation 3

Every recursive call handles ONLY ONE GROUP.

Never think:

```text
This function reverses the entire list.
```

Think:

```text
This function reverses one group.
```

Recursion handles remaining groups.

---

### Observation 4

Difference between GFG and LeetCode:

GFG:

```text
Reverse incomplete final group.
```

LeetCode:

```text
Leave incomplete final group unchanged.
```

Only the availability check changes.

---

## IMPORTANT CODE SNIPPETS:

### Standard Reversal

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

---

### Recursive Connection

```cpp
head->next = reverseKGroup(next,k);
```

---

### Return New Head

```cpp
return prev;
```

---

### LeetCode Availability Check

```cpp
Node* temp = head;

for(int i=0;i<k;i++){

    if(temp == NULL)
        return head;

    temp = temp->next;
}
```

---

## COMMON MISTAKES:

### 1. Returning head instead of prev

Wrong:

```cpp
return head;
```

`head` becomes tail after reversal.

---

### 2. Forgetting recursive connection

Wrong:

```cpp
reverseKGroup(next,k);
```

Groups become disconnected.

---

### 3. Forgetting next pointer

Wrong:

```cpp
curr->next = prev;
curr = curr->next;
```

Remaining list gets lost.

---

### 4. Returning inside if block

Wrong:

```cpp
if(next != NULL)
    return prev;
```

Last group won't return anything.

---

### 5. Missing availability check in LeetCode version

Final incomplete group gets reversed incorrectly.

---

## WHY I MIGHT FORGET THIS:

Because two concepts happen together:

1. Reversal
2. Recursion

Most confusion disappears when you remember:

```text
One function call = One group
```

Recursion handles remaining groups.

---

## INTERVIEW FLOW:

**Interviewer:** Explain your approach.

**Answer:**

"I treat the linked list as groups of size k.

For each group, I perform a standard linked list reversal using prev, curr and next.

After reversing k nodes, the remaining list is the same problem again, so I recursively solve it.

The original head becomes the tail after reversal, so I connect it to the recursive result using head->next.

Finally, I return prev because it becomes the new head of the reversed group."

### Follow-up

**What if the last group has fewer than k nodes?**

Answer:

"If it's the GFG version, I reverse it as well.

If it's the LeetCode version, I first check whether k nodes are available. If not, I return the current head unchanged."

---

## TIME COMPLEXITY:

### Recursive Solution

```text
O(N)
```

### Why?

Every node is visited exactly once.

Example:

```text
1 → 2 → 3 → 4 → 5 → 6
```

Node 1 processed once.

Node 2 processed once.

...

Node N processed once.

No node is revisited.

Therefore:

```text
O(N)
```

---

## SPACE COMPLEXITY:

### Recursive Solution

Number of groups:

```text
N / k
```

Each group creates one recursive call.

Therefore recursion stack:

```text
O(N/k)
```

### Example

```text
N = 100
k = 10
```

Groups:

```text
10
```

Recursion depth:

```text
10
```

Space:

```text
O(N/k)
```

---

### Worst Case

When:

```text
k = 1
```

Every node becomes a separate recursive call.

Depth:

```text
N
```

Space:

```text
O(N)
```

Therefore:

```text
Recursive Space = O(N/k)

Worst Case = O(N)
```

---

## EDGE CASES:

### Empty List

```text
NULL
```

Return NULL.

---

### Single Node

```text
1
```

Return 1.

---

### k = 1

No change.

---

### k = size of list

Entire list gets reversed.

---

### Final group size < k

GFG:

```text
Reverse it.
```

LeetCode:

```text
Leave it unchanged.
```

---

## PATTERN RECOGNITION:

Think of this pattern whenever:

1. Linked list reversal happens repeatedly in chunks.
2. Problem mentions groups of k nodes.
3. First part is processed and same operation repeats on remaining nodes.

Examples:

- Reverse Nodes in K Group
- Reverse Alternate K Nodes
- Reverse Linked List II
- Reverse Linked List in Groups

### Recognition Signal

If you see:

```text
Do something on first k nodes
and
Repeat on remaining nodes
```

Think:

```text
Linked List Reversal + Recursion
```

---

# CLEAN C++ CODE (GFG VERSION)

```cpp
class Solution {
public:

    Node *reverseKGroup(Node *head, int k) {

        if(head == NULL)
            return NULL;

        Node* curr = head;
        Node* prev = NULL;
        Node* next = NULL;

        int count = 0;

        while(curr != NULL && count < k){

            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;

            count++;
        }

        if(next != NULL){
            head->next = reverseKGroup(next,k);
        }

        return prev;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
Node* curr = head;
```

Start processing from current group's head.

---

```cpp
Node* prev = NULL;
```

Initially no nodes have been reversed.

---

```cpp
next = curr->next;
```

Save remaining list before changing pointers.

---

```cpp
curr->next = prev;
```

Reverse current link.

---

```cpp
prev = curr;
```

Expand reversed portion.

---

```cpp
curr = next;
```

Move to next node.

---

```cpp
head->next = reverseKGroup(next,k);
```

Attach current reversed group to recursively processed remainder.

---

```cpp
return prev;
```

`prev` always points to the new head after reversal.

---

# EASY-TO-REMEMBER SUMMARY

One recursive call handles one group.

After reversing k nodes:

```text
prev = new head
head = new tail
curr = next group
```

Then:

```cpp
head->next = reverseKGroup(curr,k);
return prev;
```

Memory Trick:

```text
Reverse
→ Connect
→ Return
```

1. Reverse k nodes
2. Connect tail to recursion
3. Return new head (prev)
````
