

## PROBLEM:

Given a list of intervals `[start, end]`, merge all overlapping intervals and return the merged intervals.

Example:

```
Input:
[[1,3],[2,4],[6,8],[9,10]]

Output:
[[1,4],[6,8],[9,10]]
```

---

# PATTERN:

**Interval Greedy (Sort + Merge Intervals)**

This is one of the most common Greedy interval patterns.

---

# WHY THIS PATTERN:

Intervals can overlap only with intervals that come after them in sorted order.

If intervals are sorted by their **starting time**, then:

- Every future interval starts after or at the current interval.
- We only need to compare with the **last merged interval**.
- Once an interval doesn't overlap, it can never overlap with any previous merged interval again.

Sorting converts a complicated overlap problem into a simple linear scan.

---

# CORE IDEA:

After sorting by starting time:

Maintain the current merged interval.

For every new interval:

- If it overlaps with the current merged interval → extend it.
- Otherwise → finalize the previous merged interval and start a new one.

---

# GREEDY OBSERVATION:

After sorting,

if two consecutive intervals overlap,

there is **never a reason to keep them separate**.

Merging them immediately can only help future intervals overlap with a larger interval.

Delaying the merge provides no benefit.

---

# WHY GREEDY WORKS:

Suppose we currently have

```
Current merged interval:
[1,5]
```

Next interval:

```
[3,7]
```

These overlap.

Keeping them separate gives

```
[1,5]
[3,7]
```

Merging gives

```
[1,7]
```

Now consider any future interval.

Example:

```
[6,8]
```

With merged interval:

```
[1,7]
```

it overlaps.

Without merging:

```
[1,5]
```

doesn't overlap,

but

```
[3,7]
```

does.

Eventually we'd merge anyway.

So merging immediately never hurts.

Instead, it simplifies future decisions.

Thus,

**local merge = globally optimal merge.**

---

# GREEDY CHOICE:

Whenever the next interval overlaps with the current merged interval,

**merge immediately** by extending the end.

```
mergedEnd = max(mergedEnd, currentEnd)
```

---

# WHY THIS CHOICE IS SAFE:

Suppose

```
[1,6]
```

and

```
[4,8]
```

Clearly,

every point covered by these intervals is

```
[1,8]
```

Keeping them separate loses no information.

Replacing them with

```
[1,8]
```

preserves every covered point.

Therefore,

future overlap decisions remain correct.

Hence the greedy choice is always safe.

---

# SORTING:

**Yes, sorting is mandatory.**

Sort by:

```
starting time
```

Why?

Without sorting,

```
[5,8]
[1,3]
[2,6]
```

The first interval isn't the earliest.

We may incorrectly decide there is no overlap.

Sorting guarantees:

- earlier intervals processed first
- overlap decisions become local
- only compare with last merged interval

Sorting parameter:

```
ascending start value
```

---

# INVARIANT:

After processing the first **i intervals**,

the answer vector contains

> correctly merged intervals for all processed intervals.

Also,

the last interval in the answer is

> the merged interval representing all overlapping intervals seen so far.

This invariant remains true after every iteration.

---

# BRUTE FORCE:

### Is brute force necessary?

**Yes (briefly).**

Although the greedy solution becomes natural after sorting, interviewers often appreciate seeing why repeatedly comparing every interval is inefficient.

### Idea

For every interval:

- Compare it with all remaining intervals.
- Merge any overlap found.
- Restart until no more merges exist.

Since merging one interval can create new overlaps, multiple passes may be required.

### Concise Code

```cpp
// Conceptual brute force (not recommended)
for(int i = 0; i < n; i++) {
    for(int j = i + 1; j < n; j++) {
        if(overlap(intervals[i], intervals[j])) {
            merge(intervals[i], intervals[j]);
            remove(intervals[j]);
            j--;
        }
    }
}
```

### Time Complexity

Worst case:

```
O(N²)
```

or worse if repeated passes are needed.

### Space Complexity

```
O(1)
```

(extra space ignored)

### Transition to Greedy

The expensive part is searching for overlaps.

After sorting,

all overlapping intervals become adjacent.

So instead of checking every interval,

we only compare with the last merged interval.

This reduces the scan to linear time after sorting.

---

# OPTIMAL APPROACH:

1. Sort intervals by starting time.
2. Insert first interval into answer.
3. Traverse remaining intervals.
4. Compare current interval with last merged interval.
5. If overlapping:
   - extend end.
6. Otherwise:
   - push as new interval.

---

# ALGORITHM:

1. Sort intervals by start.
2. Create answer.
3. Push first interval.
4. For every remaining interval:
   - let last = answer.back()
   - if

```
current.start <= last.end
```

merge

```
last.end = max(last.end, current.end)
```

else

push current.

Return answer.

---

# DRY RUN:

Example:

```
[[6,8],[1,9],[2,4],[4,7]]
```

### Step 1

Sort

```
[1,9]
[2,4]
[4,7]
[6,8]
```

Answer:

```
[1,9]
```

---

Current

```
[2,4]
```

Overlap?

```
2 <= 9

Yes
```

Merge

```
[1,9]
```

---

Current

```
[4,7]
```

```
4 <= 9

Yes
```

Still

```
[1,9]
```

---

Current

```
[6,8]
```

```
6 <= 9

Yes
```

Still

```
[1,9]
```

Final Answer

```
[[1,9]]
```

---

Example 2

```
[1,3]
[2,4]
[6,8]
[9,10]
```

Answer

```
[1,3]
```

Current

```
[2,4]
```

Overlap

```
2<=3

Yes
```

Merge

```
[1,4]
```

Current

```
[6,8]
```

```
6<=4 ?

No
```

Push

```
[6,8]
```

Current

```
[9,10]
```

```
9<=8 ?

No
```

Push

Final

```
[1,4]
[6,8]
[9,10]
```

---

# COMMON MISTAKES:

### 1. Forgetting to sort

Most common mistake.

Without sorting,

the greedy logic fails.

---

### 2. Comparing wrong interval

Always compare with

```
answer.back()
```

NOT previous input interval.

---

### 3. Updating start instead of end

Correct:

```cpp
last[1] = max(last[1], current[1]);
```

Never modify start.

---

### 4. Wrong overlap condition

Correct:

```cpp
current.start <= last.end
```

NOT

```cpp
<
```

because

```
[1,3]
[3,5]
```

are considered overlapping in this problem.

---

### 5. Forgetting first interval

Initialize answer with first interval before scanning.

---

# INTERVIEW FLOW:

> We need to merge all overlapping intervals.

Brute force would compare every interval with every other interval, resulting in O(N²).

The key observation is that overlap decisions become local if we sort by starting time.

After sorting, overlapping intervals appear consecutively.

So we maintain the last merged interval.

For each interval:

- if it overlaps, extend the end
- otherwise start a new merged interval.

Since every interval is processed once after sorting,

overall complexity becomes O(N log N).

---

# TIME COMPLEXITY:

Sorting:

```
O(N log N)
```

Traversal:

```
O(N)
```

Overall:

```
O(N log N)
```

Sorting dominates.

---

# SPACE COMPLEXITY:

Answer vector:

```
O(N)
```

Sorting:

- Usually O(log N) recursion stack for `std::sort`.

Overall auxiliary (excluding output):

```
O(log N)
```

Including output:

```
O(N)
```

---

# EDGE CASES:

### Single interval

```
[1,5]

Output

[1,5]
```

---

### Already merged

```
[1,5]
[6,8]
```

No changes.

---

### All overlap

```
[1,10]
[2,5]
[3,6]
```

Output

```
[1,10]
```

---

### Touching intervals

```
[1,3]
[3,5]
```

Merged into

```
[1,5]
```

---

### Same intervals

```
[2,5]
[2,5]
```

Still

```
[2,5]
```

---

# PATTERN RECOGNITION:

This is likely an **Interval Greedy** problem when:

- You're given intervals/ranges.
- You need to merge, schedule, or detect overlaps.
- Decisions depend on interval ordering.
- Sorting by start or end simplifies the problem.
- After sorting, each interval only needs to be compared with the most recently processed interval.

Typical problems:
- Merge Intervals
- Insert Interval
- Meeting Rooms
- Non-overlapping Intervals
- Minimum Platforms
- Activity Selection

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<vector<int>> mergeOverlap(vector<vector<int>>& arr) {

        // Step 1: Sort intervals based on starting time
        sort(arr.begin(), arr.end());

        vector<vector<int>> ans;

        // First interval becomes the current merged interval
        ans.push_back(arr[0]);

        // Process remaining intervals
        for (int i = 1; i < arr.size(); i++) {

            // Reference to the last merged interval
            vector<int> &last = ans.back();

            // If intervals overlap, extend the ending point
            if (arr[i][0] <= last[1]) {
                last[1] = max(last[1], arr[i][1]);
            }
            // Otherwise start a new merged interval
            else {
                ans.push_back(arr[i]);
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### Sort the intervals

```cpp
sort(arr.begin(), arr.end());
```

**Why?**

Sorting places intervals in increasing order of their start times.

This guarantees that any future overlap can only occur with the last merged interval.

Without sorting, we'd have to compare against many previous intervals.

---

### Store first interval

```cpp
ans.push_back(arr[0]);
```

Initially, the first interval is the only merged interval.

It becomes our current interval to compare against.

---

### Get the last merged interval

```cpp
vector<int> &last = ans.back();
```

We always merge with the **most recently merged interval**, not the previous input interval.

Using a reference avoids copying and lets us update it directly.

---

### Check overlap

```cpp
if (arr[i][0] <= last[1])
```

This means:

```
Current interval starts before the last merged interval ends.
```

So they intersect (or touch) and should be merged.

---

### Extend the merged interval

```cpp
last[1] = max(last[1], arr[i][1]);
```

We keep the earliest start (already stored) and extend the end as far as necessary to cover both intervals.

---

### No overlap

```cpp
ans.push_back(arr[i]);
```

If there's no overlap, this interval starts a completely new merged interval.

---

# Tricky Condition Explained

### Why use

```cpp
arr[i][0] <= last[1]
```

instead of

```cpp
<
```

Consider:

```
[1,3]
[3,5]
```

They touch at `3`.

The problem considers these overlapping, so they should become:

```
[1,5]
```

Hence `<=` is required.

---

# Why Each Major Decision Is Made

### Why sort by start?

Because overlap is determined by when intervals begin.

Sorting ensures overlapping intervals become adjacent.

---

### Why only compare with the last merged interval?

The invariant guarantees that all previously overlapping intervals have already been merged into `ans.back()`.

So checking older intervals is unnecessary.

---

### Why merge immediately?

Keeping overlapping intervals separate provides no advantage.

Merging preserves all covered points while simplifying future decisions.

---

# Easy-to-Remember Interview Summary

> **Sort by start time → keep one current merged interval → if the next interval overlaps, extend its end; otherwise, start a new interval.**

**Memory Trick:**

> **Sort → Compare with Last → Merge if Overlap → Else Push**

That's the complete Interval Greedy pattern used in most interval-merging problems.
````
