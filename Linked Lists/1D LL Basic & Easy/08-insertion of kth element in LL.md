

# PROBLEM:

We are given:
- head of a singly linked list
- a position `pos`
- a value `val`

We must insert a new node containing `val` at the given position and return the updated linked list.

Example:

```text
1 -> 2 -> 9
pos = 2
val = 5
```

After insertion:

```text
1 -> 5 -> 2 -> 9
```

The challenge is:
- linked lists do not support direct indexing
- we must traverse manually
- pointer updates must be done carefully

---

# PATTERN:

Linked List Traversal + Pointer Manipulation

---

# WHY THIS PATTERN:

In linked lists:
- nodes are connected using pointers
- insertion is done by changing links
- no shifting like arrays

To insert properly:
1. reach previous node
2. save remaining list
3. connect new node safely

This is a classic pointer manipulation problem.

---

# CORE IDEA:

Main idea:

```text
To insert at position pos,
we must first reach node at position pos-1
```

Then:

```cpp
newNode->next = temp->next;
temp->next = newNode;
```

These two lines insert the node safely.

---

# BRUTE FORCE:

Not needed.

The direct linked list insertion approach itself is already optimal and expected in interviews.

---

# OPTIMAL APPROACH:

1. Create new node
2. Handle insertion at beginning
3. Traverse to previous node
4. Save next connection
5. Attach new node
6. Return head

---

# ALGORITHM:

### Step 1:
Create the node to insert.

```cpp
Node* newNode = new Node(val);
```

---

### Step 2:
Handle empty linked list.

```cpp
if(head == NULL){
   return newNode;
}
```

If list is empty:
- new node becomes first node

---

### Step 3:
Handle insertion at beginning.

```cpp
if(pos==1){
    newNode->next = head;
    return newNode;
}
```

Why needed?

Because:
- head changes
- normal traversal logic cannot handle it directly

---

### Step 4:
Create traversal pointer.

```cpp
Node* temp = head;
```

Used for traversing list.

---

### Step 5:
Initialize counter.

```cpp
int count = 0;
```

Tracks current node position.

---

### Step 6:
Traverse linked list.

```cpp
while(temp!=NULL)
```

Loop until end of list.

---

### Step 7:
Increase position count.

```cpp
count++;
```

Each node visited increases count.

---

### Step 8:
Check if previous node reached.

```cpp
if(count == pos-1)
```

Why `pos-1`?

Because insertion happens AFTER previous node.

---

### Step 9:
Save remaining list.

```cpp
newNode->next = temp->next;
```

This stores connection to remaining nodes.

VERY important.

Without this:
remaining list gets lost.

---

### Step 10:
Connect previous node to new node.

```cpp
temp->next = newNode;
```

Insertion complete.

---

### Step 11:
Stop traversal.

```cpp
break;
```

No need to continue once insertion is done.

---

### Step 12:
Move forward if insertion not done yet.

```cpp
temp = temp->next;
```

Continue traversal.

---

### Step 13:
Return updated head.

```cpp
return head;
```

Head remains same unless insertion happened at beginning.

---

# DRY RUN:

Input:

```text
1 -> 2 -> 9
pos = 3
val = 5
```

---

### Initial State:

```text
temp = 1
count = 0
```

New node:

```text
5 -> NULL
```

---

### Iteration 1:

```text
count = 1
temp = 1
```

Check:

```text
count == pos-1
1 == 2 ? NO
```

Move:

```text
temp = 2
```

---

### Iteration 2:

```text
count = 2
temp = 2
```

Check:

```text
2 == 2 ? YES
```

Insertion begins.

---

### Step A:

```cpp
newNode->next = temp->next;
```

Now:

```text
5 -> 9
```

---

### Step B:

```cpp
temp->next = newNode;
```

Final list:

```text
1 -> 2 -> 5 -> 9
```

---

# IMPORTANT CODE SNIPPETS:

### Insert at beginning:

```cpp
if(pos==1){
    newNode->next = head;
    return newNode;
}
```

---

### Traversal:

```cpp
Node* temp = head;
int count = 0;
```

---

### Insertion logic:

```cpp
newNode->next = temp->next;
temp->next = newNode;
```

---

# COMMON MISTAKES:

### Mistake 1:
Wrong pointer update order.

WRONG:

```cpp
temp->next = newNode;
newNode->next = temp->next;
```

Creates self-loop.

---

### Mistake 2:
Forgetting beginning insertion case.

Then:
- head never changes
- insertion becomes incorrect

---

### Mistake 3:
Using `pos` instead of `pos-1`.

We need previous node.

---

### Mistake 4:
Not saving remaining list first.

This disconnects remaining nodes.

---

# WHY I MIGHT FORGET THIS:

Because linked list insertion involves:
- multiple pointers
- pointer order dependency
- traversal confusion

Remember this line:

```text
Save next node first,
then connect new node
```

---

# INTERVIEW FLOW:

1. Explain linked lists require traversal.
2. Mention insertion needs previous node.
3. Handle `pos == 1` separately.
4. Traverse to `pos-1`.
5. Explain pointer update order carefully.
6. Mention why order matters.
7. Discuss edge cases.
8. State complexity.

---

# TIME COMPLEXITY:

```text
O(N)
```

### Reason:
In worst case we traverse entire linked list once.

Example:
Insertion at end.

---

# SPACE COMPLEXITY:

```text
O(1)
```

### Reason:
Only one extra node is used.

No extra data structures required.

---

# EDGE CASES:

### 1. Empty list

```text
head == NULL
```

New node becomes first node.

---

### 2. Insert at beginning

```text
pos == 1
```

Need to update head.

---

### 3. Insert at end

```text
pos == size + 1
```

New node points to NULL.

---

### 4. Single node list

Still works correctly.

---

### 5. Invalid position

Normally avoided because constraints guarantee valid positions.

---

# PATTERN RECOGNITION:

Think of this pattern when:
- insertion/deletion in linked list
- position based operations
- pointer reconnection required
- traversal required before modification

Main signal:

```text
Need previous node before modifying links
```

---

# CLEAN C++ CODE:

```cpp
/*
class Node {
  public:
    int data;
    Node *next;
    Node(int x) {
        data = x;
        next = nullptr;
    }
};
*/

class Solution {
  public:
  
    Node *insertPos(Node *head, int pos, int val) {
        
        Node* newNode = new Node(val);
        
        // Empty linked list
        if(head == NULL){
           return newNode;
        }
        
        // Insert at beginning
        if(pos==1){
            newNode->next = head;
            return newNode;
        }
        
        Node* temp = head;
        int count = 0;
        
        while(temp!=NULL){
            
            count++;
            
            // Reach previous node
            if(count == pos-1){
                
                // Save remaining list
                newNode->next = temp->next;
                
                // Connect new node
                temp->next = newNode;
                
                break;
            }
            
            temp = temp->next;
        } 
        
        return head;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE:

### Create node:

```cpp
Node* newNode = new Node(val);
```

Creates node that must be inserted.

---

### Empty list handling:

```cpp
if(head == NULL)
```

If list is empty:
new node becomes first node.

---

### Beginning insertion:

```cpp
if(pos==1)
```

Head changes here.

Special handling required.

---

### Traversal pointer:

```cpp
Node* temp = head;
```

Moves through linked list.

---

### Position tracking:

```cpp
count++;
```

Tracks current node position.

---

### Previous node check:

```cpp
if(count == pos-1)
```

Insertion always requires previous node.

---

### Save remaining list:

```cpp
newNode->next = temp->next;
```

Prevents losing remaining nodes.

---

### Attach new node:

```cpp
temp->next = newNode;
```

Completes insertion.

---

# EASY-TO-REMEMBER SUMMARY:

```text
1. Create new node
2. Handle head insertion
3. Reach previous node
4. Save next node
5. Connect new node
6. Return head
```

Golden insertion formula:

```cpp
newNode->next = temp->next;
temp->next = newNode;
```
````
