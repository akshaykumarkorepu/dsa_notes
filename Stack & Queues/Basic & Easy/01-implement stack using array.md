

## PROBLEM:
Implement a **Stack using an Array** of fixed size `n`.

Support the following operations:

- `push(x)` → Insert element at the top.
- `pop()` → Remove the top element.
- `peek()` → Return the top element.
- `isEmpty()` → Check whether stack is empty.
- `isFull()` → Check whether stack is full.

---

## PATTERN:

**Stack Simulation (Array-Based Stack)**

This is a fundamental Stack pattern where we manually implement stack operations using:

```text
Array + Top Pointer
```

This does **NOT** belong to:

- Monotonic Stack
- Next Greater Element
- Histogram
- Stock Span
- Parentheses Matching

It belongs to:

✅ **Stack Simulation**

---

## WHY THIS PATTERN:

A Stack follows:

```text
LIFO (Last In First Out)
```

The most recently inserted element must be removed first.

Example:

```text
Push: 10
Push: 20
Push: 30

Stack = [10,20,30]

Pop → 30
```

To support LIFO efficiently:

```text
Need access only to the top element.
```

An array + top pointer provides:

```text
Push  -> O(1)
Pop   -> O(1)
Peek  -> O(1)
```

---

## CORE IDEA:

Maintain:

```cpp
int top;
```

where:

```cpp
top = -1;
```

means:

```text
Stack is empty
```

The entire implementation revolves around moving `top`.

```text
Push -> top++
Pop  -> top--
Peek -> arr[top]
```

---

## BRUTE FORCE:

### Not Required

Reason:

- Optimal solution is obvious.
- No meaningful optimization progression exists.
- Interviewers directly expect stack implementation using an array.

---

## OPTIMAL APPROACH:

Use:

```cpp
vector<int> arr;
int top;
```

### Invariant Maintained

```text
top always points to the current top element.
```

Examples:

```text
Stack = []
top = -1
```

```text
Stack = [10]
top = 0
```

```text
Stack = [10,20,30]
top = 2
```

---

## ALGORITHM:

### Constructor

```cpp
arr.resize(n);
top = -1;
```

Create storage and mark stack as empty.

---

### Push

If stack is full:

```cpp
top == arr.size() - 1
```

do nothing.

Otherwise:

```cpp
top++;
arr[top] = x;
```

---

### Pop

If stack is empty:

```cpp
top == -1
```

do nothing.

Otherwise:

```cpp
top--;
```

---

### Peek

If stack empty:

```cpp
return -1;
```

Otherwise:

```cpp
return arr[top];
```

---

### isEmpty

```cpp
return top == -1;
```

---

### isFull

```cpp
return top == arr.size() - 1;
```

---

## DRY RUN:

### Create Stack

```cpp
myStack st(3);
```

State:

```text
arr = [0,0,0]
top = -1
```

---

### push(10)

Check:

```text
top == 2 ?
-1 == 2 -> No
```

Move top:

```text
top = 0
```

Insert:

```text
arr[0] = 10
```

State:

```text
Stack = [10]
top = 0
```

---

### push(20)

Move top:

```text
top = 1
```

Insert:

```text
arr[1] = 20
```

State:

```text
Stack = [10,20]
top = 1
```

---

### push(30)

Move top:

```text
top = 2
```

Insert:

```text
arr[2] = 30
```

State:

```text
Stack = [10,20,30]
top = 2
```

---

### peek()

```cpp
return arr[top];
```

```text
return arr[2]
```

Output:

```text
30
```

---

### pop()

```cpp
top--;
```

State:

```text
top = 1
```

Logical Stack:

```text
[10,20]
```

---

### peek()

```text
return arr[1]
```

Output:

```text
20
```

---

## IMPORTANT CODE SNIPPETS:

### Empty Stack Check

```cpp
if(top == -1)
```

---

### Full Stack Check

```cpp
if(top == arr.size() - 1)
```

---

### Push

```cpp
top++;
arr[top] = x;
```

---

### Pop

```cpp
top--;
```

---

### Peek

```cpp
return arr[top];
```

---

## COMMON MISTAKES:

### Mistake 1

```cpp
int top = -1;
```

inside constructor.

Wrong.

Creates a local variable.

Correct:

```cpp
top = -1;
```

---

### Mistake 2

Forgetting full stack check.

```cpp
if(top == arr.size() - 1)
```

Without this:

```text
Array Out Of Bounds
```

---

### Mistake 3

Forgetting empty stack check before pop.

```cpp
if(top == -1)
```

---

### Mistake 4

Using:

```cpp
return arr[top];
```

when stack is empty.

Must first check:

```cpp
top == -1
```

---

## WHY I MIGHT FORGET THIS:

Because people focus on:

```text
Array storage
```

instead of:

```text
Top pointer movement
```

Remember:

```text
Stack implementation = managing top correctly.
```

Everything else is secondary.

---

## INTERVIEW FLOW:

### Step 1

Identify:

```text
Stack follows LIFO.
```

---

### Step 2

Use:

```text
Array + Top Pointer
```

---

### Step 3

Initialize:

```cpp
top = -1;
```

---

### Step 4

Push:

```cpp
top++;
arr[top] = x;
```

---

### Step 5

Pop:

```cpp
top--;
```

---

### Step 6

Peek:

```cpp
arr[top]
```

---

### Step 7

Handle edge cases:

```cpp
top == -1
top == arr.size()-1
```

---

## TIME COMPLEXITY:

### push()

```text
O(1)
```

Only:

```cpp
top++;
arr[top] = x;
```

---

### pop()

```text
O(1)
```

Only:

```cpp
top--;
```

---

### peek()

```text
O(1)
```

Direct array access.

---

### isEmpty()

```text
O(1)
```

Single comparison.

---

### isFull()

```text
O(1)
```

Single comparison.

---

### Overall

```text
Time Complexity: O(1) per operation
```

---

## SPACE COMPLEXITY:

Array stores at most:

```text
n elements
```

Therefore:

```text
Space Complexity: O(n)
```

---

## EDGE CASES:

### Empty Stack

```cpp
peek()
```

Output:

```text
-1
```

---

### Empty Stack

```cpp
pop()
```

Do nothing.

---

### Full Stack

```cpp
push(x)
```

Do nothing.

---

### Capacity = 1

```text
Push one element
```

Immediately:

```cpp
isFull() == true
```

---

## PATTERN RECOGNITION:

Look for keywords:

```text
Implement Stack
Design Stack
Custom Stack
Stack using Array
Fixed Size Stack
Manual Stack Operations
```

Signals:

```text
Need LIFO behavior
Need push/pop from same end
Need O(1) operations
```

Pattern:

```text
Stack Simulation
```

Data Stored:

```text
Actual elements in array
```

What is maintained?

```text
top points to current top element
```

When do we push?

```text
Whenever insertion is requested
```

When do we pop?

```text
Whenever removal is requested
```

Why is popping correct?

```text
LIFO requires removing the most recently inserted element.
top always points to that element.
```

Invariant:

```text
top always points to the last valid stack element.
```

---

# Clean C++ Code

```cpp
class myStack {

    vector<int> arr;
    int top;

public:

    myStack(int n) {

        arr.resize(n);
        top = -1;
    }

    bool isEmpty() {

        return top == -1;
    }

    bool isFull() {

        return top == arr.size() - 1;
    }

    void push(int x) {

        if (top == arr.size() - 1)
            return;

        top++;

        arr[top] = x;
    }

    void pop() {

        if (top == -1)
            return;

        top--;
    }

    int peek() {

        if (top == -1)
            return -1;

        return arr[top];
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
arr.resize(n);
```

Create storage of size `n`.

---

```cpp
top = -1;
```

Stack initially empty.

---

```cpp
top++;
```

Move to next available slot.

---

```cpp
arr[top] = x;
```

Store new element.

---

```cpp
top--;
```

Remove current top logically.

---

```cpp
return arr[top];
```

Access current top element.

---

# Easy-to-Remember Summary

```text
top = -1  -> Empty

Push:
top++
arr[top] = x

Pop:
top--

Peek:
arr[top]

Full:
top == size-1

Empty:
top == -1
```

---

# Interviewer's Expected Thought Process

> This is a Stack Simulation problem. Since a stack follows LIFO, I only need to track the top element. I'll use an array for storage and maintain a `top` pointer. Push increments `top`, pop decrements `top`, peek returns `arr[top]`, and boundary conditions are handled using `top == -1` for empty and `top == arr.size()-1` for full.

---

# Pattern Identification Hints For Future Questions

If the question asks you to:

```text
Implement a stack
Design push/pop operations
Create your own stack
Simulate stack behavior
```

Think immediately:

```text
Array + Top Pointer
```

which is the classic:

```text
Stack Simulation Pattern
```
````
