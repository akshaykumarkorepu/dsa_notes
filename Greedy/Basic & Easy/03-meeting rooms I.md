

## PROBLEM:

You are given `n` meetings, where each meeting is represented as `[start, end]`.

A person can attend **only one meeting at a time**.

Return **true** if it is possible to attend **all** meetings, otherwise return **false**.

**Important:** A meeting starting exactly when the previous meeting ends is allowed (`start >= previousEnd`).

---

# PATTERN:

**Interval Greedy (Sort + Check Overlap)**

---

# WHY THIS PATTERN:

Whenever a problem involves **intervals** (meetings, events, bookings, appointments), one of the first questions to ask is:

> "Can sorting the intervals make the problem easier?"

Here, the input meetings are in arbitrary order.

To correctly determine whether any two meetings overlap, we first need to arrange them in **chronological order**.

Once sorted, we only need to compare each meeting with the one immediately before it.

This is why this belongs to the **Interval Greedy** pattern.

---

# CORE IDEA:

Arrange all meetings in chronological order.

Then check whether every meeting starts **after or exactly when** the previous meeting ends.

- If yes → All meetings are attendable.
- If not → At least two meetings overlap, so attending all is impossible.

---

# GREEDY OBSERVATION:

After sorting by **starting time**, every possible conflict will appear between two consecutive meetings.

So instead of comparing every meeting with every other meeting, we only compare adjacent meetings.

If

Current Start < Previous End

then an overlap exists.

Immediately return **false**.

---

# WHY GREEDY WORKS:

The local decision is simple:

> "Can I safely move to the next meeting?"

After sorting,

if the current meeting does not overlap with the previous one,

then every meeting processed so far is conflict-free.

If an overlap is found,

no future rearrangement can remove that overlap because the meetings themselves overlap in time.

Thus,

checking adjacent meetings is sufficient.

This local verification guarantees the global answer.

---

# GREEDY CHOICE:

At every step,

compare only

Current Meeting

with

Previous Meeting.

If

Current Start ≥ Previous End

continue.

Otherwise,

return false.

---

# WHY THIS CHOICE IS SAFE:

Suppose two consecutive meetings overlap.

Example:

```
Meeting A

2--------8

Meeting B

      6--------10
```

Since Meeting B starts before Meeting A finishes,

both meetings cannot be attended.

Changing the order does not change their timings.

Therefore,

once an overlap is detected,

the answer must be false.

Hence the greedy choice is always safe.

---

# SORTING:

### Is sorting required?

**Yes.**

### Sort by

**Starting time (ascending).**

### Why?

The input order has no meaning.

Without sorting,

an earlier meeting may appear later in the array,

making overlap detection incorrect.

Sorting places meetings in chronological order,

allowing us to detect conflicts using only adjacent meetings.

---

# INVARIANT:

After processing the first `i` meetings,

the following property always holds:

- All processed meetings are in chronological order.
- None of them overlap.
- If we haven't returned false yet,
  then attending all processed meetings is still possible.

This invariant remains true after every iteration.

---

# BRUTE FORCE:

### Is brute force necessary?

**No.**

### Why?

The greedy solution is already straightforward and naturally derived.

A brute-force solution would compare every meeting with every other meeting.

```
For every meeting
    Compare with all remaining meetings
```

Time Complexity:

**O(n²)**

Since sorting immediately reduces the problem to checking adjacent meetings,

there is no meaningful optimization journey.

In interviews,

directly presenting the sorting solution is the preferred approach.

---

# OPTIMAL APPROACH:

1. Sort meetings according to starting time.
2. Traverse from the second meeting.
3. Compare

   Current Start

   with

   Previous End.

4. If Current Start < Previous End

   return false.

5. Otherwise continue.

6. If traversal completes,

   return true.

---

# ALGORITHM:

1. Sort the meetings by starting time.
2. Start traversing from index 1.
3. Compare

   arr[i][0]

   with

   arr[i-1][1].

4. If overlap exists,

   return false.

5. Otherwise continue checking.

6. Return true.

---

# DRY RUN:

### Example 1

Input

```
[[1,4],
 [10,15],
 [7,10]]
```

### Step 1

Sort

```
[[1,4],
 [7,10],
 [10,15]]
```

---

Compare

Meeting 1

```
1------4
```

Meeting 2

```
       7------10
```

Check

```
7 < 4 ?

False
```

No overlap.

---

Compare

Meeting 2

```
7------10
```

Meeting 3

```
        10------15
```

Check

```
10 < 10 ?

False
```

Meeting starts exactly when previous ends.

Allowed.

Answer

```
true
```

---

### Example 2

Input

```
[[2,4],
 [9,12],
 [6,10]]
```

Sort

```
[[2,4],
 [6,10],
 [9,12]]
```

Compare

```
6 < 4 ?

False
```

Continue.

---

Compare

```
9 < 10 ?

True
```

Meetings overlap.

Return

```
false
```

---

# COMMON MISTAKES:

### Mistake 1

Forgetting to sort.

The input order is arbitrary.

---

### Mistake 2

Using

```
currentStart <= previousEnd
```

instead of

```
currentStart < previousEnd
```

Equality is allowed.

---

### Mistake 3

Sorting by ending time.

Ending time sorting is useful for **Activity Selection**.

Here we only need to detect overlap.

Sorting by start time is sufficient.

---

### Mistake 4

Comparing every meeting with every other meeting.

After sorting,

only adjacent meetings need comparison.

---

# INTERVIEW FLOW:

**Interviewer:** How would you solve this?

↓

Meetings are given in random order.

↓

Let's first sort them according to starting time.

↓

Now meetings are arranged chronologically.

↓

If any overlap exists,

it must appear between consecutive meetings.

↓

Traverse once.

↓

If

Current Start < Previous End

return false.

↓

Otherwise continue.

↓

Traversal finishes.

↓

Return true.

---

# TIME COMPLEXITY:

### Sorting

```
O(n log n)
```

### Traversal

```
O(n)
```

Overall

```
O(n log n)
```

### Reason

Sorting dominates the overall complexity.

The linear scan is comparatively smaller.

---

# SPACE COMPLEXITY:

```
O(1)
```

(excluding sorting implementation)

### Reason

Only a few variables are used.

If recursive sorting stack is counted,

it becomes **O(log n)** depending on the sorting implementation.

---

# EDGE CASES:

- Only one meeting.
- Meetings already sorted.
- Meetings completely overlap.
- Meetings touch exactly at endpoints.
- Duplicate meetings.
- Large number of meetings.
- Empty overlap after sorting.

---

# PATTERN RECOGNITION:

You should immediately think of this pattern whenever you see:

- Meetings
- Events
- Calendar bookings
- Appointments
- Time intervals
- Booking conflicts
- Need to check if intervals overlap

Ask yourself:

> "If I sort these intervals, can I detect conflicts by scanning once?"

If the answer is yes,

it's usually an **Interval Greedy** problem.

### Recognition Rule

**Intervals + Need to detect overlap/conflict → Sort by Start Time + Check Adjacent Intervals**

---

# Clean C++ Code

```cpp
class Solution {
public:
    bool canAttend(vector<vector<int>>& arr) {

        // Sort meetings according to starting time
        sort(arr.begin(), arr.end());

        // Compare every meeting with the previous one
        for (int i = 1; i < arr.size(); i++) {

            // Overlap found
            if (arr[i][0] < arr[i - 1][1])
                return false;
        }

        // No overlap found
        return true;
    }
};
```

---

# Intuition Behind Every Important Line

### `sort(arr.begin(), arr.end());`

The meetings are given in random order.

Sorting arranges them chronologically.

Once sorted, every possible conflict becomes easy to detect.

---

### `for(int i = 1; i < arr.size(); i++)`

Start from the second meeting because every meeting needs to be compared with its previous meeting.

---

### `arr[i][0]`

Current meeting's starting time.

---

### `arr[i-1][1]`

Previous meeting's ending time.

---

### `if(arr[i][0] < arr[i-1][1])`

This checks whether the current meeting starts before the previous meeting finishes.

If yes,

the meetings overlap.

Attending both is impossible.

---

### `return false;`

The first overlap is enough.

There is no need to continue checking.

---

### `return true;`

The loop completed without finding any overlap.

Therefore,

all meetings can be attended.

---

# Why Each Major Decision Is Made

### Why sort first?

Because overlap can only be correctly detected when meetings are arranged in chronological order.

---

### Why compare only adjacent meetings?

After sorting,

if no adjacent meetings overlap,

then no non-adjacent meetings can overlap without also violating the adjacent comparison.

---

### Why immediately return false?

One overlap is sufficient to prove that attending every meeting is impossible.

Checking further meetings is unnecessary.

---

### Why is equality allowed?

The problem states

```
Current Start ≥ Previous End
```

Therefore

```
Current Start == Previous End
```

is perfectly valid.

Hence we use

```
<
```

instead of

```
<=
```

---

# Easy-to-Remember Interview Summary

✅ Pattern: **Interval Greedy**

✅ Sort meetings by **starting time**.

✅ Scan from left to right.

✅ If

```
Current Start < Previous End
```

an overlap exists.

Return **false**.

Otherwise continue.

If no overlap is found,

return **true**.

### Memory Trick

> **"Whenever a problem asks whether time intervals overlap, sort by start time and compare adjacent intervals."**
````
