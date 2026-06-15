
## PROBLEM:

Implement a Stack using Queue(s).

Operations:

- `push(x)` → Insert element at the top.
- `pop()` → Remove top element.
- `top()` → Return top element, else `-1`.
- `size()` → Return current stack size.

The challenge is that:

```text
Stack → LIFO (Last In First Out)
Queue → FIFO (First In First Out)
```

We must make a Queue behave like a Stack.

---

## PATTERN:

**Queue Rotation / Stack Simulation Using Queue**

This is a classic **data structure simulation** problem.

We use a queue but maintain an invariant so that the queue behaves exactly like a stack.

---

## WHY THIS PATTERN:

A queue naturally removes from the front.

A stack naturally removes from the top.

To make a queue behave like a stack:

```text
Front of Queue = Top of Stack
```

must always remain true.

Once this invariant is maintained:

```cpp
pop() = q.pop()
top() = q.front()
```

become very simple.

---

## CORE IDEA:

Maintain:

```text
Front of Queue = Stack Top
```

Whenever a new element is pushed:

1. Insert it into the queue.
2. Move all older elements behind it.

This guarantees that the newest element always reaches the front.

Example:

Before push(4):

```text
Front → 3 5
```

Insert 4:

```text
Front → 3 5 4
```

Rotate older elements:

```text
5 4 3
4 3 5
```

Final:

```text
Front → 4 3 5
```

Now:

```text
Front = Stack Top
```

---

## BRUTE FORCE:

### Using Two Queues

Maintain:

```cpp
queue<int> q1;
queue<int> q2;
```

Invariant:

```text
Front of q1 = Stack Top
```

### What is Stored?

```text
q1 stores the entire stack.

Front of q1 always stores the stack top.
```

### Why Push Elements?

We need the newly inserted element to become the stack top.

### Why Move Elements?

To place the new element before all previous elements.

### Push(x)

Put new element into q2.

Move all old elements from q1 into q2.

Swap q1 and q2.

### Code

```cpp
void push(int x) {

    q2.push(x);

    while(!q1.empty()) {
        q2.push(q1.front());
        q1.pop();
    }

    swap(q1, q2);
}
```

### Why This Works

Current stack:

```text
Top
 ↓
3
5
```

Stored as:

```text
q1 = 3 5
```

Push 4:

```text
q2 = 4
```

Move q1:

```text
q2 = 4 3 5
```

Swap:

```text
q1 = 4 3 5
```

Now:

```text
Front = 4
```

which is the new stack top.

### Dry Run

Push(5)

```text
q1 = 5
```

Push(3)

```text
q2 = 3

Move:
3 5

Swap

q1 = 3 5
```

Push(4)

```text
q2 = 4

Move:
4 3 5

Swap

q1 = 4 3 5
```

Top:

```text
4
```

Pop:

```text
3 5
```

Size:

```text
2
```

### Complexity

Push:

```text
O(n)
```

Pop:

```text
O(1)
```

Top:

```text
O(1)
```

Size:

```text
O(1)
```

Space:

```text
O(n)
```

### Transition To Optimal

Observation:

We are only rearranging elements.

Instead of using another queue, we can rotate elements inside the same queue.

---

## OPTIMAL APPROACH:

### Using One Queue

Maintain:

```text
Front of Queue = Stack Top
```

### What is Stored?

```text
The queue stores all stack elements.

Front always stores the current stack top.
```

### Why Push Elements?

To insert a new stack element.

### Why Rotate Elements?

To move all older elements behind the newly inserted element.

### Invariant Maintained

```text
Front of Queue = Stack Top
```

This invariant is maintained after every push.

---

## ALGORITHM:

### push(x)

```cpp
q.push(x);

int sz = q.size();

for(int i = 0; i < sz - 1; i++) {
    q.push(q.front());
    q.pop();
}
```

### pop()

```cpp
if(!q.empty())
    q.pop();
```

### top()

```cpp
if(q.empty())
    return -1;

return q.front();
```

### size()

```cpp
return q.size();
```

---

## DRY RUN:

Operations:

```text
push(5)
push(3)
push(4)
top()
pop()
size()
```

---

### push(5)

Insert:

```text
Front → 5
```

No rotations.

Stack:

```text
Top
 ↓
5
```

---

### push(3)

Insert:

```text
Front → 5 3
```

Rotate once:

```text
3 5
```

Final:

```text
Front → 3 5
```

Stack:

```text
Top
 ↓
3
5
```

---

### push(4)

Insert:

```text
Front → 3 5 4
```

Rotate 1:

```text
5 4 3
```

Rotate 2:

```text
4 3 5
```

Final:

```text
Front → 4 3 5
```

Stack:

```text
Top
 ↓
4
3
5
```

---

### top()

```text
Returns 4
```

---

### pop()

Remove front.

```text
Front → 3 5
```

---

### size()

```text
Returns 2
```

Output:

```text
[4, 2]
```

---

## IMPORTANT CODE SNIPPETS:

### Rotation Logic

```cpp
for(int i = 0; i < sz - 1; i++) {
    q.push(q.front());
    q.pop();
}
```

Meaning:

```text
Move every old element from front to rear.
```

This is the heart of the solution.

---

### Top Check

```cpp
if(q.empty())
    return -1;
```

Required because problem explicitly asks for `-1`.

---

### Size

```cpp
return q.size();
```

No empty check needed.

If queue is empty:

```cpp
q.size() = 0
```

which is already the correct answer.

---

## COMMON MISTAKES:

### Mistake 1

Returning -1 for size.

Wrong:

```cpp
if(q.empty())
    return -1;
```

Correct:

```cpp
return q.size();
```

---

### Mistake 2

Rotating size times instead of size-1.

Wrong:

```cpp
for(int i=0;i<sz;i++)
```

Correct:

```cpp
for(int i=0;i<sz-1;i++)
```

Reason:

The newly inserted element should not be moved.

---

### Mistake 3

Forgetting to store size before rotation.

Wrong:

```cpp
for(int i=0;i<q.size()-1;i++)
```

Queue changes during operations.

Store size first.

---

### Mistake 4

Thinking pop should return removed element.

Problem only asks:

```cpp
void pop()
```

So simply remove it.

---

## WHY I MIGHT FORGET THIS:

Most people focus on:

```text
Stack using Queue
```

instead of focusing on the invariant.

Remember:

```text
Front of Queue = Stack Top
```

Everything follows naturally from this.

---

## INTERVIEW FLOW:

### Step 1

State the challenge.

```text
Queue is FIFO.
Stack is LIFO.
Need to simulate stack behavior.
```

### Step 2

State invariant.

```text
Front of Queue must always represent Stack Top.
```

### Step 3

Explain brute force.

```text
Use two queues.
Place new element first.
Move old elements behind it.
```

### Step 4

Optimize.

```text
Instead of another queue,
rotate elements within the same queue.
```

### Step 5

Explain rotation.

```text
Move all old elements from front to rear.
New element automatically reaches front.
```

### Step 6

Discuss complexity.

---

## TIME COMPLEXITY:

### push()

Insertion:

```text
O(1)
```

Rotation:

```text
O(n)
```

Overall:

```text
O(n)
```

Reason:

```text
We may rotate all previously inserted elements once.
```

---

### pop()

Front deletion.

```text
O(1)
```

---

### top()

Access front.

```text
O(1)
```

---

### size()

Queue size retrieval.

```text
O(1)
```

---

## SPACE COMPLEXITY:

### Optimal

One queue stores all elements.

```text
O(n)
```

### Brute Force

Two queues together still store n elements.

```text
O(n)
```

Reason:

```text
All stack elements must be stored somewhere.
```

---

## EDGE CASES:

### Empty Stack

```cpp
top()
```

Return:

```text
-1
```

---

### Empty Stack

```cpp
size()
```

Return:

```text
0
```

---

### Single Element

```text
push(5)
pop()
```

Queue becomes empty.

Should work correctly.

---

### Multiple Duplicate Values

```text
5 5 5
```

Rotation logic remains valid.

---

## PATTERN RECOGNITION:

Look for:

```text
Implement Stack using Queue
Implement Queue using Stack
Simulate one data structure using another
```

Key clue:

```text
Need to reverse natural behavior of the underlying structure.
```

Ask:

```text
What invariant must be maintained?
```

Here:

```text
Front of Queue = Stack Top
```

Once that invariant is identified, the solution becomes straightforward.

---

# CLEAN C++ CODE

```cpp
class myStack {
    queue<int> q;

public:

    void push(int x) {

        q.push(x);

        int sz = q.size();

        for(int i = 0; i < sz - 1; i++) {
            q.push(q.front());
            q.pop();
        }
    }

    void pop() {

        if(!q.empty())
            q.pop();
    }

    int top() {

        if(q.empty())
            return -1;

        return q.front();
    }

    int size() {

        return q.size();
    }
};
```

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
q.push(x);
```

Insert new element.

---

```cpp
int sz = q.size();
```

Store current size so we know how many old elements exist.

---

```cpp
for(int i = 0; i < sz - 1; i++)
```

Rotate only old elements.

---

```cpp
q.push(q.front());
q.pop();
```

Move front element to rear.

---

```cpp
return q.front();
```

Front always stores stack top.

---

```cpp
return q.size();
```

Queue size equals stack size.

---

# EASY-TO-REMEMBER SUMMARY

Think only about this invariant:

```text
Front of Queue = Stack Top
```

To maintain it:

```text
Push:
Insert new element
Rotate all old elements

Pop:
Remove front

Top:
Return front

Size:
Return queue size
```

One-line memory trick:

```text
Push new element at rear, rotate old elements behind it, and the queue magically behaves like a stack.
```
````
