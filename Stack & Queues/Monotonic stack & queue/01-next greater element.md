

## PROBLEM:

Given an array `arr[]`, find the **Next Greater Element (NGE)** for every element.

The Next Greater Element of `arr[i]` is the **first element on the right side that is greater than arr[i]**.

If no such element exists, answer is `-1`.

### Example

```text
arr = [1,3,2,4]

1 → 3
3 → 4
2 → 4
4 → -1

Answer = [3,4,4,-1]
```

---

## PATTERN:

**Monotonic Decreasing Stack**

---

## WHY THIS PATTERN:

The question asks:

```text
Find the FIRST greater element on the RIGHT.
```

This is one of the strongest indicators of a **Monotonic Stack** problem.

Without a stack:

```text
For every element,
search all elements on its right.
```

which becomes O(n²).

The stack helps us:

```text
Remember useful candidates from the right side
and avoid repeated searching.
```

---

## CORE IDEA:

Process elements from:

```text
Right → Left
```

because we need information from the right side.

For every element:

```text
1. Remove all smaller/equal elements.
2. Top of stack becomes answer.
3. Push current element.
```

### Memory Trick

```text
POP
↓
ANSWER
↓
PUSH
```

---

## BRUTE FORCE:

### Idea

For every element:

```text
Search towards the right.
The first greater element found is the answer.
```

### Code

```cpp
vector<int> nextLargerElement(vector<int>& arr) {

    int n = arr.size();

    vector<int> ans(n, -1);

    for(int i = 0; i < n; i++) {

        for(int j = i + 1; j < n; j++) {

            if(arr[j] > arr[i]) {
                ans[i] = arr[j];
                break;
            }
        }
    }

    return ans;
}
```

### Dry Run

```text
arr = [1,3,2,4]

For 1:
Check 3
3 > 1
ans[0] = 3

For 3:
Check 2
Check 4
4 > 3
ans[1] = 4

For 2:
Check 4
ans[2] = 4

For 4:
No greater element
ans[3] = -1

Final Answer:
[3,4,4,-1]
```

### Time Complexity

```text
O(n²)
```

### Space Complexity

```text
O(1)
```

### Why Optimize?

For:

```text
n = 10^6
```

O(n²) becomes:

```text
10^12 operations
```

Too large.

---

## OPTIMAL APPROACH:

### Monotonic Decreasing Stack

We process from right to left.

The stack stores:

```text
Potential next greater elements.
```

### What is stored in the stack?

```text
Actual element values
```

Example:

```text
Stack = [8,5,3]
```

Not indices because we only need the next greater value.

---

### Why elements are pushed?

Current element may become the next greater element for some element on its left.

Example:

```text
[1,3]
```

When processing 3:

```text
Push 3
```

Later:

```text
3 becomes answer for 1
```

---

### Why elements are popped?

If:

```cpp
st.top() <= arr[i]
```

then that element cannot be the next greater element for current element.

Example:

```text
Current = 7

Stack:
10
6
5
```

5 cannot help.

Pop.

6 cannot help.

Pop.

Remain:

```text
10
```

which can help.

---

### Invariant Maintained

The stack always remains:

```text
Strictly decreasing
```

from bottom to top.

Example:

```text
[10,8,5,2]
```

Valid.

---

### Monotonic Property

After popping smaller/equal elements:

```text
Top always contains the nearest greater candidate.
```

---

### Why is Stack Necessary?

Without stack:

```text
Repeated scanning of right side.
```

With stack:

```text
Useful information is reused.
```

Time improves:

```text
O(n²) → O(n)
```

---

## ALGORITHM:

For every element from right to left:

### Step 1

Remove smaller/equal elements.

```cpp
while(!st.empty() && st.top() <= arr[i])
{
    st.pop();
}
```

### Step 2

Find answer.

```cpp
if(!st.empty())
{
    ans[i] = st.top();
}
```

Otherwise answer remains:

```text
-1
```

### Step 3

Push current element.

```cpp
st.push(arr[i]);
```

---

## DRY RUN:

```text
arr = [1,3,2,4]

Initially:

Stack = []
ans = [-1,-1,-1,-1]
```

### i = 3

```text
Current = 4

Stack = []

No greater element

ans[3] = -1

Push 4

Stack = [4]
```

---

### i = 2

```text
Current = 2

Stack = [4]

4 > 2

ans[2] = 4

Push 2

Stack = [4,2]
```

---

### i = 1

```text
Current = 3

Stack = [4,2]

2 <= 3

Pop 2

Stack = [4]

4 > 3

ans[1] = 4

Push 3

Stack = [4,3]
```

---

### i = 0

```text
Current = 1

Stack = [4,3]

3 > 1

ans[0] = 3

Push 1

Stack = [4,3,1]
```

Final:

```text
ans = [3,4,4,-1]
```

---

## IMPORTANT CODE SNIPPETS:

### Pop Smaller Elements

```cpp
while(!st.empty() && st.top() <= arr[i])
{
    st.pop();
}
```

### Find Answer

```cpp
if(!st.empty())
{
    ans[i] = st.top();
}
```

### Push Current Element

```cpp
st.push(arr[i]);
```

### Complete Pattern

```cpp
for(int i=n-1;i>=0;i--)
{
    while(!st.empty() && st.top() <= arr[i])
    {
        st.pop();
    }

    if(!st.empty())
    {
        ans[i] = st.top();
    }

    st.push(arr[i]);
}
```

---

## COMMON MISTAKES:

### Mistake 1

Using:

```cpp
st.top() < arr[i]
```

instead of:

```cpp
st.top() <= arr[i]
```

Example:

```text
[5,5]

Answer:
[-1,-1]
```

Equal element is NOT greater.

---

### Mistake 2

Forgetting to push current element.

```cpp
st.push(arr[i]);
```

must happen every iteration.

---

### Mistake 3

Finding answer before popping.

Wrong order:

```text
Answer
Pop
Push
```

Correct order:

```text
Pop
Answer
Push
```

---

### Mistake 4

Traversing left to right without proper logic.

For NGE, right-to-left is cleaner.

---

## WHY I MIGHT FORGET THIS:

Because all these problems look similar:

```text
Next Greater
Next Smaller
Previous Greater
Previous Smaller
```

Confusion usually comes from:

```text
1. Traversal direction
2. Pop condition
```

Remember:

```text
Need RIGHT side information
→ Traverse Right → Left
```

Need GREATER element:

```text
Remove Smaller Elements
```

---

## INTERVIEW FLOW:

### Step 1

Start with brute force.

```text
For every element,
search right side.

O(n²)
```

### Step 2

Explain inefficiency.

```text
Same elements are checked repeatedly.
```

### Step 3

Observation.

```text
Need first greater element on right.
```

This suggests:

```text
Monotonic Stack
```

### Step 4

Explain stack.

```text
Store useful greater candidates.
```

### Step 5

Process right to left.

```text
Pop useless elements.
Top becomes answer.
Push current.
```

### Step 6

Explain O(n).

```text
Each element:
Pushed once
Popped once
```

---

## TIME COMPLEXITY:

### Time

```text
O(n)
```

### Reason

Each element:

```text
Pushed once
Popped once at most
```

Total operations:

```text
≤ 2n
```

Hence:

```text
O(n)
```

### Amortized Analysis

Although:

```cpp
while(...)
```

appears inside a loop,

an element once popped never returns.

Therefore total pops across entire algorithm:

```text
≤ n
```

Overall:

```text
O(n)
```

---

## SPACE COMPLEXITY:

```text
O(n)
```

Worst case:

```text
arr = [5,4,3,2,1]
```

Entire array remains in stack.

---

## EDGE CASES:

### Single Element

```text
[10]

Answer:
[-1]
```

### Increasing Array

```text
[1,2,3,4]

Answer:
[2,3,4,-1]
```

### Decreasing Array

```text
[4,3,2,1]

Answer:
[-1,-1,-1,-1]
```

### Duplicate Values

```text
[5,5,5]

Answer:
[-1,-1,-1]
```

Must use:

```cpp
<=
```

while popping.

### Large Input

```text
n = 10^6
```

Only O(n) solution works.

---

## PATTERN RECOGNITION:

Think **Monotonic Stack** whenever you see:

```text
Next Greater Element
Next Smaller Element
Previous Greater Element
Previous Smaller Element
Nearest Greater
Nearest Smaller
First Greater on Left/Right
First Smaller on Left/Right
Stock Span
Daily Temperatures
Largest Rectangle in Histogram
```

### Quick Rule

If the question says:

```text
Find the nearest/first element
on left/right satisfying a comparison
```

Think:

```text
MONOTONIC STACK
```

### For Next Greater Specifically

```text
Need Right Side Information
→ Traverse Right → Left

Need Greater Element
→ Remove Smaller/Equal Elements

Top after popping
→ Answer
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    vector<int> nextLargerElement(vector<int>& arr) {

        int n = arr.size();

        vector<int> ans(n, -1);
        stack<int> st;

        for(int i = n - 1; i >= 0; i--) {

            while(!st.empty() && st.top() <= arr[i]) {
                st.pop();
            }

            if(!st.empty()) {
                ans[i] = st.top();
            }

            st.push(arr[i]);
        }

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Create Answer Array

```cpp
vector<int> ans(n, -1);
```

Assume no greater element exists initially.

---

### Create Stack

```cpp
stack<int> st;
```

Stores potential next greater elements.

---

### Traverse Right to Left

```cpp
for(int i=n-1;i>=0;i--)
```

Need information from the right side.

---

### Remove Useless Elements

```cpp
while(!st.empty() && st.top() <= arr[i])
```

Smaller/equal elements can never be NGE.

---

### Store Answer

```cpp
if(!st.empty())
{
    ans[i] = st.top();
}
```

Top is nearest greater element.

---

### Push Current Element

```cpp
st.push(arr[i]);
```

Current element may become answer for future elements.

---

# EASY-TO-REMEMBER SUMMARY

```text
NEXT GREATER ELEMENT

1. Traverse Right → Left

2. Pop all smaller/equal elements

3. Top becomes answer

4. Push current element

POP
↓
ANSWER
↓
PUSH
```

### One-Line Memory Trick

> Need first greater on right → Traverse right-to-left, pop smaller elements, top is answer, then push current.
````
