
## PROBLEM:

Given a linked list where every node contains:

```cpp
next
random
```

Create a deep copy of the entire linked list.

The copied list must:

- Have completely new nodes.
- Preserve all `next` relationships.
- Preserve all `random` relationships.
- Not point to any node from the original list.
- Original list must remain unchanged.

---

## PATTERN:

**Linked List Cloning / Interweaving Nodes Pattern**

Related patterns:

- Deep Copy Data Structure
- HashMap Mapping Pattern
- In-place Linked List Modification
- Interweaving Nodes Technique

---

## WHY THIS PATTERN:

The challenge is not creating new nodes.

The challenge is:

```text
Original Random Node
        ↓
How do I find its Copy Node?
```

We need a mapping:

```text
Original Node → Copy Node
```

Brute force uses a HashMap.

Optimal solution stores this mapping inside the linked list itself by placing copy nodes immediately after original nodes.

---

## CORE IDEA:

### Brute Force

Store:

```text
Original Node → Copy Node
```

inside a HashMap.

Example:

```text
A → A'
B → B'
C → C'
```

Whenever we need:

```text
A.random = C
```

we get:

```text
A'.random = map[C]
```

---

### Optimal

Insert copy nodes inside the original list.

```text
A → A' → B → B' → C → C'
```

Now:

```text
Copy of A = A.next
Copy of B = B.next
Copy of C = C.next
```

The linked list itself becomes the mapping.

No HashMap needed.

---

## BRUTE FORCE:

### Idea

Use a HashMap to store:

```cpp
unordered_map<Node*, Node*> mp;
```

Mapping:

```text
Original Node → Copy Node
```

### Algorithm

#### Pass 1

Create all copied nodes.

```text
A → A'
B → B'
C → C'
```

Store mapping.

#### Pass 2

Assign:

```cpp
copy->next = map[original->next];
copy->random = map[original->random];
```

---

### Code

```cpp
Node* cloneLinkedList(Node* head)
{
    if(head == NULL)
        return NULL;

    unordered_map<Node*, Node*> mp;

    Node* temp = head;

    while(temp)
    {
        mp[temp] = new Node(temp->data);
        temp = temp->next;
    }

    temp = head;

    while(temp)
    {
        mp[temp]->next = mp[temp->next];
        mp[temp]->random = mp[temp->random];

        temp = temp->next;
    }

    return mp[head];
}
```

### Dry Run

Original:

```text
A → B → C

A.random → C
B.random → A
C.random → B
```

Pass 1:

```text
HashMap:

A → A'
B → B'
C → C'
```

Pass 2:

```text
A'.next = B'
A'.random = C'

B'.next = C'
B'.random = A'

C'.next = NULL
C'.random = B'
```

Final:

```text
A' → B' → C'
```

---

## OPTIMAL APPROACH:

### Idea

Instead of storing:

```text
A → A'
B → B'
C → C'
```

inside a HashMap,

store it directly inside the linked list.

Convert:

```text
A → B → C
```

into:

```text
A → A' → B → B' → C → C'
```

Now:

```text
Copy Node = Original Node → next
```

which gives instant access to every copied node.

---

## ALGORITHM:

### Step 1: Insert Copy Nodes Between Original Nodes

Convert:

```text
A → B → C
```

into:

```text
A → A' → B → B' → C → C'
```

---

### Step 2: Assign Random Pointers

For every original node:

```cpp
copyNode->random = temp->random->next;
```

Why?

Because:

```text
temp->random
```

gives original random node.

and

```text
temp->random->next
```

gives copy of that random node.

---

### Step 3: Extract Cloned List

Create:

```cpp
dummyNode
```

Build:

```text
dummy → A' → B' → C'
```

while restoring:

```text
A → B → C
```

simultaneously.

Return:

```cpp
dummyNode->next
```

---

## DRY RUN:

### Initial List

```text
A → B → C

A.random → C
B.random → A
C.random → B
```

---

### Step 1

Insert copies.

```text
A → A' → B → B' → C → C'
```

---

### Step 2

#### A

```text
A.random = C

A'.random = C.next = C'
```

#### B

```text
B.random = A

B'.random = A.next = A'
```

#### C

```text
C.random = B

C'.random = B.next = B'
```

Now:

```text
A' → B' → C'

A'.random → C'
B'.random → A'
C'.random → B'
```

---

### Step 3

Extract copied nodes.

Restore original:

```text
A → B → C
```

Build copied list:

```text
dummy → A' → B' → C'
```

Return:

```text
A'
```

---

## IMPORTANT CODE SNIPPETS:

### Insert Copy Nodes

```cpp
Node* copyNode = new Node(temp->data);

copyNode->next = temp->next;
temp->next = copyNode;

temp = temp->next->next;
```

---

### Assign Random Pointers

```cpp
Node* copyNode = temp->next;

if(temp->random)
{
    copyNode->random = temp->random->next;
}
```

---

### Restore Original List

```cpp
temp->next = temp->next->next;
```

---

### Build Clone List

```cpp
res->next = temp->next;
res = res->next;
```

---

## COMMON MISTAKES:

### Mistake 1

Writing:

```cpp
copyNode->random = temp->random;
```

This points copied nodes back into the original list.

Wrong.

---

### Mistake 2

Forgetting NULL check.

```cpp
temp->random->next
```

crashes when random is NULL.

Always:

```cpp
if(temp->random)
```

---

### Mistake 3

Using:

```cpp
temp = temp->next;
```

during Step 1 or Step 2.

After insertion:

```text
A → A' → B
```

This moves to A'.

Need:

```cpp
temp = temp->next->next;
```

to move to B.

---

### Mistake 4

Returning:

```cpp
head
```

instead of:

```cpp
dummyNode->next
```

---

## WHY I MIGHT FORGET THIS:

Because most students focus on:

```text
How do I copy nodes?
```

instead of:

```text
How do I find the copied version of a random node?
```

The entire problem is actually about maintaining:

```text
Original Node → Copy Node Mapping
```

Once that clicks, the solution becomes obvious.

---

## INTERVIEW FLOW:

### Step 1

State the challenge.

```text
The difficult part is random pointers because
I need a way to convert an original node
into its copied node.
```

### Step 2

Explain brute force.

```text
Use HashMap:

Original Node → Copy Node
```

Complexities:

```text
O(N) Time
O(N) Space
```

### Step 3

Optimize.

```text
Instead of storing mapping in a HashMap,
store it directly inside the linked list.
```

Insert copies:

```text
A → A' → B → B' → C → C'
```

Now:

```text
Copy Node = Original Node → next
```

### Step 4

Assign random pointers.

```cpp
copyNode->random = temp->random->next;
```

### Step 5

Separate copied list and restore original list.

---

## TIME COMPLEXITY:

### Brute Force

```text
Pass 1 → O(N)
Pass 2 → O(N)

Total = O(N)
```

### Optimal

```text
Step 1 → O(N)
Step 2 → O(N)
Step 3 → O(N)

Total = O(N)
```

### Reasoning

Each pass traverses the linked list exactly once.

```text
O(N) + O(N) + O(N)
= O(3N)
= O(N)
```

---

## SPACE COMPLEXITY:

### Brute Force

```text
HashMap = O(N)

Total Auxiliary Space = O(N)
```

### Optimal

Only pointers:

```text
temp
copyNode
dummyNode
res
```

No HashMap used.

Therefore:

```text
O(1)
```

Auxiliary Space.

---

## EDGE CASES:

### Empty List

```text
head = NULL
```

Return:

```text
NULL
```

---

### Single Node

```text
1
```

Works correctly.

---

### Random = NULL

```text
1.random = NULL
```

Handle using:

```cpp
if(temp->random)
```

---

### Random Points To Itself

```text
A.random → A
```

Then:

```text
A'.random → A'
```

Works automatically.

---

### Multiple Nodes Having Same Value

```text
5 → 5 → 5
```

Still works because we map using node addresses, not values.

---

## PATTERN RECOGNITION:

Think of this pattern whenever you see:

```text
Clone / Deep Copy
```

and the node contains:

```text
next
random
arb
bottom
child
extra pointer
```

Ask yourself:

```text
Do I need a mapping from Original Node
to Copied Node?
```

If yes:

### Approach 1

```text
HashMap Mapping
```

### Approach 2

```text
Can I store the mapping inside the linked list itself
by interweaving copied nodes?
```

That is the key observation behind the optimal solution.

---

## CLEAN C++ CODE

```cpp
class Solution {
public:
    Node* cloneLinkedList(Node* head) {

        if(head == NULL) return NULL;

        Node* temp = head;

        // Step 1: Insert copy nodes in between
        while(temp)
        {
            Node* copyNode = new Node(temp->data);

            copyNode->next = temp->next;
            temp->next = copyNode;

            temp = temp->next->next;
        }

        // Step 2: Assign random pointers
        temp = head;

        while(temp)
        {
            Node* copyNode = temp->next;

            if(temp->random)
            {
                copyNode->random = temp->random->next;
            }

            temp = temp->next->next;
        }

        // Step 3: Extract cloned list
        Node* dummyNode = new Node(-1);
        Node* res = dummyNode;

        temp = head;

        while(temp)
        {
            res->next = temp->next;
            res = res->next;

            temp->next = temp->next->next;

            temp = temp->next;
        }

        return dummyNode->next;
    }
};
```

---

## INTUITION BEHIND EVERY IMPORTANT LINE

### Insert copied node after original

```cpp
copyNode->next = temp->next;
temp->next = copyNode;
```

Transforms:

```text
A → B
```

into:

```text
A → A' → B
```

Creates the mapping:

```text
Original → Copy
```

without a HashMap.

---

### Move to next original node

```cpp
temp = temp->next->next;
```

Skips the copied node and moves directly to the next original node.

---

### Assign random pointer

```cpp
copyNode->random = temp->random->next;
```

Because:

```text
Original Random Node → next
```

is always the copied random node.

---

### Build cloned list

```cpp
res->next = temp->next;
res = res->next;
```

Adds copied nodes one by one into the cloned list.

---

### Restore original list

```cpp
temp->next = temp->next->next;
```

Transforms:

```text
A → A' → B
```

back into:

```text
A → B
```

---

### Return cloned head

```cpp
return dummyNode->next;
```

Skips dummy node and returns the actual head of the copied list.

---

## EASY-TO-REMEMBER SUMMARY

```text
STEP 1:
Insert copies in between.

A → B → C
↓
A → A' → B → B' → C → C'

STEP 2:
Assign random pointers.

copy.random = original.random.next

STEP 3:
Extract copied nodes.

Restore:
A → B → C

Build:
A' → B' → C'

Return copied head.
```

### Memory Trick

```text
HashMap Solution:
Original → Copy mapping stored in HashMap.

Optimal Solution:
Original → Copy mapping stored as Original.next.
```
````
