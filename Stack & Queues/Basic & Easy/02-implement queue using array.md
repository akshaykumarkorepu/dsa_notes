
## PROBLEM:

Implement a Queue using an Array of fixed size `n`.

The queue must support:

- `enqueue(x)` → Insert element at rear
- `dequeue()` → Remove element from front
- `getFront()` → Return front element
- `getRear()` → Return rear element
- `isEmpty()` → Check if queue is empty
- `isFull()` → Check if queue is full

Queue follows FIFO (First In First Out).

Example:

```text
enqueue(5)
enqueue(3)
enqueue(4)

Queue:
5 → 3 → 4

dequeue()

Queue:
3 → 4
```

The first inserted element is always removed first.

---

## PATTERN:

**Queue Implementation Using Array + Front/Rear Pointers**

---

## WHY THIS PATTERN:

A queue needs two ends:

```text
Front → Removal
Rear  → Insertion
```

To perform all operations in O(1):

- Store elements inside an array/vector
- Maintain:
  - `front` pointer
  - `rear` pointer
  - `size`

This avoids shifting elements after every dequeue.

---

## CORE IDEA:

Store queue elements in an array and maintain:

```cpp
front
rear
size
capacity
```

Meaning:

```text
front → first valid element

rear → last valid element

size → current number of elements

capacity → maximum queue size
```

### Important Initialization

```cpp
front = 0;
rear = -1;
size = 0;
```

### Why?

```text
front = 0
```

The first inserted element will be placed at index 0.

```text
rear = -1
```

No element exists yet.

After first enqueue:

```cpp
rear++;
```

rear automatically becomes 0.

No special handling required.

---

## BRUTE FORCE:

Not really required.

A beginner might use:

```cpp
vector<int> arr;
```

Insertion:

```cpp
arr.push_back(x);
```

Deletion:

```cpp
arr.erase(arr.begin());
```

Example:

```text
Before:
[5,3,4]

After removing 5:
[3,4]
```

Every remaining element shifts left.

### Time Complexity

```text
enqueue → O(1)
dequeue → O(n)
```

Not acceptable.

This motivates using front and rear pointers.

---

## OPTIMAL APPROACH:

Maintain:

```cpp
vector<int> arr(n);

int front;
int rear;
int size;
int capacity;
```

### Rules

#### Enqueue

```text
Insert at rear
Move rear forward
Increase size
```

#### Dequeue

```text
Move front forward
Decrease size
```

No shifting.

All operations become O(1).

---

## ALGORITHM:

### Initialization

```cpp
capacity = n

front = 0
rear = -1
size = 0
```

### isEmpty()

```cpp
return size == 0;
```

### isFull()

```cpp
return size == capacity;
```

### enqueue(x)

```cpp
if(queue is full)
    return;

rear++;

arr[rear] = x;

size++;
```

### dequeue()

```cpp
if(queue is empty)
    return;

front++;

size--;
```

### getFront()

```cpp
if(queue is empty)
    return -1;

return arr[front];
```

### getRear()

```cpp
if(queue is empty)
    return -1;

return arr[rear];
```

---

## DRY RUN:

### Capacity = 3

### Initial State

```text
arr = [_,_,_]

front = 0
rear = -1
size = 0
```

Queue:

```text
Empty
```

---

### enqueue(5)

```text
rear = 0

arr[0] = 5

size = 1
```

State:

```text
arr = [5,_,_]

front = 0
rear = 0
size = 1
```

Queue:

```text
5
```

---

### enqueue(3)

```text
rear = 1

arr[1] = 3

size = 2
```

State:

```text
arr = [5,3,_]

front = 0
rear = 1
size = 2
```

Queue:

```text
5 → 3
```

---

### enqueue(4)

```text
rear = 2

arr[2] = 4

size = 3
```

State:

```text
arr = [5,3,4]

front = 0
rear = 2
size = 3
```

Queue:

```text
5 → 3 → 4
```

---

### getFront()

```text
arr[0]
```

Output:

```text
5
```

---

### dequeue()

Move front:

```text
front = 1

size = 2
```

State:

```text
arr = [5,3,4]

front = 1
rear = 2
size = 2
```

Logical Queue:

```text
3 → 4
```

Notice:

```text
5 still exists in memory
```

but is ignored because front moved.

---

### isEmpty()

```text
size = 2
```

Output:

```text
false
```

---

### getRear()

```text
arr[2]
```

Output:

```text
4
```

---

## IMPORTANT CODE SNIPPETS:

### Queue Empty Check

```cpp
return size == 0;
```

### Queue Full Check

```cpp
return size == capacity;
```

### Enqueue

```cpp
rear++;
arr[rear] = x;
size++;
```

### Dequeue

```cpp
front++;
size--;
```

### Front Element

```cpp
return arr[front];
```

### Rear Element

```cpp
return arr[rear];
```

---

## COMMON MISTAKES:

### 1. Forgetting to Store the Value

Wrong:

```cpp
rear++;
size++;
```

Correct:

```cpp
rear++;
arr[rear] = x;
size++;
```

---

### 2. Using

```cpp
arr.erase(arr.begin());
```

during dequeue.

This causes O(n) shifting.

---

### 3. Using

```cpp
size == capacity - 1
```

for full check.

Wrong.

Queue becomes full before all slots are used.

Correct:

```cpp
size == capacity
```

---

### 4. Returning

```cpp
arr[front]
```

without checking empty queue.

Can cause invalid access.

---

### 5. Confusing Logical Queue with Physical Array

After dequeue:

```text
arr = [5,3,4]
front = 1
```

Queue is:

```text
3 → 4
```

not

```text
5 → 3 → 4
```

---

## WHY I MIGHT FORGET THIS:

Because dequeue does not remove anything physically.

It only moves:

```cpp
front++;
```

Remember:

```text
Stack removes by popping.

Queue removes by moving front.
```

Think:

```text
Front moves forward.

Rear moves forward.
```

The elements themselves stay in memory.

---

## INTERVIEW FLOW:

Interviewer:

> Implement Queue Using Array.

You:

> A queue follows FIFO. We need insertion from rear and deletion from front.

> We can maintain four variables:

```cpp
front
rear
size
capacity
```

> front points to the first valid element.

> rear points to the last valid element.

> enqueue inserts at rear.

> dequeue simply advances front.

> This avoids shifting elements and keeps operations O(1).

Then explain:

```cpp
front = 0;
rear = -1;
size = 0;
```

Walk through:

```text
enqueue(5)
enqueue(3)
dequeue()
```

Show front moving.

Finally mention:

```text
This implementation does not reuse freed positions.

Circular Queue is the natural follow-up.
```

---

## TIME COMPLEXITY:

### enqueue()

```text
O(1)
```

Only:

```cpp
rear++;
arr[rear] = x;
```

---

### dequeue()

```text
O(1)
```

Only:

```cpp
front++;
```

---

### getFront()

```text
O(1)
```

Direct indexing.

---

### getRear()

```text
O(1)
```

Direct indexing.

---

### isEmpty()

```text
O(1)
```

---

### isFull()

```text
O(1)
```

---

## SPACE COMPLEXITY:

```text
O(n)
```

We store at most `n` elements inside the queue array.

---

## EDGE CASES:

### Empty Queue

```text
getFront() → -1
getRear() → -1
dequeue() → do nothing
```

---

### Queue Full

```text
enqueue() → do nothing
```

---

### Single Element

```text
enqueue(5)

front = 0
rear = 0
size = 1
```

Both:

```text
getFront()
getRear()
```

return:

```text
5
```

---

## PATTERN RECOGNITION:

Use this pattern when:

```text
FIFO behaviour is required
```

Keywords:

```text
Queue
FIFO
Task scheduling
Buffer
Print queue
Request processing
Level Order Traversal
```

If you see:

```text
Insert at one end
Remove from another end
```

think:

```text
Queue
```

If interviewer asks:

```text
Can we reuse freed positions?
```

Immediately think:

```text
Circular Queue
```

---

# Clean C++ Code

```cpp
class myQueue {

    vector<int> arr;
    int capacity;
    int front;
    int rear;
    int size;

public:
    myQueue(int n) {

        capacity = n;
        arr.resize(n);

        front = 0;
        rear = -1;
        size = 0;
    }

    bool isEmpty() {
        return size == 0;
    }

    bool isFull() {
        return size == capacity;
    }

    void enqueue(int x) {

        if (isFull())
            return;

        rear++;
        arr[rear] = x;
        size++;
    }

    void dequeue() {

        if (isEmpty())
            return;

        front++;
        size--;
    }

    int getFront() {

        if (isEmpty())
            return -1;

        return arr[front];
    }

    int getRear() {

        if (isEmpty())
            return -1;

        return arr[rear];
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
front = 0;
```

First valid element will appear at index 0.

---

```cpp
rear = -1;
```

No element exists initially.

---

```cpp
rear++;
```

Move to next insertion position.

---

```cpp
arr[rear] = x;
```

Store new element.

---

```cpp
front++;
```

Remove front logically without shifting.

---

```cpp
size++;
size--;
```

Track current number of elements.

---

# Easy-to-Remember Summary

```text
Queue = FIFO

Insert → Rear
Delete → Front

front = first valid element
rear = last valid element

enqueue:
    rear++
    store value
    size++

dequeue:
    front++
    size--

Empty:
    size == 0

Full:
    size == capacity

No shifting is performed.

Front moves.
Rear moves.

All operations are O(1).
```

### Biggest Interview Takeaway

```text
Queue Using Array = front + rear + size

Circular Queue = Queue Using Array + reuse freed positions.
```
````
