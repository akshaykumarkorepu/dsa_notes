
## PROBLEM:

Given an array `arr[]`, find the **sum of the minimum element of every possible subarray**.

### Example

```text
arr = [3,1,2,4]
```

Subarrays and minimums:

```text
[3]       -> 3
[3,1]     -> 1
[3,1,2]   -> 1
[3,1,2,4] -> 1
[1]       -> 1
[1,2]     -> 1
[1,2,4]   -> 1
[2]       -> 2
[2,4]     -> 2
[4]       -> 4
```

Answer:

```text
17
```

---

## PATTERN:

**Monotonic Increasing Stack + Contribution Technique**

---

## WHY THIS PATTERN:

Brute force thinks:

```text
For every subarray, find the minimum.
```

Optimal thinks:

```text
For every element,
count how many subarrays use it as the minimum.
```

This converts:

```text
O(N²) / O(N³)
```

into

```text
O(N)
```

using:

- Previous Smaller Element (PSE)
- Next Smaller Element (NSE)

---

## CORE IDEA:

Instead of finding the minimum of every subarray:

Find the contribution of every element.

Contribution of `arr[i]`:

```text
arr[i] × (Number of subarrays where arr[i] is minimum)
```

To count those subarrays:

```cpp
left = i - PSE

right = NSE - i
```

Contribution:

```cpp
arr[i] * left * right
```

Final Answer:

```text
Σ(arr[i] × left × right)
```

---

## BRUTE FORCE:

### Intuition

Generate every subarray.

Keep track of the minimum while extending the subarray.

Add the minimum to the answer.

### Code

```cpp
int sumSubMins(vector<int>& arr) {

    int n = arr.size();
    int ans = 0;

    for(int i = 0; i < n; i++) {

        int mini = arr[i];

        for(int j = i; j < n; j++) {

            mini = min(mini, arr[j]);

            ans += mini;
        }
    }

    return ans;
}
```

### Dry Run

```text
arr = [3,1,2,4]

i = 0

[3]       -> 3
[3,1]     -> 1
[3,1,2]   -> 1
[3,1,2,4] -> 1

Contribution = 6
```

Continue similarly for all starting indices.

Final Answer:

```text
17
```

### Time Complexity

```text
O(N²)
```

### Space Complexity

```text
O(1)
```

---

## OPTIMAL APPROACH:

Contribution Technique + Monotonic Increasing Stack

For every element:

Find:

1. Previous Smaller Element (PSE)
2. Next Smaller Element (NSE)

Then:

```cpp
left = i - PSE

right = NSE - i
```

Contribution:

```cpp
arr[i] * left * right
```

---

## ALGORITHM:

### Step 1: Find Previous Smaller Element (PSE)

Traverse:

```text
Left → Right
```

Maintain a Monotonic Increasing Stack.

Store:

```text
Indices
```

Condition:

```cpp
while(!st.empty() && arr[st.top()] >= arr[i])
```

Pop greater and equal elements.

Store:

```cpp
pse[i]
```

Push current index.

---

### Step 2: Find Next Smaller Element (NSE)

Traverse:

```text
Right → Left
```

Maintain a Monotonic Increasing Stack.

Store:

```text
Indices
```

Condition:

```cpp
while(!st.empty() && arr[st.top()] > arr[i])
```

Pop only strictly greater elements.

Store:

```cpp
nse[i]
```

Push current index.

---

### Step 3: Calculate Contribution

```cpp
left = i - pse[i];

right = nse[i] - i;

ans += arr[i] * left * right;
```

Return answer.

---

## DRY RUN:

### Input

```text
arr = [3,1,2,4]
```

---

### PSE

#### i = 0

```text
Stack = []

pse[0] = -1

Push 0

Stack = [0]
```

---

#### i = 1

```text
Current = 1

3 >= 1

Pop

Stack = []

pse[1] = -1

Push 1

Stack = [1]
```

---

#### i = 2

```text
Current = 2

1 >= 2 ? No

pse[2] = 1

Push 2

Stack = [1,2]
```

---

#### i = 3

```text
Current = 4

2 >= 4 ? No

pse[3] = 2

Push 3

Stack = [1,2,3]
```

---

Final:

```text
pse = [-1,-1,1,2]
```

---

### NSE

#### i = 3

```text
Stack = []

nse[3] = 4

Push 3
```

---

#### i = 2

```text
Current = 2

4 > 2

Pop

Stack = []

nse[2] = 4

Push 2
```

---

#### i = 1

```text
Current = 1

2 > 1

Pop

Stack = []

nse[1] = 4

Push 1
```

---

#### i = 0

```text
Current = 3

1 > 3 ? No

nse[0] = 1

Push 0
```

---

Final:

```text
nse = [1,4,4,4]
```

---

### Contribution

For 3:

```text
left = 1
right = 1

Contribution = 3
```

---

For 1:

```text
left = 2
right = 3

Contribution = 6
```

---

For 2:

```text
left = 1
right = 2

Contribution = 4
```

---

For 4:

```text
left = 1
right = 1

Contribution = 4
```

---

Final Answer:

```text
3 + 6 + 4 + 4 = 17
```

---

## IMPORTANT CODE SNIPPETS:

### Previous Smaller Element

```cpp
while(!st.empty() &&
      arr[st.top()] >= arr[i]) {
    st.pop();
}
```

### Next Smaller Element

```cpp
while(!st.empty() &&
      arr[st.top()] > arr[i]) {
    st.pop();
}
```

### Contribution

```cpp
long long left = i - pse[i];

long long right = nse[i] - i;

ans += 1LL * arr[i] * left * right;
```

### Store Indices

```cpp
st.push(i);
```

Not:

```cpp
st.push(arr[i]);
```

because we need distances.

---

## COMMON MISTAKES:

### 1. Storing values instead of indices

Wrong:

```cpp
st.push(arr[i]);
```

Correct:

```cpp
st.push(i);
```

---

### 2. Using `>=` on both sides

Causes duplicate counting.

---

### 3. Using `>` on both sides

Misses duplicate subarrays.

---

### 4. Wrong contribution formula

Wrong:

```cpp
left = 1 - pse[i];
```

Correct:

```cpp
left = i - pse[i];
```

---

### 5. Wrong contribution

Wrong:

```cpp
ans += left * right * ans;
```

Correct:

```cpp
ans += arr[i] * left * right;
```

---

### 6. Forgetting to clear stack before NSE

```cpp
while(!st.empty()) {
    st.pop();
}
```

---

## WHY I MIGHT FORGET THIS:

Because the question looks like:

```text
Find minimum of every subarray.
```

But the actual trick is:

```text
Count how many subarrays use each element as minimum.
```

That mindset shift is the entire problem.

---

## INTERVIEW FLOW:

### Step 1

Explain brute force.

Generate all subarrays.

Maintain running minimum.

```text
O(N²)
```

---

### Step 2

Observation:

The same element becomes minimum in many subarrays.

---

### Step 3

Reverse thinking.

Instead of:

```text
Find minimum of every subarray
```

Do:

```text
Count subarrays where each element is minimum
```

---

### Step 4

Need:

- Previous Smaller Element
- Next Smaller Element

---

### Step 5

Formula:

```cpp
left = i - PSE

right = NSE - i
```

Contribution:

```cpp
arr[i] * left * right
```

---

### Step 6

Use Monotonic Increasing Stack.

---

### Step 7

Handle duplicates:

```text
PSE -> >=

NSE -> >
```

---

### Step 8

Overall complexity:

```text
O(N)
```

---

## TIME COMPLEXITY:

### Brute Force

Outer loop = N

Inner loop = N

```text
O(N²)
```

---

### Optimal

Finding PSE:

```text
O(N)
```

Finding NSE:

```text
O(N)
```

Contribution Loop:

```text
O(N)
```

Total:

```text
O(N)
```

### Why?

Every element:

```text
Pushed once
Popped once
```

Maximum.

Total stack operations:

```text
2N
```

Therefore:

```text
O(N)
```

This is amortized O(N).

---

## SPACE COMPLEXITY:

### Brute Force

```text
O(1)
```

---

### Optimal

PSE Array:

```text
O(N)
```

NSE Array:

```text
O(N)
```

Stack:

```text
O(N)
```

Total:

```text
O(N)
```

---

## EDGE CASES:

### Single Element

```text
[5]
```

Answer:

```text
5
```

---

### Strictly Increasing

```text
[1,2,3,4]
```

---

### Strictly Decreasing

```text
[4,3,2,1]
```

---

### All Equal

```text
[2,2,2]
```

Requires:

```text
PSE -> >=

NSE -> >
```

for correct duplicate handling.

---

### Large Values

Use:

```cpp
long long
```

for contribution calculation.

---

## PATTERN RECOGNITION:

Whenever you see:

- Sum of Subarray Minimums
- Sum of Subarray Maximums
- Contribution of elements
- Count subarrays where element is minimum
- Count subarrays where element is maximum
- Previous Smaller Element
- Next Smaller Element
- Previous Greater Element
- Next Greater Element
- Nearest Smaller / Greater

Think:

```text
Contribution Technique
+
Monotonic Stack
```

Questions from the same family:

- Sum of Subarray Maximums
- Largest Rectangle in Histogram
- Max of Min for Every Window Size
- Total Strength of Wizards
- Count Subarrays Where Element Is Minimum / Maximum

---

## CLEAN C++ CODE

```cpp
class Solution {
public:
    int sumSubMins(vector<int> &arr) {
        
        int n = arr.size();

        vector<int> pse(n);
        vector<int> nse(n);

        stack<int> st;

        for(int i = 0; i < n; i++) {

            while(!st.empty() && arr[st.top()] >= arr[i]) {
                st.pop();
            }

            pse[i] = st.empty() ? -1 : st.top();

            st.push(i);
        }

        while(!st.empty()) {
            st.pop();
        }

        for(int i = n - 1; i >= 0; i--) {

            while(!st.empty() && arr[st.top()] > arr[i]) {
                st.pop();
            }

            nse[i] = st.empty() ? n : st.top();

            st.push(i);
        }

        long long ans = 0;

        for(int i = 0; i < n; i++) {

            long long left = i - pse[i];
            long long right = nse[i] - i;

            ans += 1LL * arr[i] * left * right;
        }

        return (int)ans;
    }
};
```

---

## INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
st.push(i);
```

Store index because distances are needed later.

---

```cpp
arr[st.top()]
```

Stack stores indices, so use them to access values.

---

```cpp
arr[st.top()] >= arr[i]
```

Used in PSE to remove greater and equal elements.

---

```cpp
arr[st.top()] > arr[i]
```

Used in NSE to remove only greater elements and handle duplicates correctly.

---

```cpp
left = i - pse[i];
```

Number of valid starting positions.

---

```cpp
right = nse[i] - i;
```

Number of valid ending positions.

---

```cpp
arr[i] * left * right
```

Contribution of current element.

---

## EASY-TO-REMEMBER SUMMARY

```text
Don't find minimum of every subarray.

Find how many subarrays choose each element as minimum.

Contribution:

arr[i] × left × right

where

left  = i - PSE
right = NSE - i

Use Monotonic Increasing Stack.

Store indices.

PSE -> >=
NSE -> >

Answer = Σ(arr[i] × left × right)
```

This is the standard **Monotonic Stack + Contribution Pattern**.
````
