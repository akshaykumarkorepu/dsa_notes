

## PROBLEM:

Given a list of intervals `[start, end]`, remove the **minimum number of intervals** so that the remaining intervals do **not overlap**.

Two intervals are considered non-overlapping if:

```
previous.end <= current.start
```

---

## PATTERN:

**Greedy → Interval Greedy (Sort + Select)**

This is the exact same greedy idea as:

- Activity Selection
- Maximum Non-overlapping Intervals
- Meeting Room Scheduling (selection version)

The only difference is:

- Activity Selection asks for **maximum intervals selected**
- This problem asks for **minimum intervals removed**

These are complementary:

```
Minimum Removals
=
Total Intervals
-
Maximum Non-overlapping Intervals
```

So instead of thinking:

> Which intervals should I remove?

Think:

> Which maximum set of intervals can I keep?

---

## WHY THIS PATTERN:

Whenever a problem says:

- intervals
- overlap
- maximize number selected
- minimize removals

always think:

> "Can I greedily keep the maximum number of compatible intervals?"

The answer is yes.

---

# CORE IDEA:

Instead of removing intervals one by one...

Find the **largest possible set of non-overlapping intervals**.

Everything else must be removed.

```
Answer

=
Total intervals
-
Intervals kept
```

---

## GREEDY OBSERVATION:

Among all intervals that can be chosen next,

**choose the one that finishes earliest.**

Why?

Because finishing earlier leaves the maximum amount of room for future intervals.

---

## WHY GREEDY WORKS:

Suppose two intervals overlap.

```
A : [1,8]

B : [3,5]
```

Which one should we keep?

If we keep

```
[1,8]
```

future intervals cannot start until 8.

But if we keep

```
[3,5]
```

future intervals only need to start after 5.

So choosing the interval that ends earlier can never reduce future choices.

Instead, it gives us **more opportunities**.

That is exactly what Greedy wants.

---

## GREEDY CHOICE:

Always select the interval with the **smallest ending time** that does not overlap with the previously selected interval.

---

## WHY THIS CHOICE IS SAFE:

Suppose the optimal solution chooses

```
[2,10]
```

as its first interval.

Our greedy chooses

```
[3,5]
```

instead because it ends earlier.

Notice

```
5 < 10
```

Everything that could start after 10 can also start after 5.

So replacing

```
[2,10]
```

with

```
[3,5]
```

cannot make the solution worse.

It only leaves more free time.

Hence the greedy choice is always safe.

This is called the **Greedy Choice Property**.

---

## SORTING:

### Yes, sorting is required.

We sort by:

```
Ending Time
```

because every greedy decision depends on knowing

> Which interval finishes first?

Without sorting we'd need to repeatedly search for the smallest ending interval.

Sorting allows us to process intervals once.

---

### Comparator

```cpp
sort(intervals.begin(), intervals.end(),
     [](vector<int>& a, vector<int>& b){
         return a[1] < b[1];
});
```

Here,

```
a[1]
```

means

```
Ending time of interval a
```

and

```
b[1]
```

means

```
Ending time of interval b
```

The comparator simply says:

```
If end(a) < end(b)

then

place a before b.
```

---

### Example

Suppose the intervals are

```
[
 [5,9],
 [1,4],
 [2,3],
 [6,8]
]
```

During sorting, C++ compares pairs of intervals.

Example comparison:

```
a = [5,9]
b = [1,4]

Comparator:

return 9 < 4
```

Result:

```
false
```

So

```
[5,9]
```

should **not** come before

```
[1,4]
```

Another comparison:

```
a = [2,3]
b = [1,4]

return 3 < 4
```

Result:

```
true
```

So

```
[2,3]
```

comes before

```
[1,4]
```

After many such comparisons, sorting produces

```
[
 [2,3],
 [1,4],
 [6,8],
 [5,9]
]
```

because their ending times are

```
3
4
8
9
```

Notice that we **never manually swap elements**.

The comparator only answers one question repeatedly:

> "Should `a` come before `b`?"

The `sort()` function internally (using Introsort) performs many comparisons and rearranges the elements accordingly.

---

## INVARIANT:

After processing the first `i` intervals:

- We have selected the **maximum number of non-overlapping intervals** among those processed.
- `lastEndTime` is the ending time of the **last selected interval**.
- Every selected interval is non-overlapping.

This invariant remains true after every iteration.

---

## BRUTE FORCE:

### Brute force is **not necessary**.

Reason:

The optimization naturally comes from the classic Activity Selection problem.

Trying all subsets would be:

```
2^N
```

which is impossible for

```
N = 100000
```

Interviewers generally expect recognition of the Interval Greedy pattern directly.

---

# OPTIMAL APPROACH:

1. Sort intervals by ending time.
2. Select the first interval.
3. For every remaining interval:
   - if it doesn't overlap
     - keep it
     - update ending time
4. Count kept intervals.
5. Answer

```
Total - Kept
```

---

# ALGORITHM:

```
Sort by ending time

Take first interval

count = 1

lastEnd = first interval end

For every remaining interval

    if start >= lastEnd

        keep interval

        count++

        lastEnd = current end

Return

n - count
```

---

# DRY RUN:

Input

```
[
 [1,2],
 [2,3],
 [3,4],
 [1,3]
]
```

### Step 1

Sort by end

```
[
 [1,2],
 [2,3],
 [1,3],
 [3,4]
]
```

---

Take first interval

```
Kept

[1,2]

count = 1

lastEnd = 2
```

---

Current

```
[2,3]

2 >= 2

Keep
```

```
count = 2

lastEnd = 3
```

---

Current

```
[1,3]

1 < 3

Overlap

Skip
```

---

Current

```
[3,4]

3 >= 3

Keep
```

```
count = 3
```

Total intervals

```
4
```

Kept

```
3
```

Removed

```
4 - 3 = 1
```

Correct.

---

## COMMON MISTAKES:

### 1. Sorting by start time

Wrong.

```
sort by start
```

does not maximize future choices.

Always sort by **ending time**.

---

### 2. Using

```cpp
>
```

instead of

```cpp
>=
```

Remember

```
end == next start
```

is allowed.

Example

```
[1,2]

[2,3]
```

These do **not** overlap.

---

### 3. Returning kept intervals

Question asks

```
minimum removals
```

Return

```
n - kept
```

---

### 4. Updating lastEnd incorrectly

Update only after selecting an interval.

---

## INTERVIEW FLOW:

> This is a classic Interval Greedy problem.

Instead of deciding which intervals to remove, I maximize the number of intervals that can be kept.

To maximize future choices, I always keep the interval that finishes earliest.

Therefore I sort by ending time.

I iterate through the sorted intervals and greedily select every interval whose start time is at least the ending time of the last selected interval.

If I keep `count` intervals, then the minimum removals are simply:

```
n - count
```

---

## TIME COMPLEXITY:

### Sorting

```
O(N log N)
```

because of sorting.

### Traversal

```
O(N)
```

Single pass through intervals.

Overall

```
O(N log N)
```

---

## SPACE COMPLEXITY:

Ignoring the sorting implementation,

```
O(1)
```

Extra variables:

- count
- lastEndTime

No additional data structures are used.

*(Note: `std::sort` uses `O(log N)` stack space internally due to recursion.)*

---

## EDGE CASES:

### Single interval

```
[[1,2]]
```

Answer

```
0
```

---

### Already non-overlapping

```
[1,2]

[2,3]

[4,5]
```

Answer

```
0
```

---

### Completely overlapping

```
[1,5]

[2,4]

[3,4]
```

Greedy keeps the earliest ending interval.

---

### Duplicate intervals

```
[1,3]

[1,3]

[1,3]
```

Keep one.

Remove two.

---

### Touching intervals

```
[1,2]

[2,3]
```

Valid.

No removal needed.

---

## PATTERN RECOGNITION:

Whenever you see:

- intervals
- overlap
- maximize compatible intervals
- minimize removals
- scheduling
- meetings
- activities

Immediately think:

```
Interval Greedy
        ↓
Sort by Ending Time
        ↓
Keep earliest finishing interval
        ↓
Count selected intervals
```

Ask yourself:

> Does choosing the interval that finishes earliest leave the maximum room for future choices?

If yes,

it's almost certainly the **Activity Selection / Interval Greedy** pattern.

---

# Clean C++ Code

```cpp
class Solution {
public:
    int minRemoval(vector<vector<int>> &intervals) {

        int n = intervals.size();

        // Sort intervals by their ending time
        sort(intervals.begin(), intervals.end(),
             [](vector<int> &a, vector<int> &b) {
                 return a[1] < b[1];
             });

        // First interval is always selected
        int count = 1;

        // End time of the last selected interval
        int lastEndTime = intervals[0][1];

        // Try selecting every remaining interval
        for (int i = 1; i < n; i++) {

            // Current interval starts after or exactly when
            // the previous selected interval ends
            if (intervals[i][0] >= lastEndTime) {

                count++;

                // Update last selected ending time
                lastEndTime = intervals[i][1];
            }
        }

        // Minimum removals = Total - Selected
        return n - count;
    }
};
```

---

# Line-by-Line Intuition

### `sort(...)`

Arrange intervals in increasing order of **ending time** so we always consider the earliest finishing interval first.

---

### `count = 1`

After sorting, the first interval always ends the earliest, so it's always optimal to keep it.

---

### `lastEndTime = intervals[0][1]`

Stores the end time of the last interval we've chosen. Future intervals must start at or after this time to avoid overlap.

---

### `if (intervals[i][0] >= lastEndTime)`

Checks whether the current interval starts after (or exactly when) the last chosen interval ends.

If yes, they don't overlap, so we safely keep it.

---

### `count++`

We've successfully added another non-overlapping interval.

---

### `lastEndTime = intervals[i][1]`

The current interval becomes the last selected one, so update the reference end time.

---

### `return n - count`

We kept the maximum possible number of intervals.

Everything else must be removed.

---

# Tricky Condition Explained

```cpp
if (intervals[i][0] >= lastEndTime)
```

Notice the use of `>=` instead of `>`.

The problem states that intervals touching at endpoints are **not** overlapping.

Example:

```
[1,2]
[2,5]
```

Since `2 >= 2`, these intervals are compatible and should both be kept.

Using `>` would incorrectly reject valid intervals.

---

# Easy Interview Summary

- Recognize it as an **Interval Greedy** problem.
- Don't think about removing intervals—think about **keeping the maximum number** of non-overlapping intervals.
- Sort by **ending time**, because finishing earlier leaves the most room for future intervals.
- Greedily select every interval whose `start >= lastEndTime`.
- Answer = **Total Intervals − Kept Intervals**.
- Time: **O(N log N)**, Space: **O(1)** (excluding sort recursion).
````
