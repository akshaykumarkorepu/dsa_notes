
## PROBLEM:
Given an array, find all **leaders**.

A **leader** is an element that is **greater than or equal to every element on its right**.

- The last element is always a leader.
- Equality is allowed (`>=`).

Example:

```text
arr = [16,17,4,3,5,2]

Output = [17,5,2]
```

---

## PATTERN:
**Reverse Traversal + Running Maximum (Suffix Maximum)**

---

## WHY THIS PATTERN:

The condition depends **only on elements to the right**.

Whenever a question asks:

- compare with all elements on the right
- greatest/smallest element on the right
- future elements decide the answer

Think:

> **Traverse from right to left while maintaining suffix information.**

Here, the only suffix information needed is:

> **Maximum element seen so far from the right.**

---

## CORE IDEA:

Instead of asking

> "Is there any element greater than me on the right?"

Ask

> "What is the maximum element on my right?"

If

```cpp
current >= maxRight
```

then current is automatically greater than or equal to every element on its right.

So it is a leader.

---

## BRUTE FORCE:

### Idea

For every element,

scan every element on its right.

If any element is larger,

it is not a leader.

Otherwise it is.

### Code

```cpp
vector<int> leaders(vector<int>& arr) {

    vector<int> ans;

    int n = arr.size();

    for (int i = 0; i < n; i++) {

        bool leader = true;

        for (int j = i + 1; j < n; j++) {

            if (arr[j] > arr[i]) {
                leader = false;
                break;
            }
        }

        if (leader)
            ans.push_back(arr[i]);
    }

    return ans;
}
```

### Dry Run

```text
arr = [16,17,4,3,5,2]

16
↓

17 is greater

Not leader

----------------

17

4
3
5
2

No greater element

Leader

----------------

4

3
5 ← greater

Not leader

----------------

3

5 ← greater

Not leader

----------------

5

2

No greater

Leader

----------------

2

No elements on right

Leader

Answer = [17,5,2]
```

### Time Complexity

```
O(N²)
```

Every element scans all elements on its right.

### Space Complexity

```
O(1)
```

Ignoring output array.

---

## OPTIMAL APPROACH:

Traverse **from right to left** while maintaining

```cpp
maxRight
```

If

```cpp
arr[i] >= maxRight
```

then current element is a leader.

Store it.

Update

```cpp
maxRight = max(maxRight, arr[i]);
```

Finally reverse the answer.

---

## ALGORITHM:

### Step 1

Initialize

```cpp
maxRight = INT_MIN;
```

### Step 2

Traverse from

```
Right → Left
```

### Step 3

For every element

If

```cpp
arr[i] >= maxRight
```

Store it.

### Step 4

Update

```cpp
maxRight = max(maxRight, arr[i]);
```

### Step 5

Reverse answer.

Return.

---

## DRY RUN:

```text
arr = [16,17,4,3,5,2]

ans = []

maxRight = -∞
```

### i = 5

```text
Current = 2

2 >= -∞

Leader

ans = [2]

maxRight = 2
```

### i = 4

```text
Current = 5

5 >= 2

Leader

ans = [2,5]

maxRight = 5
```

### i = 3

```text
Current = 3

3 >= 5

No

ans = [2,5]

maxRight = 5
```

### i = 2

```text
Current = 4

4 >= 5

No

ans = [2,5]

maxRight = 5
```

### i = 1

```text
Current = 17

17 >= 5

Leader

ans = [2,5,17]

maxRight = 17
```

### i = 0

```text
Current = 16

16 >= 17

No

ans = [2,5,17]

maxRight = 17
```

Current answer

```text
[2,5,17]
```

Reverse

```text
[17,5,2]
```

Done.

---

## IMPORTANT CODE SNIPPETS:

### Reverse Traversal

```cpp
for (int i = n - 1; i >= 0; i--)
```

### Leader Check

```cpp
if (arr[i] >= maxRight)
```

Remember:

```
>=

NOT >
```

### Update Running Maximum

```cpp
maxRight = max(maxRight, arr[i]);
```

### Reverse Result

```cpp
reverse(ans.begin(), ans.end());
```

---

## COMMON MISTAKES:

### Mistake 1

Using

```cpp
>
```

instead of

```cpp
>=
```

Fails for

```text
[10,4,2,4,1]
```

Both 4s are leaders.

---

### Mistake 2

Updating

```cpp
maxRight
```

before checking.

Wrong

```cpp
maxRight = max(maxRight, arr[i]);

if(arr[i] >= maxRight)
```

Always compare first.

Then update.

---

### Mistake 3

Forgetting to reverse.

Collected order is

```
Right → Left
```

Need

```
Left → Right
```

---

### Mistake 4

Thinking leader means strictly greater.

Question clearly allows equality.

---

## WHY I MIGHT FORGET THIS:

Because the first instinct is

> "Check every element on the right."

Instead think

> "What is the only information I need from the right?"

Answer:

```
Maximum on the right.
```

Once you know the maximum,

everything else becomes irrelevant.

---

## INTERVIEW FLOW:

> A leader is an element greater than or equal to every element on its right.

> The brute-force approach checks every right element for every index, taking O(N²).

> The key observation is that we don't need every right element—we only need the maximum among them.

> So I traverse from right to left while maintaining a running maximum (`maxRight`).

> If the current element is greater than or equal to `maxRight`, it's a leader.

> I store it, update `maxRight`, and finally reverse the answer because I collected leaders from right to left.

---

## TIME COMPLEXITY:

### Brute Force

```
O(N²)
```

Each element scans all elements on its right.

### Optimal

Traversal

```
O(N)
```

Reverse

```
O(N)
```

Overall

```
O(N)
```

Reason:

Each element is visited exactly once, and reversing also takes linear time.

---

## SPACE COMPLEXITY:

Ignoring output array

```
O(1)
```

Including output

```
O(K)
```

where K is the number of leaders.

---

## EDGE CASES:

### Single Element

```text
[5]

Output

[5]
```

### Increasing Array

```text
[1,2,3,4]

Output

[4]
```

### Decreasing Array

```text
[4,3,2,1]

Output

[4,3,2,1]
```

### Equal Elements

```text
[5,5,5]

Output

[5,5,5]
```

### Duplicate Leaders

```text
[10,4,2,4,1]

Output

[10,4,4,1]
```

### All Zeros

```text
[0,0,0]

Output

[0,0,0]
```

---

## PATTERN RECOGNITION:

Whenever you see:

- Compare with every element on the right
- Greater/smaller than all future elements
- Right-side maximum/minimum
- Suffix information
- Future elements determine the answer

Think:

> **Reverse Traversal + Running Suffix Information**

Maintain:

- suffix maximum
- suffix minimum
- running best from the right

instead of scanning the right side repeatedly.

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<int> leaders(vector<int>& arr) {

        vector<int> ans;

        int n = arr.size();
        int maxRight = INT_MIN;

        for (int i = n - 1; i >= 0; i--) {

            if (arr[i] >= maxRight) {
                ans.push_back(arr[i]);
            }

            maxRight = max(maxRight, arr[i]);
        }

        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
vector<int> ans;
```

Stores all the leaders found while traversing from right to left.

---

```cpp
int maxRight = INT_MIN;
```

Keeps track of the **largest element seen so far** from the right.

`INT_MIN` ensures the last element is always treated as a leader.

---

```cpp
for (int i = n - 1; i >= 0; i--)
```

Traverse from **right to left** because leader status depends on future (right-side) elements.

---

```cpp
if (arr[i] >= maxRight)
```

If the current element is at least as large as the largest element on its right, it is a leader.

---

```cpp
ans.push_back(arr[i]);
```

Store the leader.

Since we're traversing backwards, leaders are collected in reverse order.

---

```cpp
maxRight = max(maxRight, arr[i]);
```

Update the running suffix maximum for the next element on the left.

**Always update after checking**, because `maxRight` should represent only the elements to the right of the current index.

---

```cpp
reverse(ans.begin(), ans.end());
```

Restore the leaders to their original left-to-right order.

---

# Easy-to-Remember Summary

- **Pattern:** Reverse Traversal + Running Maximum
- **Trigger:** "Compare with all elements on the right."
- **Observation:** You don't need all right elements—only the **maximum**.
- **Rule:** If `arr[i] >= maxRight`, it's a leader.
- **Then:** Update `maxRight`, continue left, and reverse the result.
- **Complexity:** **O(N)** time, **O(1)** extra space (excluding output).

### Memory Trick

> **If the answer depends only on the right side, walk backwards and keep the best seen so far.**
