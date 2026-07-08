
## PROBLEM:
Given a binary array `arr[]` consisting only of `0`s and `1`s, return the **maximum number of consecutive identical bits** (either consecutive `0`s or consecutive `1`s).

**Example:**

```text
Input : [0,1,0,1,1,1,1]

Output : 4
```

Explanation:

```text
0 1 0 1 1 1 1
      └──────┘
       4 consecutive 1's
```

---

# PATTERN:

**Linear Traversal + Consecutive Count (Run Length Counting)**

---

# WHY THIS PATTERN:

Whenever a question asks for

- longest consecutive
- maximum streak
- continuous block
- consecutive occurrences
- longest run

you don't need to compare every element with every other element.

You only need to know

> **"Does the current element continue the previous streak or start a new one?"**

This naturally leads to a single linear traversal.

---

# CORE IDEA:

Think of the array as groups.

Example

```text
0 0 | 1 | 0 | 1 1 1 | 0 0
```

Groups are

```text
00
1
0
111
00
```

We only need the **largest group length**.

Instead of counting every group multiple times, process each group only once while moving left to right.

---

# BRUTE FORCE:

## Approach

The brute force idea is:

> Start from every index and count how many consecutive elements are the same.

For every position,

- assume it is the beginning of a streak
- move forward until the streak ends
- store its length
- repeat for every index

Finally return the maximum streak.

### Example

```text
0 0 1 1 1 0
```

Start from

```text
Index 0 → count = 2

Index 1 → count = 1

Index 2 → count = 3

Index 3 → count = 2

Index 4 → count = 1

Index 5 → count = 1
```

Maximum = **3**

### Brute Force Code

```cpp
int maxCount = 1;
int n = arr.size();

for(int i = 0; i < n; i++)
{
    int count = 1;

    for(int j = i + 1; j < n; j++)
    {
        if(arr[j] == arr[i])
            count++;
        else
            break;
    }

    maxCount = max(maxCount, count);
}

return maxCount;
```

### Dry Run

Take

```text
arr = [0,0,1,1,1,0]
```

### i = 0

Current element

```text
0 0 1 1 1 0
^
```

```cpp
count = 1;
```

Current element itself is counted.

Inner loop

```text
j = 1

arr[1] == arr[0]

0 == 0
```

True

```cpp
count++;
```

Now

```text
count = 2
```

Next

```text
j = 2

1 == 0
```

False

```cpp
break;
```

Current streak ends.

Update

```text
maxCount = max(1,2)

maxCount = 2
```

---

### i = 1

Only one consecutive 0.

```text
count = 1

maxCount = 2
```

---

### i = 2

Current element

```text
1 1 1
```

Count becomes

```text
1

↓

2

↓

3
```

Encounter

```text
0
```

Break.

Update

```text
maxCount = 3
```

Remaining iterations never exceed 3.

Answer

```text
3
```

### Time Complexity

```text
O(N²)
```

Every index may scan almost the remaining array.

### Space Complexity

```text
O(1)
```

---

# OPTIMAL APPROACH:

## Approach

Notice what happens in brute force.

For

```text
0 0 1 1 1 0
```

the streak

```text
1 1 1
```

is counted

- once from index 2
- again from index 3
- again from index 4

This repeated work is unnecessary.

Instead,

walk through the array only once.

At every element ask

> **Is this element equal to the previous element?**

If yes

→ extend the current streak.

If no

→ previous streak ends and a new streak starts.

Continuously maintain the maximum streak.

---

# ALGORITHM:

1. Initialize

```text
currentCount = 1
maxCount = 1
```

2. Traverse from index `1`.

3. If

```text
arr[i] == arr[i-1]
```

increase the current streak.

4. Otherwise,

start a new streak.

5. Update the maximum streak after every iteration.

6. Return the answer.

---

# DRY RUN:

Input

```text
arr = [0,0,1,1,1,0]
```

Initially

```text
currentCount = 1

maxCount = 1
```

---

### i = 1

Compare

```text
arr[1] == arr[0]

0 == 0
```

True

```cpp
currentCount++;
```

Now

```text
currentCount = 2
```

Update

```text
maxCount = max(1,2)

maxCount = 2
```

---

### i = 2

Compare

```text
1 == 0
```

False

```cpp
currentCount = 1;
```

Why?

A new streak starts from this element.

Update

```text
maxCount = 2
```

---

### i = 3

Compare

```text
1 == 1
```

True

```text
currentCount = 2
```

Update

```text
maxCount = 2
```

---

### i = 4

Compare

```text
1 == 1
```

True

```text
currentCount = 3
```

Update

```text
maxCount = 3
```

---

### i = 5

Compare

```text
0 == 1
```

False

Reset

```text
currentCount = 1
```

Maximum remains

```text
3
```

Return

```text
3
```

---

# IMPORTANT CODE SNIPPETS:

### Continue the streak

```cpp
if(arr[i] == arr[i-1])
    currentCount++;
```

### Start a new streak

```cpp
else
    currentCount = 1;
```

### Update answer

```cpp
maxCount = max(maxCount, currentCount);
```

---

# COMMON MISTAKES:

### Mistake 1

Starting loop from index `0`.

```cpp
arr[i-1]
```

becomes invalid.

Always start from

```cpp
i = 1;
```

---

### Mistake 2

Resetting

```cpp
currentCount = 0;
```

Wrong.

Correct

```cpp
currentCount = 1;
```

The current element itself starts the new streak.

---

### Mistake 3

Updating maximum only when elements are equal.

Always update after every iteration.

---

### Mistake 4

Initializing

```cpp
currentCount = 0;
```

The first element itself forms a streak.

Initialize with

```cpp
1
```

---

# WHY I MIGHT FORGET THIS:

I may think separately about

- counting 0's
- counting 1's

Actually, the values don't matter.

The only thing that matters is

```text
Is current element equal to previous element?
```

If yes

→ continue the streak.

Otherwise

→ start a new streak.

---

# INTERVIEW FLOW:

> "A brute force solution starts counting consecutive elements from every index, but it repeatedly scans the same streaks. We can avoid this by traversing the array only once. I maintain the length of the current consecutive streak. If the current element matches the previous one, I extend the streak; otherwise, I reset it to 1 because the current element starts a new streak. During the traversal, I continuously update the maximum streak. This gives an O(N) solution with O(1) extra space."

---

# TIME COMPLEXITY:

### Brute Force

```text
O(N²)
```

Reason:

For every index, we may scan almost the remaining array.

### Optimal

```text
O(N)
```

Reason:

Each element is visited exactly once.

---

# SPACE COMPLEXITY:

### Brute Force

```text
O(1)
```

### Optimal

```text
O(1)
```

Only two variables are maintained.

---

# EDGE CASES:

### Single element

```text
[0]

Answer = 1
```

### All zeros

```text
0 0 0 0

Answer = 4
```

### All ones

```text
1 1 1 1

Answer = 4
```

### Alternating bits

```text
0 1 0 1 0 1

Answer = 1
```

### Long streak at beginning

```text
0 0 0 1

Answer = 3
```

### Long streak at end

```text
0 1 1 1 1

Answer = 4
```

---

# PATTERN RECOGNITION:

Whenever the question contains words like

- longest consecutive
- continuous streak
- longest block
- maximum run
- consecutive occurrences
- repeated adjacent elements

immediately think

> **Linear Traversal + Current Count + Maximum Count**

General template

```cpp
current = 1;
maximum = 1;

for(int i = 1; i < n; i++)
{
    if(arr[i] == arr[i-1])
        current++;
    else
        current = 1;

    maximum = max(maximum, current);
}
```

This same pattern is used in

- Longest consecutive characters in a string
- Longest repeating character sequence
- Maximum consecutive vowels
- Maximum consecutive equal numbers
- Run Length Encoding (RLE)

---

# Clean C++ Code

```cpp
class Solution {
public:
    int maxConsecutiveCount(vector<int> &arr) {

        int currentCount = 1;
        int maxCount = 1;

        for(int i = 1; i < arr.size(); i++)
        {
            if(arr[i] == arr[i-1])
            {
                currentCount++;
            }
            else
            {
                currentCount = 1;
            }

            maxCount = max(maxCount, currentCount);
        }

        return maxCount;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int currentCount = 1;
```

The first element itself forms a streak of length **1**.

---

```cpp
int maxCount = 1;
```

The minimum possible answer for a non-empty array is **1**.

---

```cpp
for(int i = 1; i < arr.size(); i++)
```

Start from index `1` because every element is compared with its previous element.

---

```cpp
if(arr[i] == arr[i-1])
```

Check whether the current streak continues.

---

```cpp
currentCount++;
```

Extend the current streak.

---

```cpp
currentCount = 1;
```

The previous streak has ended. The current element starts a new streak.

---

```cpp
maxCount = max(maxCount, currentCount);
```

Store the longest streak found so far.

---

```cpp
return maxCount;
```

Return the maximum consecutive run.

---

# EASY-TO-REMEMBER SUMMARY

Think in terms of **groups**, not individual elements.

```text
0 0 | 1 | 0 | 1 1 1 | 0 0
```

Every time you move to the next element, ask only one question:

> **"Is it the same as the previous one?"**

- Same → Continue the current group (`currentCount++`)
- Different → Start a new group (`currentCount = 1`)
- After every step → Update the best answer (`maxCount`)

### One-line Memory Trick

> **Same as previous → Extend the streak. Different → Start a new streak. Keep track of the maximum.**
