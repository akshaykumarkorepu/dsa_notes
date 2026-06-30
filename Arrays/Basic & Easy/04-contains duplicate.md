# Remove Duplicates from Sorted Array


## Question
Given a **sorted** array, remove all duplicate elements so that each element appears only once while maintaining the original order.

---

# 2. PROBLEM

### One-line Statement

Remove duplicates from a **sorted array** and keep only one occurrence of each element.

### Example

```text
Input:
[1,1,2,2,3,4,4]

Output:
[1,2,3,4]
```

---

# 3. FIRST OBSERVATION ⭐⭐⭐⭐⭐

Before thinking about code, ask yourself these three interview questions:

### 1. Is the array sorted?

This is the most important observation because it often changes the entire solution.

### 2. Do I need to preserve the order?

If order doesn't matter, many different approaches become possible.

If order must be preserved, we need a stable solution.

### 3. Is in-place modification required?

If yes, avoid extra arrays or hash tables whenever possible.

---

### Decision Tree

Array NOT Sorted

↓

Need to preserve order

↓

Use HashSet (Typical Optimal)

↓

Time : O(N)

Space : O(N)

-------------------------------------------------

Array Sorted

↓

Duplicates become adjacent

↓

No hashing required

↓

Two Pointers

↓

Time : O(N)

Space : O(1)

-------

# INTERVIEW THINKING PROCESS ⭐⭐⭐⭐⭐

The interviewer is usually testing whether you recognise the structure of the problem before jumping into a solution.

A good way to start is:

> "Before deciding the algorithm, I'd like to clarify a few properties of the input."

Then ask:

• Is the array sorted?
• Do I need to preserve the order?
• Is the modification required in-place?

These three questions immediately narrow down the possible approaches.

-----

# 4. PATTERN

## Pattern Used

> **Two Pointers (Read Pointer + Write Pointer)**

---

# 5. WHY THIS PATTERN?

Since the array is sorted,

all duplicate elements are adjacent.

Instead of storing unique elements in another array,

we can overwrite duplicates inside the same array.

Hence,

Two Pointers is the most efficient approach.

---

# 6. CORE IDEA

Think of the array as two regions.

```text
Unique Region

1 2 3
    ↑
    j

Remaining Elements

3 3 4 5 5
↑
i
```

### Read Pointer (i)

- Reads every element.
- Always moves forward.

### Write Pointer (j)

- Points to the last unique element.
- Moves only when a new unique element is found.

Whenever a new unique element is found,

move `j` one step ahead and copy the element there.

---

## Invariant ⭐⭐⭐

At every moment,

```text
arr[0...j]
```

contains **all unique elements found so far**.

This invariant is maintained throughout the algorithm.

---

# 7. BRUTE FORCE

## Case 1 : Array NOT Sorted

### Idea

Use a HashSet.

Traverse the array.

If an element is not present,

insert it into the set and answer array.

---

### Why HashSet?

Because duplicates may occur anywhere.

HashSet provides average O(1) lookup.

---

### Why Set instead of Map?

We only need uniqueness.

No frequencies are required.

Use HashMap only when frequency/count is needed.

---

### Algorithm

- Create HashSet.
- Create Answer Array.
- Traverse array.
- If element is not present:
  - Insert into HashSet.
  - Push into Answer Array.
- Return Answer.

---

### Time Complexity

```text
O(N)
```

---

### Space Complexity

```text
O(N)
```

---

### Code

```cpp
vector<int> removeDuplicates(vector<int>& arr){

    unordered_set<int> st;
    vector<int> ans;

    for(int x : arr){

        if(st.find(x)==st.end()){

            st.insert(x);
            ans.push_back(x);

        }

    }

    return ans;

}
```

---

### Dry Run

```text
Input

3 1 2 3 2

Set {}

3 -> Insert

1 -> Insert

2 -> Insert

3 -> Already Present

2 -> Already Present

Answer

3 1 2
```

---

# Case 2 : Sorted Array

Since duplicates are adjacent,

we don't even need hashing.

Simply compare neighbouring elements.

---

### Idea

Create another array.

Copy only when current element differs from previous.

---

### Algorithm

- Push first element.
- Traverse from second element.
- Compare with previous element.
- If different, push.
- Return Answer.

---

### Time Complexity

```text
O(N)
```

---

### Space Complexity

```text
O(N)
```

---

### Code

```cpp
vector<int> removeDuplicates(vector<int>& arr){

    vector<int> ans;

    ans.push_back(arr[0]);

    for(int i=1;i<arr.size();i++){

        if(arr[i]!=arr[i-1])

            ans.push_back(arr[i]);

    }

    return ans;

}
```

---

### Dry Run

```text
1 1 2 2 3

Answer

1

Skip second 1

Add 2

Skip second 2

Add 3

Final

1 2 3
```

---

# Optimization Thought Process ⭐⭐⭐⭐⭐

Instead of directly jumping to Two Pointers,

think like this:

```text
General Problem

↓

HashSet

↓

Array is Sorted

↓

Adjacent Comparison Works

↓

Extra Array

↓

Can we remove the extra array?

↓

Overwrite Original Array

↓

Two Pointers
```

This is exactly how interviewers expect you to derive the optimal solution.

---

# 8. OPTIMAL APPROACH

Instead of storing unique elements separately,

store them back inside the original array.

Maintain two pointers.

```text
i → Read Pointer

Reads every element.

Moves every iteration.


j → Write Pointer

Points to last unique element.

Moves only when a new unique element is found.
```

Whenever

```cpp
arr[i] != arr[j]
```

we have found a new unique element.

Move j forward

and overwrite.

---

# 9. ALGORITHM

1. If array is empty, return empty.
2. Initialize `j = 0`.
3. Traverse from index `1`.
4. Compare current element with `arr[j]`.
5. If different:
   - Increment `j`.
   - Copy current element to `arr[j]`.
6. Continue till end.
7. Return first `j + 1` elements.

---

# 10. CLEAN C++ CODE

```cpp
class Solution {
public:

    vector<int> removeDuplicates(vector<int>& arr) {

        if(arr.empty())
            return {};

        int j = 0;

        for(int i = 1; i < arr.size(); i++) {

            if(arr[i] != arr[j]) {

                j++;

                arr[j] = arr[i];

            }

        }

        return vector<int>(arr.begin(), arr.begin() + j + 1);

    }
};
```

---

# 11. INTUITION BEHIND EVERY IMPORTANT LINE ⭐⭐⭐⭐⭐

### `if(arr.empty())`

Avoid accessing `arr[0]`.

Handles empty array.

---

### `int j = 0;`

First element is always unique.

Start unique region here.

---

### `for(int i=1; ...)`

First element already processed.

Read remaining elements.

---

### `if(arr[i] != arr[j])`

Compare current element with last unique element.

If different,

a new unique value is found.

---

### `j++;`

Expand the unique region.

Create space for the next unique element.

---

### `arr[j] = arr[i];`

Overwrite duplicate region.

Store newly found unique element.

---

### Return first `j+1` elements

Only these positions contain valid unique values.

Everything after that is irrelevant.

---

# 12. DETAILED DRY RUN ⭐⭐⭐⭐⭐

Input

```text
1 1 2 2 3 4 4
```

Initial State

```text
j = 0

Unique Region

1
↑
j

i = 1
```

---

## Iteration 1

```text
i = 1

arr[i] = 1

arr[j] = 1
```

Equal.

Duplicate.

Do nothing.

```text
j = 0

Array

1 1 2 2 3 4 4
```

---

## Iteration 2

```text
i = 2

arr[i] = 2

arr[j] = 1
```

Different.

Move j.

```text
j = 1
```

Copy.

```text
arr[1] = 2
```

Array

```text
1 2 2 2 3 4 4
```

Unique Region

```text
1 2
```

---

## Iteration 3

```text
i = 3

arr[i] = 2

arr[j] = 2
```

Duplicate.

Skip.

---

## Iteration 4

```text
i = 4

arr[i] = 3

arr[j] = 2
```

Different.

Move j.

```text
j = 2
```

Copy.

```text
1 2 3 2 3 4 4
```

Unique Region

```text
1 2 3
```

---

## Iteration 5

```text
i = 5

arr[i] = 4

arr[j] = 3
```

Different.

Move j.

```text
j = 3
```

Copy.

```text
1 2 3 4 3 4 4
```

Unique Region

```text
1 2 3 4
```

---

## Iteration 6

```text
i = 6

arr[i] = 4

arr[j] = 4
```

Duplicate.

Skip.

Finished.

Return first `j+1 = 4` elements.

```text
Answer

1 2 3 4
```

---

# 13. IMPORTANT OBSERVATIONS

- Sorted property changes the entire solution.
- Duplicates become adjacent.
- Hashing becomes unnecessary.
- Only first `j+1` elements matter.
- Remaining values are irrelevant.
- `i` only reads.
- `j` only writes.

---

# 14. COMMON MISTAKES

❌ Using HashSet even though array is sorted.

❌ Forgetting `j++`.

❌ Returning the whole array.

❌ Starting loop from index 0.

❌ Comparing with wrong pointer.

Always compare with

```cpp
arr[j]
```

because `j` always points to the last unique element.

---

# 15. WHY I MIGHT FORGET THIS

Most common confusion:

> Why compare with `j` instead of `i-1`?

Remember:

After overwriting,

`j` represents the **last unique element**,

not necessarily `i-1`.

Memory Trick:

```text
i → Reads

j → Writes
```

---

# 16. IMPORTANT CODE SNIPPETS

```cpp
if(arr[i] != arr[j])
```

```cpp
j++;
```

```cpp
arr[j] = arr[i];
```

```cpp
return vector<int>(arr.begin(), arr.begin()+j+1);
```

---

# 17. INTERVIEW FLOW ⭐⭐⭐⭐⭐

Interviewer:
"Remove duplicates."

↓

You:

"Before deciding the approach, I'd like to clarify a few things."

• Is the array sorted?
• Should the original order be preserved?
• Is the solution expected to be in-place?

---

Case 1

If the interviewer says:

"No, the array isn't sorted."

You should immediately say:

"I would use a HashSet because duplicates can appear anywhere. This gives O(N) time and O(N) extra space while preserving the original order."

Mention brute force only if asked to compare approaches.

---

Case 2

If the interviewer says:

"Yes, the array is sorted."

Immediately say:

"Since the array is sorted, duplicates are adjacent. That allows me to avoid hashing completely. I'll use a Two Pointer technique to overwrite duplicates in-place."

This demonstrates that you've recognised and exploited the sorted property.

Avoid proposing a HashSet here—it works, but it ignores useful information provided in the problem and misses the optimal constant-space solution.

```text
Problem

↓

First Observation

Is the array sorted?

↓

General Solution

If unsorted,

I'd use a HashSet.

↓

Observation

Since it is sorted,

duplicates become adjacent.

↓

Brute Force

Use an extra array.

↓

Optimization

Can we eliminate the extra array?

↓

Yes.

Overwrite the original array.

↓

Pattern

Two Pointers

↓

Complexities

Time : O(N)

Space : O(1)
```

---

# 18. TIME COMPLEXITY

## HashSet

```text
O(N)
```

Each element is inserted/looked up once.

---

## Extra Array

```text
O(N)
```

Single traversal.

---

## Optimal

```text
O(N)
```

Single traversal.

---

# 19. SPACE COMPLEXITY

## HashSet

```text
O(N)
```

---

## Extra Array

```text
O(N)
```

---

## Optimal

```text
O(1)
```

Only two pointers are used.

---

# 20. EDGE CASES

- Empty array.
- Single element.
- All duplicates.
- No duplicates.
- Very large array.
- Only one distinct value.

---

# 21. PATTERN RECOGNITION ⭐⭐⭐⭐⭐

# HOW TO RECOGNIZE THIS QUESTION ⭐⭐⭐⭐⭐

Whenever you see words like:

• Sorted Array
• Remove Duplicates
• In-place
• Constant Extra Space
• Compress Array

Immediately ask yourself:

1. Is the array sorted?
2. Do I need to preserve order?
3. Can I overwrite the original array?

If the answer is yes,

↓

Think

Two Pointers

(Read Pointer + Write Pointer)
---

# 22. EASY TO REMEMBER SUMMARY

- First ask: **Is the array sorted?**
- Unsorted → HashSet.
- Sorted → Adjacent comparison.
- Pattern → Two Pointers.
- `i` = Read Pointer.
- `j` = Write Pointer.
- Compare `arr[i]` with `arr[j]`.
- If different:
  - `j++`
  - Copy element.
- Invariant:
  `arr[0...j]` always stores all unique elements.
- Time → `O(N)`
- Space → `O(1)`

---

# 23. INTERVIEW CHEAT SHEET (30 Seconds)

Interview Opening:

"Before choosing the algorithm, I'd first check whether the array is sorted, whether the order must be preserved, and whether the operation should be done in-place."

If Unsorted

↓

HashSet

O(N) Time

O(N) Space

If Sorted

↓

Two Pointers

O(N) Time

O(1) Space

Reason:

The sorted property makes all duplicates adjacent, so we can overwrite duplicates instead of storing them separately.

```text
Problem

↓

Remove duplicates from a sorted array.

↓

Observation

Sorted ⇒ duplicates are adjacent.

↓

Pattern

Two Pointers

↓

Algorithm

j = 0

Traverse using i

If arr[i] != arr[j]

j++

arr[j] = arr[i]

Return first j+1 elements.

↓

Time

O(N)

↓

Space

O(1)

↓

One-liner

Since the array is sorted, duplicates are adjacent, allowing us to maintain a compact unique prefix using a read pointer and a write pointer without extra space.
```
