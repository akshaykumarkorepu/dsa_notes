

# PROBLEM:

You are given a list of **non-overlapping intervals sorted by starting time** and a new interval.

Insert the new interval into the correct position while ensuring:

- Intervals remain sorted.
- No overlapping intervals remain (merge wherever necessary).

---

# PATTERN:

**Interval Greedy (Three-Phase Interval Processing)**

This is an Interval Greedy problem because we process intervals from left to right and make an immediate greedy decision whenever an overlap occurs.

---

# WHY THIS PATTERN:

The problem already guarantees:

- Intervals are sorted by start time.
- Existing intervals do not overlap.

Because of these guarantees, every interval can belong to exactly one of three categories:

1. Completely before the new interval
2. Overlapping the new interval
3. Completely after the new interval

Since the intervals are sorted, these categories also appear in the same order.

Therefore, we only need one linear pass.

---

# CORE IDEA:

Think of the new interval as a **growing interval**.

Initially,

```
newInterval = [5,6]
```

As we encounter overlapping intervals,

it expands:

```
[5,6]
↓

[4,6]
↓

[4,7]
↓

[4,10]
```

Finally, we insert this fully merged interval exactly once.

---

# GREEDY OBSERVATION:

Whenever an interval overlaps with the current merged interval,

**merge it immediately.**

There is never any benefit in delaying the merge because overlapping intervals must eventually become one interval.

---

# WHY GREEDY WORKS:

Suppose we have

```
Current merged interval

[5,8]

Next interval

[7,12]
```

These overlap.

Whether we merge them now or later, the final answer is always

```
[5,12]
```

Therefore, merging immediately never changes the optimal answer.

The local optimum (merge now) is also part of the global optimum.

---

# GREEDY CHOICE:

Whenever

```
interval.start <= merged.end
```

merge immediately by expanding the merged interval.

```
merged.start=min(...)

merged.end=max(...)
```

---

# WHY THIS CHOICE IS SAFE:

Once two intervals overlap,

they must belong to the same merged interval.

No future interval can separate them again.

Therefore, merging immediately never hurts the optimal solution.

This is exactly the **Greedy Choice Property**.

---

# SORTING:

### Is sorting required?

**No.**

Why?

The problem already guarantees:

- intervals are sorted
- intervals are non-overlapping

Therefore,

we never need to reorder anything.

The answer automatically remains sorted because we process in order.

---

# INVARIANT:

At every iteration:

- `ans` contains finalized non-overlapping intervals.
- `newInterval` represents the merged interval of all overlaps seen so far.
- Remaining intervals are still unprocessed.

This invariant remains true throughout the algorithm.

---

# BRUTE FORCE:

## Is brute force necessary?

**No.**

However, interviewers may expect you to mention it briefly before giving the optimal solution.

### Brute Force Idea

1. Insert new interval.
2. Sort intervals.
3. Run Merge Intervals.

### Code

```cpp
intervals.push_back(newInterval);

sort(intervals.begin(), intervals.end());

return merge(intervals);
```

### Time Complexity

```
O(n log n)
```

Sorting dominates.

### Why optimize?

The input is already sorted.

Sorting again wastes time.

Hence we exploit the sorted property to obtain an O(n) solution.

---

# OPTIMAL APPROACH:

Process the intervals in **three phases**.

### Phase 1

Copy all intervals that end before the new interval starts.

```
Current

[1,2]

New

[5,6]
```

Since

```
2 < 5
```

they cannot overlap.

Copy them directly.

---

### Phase 2

Merge every overlapping interval.

Expand

```
newInterval
```

using

```
start=min(...)

end=max(...)
```

---

### Phase 3

Insert the merged interval once.

Then copy all remaining intervals.

---

# ALGORITHM:

1. Create answer vector.
2. Copy intervals before the new interval.
3. Merge all overlapping intervals.
4. Push merged interval.
5. Copy remaining intervals.
6. Return answer.

---

# DRY RUN:

Example

```
Intervals

[1,3]
[4,5]
[6,7]
[8,10]

New

[5,6]
```

### Phase 1

```
[1,3]
```

Ends before 5.

Copy.

Answer

```
[1,3]
```

---

### Phase 2

Current merged

```
[5,6]
```

Overlap with

```
[4,5]
```

```
Merged

[4,6]
```

Next

```
[6,7]
```

```
Merged

[4,7]
```

Next

```
[8,10]
```

No overlap.

Stop.

---

Push merged interval.

```
Answer

[1,3]
[4,7]
```

---

### Phase 3

Copy remaining.

```
[8,10]
```

Final answer

```
[1,3]
[4,7]
[8,10]
```

---

# COMMON MISTAKES:

### 1. Checking the wrong endpoint

Wrong

```cpp
intervals[i][0] < newInterval[0]
```

Correct

```cpp
intervals[i][1] < newInterval[0]
```

We care about where the interval ends.

Example

```
Current

[2,10]

New

[5,6]
```

Although

```
2 < 5
```

they overlap.

---

### 2. Using <= in Phase 1

Wrong

```cpp
intervals[i][1] <= newInterval[0]
```

Example

```
Current

[4,5]

New

[5,6]
```

Touching intervals should merge.

Hence

```
<
```

must be used.

---

### 3. Using < in overlap detection

Wrong

```cpp
intervals[i][0] < newInterval[1]
```

Correct

```cpp
intervals[i][0] <= newInterval[1]
```

Touching endpoints also overlap.

---

### 4. Forgetting that newInterval changes

Many people compare with the original interval.

Wrong.

Always compare with the updated merged interval.

---

### 5. Pushing newInterval too early

Never do

```
ans.push_back(newInterval)
```

before all merges finish.

Otherwise the answer itself may contain overlapping intervals.

---

### 6. Forgetting to copy remaining intervals

After inserting the merged interval,

don't forget Phase 3.

---

# INTERVIEW FLOW:

**Step 1**

The intervals are already sorted and non-overlapping.

So sorting again would be unnecessary.

---

**Step 2**

Every interval must be either:

- before
- overlapping
- after

Because of sorting, these groups appear consecutively.

---

**Step 3**

Copy all intervals before.

---

**Step 4**

Whenever an interval overlaps,

merge immediately by expanding the current merged interval.

---

**Step 5**

Insert the merged interval once.

---

**Step 6**

Copy remaining intervals.

Done in one traversal.

---

# TIME COMPLEXITY:

## Time

```
O(n)
```

Reason:

Each interval is visited exactly once.

No interval is revisited.

No sorting.

---

# SPACE COMPLEXITY:

Auxiliary Space:

```
O(1)
```

Reason:

Extra variables used are only:

- `i`
- `n`
- `newInterval`

Output vector is not counted as auxiliary space.

If output is included,

overall space becomes

```
O(n)
```

---

# EDGE CASES:

### 1. Insert at beginning

```
Intervals

[5,7]

New

[1,2]
```

---

### 2. Insert at end

```
Intervals

[1,3]

New

[8,10]
```

---

### 3. Merge every interval

```
New

[0,20]
```

---

### 4. No overlap

```
[1,2]

[5,6]
```

---

### 5. Touching endpoints

```
[4,5]

[5,6]
```

Must merge.

---

### 6. Single interval

Works naturally.

---

### 7. New interval already inside an existing interval

```
Existing

[2,10]

New

[4,5]
```

Result

```
[2,10]
```

---

# PATTERN RECOGNITION:

You should immediately think of this pattern when you see:

- Intervals
- Already sorted
- Non-overlapping
- Insert one interval
- Merge overlaps
- Maintain sorted order

This almost always indicates:

**Interval Greedy (Three-Phase Processing)**

The template becomes:

```
Copy before

↓

Merge overlaps

↓

Copy after
```

---

# Alternative Greedy Choices (Why They Fail)

### Greedy Choice 1: Insert and sort later

Works, but costs:

```
O(n log n)
```

Unnecessary because the input is already sorted.

---

### Greedy Choice 2: Delay merging until the end

Doesn't simplify anything.

You'll still merge the same intervals later.

Immediate merging is simpler and equally optimal.

---

### Greedy Choice 3: Compare using the original new interval

Fails.

Example:

```
Intervals

[4,5]
[6,7]

New

[5,6]
```

After first merge:

```
new = [4,6]
```

Now the second interval overlaps.

If you still compare with the original

```
[5,6]
```

the logic breaks.

Always compare against the **updated merged interval**.

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<vector<int>> insertInterval(vector<vector<int>>& intervals,
                                       vector<int>& newInterval) {

        vector<vector<int>> ans;

        int n = intervals.size();
        int i = 0;

        // ----------------------------
        // Phase 1:
        // Copy all intervals completely before newInterval
        // ----------------------------
        while (i < n && intervals[i][1] < newInterval[0]) {
            ans.push_back(intervals[i]);
            i++;
        }

        // ----------------------------
        // Phase 2:
        // Merge all overlapping intervals
        // ----------------------------
        while (i < n && intervals[i][0] <= newInterval[1]) {

            // Expand merged interval
            newInterval[0] = min(newInterval[0], intervals[i][0]);
            newInterval[1] = max(newInterval[1], intervals[i][1]);

            i++;
        }

        // Insert merged interval exactly once
        ans.push_back(newInterval);

        // ----------------------------
        // Phase 3:
        // Copy remaining intervals
        // ----------------------------
        while (i < n) {
            ans.push_back(intervals[i]);
            i++;
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

## Phase 1

```cpp
while(i<n && intervals[i][1] < newInterval[0])
```

We compare the **end** of the current interval with the **start** of the new interval.

If the current interval ends before the new interval starts, they can never overlap.

So we safely copy it.

Notice the strict `<`.

If they merely touch (e.g. `[4,5]` and `[5,6]`), they should merge, so we must **not** copy them here.

---

## Phase 2

```cpp
while(i<n && intervals[i][0] <= newInterval[1])
```

Now we check if the current interval starts before (or exactly at) the end of the merged interval.

If yes, they overlap.

The `<=` is important because touching endpoints are also considered overlapping.

---

## Expanding the merged interval

```cpp
newInterval[0] = min(newInterval[0], intervals[i][0]);
newInterval[1] = max(newInterval[1], intervals[i][1]);
```

Instead of creating a new interval every time, we grow the existing `newInterval`.

After each merge, `newInterval` represents the merged result of **all** overlapping intervals seen so far.

This is why it correctly handles multiple consecutive overlaps.

---

## Push only once

```cpp
ans.push_back(newInterval);
```

We wait until **all** overlaps are processed.

If we inserted it earlier, later overlaps would create overlapping intervals inside the answer.

---

## Phase 3

```cpp
while(i<n)
```

Once we leave the merge loop, all remaining intervals start after the merged interval ends.

Since the input is sorted and non-overlapping, they can be copied directly.

---

# Interview Summary (Easy Memory Trick)

Remember just **three ideas**:

1. **Three Phases**

```
Before
↓

Merge
↓

After
```

2. **`newInterval` is not fixed—it keeps growing** as overlaps are merged.

3. **Always compare with the current merged interval**, not the original new interval.

If you remember these three concepts, you can derive the entire O(n) greedy solution during an interview without memorizing the code.
````
