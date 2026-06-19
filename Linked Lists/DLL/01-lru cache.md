

## PROBLEM

Design an **LRU (Least Recently Used) Cache** supporting the following operations:

- `GET(key)`
- `PUT(key, value)`

Requirements:

- `GET(key)` returns the value if the key exists, otherwise `-1`.
- Every successful `GET` makes the key the **Most Recently Used (MRU)**.
- `PUT(key, value)`:
  - Updates the value if the key already exists.
  - Inserts a new key if it doesn't exist.
  - If the cache is full, removes the **Least Recently Used (LRU)** key before inserting.
- Both **GET** and **PUT** must work in **O(1)** time.

---

## PATTERN

**Design Data Structure**

**HashMap + Doubly Linked List**

---

## WHY THIS PATTERN

A single data structure cannot satisfy all requirements.

### HashMap

Provides:

- O(1) lookup by key

But it **cannot maintain usage order**.

### Doubly Linked List

Provides:

- O(1) insertion
- O(1) deletion
- Maintains order of usage

Combining both gives:

- O(1) GET
- O(1) PUT

---

## CORE IDEA

Maintain two data structures.

### 1. HashMap

Stores:

```
Key → Address of corresponding DLL node
```

This allows direct access to any node in **O(1)**.

---

### 2. Doubly Linked List

Maintains usage order.

```
Head
 ↓
Least Recently Used (LRU)
 ↓
...
 ↓
Most Recently Used (MRU)
 ↓
Tail
```

Use **dummy Head** and **dummy Tail** to avoid edge cases.

### Invariant

- Every accessed node moves before **Tail**.
- Every newly inserted node goes before **Tail**.
- `Head->Next` is always the **LRU**.
- `Tail->Prev` is always the **MRU**.

---

## BRUTE FORCE

Maintain the cache in a vector/list.

### GET

- Search linearly.
- Return value.
- Move element to end.

### PUT

- Search linearly.
- Update if found.
- Otherwise insert.
- Remove first element if cache becomes full.

### Time Complexity

GET : **O(n)**

PUT : **O(n)**

### Space Complexity

**O(capacity)**

### Interview Tip

Do **not** spend time coding brute force.

Simply explain:

> "A naive approach is to maintain the cache in a list/vector. Every GET and PUT requires linear search and moving elements, giving O(n). Since the problem explicitly requires O(1), we need a HashMap for lookup and a Doubly Linked List for maintaining recency."

---

## OPTIMAL APPROACH

Use:

- **HashMap<Key, Node*>**
- **Doubly Linked List**

Each node stores:

- key
- value
- prev
- next

HashMap stores the address of every node.

Linked List maintains recency.

Dummy Head:

```
Head->Next = LRU
```

Dummy Tail:

```
Tail->Prev = MRU
```

Create two helper functions:

### remove(node)

Detach any node in O(1).

### insert(node)

Insert any node before Tail in O(1).

Then,

### GET

```
Find
↓

Remove
↓

Insert
↓

Return Value
```

### PUT

```
Update existing

OR

Remove LRU (if full)

↓

Create New Node

↓

Insert

↓

Store in HashMap
```

---

## ALGORITHM

### Initialization

- Store capacity.
- Create dummy Head.
- Create dummy Tail.
- Connect:

```
Head ⇄ Tail
```

HashMap is initially empty.

---

### remove(node)

Connect:

```
Previous → Next

Next → Previous
```

The node becomes detached.

**Do NOT delete it here.**

---

### insert(node)

Insert node between:

```
Tail->Prev

and

Tail
```

The node becomes the **MRU**.

---

### GET(key)

If key doesn't exist:

```
Return -1
```

Else:

- Get node from HashMap.
- Remove node.
- Insert node before Tail.
- Return value.

---

### PUT(key,value)

### Case 1 : Key already exists

- Update value.
- Remove node.
- Insert node.
- Return.

---

### Case 2 : Key doesn't exist & Cache full

- Find LRU (`Head->Next`)
- Remove it.
- Erase from HashMap.
- Delete memory.

---

### Case 3 : Insert new node

- Create node.
- Insert before Tail.
- Store in HashMap.

---

## DRY RUN

### Capacity = 2

Queries:

```
PUT(1,10)

PUT(2,20)

GET(1)

PUT(3,30)

GET(2)
```

Initially

```
Head ⇄ Tail

Map

{}
```

---

### PUT(1,10)

```
Head ⇄ (1,10) ⇄ Tail

Map

1 → Node1
```

---

### PUT(2,20)

```
Head ⇄ (1,10) ⇄ (2,20) ⇄ Tail

Map

1 → Node1

2 → Node2
```

---

### GET(1)

Find node.

Remove.

Insert before Tail.

```
Head ⇄ (2,20) ⇄ (1,10) ⇄ Tail
```

Return:

```
10
```

---

### PUT(3,30)

Cache full.

LRU:

```
Head->Next

↓

(2,20)
```

Remove.

Erase key.

Delete node.

Create Node(3,30).

Insert.

```
Head ⇄ (1,10) ⇄ (3,30) ⇄ Tail
```

Map:

```
1 → Node1

3 → Node3
```

---

### GET(2)

Key absent.

Return

```
-1
```

---

## IMPORTANT OBSERVATIONS

### HashMap stores

```
Key → Node*
```

NOT

```
Key → Value
```

Reason:

We must move nodes in O(1).

---

### remove()

Does **NOT** delete the node.

It only disconnects it.

Deletion only happens when removing the LRU permanently.

---

### insert()

Always inserts before Tail.

Hence every accessed node automatically becomes the MRU.

---

### Head->Next

Always points to the **LRU**.

---

### Tail->Prev

Always points to the **MRU**.

---

### Dummy nodes

Avoid edge cases.

No separate handling for:

- Empty list
- One node
- Removing first node
- Removing last node

---

## IMPORTANT CODE SNIPPETS

### Find Node

```cpp
Node* node = mp[key];
```

---

### Remove Node

```cpp
node->prev->next = node->next;
node->next->prev = node->prev;
```

---

### Insert Node

```cpp
node->prev = tail->prev;
node->next = tail;

tail->prev->next = node;
tail->prev = node;
```

---

### Find LRU

```cpp
Node* lru = head->next;
```

---

### Delete LRU

```cpp
remove(lru);

mp.erase(lru->key);

delete lru;
```

---

### Insert New Node

```cpp
Node* node = new Node(key,value);

insert(node);

mp[key] = node;
```

---

## COMMON MISTAKES

- Using `unordered_map<int,int>` instead of `unordered_map<int, Node*>`.
- Deleting node inside `remove()`.
- Forgetting `mp.erase(lru->key)`.
- Updating value but forgetting to move node to MRU.
- Forgetting `return` after updating an existing key.
- Writing insert pointer updates in the wrong order.

---

## WHY I MIGHT FORGET THIS

Don't memorize the code.

Remember the invariant.

### HashMap

Find any node instantly.

### Doubly Linked List

Maintain usage order.

### Helper Functions

- remove()
- insert()

Everything else simply uses these helpers.

---

## INTERVIEW FLOW

### Step 1

Clarify the requirement.

Both **GET** and **PUT** must be **O(1)**.

---

### Step 2

Mention brute force.

Linear search.

O(n).

Not acceptable.

---

### Step 3

Introduce:

- HashMap
- Doubly Linked List

Explain why each is needed.

---

### Step 4

Explain Node structure.

```
key

value

prev

next
```

---

### Step 5

Explain dummy Head and Tail.

```
Head->Next = LRU

Tail->Prev = MRU
```

---

### Step 6

Explain helper functions.

- remove()
- insert()

---

### Step 7

Explain GET.

```
Find

↓

Move to MRU

↓

Return Value
```

---

### Step 8

Explain PUT.

```
Update Existing

OR

Evict LRU

↓

Insert New Node
```

---

### Step 9

Dry run one example.

---

### Step 10

Mention complexities.

---

## TIME COMPLEXITY

### GET

HashMap lookup → O(1)

remove() → O(1)

insert() → O(1)

Overall:

**O(1)**

---

### PUT

HashMap lookup → O(1)

remove() → O(1)

insert() → O(1)

erase() → O(1)

Overall:

**O(1)**

### Reason

Every operation performs a constant number of HashMap operations and pointer updates.

---

## SPACE COMPLEXITY

HashMap:

O(capacity)

Linked List:

O(capacity)

Overall:

**O(capacity)**

---

## EDGE CASES

- Capacity = 1
- Updating an existing key
- GET on missing key
- Multiple GETs on same key
- Multiple PUTs on same key
- Cache becomes exactly full
- Empty cache

---

## PATTERN RECOGNITION

Think of this pattern whenever you see:

- Cache implementation
- O(1) insertion/deletion/lookup
- Recently used ordering
- Eviction policy
- LRU / MRU
- Design Data Structure

Typical keywords:

- Cache
- LRU
- O(1) GET
- O(1) PUT
- Recently Used
- Eviction
- Design

---

# Clean C++ Code

*(Use your final implementation that you derived during practice.)*

---

# Intuition Behind Every Important Line

```cpp
unordered_map<int, Node*> mp;
```

Stores the address of each node for O(1) lookup.

---

```cpp
Node* head;
Node* tail;
```

Dummy nodes simplify insertion and deletion.

---

```cpp
remove(node);
```

Disconnects a node from the linked list.

---

```cpp
insert(node);
```

Places the node before Tail, making it the MRU.

---

```cpp
Node* node = mp[key];
```

Instantly finds the linked-list node.

---

```cpp
Node* lru = head->next;
```

Gets the Least Recently Used node.

---

```cpp
mp.erase(lru->key);
```

Removes the deleted node's key from the HashMap.

---

```cpp
mp[key] = node;
```

Stores the new node's address in the HashMap.

---

# Easy-to-Remember Summary

Think of LRU Cache in one sentence:

> **HashMap tells me where a node is. Doubly Linked List tells me how recently it was used.**

Everything else follows naturally:

```
remove() = Disconnect node

insert() = Insert before Tail (MRU)

get() = Find → Move → Return

put() = Update OR Evict LRU → Insert New → Store in HashMap
```

### The Invariant

```
Head->Next = LRU

Tail->Prev = MRU
```

If you remember these two rules and understand the helper functions, you can reconstruct the entire solution in an interview without memorizing the code.
````
