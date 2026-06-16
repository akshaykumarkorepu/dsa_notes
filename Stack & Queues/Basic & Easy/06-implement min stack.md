
## PROBLEM:

Design a stack that supports:

- `push(x)`
- `pop()`
- `peek()`
- `getMin()`
- `isEmpty()`

with:

```text
getMin() → O(1)
```

The challenge is maintaining the minimum element efficiently even after multiple push and pop operations.

---

## PATTERN:

**Auxiliary Information Per Stack Level (Encoded Min Stack Pattern)**

This is a special stack design pattern where we store additional information inside the stack itself to recover previous minimums.

---

## WHY THIS PATTERN:

A normal stack only gives access to:

```cpp
top()
```

If we want the minimum element:

```text
We would need to scan the entire stack.
```

which costs:

```text
O(n)
```

We need:

```text
getMin() = O(1)
```

Therefore we must somehow remember minimum information during every push/pop.

Instead of using:

```cpp
stack<pair<int,int>>
```

or

```cpp
mainStack + minStack
```

we encode minimum information directly inside the stack.

---

## CORE IDEA:

Maintain:

```cpp
stack<long long> st;
long long mini;
```

### What is stored?

The stack may contain:

#### Normal Values

```text
5
8
10
```

These are actual values.

#### Encoded Values

```text
1
-2
0
```

These are special markers created when a new minimum arrives.

Encoded values indicate:

```text
A new minimum was inserted here.
```

---

### Why elements are pushed?

To store user values.

If the new value becomes the minimum:

```cpp
x < mini
```

we encode it:

```cpp
2*x - mini
```

so the previous minimum can be recovered later.

---

### Why elements are popped?

To remove stack elements.

If the popped element is encoded:

```text
Current minimum is being removed.
```

so we restore the previous minimum.

---

### Invariant Maintained

At all times:

```cpp
mini
```

stores:

```text
Minimum element of the current stack.
```

---

### Why the stack is necessary

The stack preserves insertion order and allows us to restore minimums in reverse order when popping.

---

## BRUTE FORCE:

### Idea

Use:

```cpp
stack<pair<int,int>> st;
```

Store:

```cpp
{value, minTillHere}
```

Example:

```text
Push(5)

(5,5)
```

```text
Push(3)

(5,5)
(3,3)
```

```text
Push(7)

(5,5)
(3,3)
(7,3)
```

Current minimum:

```cpp
st.top().second
```

---

### Code

```cpp
stack<pair<int,int>> st;

void push(int x) {

    if(st.empty())
        st.push({x,x});
    else
        st.push({x, min(x, st.top().second)});
}

void pop() {

    if(st.empty()) return;

    st.pop();
}

int peek() {

    if(st.empty()) return -1;

    return st.top().first;
}

int getMin() {

    if(st.empty()) return -1;

    return st.top().second;
}
```

---

### Complexity

```text
push      O(1)
pop       O(1)
peek      O(1)
getMin    O(1)
```

Space:

```text
O(n)
```

because every element stores two integers.

---

### Why Optimize Further?

Although already O(1), we still store:

```cpp
(value, minTillHere)
```

for every element.

Can we achieve:

```text
O(1) extra space?
```

Yes.

That leads to the encoded min-stack approach.

---

## OPTIMAL APPROACH:

Use:

```cpp
stack<long long> st;
long long mini;
```

and encode values whenever a new minimum appears.

---

### Push Cases

#### Case 1: Stack Empty

```cpp
mini = x;
st.push(x);
```

---

#### Case 2: x >= mini

Normal push.

```cpp
st.push(x);
```

Minimum unchanged.

---

#### Case 3: x < mini

New minimum arrives.

Store:

```cpp
2*x - mini
```

Update:

```cpp
mini = x;
```

---

### Why Encoding Works

Suppose:

```text
old minimum = 5
new minimum = 3
```

Store:

```cpp
2*3 - 5
= 1
```

Notice:

```cpp
1 < 3
```

Encoded values are always smaller than the current minimum.

Therefore:

```cpp
st.top() < mini
```

means:

```text
Encoded value detected.
```

---

### Pop Cases

#### Normal Value

```cpp
st.top() >= mini
```

Simply pop.

---

#### Encoded Value

```cpp
st.top() < mini
```

Restore previous minimum:

```cpp
mini = 2*mini - st.top();
```

Then pop.

---

### Peek Cases

#### Normal Value

```cpp
st.top() >= mini
```

Return:

```cpp
st.top()
```

---

#### Encoded Value

```cpp
st.top() < mini
```

Return:

```cpp
mini
```

because actual top value equals current minimum.

---

## ALGORITHM:

### Push

```cpp
if stack empty
    mini = x
    push x

else if x >= mini
    push x

else
    push(2*x - mini)
    mini = x
```

---

### Pop

```cpp
if empty return

if top >= mini
    pop

else
    mini = 2*mini - top
    pop
```

---

### Peek

```cpp
if empty return -1

if top >= mini
    return top

return mini
```

---

### getMin

```cpp
return mini
```

---

## DRY RUN:

Operations:

```text
push(5)
push(8)
push(3)
push(2)
pop()
peek()
```

---

### push(5)

```text
Stored Stack = [5]
mini = 5
```

---

### push(8)

Normal value.

```text
Stored Stack = [5,8]
mini = 5
```

---

### push(3)

New minimum.

Encode:

```cpp
2*3 - 5
= 1
```

Store:

```text
Stored Stack = [5,8,1]
mini = 3
```

Actual stack:

```text
[5,8,3]
```

---

### push(2)

New minimum.

Encode:

```cpp
2*2 - 3
= 1
```

Store:

```text
Stored Stack = [5,8,1,1]
mini = 2
```

Actual stack:

```text
[5,8,3,2]
```

---

### pop()

Top:

```text
1
```

Since:

```cpp
1 < 2
```

encoded.

Restore:

```cpp
mini = 2*2 - 1
     = 3
```

Pop.

```text
Stored Stack = [5,8,1]
mini = 3
```

Actual stack:

```text
[5,8,3]
```

---

### peek()

Top:

```text
1
```

Since:

```cpp
1 < 3
```

encoded.

Return:

```cpp
mini
```

Output:

```text
3
```

---

## IMPORTANT CODE SNIPPETS:

### Encoding

```cpp
st.push(2LL*x - mini);
```

---

### Encoded Detection

```cpp
st.top() < mini
```

---

### Restore Previous Minimum

```cpp
mini = 2*mini - st.top();
```

---

### Return Actual Top

```cpp
if(st.top() < mini)
    return mini;
```

---

## COMMON MISTAKES:

### Mistake 1

Using:

```cpp
st.top
```

instead of:

```cpp
st.top()
```

---

### Mistake 2

Popping before restoring minimum.

Wrong:

```cpp
st.pop();
mini = 2*mini - st.top();
```

---

### Mistake 3

Using:

```cpp
stack<int>
```

instead of:

```cpp
stack<long long>
```

Encoding calculations can overflow.

---

### Mistake 4

Returning encoded value in peek().

Wrong:

```cpp
return st.top();
```

for encoded values.

---

## WHY I MIGHT FORGET THIS:

Because:

```cpp
2*x - mini
```

looks like a magic formula.

Remember:

```text
Encoded value stores enough information
to recover the previous minimum later.
```

And:

```cpp
mini = 2*mini - encoded
```

simply undoes the encoding.

---

## INTERVIEW FLOW:

### Step 1

Explain pair-stack solution.

```cpp
stack<pair<int,int>>
```

Store:

```cpp
(value, minTillHere)
```

All operations become O(1).

---

### Step 2

Observation:

```text
Still storing two values per element.
```

Can we reduce space?

---

### Step 3

Introduce encoding.

```cpp
2*x - mini
```

for new minimums.

---

### Step 4

Explain:

```cpp
st.top() < mini
```

detects encoded values.

---

### Step 5

Explain recovery:

```cpp
mini = 2*mini - st.top();
```

---

### Step 6

Conclude:

```text
All operations O(1)
Extra space O(1)
```

---

## TIME COMPLEXITY:

### push

```text
O(1)
```

---

### pop

```text
O(1)
```

---

### peek

```text
O(1)
```

---

### getMin

```text
O(1)
```

---

### isEmpty

```text
O(1)
```

---

## SPACE COMPLEXITY:

### Pair Stack Solution

```text
O(n)
```

Stores:

```cpp
(value, minTillHere)
```

for every element.

---

### Encoded Min Stack Solution

```text
O(1) extra space
```

Only:

```cpp
long long mini;
```

is maintained.

The stack itself stores exactly one value per element.

---

## EDGE CASES:

### Empty Stack

```cpp
peek() = -1
getMin() = -1
```

---

### Single Element

```text
push(5)

peek() = 5
getMin() = 5
```

---

### Strictly Increasing

```text
5 6 7 8
```

No encoded values.

---

### Strictly Decreasing

```text
5 4 3 2
```

Every push creates an encoded value.

---

### Duplicate Minimum

```text
5 3 3
```

Second 3 is pushed normally because:

```cpp
x >= mini
```

---

## PATTERN RECOGNITION:

Look for phrases like:

```text
Design a stack
Support getMin()/getMax()
All operations O(1)
```

Ask yourself:

```text
Can I store extra information
at each stack level?
```

First solution:

```cpp
stack<pair<int,int>>
```

If interviewer asks:

```text
Can you reduce extra space?
```

think:

```text
Encoded Min Stack Pattern
```

where:

```cpp
2*x - mini
```

stores hidden state and

```cpp
2*mini - encoded
```

recovers previous minimum.

---

# Clean C++ Code

```cpp
class SpecialStack {

    stack<long long> st;
    long long mini;

public:

    SpecialStack() {
    }

    void push(int x) {

        if(st.empty()) {
            mini = x;
            st.push(x);
        }

        else if(x >= mini) {
            st.push(x);
        }

        else {
            st.push(2LL * x - mini);
            mini = x;
        }
    }

    void pop() {

        if(st.empty()) return;

        if(st.top() >= mini) {
            st.pop();
        }
        else {
            mini = 2 * mini - st.top();
            st.pop();
        }
    }

    int peek() {

        if(st.empty()) return -1;

        if(st.top() >= mini)
            return st.top();

        return mini;
    }

    bool isEmpty() {

        return st.empty();
    }

    int getMin() {

        if(st.empty()) return -1;

        return mini;
    }
};
```

# Intuition Behind Every Important Line

### New Minimum Push

```cpp
st.push(2LL * x - mini);
```

Store a marker that remembers the old minimum.

---

### Update Minimum

```cpp
mini = x;
```

Current minimum changes.

---

### Detect Encoded Value

```cpp
st.top() < mini
```

Only encoded values satisfy this.

---

### Restore Previous Minimum

```cpp
mini = 2 * mini - st.top();
```

Undo the encoding.

---

### Encoded Top

```cpp
return mini;
```

Encoded value is not real top.

Current minimum is the actual top.

---

# Easy-to-Remember Summary

```text
Brute Force:
stack<pair<int,int>>
(value, minTillHere)

Optimal:
stack<long long> + mini

New minimum:
push(2*x - mini)

Detect encoded:
top < mini

Recover old minimum:
mini = 2*mini - top

Invariant:
mini always stores current minimum.

All operations:
O(1)

Extra Space:
O(1)
```
````
