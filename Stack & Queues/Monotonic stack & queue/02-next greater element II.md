
## PROBLEM:

Given a **circular array**, find the **Next Greater Element (NGE)** for every element.

The Next Greater Element of `arr[i]` is the first element greater than `arr[i]` encountered while moving forward circularly.

Example:

```cpp
arr = [1,3,2,4]

Output:
[3,4,4,-1]
```

Explanation:

```text
1 -> 3
3 -> 4
2 -> 4
4 -> -1
```

---

## PATTERN:

**Monotonic Decreasing Stack + Circular Array Traversal**

---

## WHY THIS PATTERN:

This is a classic:

```text
Next Greater Element
```

problem.

Whenever you see:

```text
Next Greater
Next Smaller
Previous Greater
Previous Smaller
Nearest Greater/Smaller
First Greater on Left/Right
```

Think:

```text
Monotonic Stack
```

Because we need:

```text
First greater element on the right
```

and a monotonic stack can maintain only the useful candidates.

The only extra twist is:

```text
Circular Array
```

which means:

```text
Elements at the end may find answers at the beginning.
```

---

## CORE IDEA:

### Normal NGE

For:

```cpp
[5,1,2]
```

NGE of:

```cpp
2
```

Normally:

```text
Nothing exists on right
```

Answer:

```cpp
-1
```

---

### Circular NGE

Now:

```text
2 -> 5 -> 1
```

Answer:

```cpp
5
```

Therefore:

```text
We need access to the beginning of the array
after reaching the end.
```

To simulate this:

```cpp
i % n
```

and

```cpp
2*n traversal
```

---

## BRUTE FORCE:

### Intuition

For every element:

```cpp
arr[i]
```

check every other element circularly until:

```cpp
arr[idx] > arr[i]
```

If found:

```cpp
ans[i] = arr[idx]
```

Otherwise:

```cpp
-1
```

---

### Code

```cpp
vector<int> nextLargerElement(vector<int>& arr) {

    int n = arr.size();

    vector<int> ans(n, -1);

    for(int i = 0; i < n; i++) {

        for(int j = 1; j < n; j++) {

            int idx = (i + j) % n;

            if(arr[idx] > arr[i]) {
                ans[i] = arr[idx];
                break;
            }
        }
    }

    return ans;
}
```

---

### Circular Traversal Trick

```cpp
idx = (i+j)%n;
```

Meaning:

```text
Move j steps ahead circularly.
```

Example:

```cpp
arr = [5,1,2]
i = 2
```

| j | idx |
|---|---|
|1|0|
|2|1|

Visits:

```text
5 → 1
```

---

### Time Complexity

```text
O(n²)
```

---

### Space Complexity

```text
O(1)
```

---

### Why Optimize?

Same elements are scanned repeatedly.

Example:

```cpp
[1,3,2,4]
```

For:

```text
1 -> check 3,2,4
3 -> check 2,4
2 -> check 4
```

Repeated work.

---

## OPTIMAL APPROACH:

Use a:

```text
Monotonic Decreasing Stack
```

and process the array:

```text
from Right → Left
```

twice.

---

### What is stored in the stack?

```cpp
stack<int>
```

stores:

```text
Potential Next Greater Elements
```

Example:

```text
Stack:
4
3
1
```

means:

```text
4 can answer somebody
3 can answer somebody
1 can answer somebody
```

---

### Why are elements pushed?

Current element may become NGE for elements further left.

```cpp
st.push(arr[idx]);
```

---

### Why are elements popped?

If:

```cpp
st.top() <= current
```

then that element can never be NGE.

Example:

```text
Current = 5

Stack:
10
8
4
```

Can:

```text
4
```

be NGE for 5?

No.

Remove it.

---

### Invariant Maintained

Stack always remains:

```text
Strictly Decreasing
```

Example:

```text
10
8
6
3
```

Top is smallest.

This is called:

```text
Monotonic Decreasing Stack
```

---

### Why is stack necessary?

Without stack:

```text
Need repeated right-side scans.
```

With stack:

```text
Nearest greater candidate always available at top.
```

---

### Monotonic Property

After processing any element:

```text
All smaller/equal elements are removed.
```

Therefore:

```text
Stack contains only useful greater candidates.
```

---

### Amortized O(n) Analysis

Many people think:

```cpp
while(!st.empty())
```

makes it O(n²).

Wrong.

Each element:

```text
Pushed once
Popped once
```

Maximum operations:

```text
n pushes + n pops
```

Total:

```text
O(2n)
=
O(n)
```

---

## ALGORITHM:

### Step 1

Create:

```cpp
ans(n,-1)
stack<int> st
```

---

### Step 2

Traverse:

```cpp
for(int i=2*n-1;i>=0;i--)
```

Why?

```text
Process array twice.
```

Simulates circular traversal.

---

### Step 3

Get actual index.

```cpp
idx = i%n;
```

Example:

```text
i = 7 6 5 4 3 2 1 0

idx:
3 2 1 0 3 2 1 0
```

Array processed twice.

---

### Step 4

Remove useless elements.

```cpp
while(!st.empty() && st.top() <= arr[idx])
    st.pop();
```

---

### Step 5

Store answer.

```cpp
if(i<n)
```

Only during second pass.

```cpp
ans[idx] = st.empty() ? -1 : st.top();
```

---

### Step 6

Push current element.

```cpp
st.push(arr[idx]);
```

---

## DRY RUN:

Input:

```cpp
arr = [1,3,2,4]
```

---

### First Pass (Build Stack)

```text
i=7 idx=3 -> push 4

Stack:
[4]
```

```text
i=6 idx=2 -> push 2

Stack:
[4,2]
```

```text
i=5 idx=1

pop 2

push 3

Stack:
[4,3]
```

```text
i=4 idx=0

push 1

Stack:
[4,3,1]
```

---

### Second Pass (Actual Answers)

#### i=3 idx=3

Current:

```cpp
4
```

Pop:

```text
1
3
4
```

Stack empty.

```cpp
ans[3]=-1
```

Push:

```cpp
4
```

---

#### i=2 idx=2

Current:

```cpp
2
```

Top:

```cpp
4
```

Answer:

```cpp
ans[2]=4
```

Push:

```cpp
2
```

---

#### i=1 idx=1

Current:

```cpp
3
```

Pop:

```cpp
2
```

Top:

```cpp
4
```

Answer:

```cpp
ans[1]=4
```

Push:

```cpp
3
```

---

#### i=0 idx=0

Current:

```cpp
1
```

Top:

```cpp
3
```

Answer:

```cpp
ans[0]=3
```

---

Final:

```cpp
[3,4,4,-1]
```

---

## IMPORTANT CODE SNIPPETS:

### Circular Traversal (Brute Force)

```cpp
idx = (i+j)%n;
```

### Circular Traversal (Optimal)

```cpp
idx = i%n;
```

### Traverse Twice

```cpp
for(int i=2*n-1;i>=0;i--)
```

### Maintain Monotonic Stack

```cpp
while(!st.empty() && st.top() <= arr[idx])
```

### Store Answer

```cpp
if(i<n)
    ans[idx] = st.empty() ? -1 : st.top();
```

---

## COMMON MISTAKES:

### Mistake 1

Using:

```cpp
<
```

instead of:

```cpp
<=
```

Correct:

```cpp
while(st.top() <= arr[idx])
```

---

### Mistake 2

Traversing only once.

```cpp
for(int i=n-1;i>=0;i--)
```

Fails for circular arrays.

---

### Mistake 3

Forgetting:

```cpp
i%n
```

No circular behavior.

---

### Mistake 4

Storing answers during first pass.

Need:

```cpp
if(i<n)
```

---

## WHY I MIGHT FORGET THIS:

Because there are two patterns mixed together.

```text
Pattern 1:
Next Greater Element
→ Monotonic Stack

Pattern 2:
Circular Array
→ Traverse Twice
```

Remember:

```text
Circular NGE
=
Normal NGE
+
2*n traversal
+
i%n
```

---

## INTERVIEW FLOW:

1. Explain brute force O(n²).
2. Point out repeated scans.
3. Introduce monotonic decreasing stack.
4. Explain circular traversal requires 2*n processing.
5. Explain `i%n`.
6. Explain popping smaller/equal elements.
7. Explain stack top gives answer.
8. Explain amortized O(n).

---

## TIME COMPLEXITY:

### O(n)

Reason:

```text
Each element:
Pushed once
Popped once
```

Even though loop runs:

```cpp
2*n
```

still:

```text
O(2n)
=
O(n)
```

---

## SPACE COMPLEXITY:

### O(n)

Stack may contain all elements.

---

## EDGE CASES:

### Single Element

```cpp
[5]

Output:
[-1]
```

### All Equal

```cpp
[2,2,2]

Output:
[-1,-1,-1]
```

### Strictly Increasing

```cpp
[1,2,3,4]

Output:
[2,3,4,-1]
```

### Strictly Decreasing

```cpp
[4,3,2,1]

Output:
[-1,4,4,4]
```

### Duplicates

```cpp
[3,3,3,4]

Output:
[4,4,4,-1]
```

---

## PATTERN RECOGNITION:

Look for:

```text
Next Greater
Next Smaller
Previous Greater
Previous Smaller
Nearest Greater
Nearest Smaller
First Greater on Left/Right
```

Immediately think:

```text
Monotonic Stack
```

If question also says:

```text
Circular Array
```

Add:

```text
Traverse Twice
+
Modulo Indexing
```

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<int> nextLargerElement(vector<int>& arr) {

        int n = arr.size();

        vector<int> ans(n, -1);

        stack<int> st;

        for(int i = 2*n - 1; i >= 0; i--) {

            int idx = i % n;

            while(!st.empty() && st.top() <= arr[idx]) {
                st.pop();
            }

            if(i < n) {
                ans[idx] = st.empty() ? -1 : st.top();
            }

            st.push(arr[idx]);
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
for(int i=2*n-1;i>=0;i--)
```

Process array twice to simulate circular traversal.

```cpp
int idx=i%n;
```

Convert loop index into actual array index.

```cpp
while(!st.empty() && st.top() <= arr[idx])
```

Remove elements that can never be NGE.

```cpp
if(i<n)
```

Only second pass stores answers.

```cpp
ans[idx] = st.empty() ? -1 : st.top();
```

Top is nearest greater candidate.

```cpp
st.push(arr[idx]);
```

Current element may become NGE for future elements.

---

# Difference from Next Greater Element I

## Normal NGE

```cpp
for(int i=n-1;i>=0;i--)
```

Only one traversal.

No circular behavior.

---

## Circular NGE

```cpp
for(int i=2*n-1;i>=0;i--)
```

Need two traversals.

---

## Normal NGE

```cpp
arr[i]
```

---

## Circular NGE

```cpp
arr[i%n]
```

Wrap around to beginning.

---

## Memory Trick

```text
Normal NGE
=
Monotonic Stack

Circular NGE
=
Monotonic Stack
+
Traverse Twice
+
Modulo Indexing
```

# Easy-to-Remember Summary

```text
Next Greater Element?
→ Monotonic Decreasing Stack

Circular Array?
→ Traverse 2*n times

Need actual index?
→ i % n

Remove smaller/equal elements.
Stack top = answer.
Push current element.

Formula:

Circular NGE
=
Normal NGE
+
2*n Loop
+
i%n
```
````
