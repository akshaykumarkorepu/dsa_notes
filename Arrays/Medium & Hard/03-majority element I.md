

## PROBLEM
Given an array, return the element that appears **more than n/2 times**. If no such element exists, return **-1**.

---

# PATTERN
**Boyer-Moore Voting Algorithm (Voting / Pairwise Cancellation)**

---

# WHY THIS PATTERN

The majority element appears **more than half the time**, meaning:

- Majority Count > Count of All Other Elements Combined
- Even if every non-majority element cancels one majority element, the majority element still survives.

This observation allows us to solve the problem without storing frequencies.

---

# CORE IDEA

Imagine every different element cancels one occurrence of the current candidate.

```
Same element      → count++
Different element → count--
```

Whenever `count` becomes **0**, it means the previous candidate has been completely cancelled.

Choose the current element as the new candidate.

At the end of the first pass, the remaining candidate is the **only possible majority element**.

Since this problem does **not guarantee** a majority element, verify it in a second pass.

---

# BRUTE FORCE

## Intuition

For every element,

count how many times it appears.

If its frequency is greater than `n/2`, return it.

Otherwise return -1.

### Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& arr) {

        int n = arr.size();

        for (int i = 0; i < n; i++) {

            int count = 0;

            for (int j = 0; j < n; j++) {

                if (arr[i] == arr[j])
                    count++;
            }

            if (count > n / 2)
                return arr[i];
        }

        return -1;
    }
};
```

### Dry Run

```
[1,1,2,1,3]

Check 1

Frequency = 3

3 > 5/2

Return 1
```

### Time Complexity

```
O(n²)
```

### Space Complexity

```
O(1)
```

---

# BETTER APPROACH (HashMap)

## Intuition

Instead of counting repeatedly,

store frequency of every element in a HashMap.

Then check which frequency is greater than `n/2`.

### Code

```cpp
class Solution {
public:
    int majorityElement(vector<int>& arr) {

        unordered_map<int,int> mp;
        int n = arr.size();

        for (int num : arr)
            mp[num]++;

        for (auto it : mp) {

            if (it.second > n / 2)
                return it.first;
        }

        return -1;
    }
};
```

### Dry Run

```
[1,1,2,1,3]

HashMap

1 → 3
2 → 1
3 → 1

3 > 5/2

Return 1
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(n)
```

---

# OPTIMAL APPROACH

## Pattern

**Boyer-Moore Voting Algorithm**

### Intuition

Think of cancelling pairs of different elements.

Example

```
1 1 2 1 3 1

Cancel

1 with 2

1 with 3

Remaining

1 1

Majority survives.
```

Instead of actually removing elements,

we simulate cancellation using a counter.

---

# ALGORITHM

## Phase 1 — Find Candidate

Maintain

```
candidate
count
```

Rules

```
count == 0
Choose current element as candidate

Current == candidate
count++

Current != candidate
count--
```

After one traversal,

the remaining candidate is the only possible majority element.

---

## Phase 2 — Verification

The candidate is only a **possible** majority.

Example

```
[1,2,3]

Candidate = 3

But frequency = 1

Not majority
```

Count its frequency again.

If

```
frequency > n/2
```

return candidate.

Else

```
return -1
```

---

# DRY RUN

### Example 1

```
Array

[1,1,2,1,3,5,1]
```

| Element | Candidate | Count |
|---------|-----------|-------|
|1|1|1|
|1|1|2|
|2|1|1|
|1|1|2|
|3|1|1|
|5|1|0|
|1|1|1|

Candidate after Phase 1

```
1
```

Verification

```
Frequency of 1 = 4

4 > 7/2

Return 1
```

---

### Example 2 (No Majority)

```
Array

[2,3,2,4]
```

| Element | Candidate | Count |
|---------|-----------|-------|
|2|2|1|
|3|2|0|
|2|2|1|
|4|2|0|

Candidate

```
2
```

Verification

```
Frequency = 2

Need > 2

False

Return -1
```

---

# IMPORTANT CODE SNIPPETS

### Reset Candidate

```cpp
if (count == 0) {
    candidate = num;
    count = 1;
}
```

---

### Same Candidate

```cpp
else if (num == candidate) {
    count++;
}
```

---

### Different Candidate

```cpp
else {
    count--;
}
```

---

### Verification

```cpp
int freq = 0;

for (int num : arr) {

    if (num == candidate)
        freq++;
}

if (freq > n / 2)
    return candidate;

return -1;
```

---

# COMMON MISTAKES

### 1. Forgetting Verification

LeetCode guarantees a majority element.

GFG does **not**.

Always verify.

---

### 2. Using

```cpp
>= n/2
```

instead of

```cpp
> n/2
```

Majority means **strictly greater**.

---

### 3. Forgetting to reset count

Wrong

```cpp
if (count == 0)
    candidate = num;
```

Better

```cpp
if (count == 0) {
    candidate = num;
    count = 1;
}
```

---

### 4. Returning candidate directly

Without verification,

```
[1,2,3]
```

incorrectly returns `3`.

---

# WHY I MIGHT FORGET THIS

The algorithm looks magical.

Just remember one sentence:

```
Same element → Vote++

Different element → Vote--

Vote becomes zero

Previous candidate eliminated

Start a new election
```

---

# INTERVIEW FLOW

### Step 1

Brute Force

```
Count every element.

O(n²)
```

↓

### Step 2

HashMap

```
Store frequencies.

O(n)

Space O(n)
```

↓

### Step 3

Observation

```
Majority survives pairwise cancellation.
```

↓

### Step 4

Use Boyer-Moore Voting

```
Phase 1

Find candidate

↓

Phase 2

Verify candidate
```

↓

Final Complexity

```
O(n)

O(1)
```

---

# TIME COMPLEXITY

### Brute Force

```
O(n²)

For every element,
scan entire array.
```

### Better

```
O(n)

One pass to build HashMap.

One pass over HashMap.
```

### Optimal

```
Phase 1 → O(n)

Phase 2 → O(n)

Total

O(n)
```

---

# SPACE COMPLEXITY

### Brute Force

```
O(1)
```

### Better

```
O(n)

HashMap stores frequencies.
```

### Optimal

```
O(1)

Only candidate, count and frequency variables.
```

---

# EDGE CASES

### Single Element

```
[7]

Return 7
```

---

### No Majority

```
[1,2]

Return -1
```

---

### All Same

```
[5,5,5]

Return 5
```

---

### Majority at Beginning

```
[3,3,3,2]

Return 3
```

---

### Majority at End

```
[2,1,1,1]

Return 1
```

---

### Negative Numbers (if allowed)

Still works because only equality comparison is used.

---

# PATTERN RECOGNITION

Think of **Boyer-Moore Voting Algorithm** whenever you see:

- "Element occurring more than n/2 times."
- Need **O(n)** time.
- Need **O(1)** space.
- Majority dominates all other elements combined.
- Pairwise cancellation is possible.
- Find candidate first, verify later.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int majorityElement(vector<int>& arr) {

        int n = arr.size();

        int candidate = 0;
        int count = 0;

        // Phase 1: Find the candidate
        for (int num : arr) {

            if (count == 0) {
                candidate = num;
                count = 1;
            }
            else if (num == candidate) {
                count++;
            }
            else {
                count--;
            }
        }

        // Phase 2: Verify the candidate
        int freq = 0;

        for (int num : arr) {
            if (num == candidate) {
                freq++;
            }
        }

        if (freq > n / 2) {
            return candidate;
        }

        return -1;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int candidate = 0;
int count = 0;
```

- `candidate` stores the current possible majority.
- `count` stores its vote balance after cancellations.

---

```cpp
if (count == 0) {
    candidate = num;
    count = 1;
}
```

All previous votes have been cancelled.

Start a new election.

---

```cpp
else if (num == candidate) {
    count++;
}
```

Current candidate gains one vote.

---

```cpp
else {
    count--;
}
```

Different element cancels one vote.

---

```cpp
for (int num : arr)
```

Second pass verifies whether the candidate is actually the majority.

---

```cpp
if (freq > n / 2)
```

Majority means **strictly greater than half**.

---

# EASY-TO-REMEMBER SUMMARY

- **Brute:** Count every element → **O(n²)**.
- **Better:** Use a HashMap → **O(n)** time, **O(n)** space.
- **Optimal:** Boyer-Moore Voting Algorithm.
  - **Phase 1:** Find the candidate using pairwise cancellation.
  - **Phase 2:** Verify its frequency.
- **Golden Rule:** If the problem does **not guarantee** a majority element, **always perform the verification pass**.
