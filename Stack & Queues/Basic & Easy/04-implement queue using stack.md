

## PROBLEM:

Implement a Queue using only Stack data structures.

Queue Operations:
- `enqueue(x)` → Insert at rear
- `dequeue()` → Remove from front
- `front()` → Return front element
- `size()` → Return current size

Queue follows:

**FIFO (First In First Out)**

Example:

```text
5 3 4

dequeue() removes 5
```

Stack follows:

**LIFO (Last In First Out)**

Example:

```text
push(5)
push(3)
push(4)

pop() removes 4
```

The challenge is to simulate FIFO behavior using only LIFO structures.

---

## PATTERN:

### Queue Using Two Stacks

Data Structures:

```cpp
stack<int> s1;
stack<int> s2;
```

Meaning:

```text
s1 → Incoming Stack
s2 → Outgoing Stack
```

---

## WHY THIS PATTERN:

A stack naturally removes the most recently inserted element.

A queue must remove the oldest inserted element.

Using two stacks allows us to reverse the order when needed.

Important property:

```text
Stack → Stack transfer reverses order
```

Example:

Before transfer:

```text
s1

Top
4
3
5
```

After transfer:

```text
s2

Top
5
3
4
```

Now the oldest element (`5`) becomes accessible at the top.

This gives queue behavior.

---

## CORE IDEA:

### Brute Force Idea

Keep the queue front always on top of a stack.

### Optimal Idea

Delay the reversal until it is actually needed.

Use:

```text
s1 → insertions
s2 → deletions/front access
```

Only transfer from `s1` to `s2` when `s2` becomes empty.

---

## BRUTE FORCE:

### Idea

Maintain queue order inside `s1` at all times.

For every enqueue:

1. Move all elements from `s1 → s2`
2. Insert new element into `s1`
3. Move all elements back from `s2 → s1`

This guarantees:

```text
Front element is always at top of s1
```

### Example

#### enqueue(5)

```text
s1

Top
5
```

#### enqueue(3)

Move 5 → s2

Insert 3

Move back

```text
s1

Top
5
3
```

#### enqueue(4)

Move all to s2

Insert 4

Move back

```text
s1

Top
5
3
4
```

Notice:

```text
Queue:
Front → 5 3 4

Top of s1 = 5
```

Front always stays on top.

### Brute Force Code

```cpp
class myQueue {
    stack<int> s1, s2;

public:

    void enqueue(int x) {

        while(!s1.empty()) {
            s2.push(s1.top());
            s1.pop();
        }

        s1.push(x);

        while(!s2.empty()) {
            s1.push(s2.top());
            s2.pop();
        }
    }

    void dequeue() {
        if(!s1.empty())
            s1.pop();
    }

    int front() {
        if(s1.empty())
            return -1;

        return s1.top();
    }

    int size() {
        return s1.size();
    }
};
```

### Brute Force Time Complexity

| Operation | Complexity |
|------------|------------|
| enqueue | O(N) |
| dequeue | O(1) |
| front | O(1) |
| size | O(1) |

### Why?

For every enqueue:

```text
Move N elements out
Insert new element
Move N elements back
```

### Brute Force Space Complexity

```text
O(N)
```

All queue elements are stored across stacks.

---

## OPTIMAL APPROACH:

### Observation

Why are we rearranging elements during every enqueue?

Most inserted elements are never accessed immediately.

Instead:

```text
Insert everything into s1
```

Only when front/dequeue is requested and `s2` is empty:

```text
Transfer all elements from s1 → s2
```

This postpones the expensive reversal until it is actually needed.

---

## ALGORITHM:

### Data Structures

```cpp
stack<int> s1;
stack<int> s2;
```

### What Is Stored?

#### s1

Newly inserted elements.

#### s2

Elements ready to be removed.

### Invariant Maintained

Whenever `s2` is non-empty:

```text
s2.top() is always the queue front
```

### enqueue(x)

Push into `s1`.

### dequeue()

If `s2` is empty:

Transfer all elements from `s1 → s2`.

Then pop from `s2`.

### front()

If queue is empty:

Return `-1`.

If `s2` is empty:

Transfer all elements from `s1 → s2`.

Return `s2.top()`.

### size()

```cpp
return s1.size() + s2.size();
```

---

## DRY RUN:

### enqueue(5)

```text
s1

Top
5

s2

empty
```

---

### enqueue(3)

```text
s1

Top
3
5

s2

empty
```

---

### enqueue(4)

```text
s1

Top
4
3
5

s2

empty
```

Queue:

```text
Front → 5 3 4
```

---

### front()

`s2` is empty.

Transfer:

Move 4

```text
s2

Top
4
```

Move 3

```text
s2

Top
3
4
```

Move 5

```text
s2

Top
5
3
4
```

Return:

```cpp
s2.top()
```

Output:

```text
5
```

---

### dequeue()

`s2` already contains:

```text
Top
5
3
4
```

Pop:

```text
5
```

Now:

```text
s2

Top
3
4
```

Queue:

```text
Front → 3 4
```

---

### front()

`s2` not empty.

No transfer.

Return:

```text
3
```

---

### enqueue(10)

```text
s1

Top
10

s2

Top
3
4
```

Queue:

```text
Front → 3 4 10
```

---

### dequeue()

Remove:

```text
3
```

Now:

```text
s2

Top
4
```

---

### dequeue()

Remove:

```text
4
```

Now:

```text
s2 = empty
```

---

### front()

`s2` empty.

Transfer:

```text
s1

Top
10
```

↓

```text
s2

Top
10
```

Return:

```text
10
```

Correct queue front.

---

## IMPORTANT CODE SNIPPETS:

### Transfer Logic

```cpp
while(!s1.empty()) {
    s2.push(s1.top());
    s1.pop();
}
```

### Enqueue

```cpp
s1.push(x);
```

### Front Empty Check

```cpp
if(s1.empty() && s2.empty())
    return -1;
```

### Size

```cpp
return s1.size() + s2.size();
```

---

## COMMON MISTAKES:

### 1. Using pop() instead of top()

Wrong:

```cpp
return s2.pop();
```

Correct:

```cpp
return s2.top();
```

---

### 2. Popping Inside front()

`front()` should only view.

Never remove.

---

### 3. Forgetting Empty Queue Check

Need:

```cpp
if(s1.empty() && s2.empty())
    return -1;
```

---

### 4. Returning Only s1.size()

Wrong:

```cpp
return s1.size();
```

Correct:

```cpp
return s1.size() + s2.size();
```

---

### 5. Transferring Every Time

Transfer only when:

```text
s2 becomes empty
```

Otherwise amortized O(1) is lost.

---

## WHY I MIGHT FORGET THIS:

Because people focus on the code instead of the role of the stacks.

Remember:

```text
s1 = incoming
s2 = outgoing
```

New elements enter `s1`.

Old elements leave `s2`.

When `s2` becomes empty:

```text
Pour everything from s1 into s2
```

That's the entire solution.

---

## INTERVIEW FLOW:

1. Queue is FIFO.
2. Stack is LIFO.
3. One stack alone cannot efficiently support queue operations.
4. Use two stacks.
5. Brute Force:
   - Keep front on top.
   - Enqueue becomes O(N).
6. Optimization:
   - Delay reversal.
   - Use s1 for insertion.
   - Use s2 for deletion.
7. Transfer only when s2 becomes empty.
8. Each element moves at most once from s1 to s2.
9. Therefore dequeue/front become amortized O(1).

---

## TIME COMPLEXITY:

### Brute Force

| Operation | Complexity |
|------------|------------|
| enqueue | O(N) |
| dequeue | O(1) |
| front | O(1) |
| size | O(1) |

#### Reason

For every insertion:

```text
Move N elements out
Insert new element
Move N elements back
```

---

### Optimal

#### enqueue

```text
O(1)
```

Single push into s1.

---

#### dequeue

Worst Case:

```text
O(N)
```

Transfer may occur.

Amortized:

```text
O(1)
```

Each element is transferred at most once.

---

#### front

Worst Case:

```text
O(N)
```

Transfer may occur.

Amortized:

```text
O(1)
```

Each element is transferred at most once.

---

#### size

```text
O(1)
```

---

## SPACE COMPLEXITY:

### Brute Force

```text
O(N)
```

All queue elements are stored in stacks.

---

### Optimal

```text
O(N)
```

All queue elements are stored across `s1` and `s2`.

No extra storage proportional to N beyond the queue elements themselves.

---

## EDGE CASES:

### Empty Queue

```cpp
front() → -1
```

---

### dequeue() on Empty Queue

Should do nothing.

---

### Single Element Queue

```text
enqueue(5)

front() → 5

dequeue()

Queue becomes empty
```

---

### Multiple front() Calls

Should not remove elements.

---

### Elements Split Between Both Stacks

```cpp
size() = s1.size() + s2.size()
```

---

## PATTERN RECOGNITION:

Look for:

```text
Implement Queue using Stack
```

or

```text
Need FIFO behavior using LIFO structures
```

or

```text
Reverse insertion order before removal
```

### Key Signal

One stack stores incoming elements.

Another stack stores outgoing elements.

Transfer only when outgoing stack becomes empty.

This is the classic:

```text
Two Stack Queue Pattern
```

---

## CLEAN C++ CODE

```cpp
class myQueue {
    stack<int> s1, s2;

public:

    void enqueue(int x) {
        s1.push(x);
    }

    void dequeue() {

        if(s2.empty()) {
            while(!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }

        if(!s2.empty())
            s2.pop();
    }

    int front() {

        if(s1.empty() && s2.empty())
            return -1;

        if(s2.empty()) {
            while(!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }

        return s2.top();
    }

    int size() {
        return s1.size() + s2.size();
    }
};
```

---

## INTUITION BEHIND IMPORTANT LINES

### `s1.push(x);`

New arrivals always enter the incoming stack.

---

### `if(s2.empty())`

Only transfer when outgoing stack has no elements left.

---

### Transfer Logic

```cpp
while(!s1.empty()) {
    s2.push(s1.top());
    s1.pop();
}
```

Reverses the order.

Oldest element comes to the top.

---

### `return s2.top();`

Invariant guarantees:

```text
s2.top() is always the queue front
```

---

### `return s1.size() + s2.size();`

Queue elements can exist in either stack.

---

## EASY-TO-REMEMBER SUMMARY

```text
s1 = Incoming Stack
s2 = Outgoing Stack
```

### enqueue

```text
Push into s1
```

### front / dequeue

```text
If s2 is empty:
    Transfer s1 → s2

Then use s2
```

### Key Invariant

```text
Whenever s2 is non-empty,
s2.top() is the queue front.
```

### Why Amortized O(1)?

Each element:

1. Pushed once into s1
2. Transferred once to s2
3. Popped once from s2

Therefore:

enqueue → O(1)

front → O(1) amortized

dequeue → O(1) amortized
````
