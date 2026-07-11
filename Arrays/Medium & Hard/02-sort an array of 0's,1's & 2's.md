

## PROBLEM:

Given an array containing only **0s, 1s, and 2s**, sort it in ascending order **without using the built-in sort function**.

**Follow-up:**
- Solve it in **one pass**
- Use **constant extra space**

---

# PATTERN:

**Dutch National Flag Algorithm (Three Pointer Partitioning)**

---

# WHY THIS PATTERN:

The array contains only **3 distinct values (0, 1, 2)**.

Instead of using a general sorting algorithm, we can **partition** the array into three regions.

The interviewer specifically asks for:

- One Pass
- Constant Extra Space

This is exactly what the Dutch National Flag Algorithm is designed for.

---

# CORE IDEA:

### Brute Force

```
Count → Rewrite
```

Requires **2 passes**.

### Optimal

Instead of counting first,

**place every element into its correct region the moment you see it.**

Maintain four regions:

```
|------0s------|------1s------|------Unknown------|------2s------|
```

Initially,

```
0 Region = Empty

1 Region = Empty

Unknown = Entire Array

2 Region = Empty
```

The goal is simple:

> **Shrink the Unknown region until nothing remains.**

---

# BRUTE FORCE:

## Intuition

Since the array contains only

```
0
1
2
```

we don't need comparisons.

Simply count:

- Number of 0s
- Number of 1s
- Number of 2s

Then rewrite the array.

---

## Algorithm

### Pass 1

Count frequencies.

```
count0
count1
count2
```

### Pass 2

Overwrite the array.

```
0 0 ... 1 1 ... 2 2 ...
```

---

## Code

```cpp
void sort012(vector<int>& arr)
{
    int count0 = 0;
    int count1 = 0;
    int count2 = 0;

    for(int num : arr)
    {
        if(num == 0)
            count0++;
        else if(num == 1)
            count1++;
        else
            count2++;
    }

    int index = 0;

    while(count0--)
        arr[index++] = 0;

    while(count1--)
        arr[index++] = 1;

    while(count2--)
        arr[index++] = 2;
}
```

---

## Dry Run

Input

```
2 0 1 2 0 1
```

Count

```
count0 = 2
count1 = 2
count2 = 2
```

Rewrite

```
0 0 _ _ _ _
```

↓

```
0 0 1 1 _ _
```

↓

```
0 0 1 1 2 2
```

---

## Time Complexity

```
O(N) + O(N)

= O(2N)

= O(N)
```

---

## Space Complexity

```
O(1)
```

---

## Why Optimize?

This solution requires

```
Pass 1 → Count

Pass 2 → Rewrite
```

The interviewer asks:

> Can you solve it in **ONE PASS?**

That leads to the Dutch National Flag Algorithm.

---

# OPTIMAL APPROACH:

Instead of counting,

place every element directly into its correct region.

Maintain three pointers.

```cpp
low
mid
high
```

---

## Meaning of Every Pointer

### low

Everything before low is already **0**.

```
0 .... low-1
```

---

### mid

Current element being processed.

---

### high

Everything after high is already **2**.

```
high+1 .... end
```

---

## The Most Important Picture

```
0 ... low-1        → All 0s

low ... mid-1      → All 1s

mid ... high       → Unknown

high+1 ... end     → All 2s
```

Everything revolves around shrinking the **Unknown** region.

---

# ALGORITHM

Initialize

```cpp
low = 0;
mid = 0;
high = n-1;
```

Run

```cpp
while(mid <= high)
```

There are only three cases.

---

## CASE 1

```cpp
arr[mid] == 0
```

Move 0 to the front.

```cpp
swap(arr[low], arr[mid]);
low++;
mid++;
```

---

## CASE 2

```cpp
arr[mid] == 1
```

Already in the correct region.

```cpp
mid++;
```

---

## CASE 3

```cpp
arr[mid] == 2
```

Move 2 to the end.

```cpp
swap(arr[mid], arr[high]);
high--;
```

**Do NOT do**

```cpp
mid++;
```

because the swapped element has not been processed yet.

---

# DRY RUN

Input

```
2 0 2 1 1 0
```

Initially

```
L,M             H

2 0 2 1 1 0
```

Regions

```
0 Region = Empty

1 Region = Empty

Unknown = Entire Array

2 Region = Empty
```

---

## Step 1

Current

```
arr[mid] = 2
```

Swap with high.

```
0 0 2 1 1 2
```

Move

```
high--
```

Pointers

```
L,M          H
```

Notice

```
mid did NOT move
```

because the new value at mid is still unknown.

---

## Step 2

Current

```
arr[mid]=0
```

Swap

```
swap(low, mid)
```

Array

```
0 0 2 1 1 2
```

Move

```
low++
mid++
```

---

## Step 3

Current

```
arr[mid]=0
```

Again

```
swap(low, mid)

low++

mid++
```

Array

```
0 0 2 1 1 2
```

---

## Step 4

Current

```
arr[mid]=2
```

Swap

```
0 0 1 1 2 2
```

Move

```
high--
```

Again

```
mid stays
```

because the new element hasn't been processed.

---

## Step 5

Current

```
arr[mid]=1
```

Simply

```
mid++
```

---

## Step 6

Current

```
arr[mid]=1
```

Again

```
mid++
```

Now

```
mid > high
```

Loop ends.

Final Array

```
0 0 1 1 2 2
```

---

# IMPORTANT OBSERVATIONS

- This is **NOT** a sorting algorithm.
- It is a **partitioning algorithm**.
- We only process the **Unknown region**.
- Every iteration shrinks the Unknown region.

---

# IMPORTANT CODE SNIPPETS

### Initialization

```cpp
int low = 0;
int mid = 0;
int high = arr.size()-1;
```

---

### Loop

```cpp
while(mid <= high)
```

---

### 0 Case

```cpp
swap(arr[low], arr[mid]);
low++;
mid++;
```

---

### 1 Case

```cpp
mid++;
```

---

### 2 Case

```cpp
swap(arr[mid], arr[high]);
high--;
```

Never

```cpp
mid++;
```

here.

---

# COMMON MISTAKES

### Mistake 1

Doing

```cpp
mid++;
```

after swapping with high.

Wrong because the new element at mid hasn't been processed.

---

### Mistake 2

Using

```cpp
while(mid < high)
```

Correct

```cpp
while(mid <= high)
```

---

### Mistake 3

Forgetting the regions.

Always remember

```
Before low → 0s

low...mid-1 → 1s

mid...high → Unknown

After high → 2s
```

---

### Mistake 4

Thinking this is a sorting algorithm.

It is actually a partitioning algorithm.

---

# WHY I MIGHT FORGET THIS

Because most students memorize

```
low

mid

high
```

instead of understanding their purpose.

Just remember one picture:

```
|----0s----|----1s----|----Unknown----|----2s----|
```

Everything is about shrinking the Unknown region.

---

# INTERVIEW FLOW

"I'll first discuss the counting approach.

Since there are only three distinct values, I can count the frequencies of 0, 1 and 2 and then rewrite the array. This takes O(N) time and O(1) space but requires two passes.

Since the follow-up asks for one pass, I'll use the Dutch National Flag Algorithm.

I maintain four regions:

- before low → 0s
- low to mid-1 → 1s
- mid to high → unknown
- after high → 2s

Then I process only the unknown region.

- If I see 0, I move it to the front.
- If I see 1, I leave it.
- If I see 2, I move it to the end.

After swapping with high, I don't move mid because the swapped element is still unprocessed."

---

# TIME COMPLEXITY

## Brute Force

```
O(N) + O(N)

= O(2N)

= O(N)
```

---

## Optimal

Every iteration either

- increases mid
- or decreases high

The Unknown region keeps shrinking.

Therefore,

```
O(N)
```

---

# SPACE COMPLEXITY

Brute Force

```
O(1)
```

Optimal

```
O(1)
```

Only three pointers are used.

---

# EDGE CASES

Single element

```
0
```

All zeros

```
0 0 0
```

All ones

```
1 1 1
```

All twos

```
2 2 2
```

Already sorted

```
0 0 1 1 2 2
```

Reverse grouped

```
2 2 1 1 0 0
```

Only two values

```
0 0 1 1
```

```
2 2 0 0
```

---

# PATTERN RECOGNITION

Think of Dutch National Flag whenever you see:

- Exactly 3 distinct values
- One-pass requirement
- Constant extra space
- Partition into groups
- "Sort Colors" type problems
- Three-way partitioning

---

# Clean C++ Code

```cpp
class Solution {
public:
    void sort012(vector<int>& arr) {

        int low = 0;
        int mid = 0;
        int high = arr.size() - 1;

        while(mid <= high)
        {
            if(arr[mid] == 0)
            {
                swap(arr[low], arr[mid]);
                low++;
                mid++;
            }

            else if(arr[mid] == 1)
            {
                mid++;
            }

            else
            {
                swap(arr[mid], arr[high]);
                high--;
            }
        }
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int low = 0;
```

→ Next position where a **0** should go.

---

```cpp
int mid = 0;
```

→ Current unknown element being processed.

---

```cpp
int high = arr.size()-1;
```

→ Next position where a **2** should go.

---

```cpp
while(mid <= high)
```

→ Continue until the Unknown region becomes empty.

---

```cpp
if(arr[mid] == 0)
```

→ Found a 0.

Move it to the front.

---

```cpp
swap(arr[low], arr[mid]);
```

→ Place the 0 into the 0 region.

---

```cpp
low++;
mid++;
```

→ Expand the 0 region and process the next unknown element.

---

```cpp
else if(arr[mid] == 1)
```

→ 1 already belongs in the middle.

---

```cpp
mid++;
```

→ Move to the next unknown element.

---

```cpp
swap(arr[mid], arr[high]);
```

→ Move the 2 into the 2 region.

---

```cpp
high--;
```

→ Expand the 2 region.

---

### Why NOT `mid++` here?

Because the element swapped from the end has **never been processed**.

It must be checked in the next iteration.

---

# EASY-TO-REMEMBER SUMMARY

### Brute Force

```
Count

↓

Rewrite
```

Two passes.

---

### Optimal

Grow three regions while shrinking one.

```
Before low       → All 0s

low...mid-1      → All 1s

mid...high       → Unknown

After high       → All 2s
```

### Three Rules

```
0 → swap(low, mid), low++, mid++

1 → mid++

2 → swap(mid, high), high--
```

**Never move `mid` after swapping with `high`.**

### Golden Rule

> **Move `mid` only after the current element has been completely processed.**
