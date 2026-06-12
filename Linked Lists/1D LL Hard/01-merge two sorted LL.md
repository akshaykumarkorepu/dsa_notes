

## PROBLEM:

Merge two already sorted linked lists and return a single sorted linked list.

Example:

```text
List1: 2 → 5 → 15 → 20

List2: 3 → 10 → 20 → 40

Output:
2 → 3 → 5 → 10 → 15 → 20 → 20 → 40
```

The challenge is to merge them while maintaining sorted order.

---

## PATTERN:

**Two Pointers + Merge Process (Merge Sort Merge Step)**

Pointers used:

```text
t1 → current node of first list
t2 → current node of second list
temp → tail of merged list
```

---

## WHY THIS PATTERN:

The most important observation is:

```text
Both linked lists are already sorted.
```

When two structures are sorted:

```text
smallest remaining element
=
minimum(current element of list1,
        current element of list2)
```

Therefore we never need sorting again.

We only need to compare the current nodes and attach the smaller one.

This is exactly the merge phase of Merge Sort.

---

## CORE IDEA:

At every step:

```text
Compare current nodes
↓
Take smaller node
↓
Attach it to answer
↓
Move that pointer forward
```

When one list finishes:

```text
Attach remaining nodes of the other list directly.
```

---

## BRUTE FORCE:

### Intuition

Ignore the fact that lists are sorted.

1. Store all elements in a vector.
2. Sort the vector.
3. Convert the sorted vector back into a linked list.

### Why Consider This?

This is not optimal, but it helps understand why sorting again is wasteful.

### Code

```cpp
Node* convertArrToLL(vector<int>& arr) {

    Node* head = new Node(arr[0]);
    Node* tail = head;

    for(int i = 1; i < arr.size(); i++) {
        tail->next = new Node(arr[i]);
        tail = tail->next;
    }

    return head;
}

Node* sortedMerge(Node* head1, Node* head2) {

    vector<int> arr;

    Node* temp = head1;

    while(temp) {
        arr.push_back(temp->data);
        temp = temp->next;
    }

    temp = head2;

    while(temp) {
        arr.push_back(temp->data);
        temp = temp->next;
    }

    sort(arr.begin(), arr.end());

    return convertArrToLL(arr);
}
```

### Dry Run

Input:

```text
2 → 5 → 15 → 20

3 → 10 → 20 → 40
```

Store values:

```text
[2,5,15,20,3,10,20,40]
```

Sort:

```text
[2,3,5,10,15,20,20,40]
```

Convert back:

```text
2 → 3 → 5 → 10 → 15 → 20 → 20 → 40
```

### Complexity

Time Complexity:

```text
O((N + M) log(N + M))
```

Sorting dominates.

Space Complexity:

```text
O(N + M)
```

Vector stores all elements.

### Drawback

We completely ignore the fact that the lists are already sorted.

---

## OPTIMAL APPROACH:

### Intuition

Since both lists are sorted:

```text
2 → 5 → 15 → 20

3 → 10 → 20 → 40
```

The smallest available element must always be among:

```text
t1->data
t2->data
```

So:

```text
Compare
↓
Take smaller
↓
Move corresponding pointer
```

Repeat until one list ends.

---

## ALGORITHM:

### Step 1

Create pointers:

```cpp
Node* t1 = head1;
Node* t2 = head2;
```

### Step 2

Create a dummy node.

```cpp
Node* dummyNode = new Node(-1);
Node* temp = dummyNode;
```

### Step 3

While both lists exist:

```cpp
while(t1 && t2)
```

Compare:

```cpp
t1->data
t2->data
```

Attach the smaller node.

### Step 4

Move:

```cpp
temp = temp->next;
```

Move the pointer whose node was used.

### Step 5

One list finishes.

Attach the remaining list:

```cpp
if(t1) temp->next = t1;

if(t2) temp->next = t2;
```

### Step 6

Return:

```cpp
dummyNode->next
```

---

## DRY RUN:

Input:

```text
List1:
2 → 5 → 15 → 20

List2:
3 → 10 → 20 → 40
```

Initial:

```text
dummy
```

### Compare

```text
2 vs 3
```

Take 2

```text
dummy → 2
```

Move t1

---

### Compare

```text
5 vs 3
```

Take 3

```text
dummy → 2 → 3
```

Move t2

---

### Compare

```text
5 vs 10
```

Take 5

```text
dummy → 2 → 3 → 5
```

Move t1

---

### Compare

```text
15 vs 10
```

Take 10

```text
dummy → 2 → 3 → 5 → 10
```

Move t2

---

### Compare

```text
15 vs 20
```

Take 15

```text
dummy → 2 → 3 → 5 → 10 → 15
```

Move t1

---

### Compare

```text
20 vs 20
```

Take first 20

```text
dummy → 2 → 3 → 5 → 10 → 15 → 20
```

Move t1

Now:

```text
t1 = NULL
```

Remaining:

```text
20 → 40
```

Attach directly.

Final:

```text
2 → 3 → 5 → 10 → 15 → 20 → 20 → 40
```

---

## IMPORTANT CODE SNIPPETS:

### Creating Dummy Node

```cpp
Node* dummyNode = new Node(-1);
Node* temp = dummyNode;
```

### Attach Smaller Node

```cpp
if(t1->data <= t2->data){
    temp->next = t1;
    temp = t1;
    t1 = t1->next;
}
else{
    temp->next = t2;
    temp = t2;
    t2 = t2->next;
}
```

### Attach Remaining Nodes

```cpp
if(t1) temp->next = t1;

if(t2) temp->next = t2;
```

### Return Actual Head

```cpp
return dummyNode->next;
```

---

## COMMON MISTAKES:

### 1. Attaching Remaining Nodes Inside Loop

Wrong:

```cpp
while(t1 && t2){

    ...

    if(t1) temp->next = t1;
}
```

Remaining nodes must be attached **after the loop finishes**.

### 2. Returning Dummy Node

Wrong:

```cpp
return dummyNode;
```

Correct:

```cpp
return dummyNode->next;
```

### 3. Forgetting To Move temp

Wrong:

```cpp
temp->next = t1;
```

Without:

```cpp
temp = temp->next;
```

merged list never grows correctly.

### 4. Creating New Nodes Unnecessarily

Wrong:

```cpp
new Node(t1->data)
```

We can reuse existing nodes.

---

## WHY I MIGHT FORGET THIS:

Because many students think:

```text
Merge two lists
↓
Store everything
↓
Sort
```

They forget the key observation:

```text
Lists are already sorted.
```

The entire problem becomes easy once you see:

```text
Merge Sort Merge Step
```

---

## INTERVIEW FLOW:

Step 1:

Say:

```text
Both linked lists are already sorted.
```

Step 2:

Say:

```text
Instead of sorting again, I can merge them directly.
```

Step 3:

Use:

```text
Two pointers
```

Step 4:

Compare current nodes.

Attach smaller node.

Step 5:

Attach remaining nodes.

Step 6:

Complexity:

```text
Time: O(N + M)
Space: O(1)
```

---

## TIME COMPLEXITY:

### Brute Force

Traversal:

```text
O(N + M)
```

Sorting:

```text
O((N + M) log(N + M))
```

Total:

```text
O((N + M) log(N + M))
```

### Optimal

Each node is visited exactly once.

```text
O(N + M)
```

---

## SPACE COMPLEXITY:

### Brute Force

Vector stores all elements.

```text
O(N + M)
```

### Optimal

Only pointers are used.

```text
O(1)
```

No extra data structure.

---

## EDGE CASES:

### Case 1

```text
1

2
```

Output:

```text
1 → 2
```

### Case 2

```text
1 → 2 → 3

4 → 5 → 6
```

Entire first list comes first.

### Case 3

```text
4 → 5 → 6

1 → 2 → 3
```

Entire second list comes first.

### Case 4

Duplicates

```text
1 → 3 → 5

1 → 2 → 5
```

Output:

```text
1 → 1 → 2 → 3 → 5 → 5
```

---

## PATTERN RECOGNITION:

Use this pattern whenever you see:

```text
Two sorted linked lists
Two sorted arrays
Merge k sorted lists
Merge intervals after sorting
Merge Sort merge phase
```

Recognition clue:

```text
Two sorted structures are given
and
you need one sorted structure.
```

Immediately think:

```text
Two Pointers + Merge Process
```

---

# Clean C++ Code

```cpp
class Solution {
public:
    Node* sortedMerge(Node* head1, Node* head2) {

        Node* t1 = head1;
        Node* t2 = head2;

        Node* dummyNode = new Node(-1);
        Node* temp = dummyNode;

        while(t1 != NULL && t2 != NULL) {

            if(t1->data <= t2->data) {
                temp->next = t1;
                temp = t1;
                t1 = t1->next;
            }
            else {
                temp->next = t2;
                temp = t2;
                t2 = t2->next;
            }
        }

        if(t1 != NULL)
            temp->next = t1;

        if(t2 != NULL)
            temp->next = t2;

        return dummyNode->next;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
Node* dummyNode = new Node(-1);
```

Creates a fake starting node so we don't need special handling for the first insertion.

```cpp
Node* temp = dummyNode;
```

Always points to the last node of the merged list.

```cpp
while(t1 && t2)
```

Keep merging until one list is exhausted.

```cpp
if(t1->data <= t2->data)
```

Choose the smaller node because the final list must remain sorted.

```cpp
temp->next = t1;
```

Attach the chosen node.

```cpp
temp = temp->next;
```

Move tail forward.

```cpp
t1 = t1->next;
```

Move the pointer whose node was used.

```cpp
if(t1) temp->next = t1;
```

Attach remaining nodes directly.

No more comparisons are needed.

```cpp
return dummyNode->next;
```

Skip the dummy node and return the actual head.

---

# Easy-to-Remember Summary

```text
Two sorted lists
↓
Use two pointers
↓
Compare current nodes
↓
Attach smaller node
↓
Move corresponding pointer
↓
One list ends
↓
Attach remaining list
↓
Return dummy->next

Time = O(N + M)
Space = O(1)
```

Think:

```text
Merge Two Sorted Lists
=
Merge Sort's Merge Step
```
````
