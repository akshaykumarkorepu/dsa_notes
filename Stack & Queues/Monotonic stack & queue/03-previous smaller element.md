

## PROBLEM:

Given an array `arr[]`, find the **Previous Smaller Element (PSE)** for every element.

The Previous Smaller Element of `arr[i]` is:

> The first element on its left that is strictly smaller than `arr[i]`.

If no such element exists, return `-1`.

### Example

```cpp
arr = [1, 5, 0, 3, 4, 5]

Output:
[-1, 1, -1, 0, 3, 4]
```

---

## PATTERN:

### Brute Force Pattern

```text
Linear Search on Left Side
```

For every element, scan all elements to its left until a smaller element is found.

### Optimal Pattern

```text
Monotonic Increasing Stack
```

Maintain useful smaller candidates from the left.

---

## WHY THIS PATTERN:

For every element we need:

```text
Nearest Smaller Element on the LEFT
```

Keywords:

- Previous
- Nearest
- Smaller

These keywords strongly indicate a **Monotonic Stack**.

Instead of repeatedly searching the left side, we keep only useful candidates inside a stack.

---

## CORE IDEA:

For current element:

```cpp
arr[i]
```

We need the nearest smaller element on its left.

### Observation

If stack top is:

```cpp
>= arr[i]
```

then it cannot be the answer because:

```text
Previous Smaller requires STRICTLY smaller.
```

So remove it.

After removing all such elements:

```cpp
stack.top()
```

becomes the nearest smaller element.

If stack becomes empty:

```cpp
answer = -1
```

Then push current element because it may help future elements.

---

## BRUTE FORCE:

### Intuition

For every element:

1. Move left.
2. Find first smaller element.
3. Store it.
4. If none found → -1.

### Code

```cpp
class Solution {
  public:
    vector<int> prevSmaller(vector<int>& arr) {
        int n = arr.size();
        
        vector<int> ans(n,-1);
        
        for(int i=n-1;i>=0;i--){
            for(int j=i-1;j>=0;j--){
                
                if(arr[j]<arr[i]){
                    ans[i]=arr[j];
                    break;
                }
            }
        }
        
        return ans;
    }
};
```

### Dry Run

```cpp
arr = [1,6,2]
```

#### i = 2

```cpp
arr[2] = 2
```

Check left:

```cpp
6 < 2 ? No
1 < 2 ? Yes
```

Answer:

```cpp
ans[2] = 1
```

#### i = 1

```cpp
arr[1] = 6
```

Check left:

```cpp
1 < 6
```

Answer:

```cpp
ans[1] = 1
```

#### i = 0

No left elements.

```cpp
ans[0] = -1
```

Final:

```cpp
[-1,1,1]
```

### Time Complexity

```cpp
O(N²)
```

### Space Complexity

```cpp
O(N)
```

---

## OPTIMAL APPROACH:

### Monotonic Increasing Stack

Maintain stack such that:

```text
Bottom → Top

Increasing Order
```

Example:

```cpp
[0,3,4]
```

### What is stored in the stack?

```cpp
Array values
```

Example:

```cpp
stack = [0,3,4]
```

Each value is a potential previous smaller element for future elements.

### Why are elements pushed?

Current element may become the previous smaller element for upcoming elements.

```cpp
st.push(arr[i]);
```

### Why are elements popped?

If:

```cpp
st.top() >= arr[i]
```

then top cannot be previous smaller for current element.

So remove it.

```cpp
st.pop();
```

### What invariant is maintained?

The stack is always:

```text
Strictly Increasing
```

from bottom to top.

Example:

```cpp
[1,3,7]
```

### Why is stack necessary?

Without stack:

```cpp
For every element,
search left side again.
```

Cost:

```cpp
O(N²)
```

Stack stores only useful candidates.

Result:

```cpp
O(N)
```

---

## ALGORITHM:

For every element from left to right:

### Step 1

Remove all invalid candidates.

```cpp
while(!st.empty() && st.top() >= arr[i])
{
    st.pop();
}
```

### Step 2

Find answer.

```cpp
if(st.empty())
    ans[i] = -1;
else
    ans[i] = st.top();
```

### Step 3

Push current element.

```cpp
st.push(arr[i]);
```

---

## DRY RUN:

```cpp
arr = [1,5,0,3,4,5]
```

### Start

```cpp
stack = []
ans = []
```

### i = 0

```cpp
current = 1
```

Stack empty.

```cpp
ans = [-1]
```

Push.

```cpp
stack = [1]
```

### i = 1

```cpp
current = 5
```

Check:

```cpp
1 >= 5 ? No
```

Answer:

```cpp
1
```

Push:

```cpp
stack = [1,5]
```

### i = 2

```cpp
current = 0
```

Pop:

```cpp
5 >= 0 → pop
1 >= 0 → pop
```

Stack:

```cpp
[]
```

Answer:

```cpp
-1
```

Push:

```cpp
stack = [0]
```

### i = 3

```cpp
current = 3
```

Check:

```cpp
0 >= 3 ? No
```

Answer:

```cpp
0
```

Push:

```cpp
stack = [0,3]
```

### i = 4

```cpp
current = 4
```

Answer:

```cpp
3
```

Push:

```cpp
stack = [0,3,4]
```

### i = 5

```cpp
current = 5
```

Answer:

```cpp
4
```

Push:

```cpp
stack = [0,3,4,5]
```

Final:

```cpp
[-1,1,-1,0,3,4]
```

---

## IMPORTANT CODE SNIPPETS:

### Remove Bigger/Equal Elements

```cpp
while(!st.empty() && st.top() >= arr[i]){
    st.pop();
}
```

### Find Previous Smaller

```cpp
if(st.empty())
    ans[i] = -1;
else
    ans[i] = st.top();
```

### Push Current Element

```cpp
st.push(arr[i]);
```

### Complete Template

```cpp
for(int i=0;i<n;i++){

    while(!st.empty() && st.top() >= arr[i]){
        st.pop();
    }

    ans[i] = st.empty() ? -1 : st.top();

    st.push(arr[i]);
}
```

---

## COMMON MISTAKES:

### Mistake 1

Using:

```cpp
st.top() > arr[i]
```

instead of

```cpp
st.top() >= arr[i]
```

Example:

```cpp
[2,2]
```

Output becomes:

```cpp
[-1,2]
```

Wrong.

Need:

```cpp
[-1,-1]
```

because smaller must be strict.

### Mistake 2

Pushing before finding answer.

Wrong:

```cpp
st.push(arr[i]);
ans[i] = st.top();
```

Current element becomes its own answer.

### Mistake 3

Forgetting empty check.

```cpp
st.top();
```

on empty stack causes runtime error.

### Mistake 4

Using decreasing stack.

For Previous Smaller:

```text
Increasing Stack
```

is required.

---

## WHY I MIGHT FORGET THIS:

Because the brute-force solution feels natural:

```cpp
For every element,
look left.
```

But interviewers expect:

```text
Nearest Smaller
→ Monotonic Stack
```

Remember:

```text
Previous + Smaller
=
Monotonic Increasing Stack
```

---

## INTERVIEW FLOW:

### Step 1

Explain brute force.

```text
For every element,
scan left until smaller found.

O(N²)
```

### Step 2

Observation.

Many larger elements become useless.

Example:

```cpp
1 5 4
```

When 4 arrives:

```cpp
5 can never help.
```

Remove it.

### Step 3

Use Monotonic Increasing Stack.

Maintain:

```text
Increasing Order
```

### Step 4

For every element:

```text
Pop >= current
Top = answer
Push current
```

### Step 5

Complexity.

```cpp
O(N)
```

because every element is pushed once and popped once.

---

## TIME COMPLEXITY:

### Brute Force

```cpp
O(N²)
```

Reason:

For every element we may scan the entire left side.

### Optimal

```cpp
O(N)
```

### Amortized Analysis

Each element:

```cpp
Pushed once
Popped once
```

Maximum stack operations:

```cpp
2N
```

Therefore:

```cpp
O(N)
```

---

## SPACE COMPLEXITY:

### Brute Force

```cpp
O(N)
```

for answer array.

### Optimal

Answer array:

```cpp
O(N)
```

Stack:

```cpp
O(N)
```

Total auxiliary:

```cpp
O(N)
```

---

## EDGE CASES:

### Single Element

```cpp
[5]

Output:
[-1]
```

### Strictly Increasing

```cpp
[1,2,3,4]

Output:
[-1,1,2,3]
```

### Strictly Decreasing

```cpp
[4,3,2,1]

Output:
[-1,-1,-1,-1]
```

### All Equal

```cpp
[2,2,2]

Output:
[-1,-1,-1]
```

### Duplicates

```cpp
[1,2,2,3]

Output:
[-1,1,1,2]
```

---

## PATTERN RECOGNITION:

Whenever you see:

- Previous Smaller
- Previous Greater
- Next Smaller
- Next Greater
- Nearest Smaller
- Nearest Greater

Think:

```text
MONOTONIC STACK
```

### Quick Mapping

| Problem | Stack Type |
|----------|------------|
| Previous Smaller | Increasing Stack |
| Next Smaller | Increasing Stack |
| Previous Greater | Decreasing Stack |
| Next Greater | Decreasing Stack |

---

## CLEAN C++ CODE (BRUTE FORCE)

```cpp
class Solution {
  public:
    vector<int> prevSmaller(vector<int>& arr) {
        int n = arr.size();
        
        vector<int> ans(n,-1);
        
        for(int i=n-1;i>=0;i--){
            for(int j=i-1;j>=0;j--){
                
                if(arr[j]<arr[i]){
                    ans[i]=arr[j];
                    break;
                }
            }
        }
        
        return ans;
    }
};
```

---

## CLEAN C++ CODE (OPTIMAL)

```cpp
class Solution {
  public:
    vector<int> prevSmaller(vector<int>& arr) {
        int n = arr.size();
        
        vector<int> ans(n);
        stack<int> st;
        
        for(int i=0;i<n;i++){
            
            while(!st.empty() && st.top() >= arr[i]){
                st.pop();
            }
            
            if(st.empty()){
                ans[i] = -1;
            }
            else{
                ans[i] = st.top();
            }
            
            st.push(arr[i]);
        }
        
        return ans;
    }
};
```

---

## EASY-TO-REMEMBER SUMMARY

```text
Previous Smaller Element

Brute Force:
For every element,
search left until smaller found.

O(N²)

Optimal:
Use Monotonic Increasing Stack.

Steps:
1. Pop all elements >= current.
2. Top becomes Previous Smaller.
3. If stack empty → -1.
4. Push current.

Stack stores:
Potential previous smaller candidates.

Why O(N)?
Every element is pushed once and popped once.

Pattern:
Previous/Next + Greater/Smaller
=> Monotonic Stack.
```

### One-Line Memory Trick

> **For Previous Smaller, remove all bigger/equal elements; whatever remains on top is the nearest smaller element.**
````
