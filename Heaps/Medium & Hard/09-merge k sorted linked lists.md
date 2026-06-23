

# PROBLEM:

You are given **K sorted linked lists**.

Merge them into **one sorted linked list**.

Example:

List 1:
1 → 3 → 7

List 2:
2 → 4 → 8

List 3:
9

Output:

1 → 2 → 3 → 4 → 7 → 8 → 9

---

# PATTERN:

**K-Way Merge using a Min Heap (Priority Queue)**

This is one of the most important Heap patterns.

Whenever you have:

- K sorted arrays
- K sorted linked lists
- K sorted streams

and need the **overall smallest element**, think:

> **Min Heap**

---

# WHY THIS PATTERN:

At every step we need **the smallest node among K lists**.

Imagine each list as:

```
List1 : 1 → 3 → 7
List2 : 2 → 4 → 8
List3 : 9
```

Initially only these nodes matter:

```
1
2
9
```

The answer must start with the smallest of these.

After removing 1, only List1 changes.

Now candidates become:

```
3
2
9
```

Again choose the smallest.

Notice something important:

We **never** need to compare every node.

We only compare **the current front node of every list.**

That is exactly what a Min Heap gives us.

---

# CORE IDEA:

Keep only one node from every list inside the heap.

The heap always stores:

> "The smallest remaining node of every list."

Whenever we remove one node:

- append it to the answer
- insert its next node into the heap

Repeat until heap becomes empty.

---

# WHY A HEAP?

Suppose there are K lists.

At every step we need:

> "Which list currently has the smallest element?"

Without a heap:

We scan all K lists.

Cost = O(K)

With a heap:

Smallest element is always on top.

Cost = O(log K)

Huge improvement.

---

# WHICH HEAP?

**Min Heap**

Because every time we want the **smallest element**.

A Max Heap would always give the largest element, which is useless here.

---

# WHAT IS STORED INSIDE THE HEAP?

We store

```
(value, node pointer)
```

i.e.

```
(head->data, head)
```

Example:

```
(1, address1)
(2, address2)
(9, address3)
```

Why store the pointer?

Because after removing the smallest node,

we need

```
curr->next
```

Without the pointer we wouldn't know where the node came from.

---

# WHAT PROPERTY DOES THE HEAP MAINTAIN?

Invariant:

**The heap always contains the smallest unprocessed node from every non-empty list.**

Example:

Initially

```
1
2
9
```

Heap:

```
1
2
9
```

Remove 1.

Push 3.

Heap becomes

```
2
3
9
```

Still one node from each list.

Invariant never breaks.

---

# WHY DO WE PUSH?

When we remove

```
1
```

List1 becomes

```
3 → 7
```

Now 3 is the smallest remaining candidate from List1.

So we push it.

---

# WHY DO WE POP?

Because the top is the globally smallest node.

That node definitely belongs next in the merged list.

---

# BRUTE FORCE:

## Idea

Forget that the lists are already sorted.

Traverse every list.

Store every value.

Sort the values.

Create a new linked list.

---

## Code

```cpp
class Solution {
public:
    Node* mergeKLists(vector<Node*>& arr) {

        vector<int> values;

        for (Node* head : arr) {

            while (head != NULL) {
                values.push_back(head->data);
                head = head->next;
            }
        }

        sort(values.begin(), values.end());

        Node* dummy = new Node(0);
        Node* tail = dummy;

        for (int x : values) {

            tail->next = new Node(x);
            tail = tail->next;
        }

        return dummy->next;
    }
};
```

---

## Dry Run

Lists

```
1 3 7

2 4 8

9
```

Collect

```
1 3 7 2 4 8 9
```

Sort

```
1 2 3 4 7 8 9
```

Create

```
1→2→3→4→7→8→9
```

Done.

---

# BRUTE FORCE TIME COMPLEXITY:

Let

```
N = total number of nodes
```

### Traversing

```
O(N)
```

### Sorting

```
O(N log N)
```

### Creating answer

```
O(N)
```

Total

```
O(N log N)
```

---

# BRUTE FORCE SPACE COMPLEXITY:

Vector

```
O(N)
```

New linked list

```
O(N)
```

Ignoring output list,

Auxiliary Space

```
O(N)
```

---

# OPTIMAL APPROACH:

Use a Min Heap.

Only keep K nodes inside the heap.

Those K nodes are:

The current heads of every list.

---

# HOW THE FOR LOOP WORKS

```cpp
for(Node* head : arr)
```

Suppose

```
arr

↓

[
head1,
head2,
head3
]
```

Iteration 1

```
head = head1
```

Iteration 2

```
head = head2
```

Iteration 3

```
head = head3
```

So this simply visits every linked list.

Equivalent to

```cpp
for(int i=0;i<arr.size();i++){

    Node* head = arr[i];

}
```

Range-based for loop is just cleaner.

---

# WHY CHECK

```cpp
if(head != NULL)
```

Some lists may be empty.

Example

```
List1 : NULL
List2 : 1→2
List3 : NULL
```

We should only insert valid nodes.

---

# ALGORITHM:

### Step 1

Create a Min Heap.

---

### Step 2

Insert every non-empty head.

Heap

```
1
2
9
```

---

### Step 3

Create

```
dummy
tail
```

---

### Step 4

While heap isn't empty

Remove smallest node.

Append it.

If that node has a next node,

insert next node.

Repeat.

---

### Step 5

Return

```
dummy->next
```

---

# DRY RUN:

Lists

```
L1

1→3→7

L2

2→4→8

L3

9
```

Initial Heap

```
1
2
9
```

Answer

```
dummy
```

---

Remove

```
1
```

Answer

```
1
```

Push

```
3
```

Heap

```
2
3
9
```

---

Remove

```
2
```

Answer

```
1→2
```

Push

```
4
```

Heap

```
3
4
9
```

---

Remove

```
3
```

Answer

```
1→2→3
```

Push

```
7
```

Heap

```
4
7
9
```

---

Remove

```
4
```

Answer

```
1→2→3→4
```

Push

```
8
```

Heap

```
7
8
9
```

---

Remove

```
7
```

Answer

```
1→2→3→4→7
```

No next.

Heap

```
8
9
```

---

Remove

```
8
```

Answer

```
1→2→3→4→7→8
```

No next.

Heap

```
9
```

---

Remove

```
9
```

Answer

```
1→2→3→4→7→8→9
```

Done.

---

# WHY DO WE USE

```cpp
Node* curr = pq.top().second;
```

Heap stores

```
(value, pointer)
```

Example

```
(4, address_of_node)
```

`pq.top()` gives

```
(4, address)
```

`.second`

returns

```
address
```

So

```cpp
Node* curr
```

points to the actual node.

Now we can write

```cpp
tail->next = curr;
```

and later

```cpp
curr->next
```

Without storing the pointer,

if we only stored

```
4
```

we would have no idea where the next node is.

The pointer lets us continue traversing that linked list.

---

# IMPORTANT CODE SNIPPETS:

## Heap

```cpp
priority_queue<
pair<int, Node*>,
vector<pair<int, Node*>>,
greater<pair<int, Node*>>
> pq;
```

---

Insert

```cpp
pq.push({head->data, head});
```

---

Extract

```cpp
Node* curr = pq.top().second;
pq.pop();
```

---

Append

```cpp
tail->next = curr;
tail = tail->next;
```

---

Insert next node

```cpp
if(curr->next!=NULL)
    pq.push({curr->next->data,curr->next});
```

---

# COMMON MISTAKES:

### Mistake 1

Using Max Heap.

Need smallest node.

Use Min Heap.

---

### Mistake 2

Only storing value.

Need pointer also.

Otherwise next node cannot be accessed.

---

### Mistake 3

Forgetting

```cpp
if(curr->next)
```

NULL cannot be inserted.

---

### Mistake 4

Creating new nodes unnecessarily.

Reuse existing nodes.

---

### Mistake 5

Not using dummy node.

Makes handling first node messy.

---

# WHY I MIGHT FORGET THIS:

Because people focus on

"Merge K lists"

instead of

"What do I need next?"

Ask yourself:

> At every step, what do I need?

Answer:

The smallest node.

Smallest instantly suggests

**Min Heap.**

---

# INTERVIEW FLOW:

"I have K already sorted linked lists.

At every step I only need the smallest current node among all lists.

Instead of scanning K heads every time, I'll keep one node from each list inside a Min Heap.

Initially I insert every list's head.

The heap always contains the smallest remaining node from every list.

I repeatedly remove the smallest node, append it to the answer, and insert its next node.

Since the heap size never exceeds K, every push and pop costs O(log K), giving an overall complexity of O(N log K)."

---

# TIME COMPLEXITY:

## Brute Force

Traversal

```
O(N)
```

Sorting

```
O(N log N)
```

Creating list

```
O(N)
```

Total

```
O(N log N)
```

---

## Optimal

Let

```
N = total nodes

K = number of linked lists
```

Each node is:

- inserted exactly once
- removed exactly once

Each heap operation costs

```
O(log K)
```

There are

```
N insertions

N removals
```

So,

```
O(N log K)
```

This is much better than sorting all values because the heap size is only **K**, not **N**.

---

# SPACE COMPLEXITY:

## Brute Force

Vector stores all values:

```
O(N)
```

New linked list is created:

```
O(N) output space
```

Auxiliary Space:

```
O(N)
```

---

## Optimal

Heap stores **at most one node from each list**.

Maximum heap size:

```
K
```

Therefore,

Auxiliary Space:

```
O(K)
```

No new nodes are created—the existing nodes are reused by changing their `next` pointers.

---

# EDGE CASES:

- Only one linked list.
- Empty lists present.
- All lists empty.
- Duplicate values.
- Different list sizes.
- One very large list and many small lists.

---

# PATTERN RECOGNITION:

Whenever you see:

- Merge K sorted arrays
- Merge K sorted linked lists
- Merge K sorted streams
- Smallest element among K sorted sources

Think:

✅ **K-Way Merge**

Ask yourself:

> "At every step, do I only need the smallest current element from each sorted source?"

If yes,

Use a **Min Heap** storing one element from each source.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    Node* mergeKLists(vector<Node*>& arr) {

        priority_queue<
            pair<int, Node*>,
            vector<pair<int, Node*>>,
            greater<pair<int, Node*>>
        > pq;

        // Insert the head of every non-empty list
        for (Node* head : arr) {
            if (head != NULL) {
                pq.push({head->data, head});
            }
        }

        Node* dummy = new Node(0);
        Node* tail = dummy;

        while (!pq.empty()) {

            Node* curr = pq.top().second;
            pq.pop();

            tail->next = curr;
            tail = tail->next;

            if (curr->next != NULL) {
                pq.push({curr->next->data, curr->next});
            }
        }

        return dummy->next;
    }
};
```

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
priority_queue<...> pq;
```
Creates a **Min Heap** that always gives the smallest current node among all lists.

```cpp
for (Node* head : arr)
```
Visits the head of each linked list in the vector.

```cpp
if (head != NULL)
```
Ignore empty lists.

```cpp
pq.push({head->data, head});
```
Insert the first node of every non-empty list into the heap.

```cpp
Node* dummy = new Node(0);
```
Dummy node simplifies list construction by avoiding special handling for the first node.

```cpp
Node* tail = dummy;
```
`tail` always points to the last node in the merged list.

```cpp
while (!pq.empty())
```
Continue until all nodes from all lists have been processed.

```cpp
Node* curr = pq.top().second;
```
Retrieve the actual node pointer from the heap so we can link it and access `curr->next`.

```cpp
pq.pop();
```
Remove the smallest node because it has now been processed.

```cpp
tail->next = curr;
```
Attach the smallest node to the merged list.

```cpp
tail = tail->next;
```
Move `tail` to the newly added last node.

```cpp
if (curr->next != NULL)
```
If the same list still has nodes left, its next node becomes the new candidate.

```cpp
pq.push({curr->next->data, curr->next});
```
Insert the next node from that list back into the heap, preserving the invariant of one candidate per non-empty list.

```cpp
return dummy->next;
```
Skip the dummy node and return the real head of the merged list.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** K-Way Merge
- **Data Structure:** Min Heap
- **Heap stores:** `(node value, node pointer)`
- **Heap size:** At most `K`
- **Pop:** Smallest current node
- **Push:** Next node from the same list
- **Invariant:** Heap always contains the smallest unprocessed node from every non-empty list.
- **Brute Force:** Collect → Sort → Rebuild → **O(N log N)**
- **Optimal:** Min Heap → **O(N log K)** with **O(K)** auxiliary space.
````
