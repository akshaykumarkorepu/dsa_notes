
## PROBLEM:

Given `N` meetings with their `start[]` and `end[]` times and only **one meeting room**, find the **maximum number of meetings** that can be conducted.

**Condition:** A meeting can be selected only if its **start time is strictly greater than** the end time of the previously selected meeting.

---

# PATTERN:

**Interval Greedy (Scheduling Greedy)**

More specifically:

> **Sort Intervals by Earliest Finishing Time + Greedily Select Compatible Intervals**

This is one of the most famous Greedy algorithms.

---

# WHY THIS PATTERN:

Whenever we have

- intervals
- meetings
- jobs
- activities
- schedules

and we want to

- maximize the number selected
- minimize removals
- avoid overlaps

the first question should always be:

> **Can sorting by ending time leave maximum room for future intervals?**

Here, the answer is **Yes**.

---

# CORE IDEA:

Imagine the meeting room as a timeline.

```
|------------------------------>
```

Every meeting occupies some portion.

If we finish one meeting **earlier**, we leave more free space afterwards.

More free space means:

- more future meetings can fit.

So,

> **Always finish as early as possible.**

That is exactly why we sort by ending time.

---

# GREEDY OBSERVATION:

Suppose two meetings overlap.

```
A : -----------

B : -----
```

If we pick A

```
Remaining free time = less
```

If we pick B

```
Remaining free time = more
```

Finishing earlier **can never reduce** the number of meetings we can take later.

Instead,

it can only increase or keep it same.

That is the Greedy observation.

---

# WHY GREEDY WORKS:

Suppose an optimal solution picks some meeting that finishes later instead of the earliest finishing meeting.

Replace it with the earlier finishing meeting.

Example

Chosen:

```
Meeting X
1 ------ 8
```

Instead choose

```
Meeting Y
1 --- 4
```

Everything after time 8 was possible before.

Everything after time 4 is still possible.

Actually,

even **more meetings become possible**.

Therefore,

replacing a later-ending meeting with an earlier-ending one never hurts.

This is called the **Greedy Choice Property**.

---

# GREEDY CHOICE:

After sorting,

**Always pick the first meeting whose start time is greater than the last selected meeting's end time.**

That meeting finishes earliest among all available choices.

---

# WHY THIS CHOICE IS SAFE:

Suppose current time is

```
lastEnd = 10
```

Possible meetings are

```
(11,12)

(11,15)

(11,20)
```

Which should we choose?

Obviously

```
(11,12)
```

because

```
After choosing:

Ends at 12

Remaining time:
12 ----------------------->
```

Instead of

```
Ends at 20

Remaining time:
20 ---------->
```

The earlier finish always leaves more opportunities.

Hence it is always safe.

---

# SORTING:

### Is sorting required?

**Yes.**

Without sorting,

you may pick a meeting that ends late and lose many future meetings.

Example

```
(1,10)

(2,3)

(4,5)

(6,7)
```

If you don't sort

Pick

```
(1,10)
```

Answer = 1

But optimal answer is

```
(2,3)

(4,5)

(6,7)
```

Answer = 3

Sorting by end time avoids this mistake.

### Sorting parameter

```
Increasing ending time
```

---

# INVARIANT:

After selecting meetings till index `i`:

- We have selected the **maximum possible meetings** among all meetings processed.
- `lastEndTime` is the ending time of the **last selected meeting**.
- The selected meetings never overlap.

This invariant remains true after every iteration.

---

# BRUTE FORCE:

### Is brute force necessary?

**No.**

Reason:

The greedy solution is the standard and most natural approach for the Activity Selection Problem.

A brute-force solution would check every subset of meetings (or recursively include/exclude meetings), which is exponential and offers little additional insight in an interview.

For completeness:

- Try every subset.
- Check if meetings overlap.
- Return the maximum valid subset.

### Pseudocode

```cpp
int ans = 0;

Generate all subsets of meetings

For every subset:
    Check if meetings overlap
    If valid:
        ans = max(ans, subset size)

Return ans;
```

### Time Complexity

```
O(2^N)
```

### Space Complexity

```
O(N)
```

Not practical.

---

# OPTIMAL APPROACH:

### Step 1

Store every meeting

```
(start,end)
```

---

### Step 2

Sort by

```
end time
```

---

### Step 3

Pick the first meeting.

```
count = 1

lastEnd = first meeting end
```

---

### Step 4

Traverse remaining meetings.

If

```
currentStart > lastEnd
```

take it.

Otherwise skip.

---

Continue till end.

---

# ALGORITHM:

```
Store all meetings

↓

Sort by ending time

↓

Pick first meeting

↓

For every remaining meeting

    if(start > lastEnd)

        take it

        update lastEnd

↓

Return count
```

---

# DRY RUN:

### Input

```
Start

1 3 0 5 8 5

End

2 4 6 7 9 9
```

---

### Step 1: Store Meetings

```
(1,2)

(3,4)

(0,6)

(5,7)

(8,9)

(5,9)
```

Already sorted by ending time.

---

### Step 2: Pick First Meeting

```
Selected

(1,2)

count = 1

lastEnd = 2
```

---

### Step 3: Check (3,4)

```
3 > 2

Yes

Take it

count = 2

lastEnd = 4
```

---

### Step 4: Check (0,6)

```
0 > 4

No

Skip
```

---

### Step 5: Check (5,7)

```
5 > 4

Yes

Take

count = 3

lastEnd = 7
```

---

### Step 6: Check (8,9)

```
8 > 7

Yes

Take

count = 4

lastEnd = 9
```

---

### Step 7: Check (5,9)

```
5 > 9

No

Skip
```

Final Answer

```
4
```

---

# COMMON MISTAKES:

### 1. Sorting by start time

❌ Wrong.

Always sort by **ending time**.

---

### 2. Updating `lastEndTime` even when skipping

❌ Wrong.

Update it **only when selecting** a meeting.

---

### 3. Forgetting to pick the first meeting

After sorting,

the first meeting always finishes earliest.

So it should always be selected.

---

### 4. Using `>=` instead of `>`

This problem says

```
start > previous end
```

not

```
start >= previous end
```

Many platforms use `>=`, so always read the statement carefully.

---

### 5. Empty Input

If `n == 0`, return `0` before accessing `meetings[0]`.

---

# INTERVIEW FLOW:

> We need to maximize the number of non-overlapping meetings. Since selecting meetings that finish earlier leaves more room for future meetings, I sort all meetings by their end times. Then I greedily select every meeting whose start time is strictly greater than the end time of the last selected meeting. This greedy choice is safe because replacing any later-ending meeting with an earlier-ending compatible meeting never reduces the number of meetings that can be scheduled. After sorting, only one linear scan is required.

---

# TIME COMPLEXITY:

Sorting

```
O(N log N)
```

Scanning

```
O(N)
```

Overall

```
O(N log N)
```

---

# SPACE COMPLEXITY:

Meeting vector

```
O(N)
```

If pairs are stored separately, the extra space is `O(N)`.

---

# EDGE CASES:

### Single Meeting

```
Answer = 1
```

---

### All Meetings Overlap

```
Only first meeting selected

Answer = 1
```

---

### No Meetings Overlap

```
Take every meeting

Answer = N
```

---

### Same Ending Time

Sorting still works.

Compatibility check determines which meetings can be selected.

---

### Empty Input

```
Return 0
```

---

# PATTERN RECOGNITION:

Whenever you see:

- Meetings
- Activities
- Appointments
- Schedules
- Time intervals

and you're asked to:

- maximize the number selected
- minimize removals
- avoid overlaps

immediately think:

> **Can I sort by ending time?**

If choosing the interval that finishes earliest leaves maximum room for future intervals, the solution is almost certainly **Interval Greedy (Activity Selection)**.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int maxMeetings(vector<int>& start, vector<int>& end) {

        int n = start.size();

        // Handle empty input
        if (n == 0)
            return 0;

        // Store each meeting as {start, end}
        vector<pair<int, int>> meetings;

        for (int i = 0; i < n; i++) {
            meetings.push_back({start[i], end[i]});
        }

        // Sort meetings by ending time
        sort(meetings.begin(), meetings.end(),
             [](const pair<int, int>& a, const pair<int, int>& b) {
                 return a.second < b.second;
             });

        // Select the first meeting
        int count = 1;
        int lastEndTime = meetings[0].second;

        // Try selecting remaining meetings
        for (int i = 1; i < n; i++) {

            // This problem requires strict inequality
            if (meetings[i].first > lastEndTime) {
                count++;
                lastEndTime = meetings[i].second;
            }
        }

        return count;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### `vector<pair<int,int>> meetings;`

We combine each meeting's start and end time so they stay together while sorting.

---

### `meetings.push_back({start[i], end[i]});`

Creates a single object representing one meeting.

---

### `sort(... by end time ...)`

This is the heart of the greedy strategy.

Choosing the meeting that finishes earliest leaves maximum time for future meetings.

---

### `count = 1;`

The first meeting after sorting always finishes the earliest.

So selecting it is always optimal.

---

### `lastEndTime = meetings[0].second;`

Stores the end time of the last selected meeting.

Future meetings must start after this.

---

### `if(meetings[i].first > lastEndTime)`

Checks whether the current meeting can be scheduled without overlapping.

---

### `count++;`

A compatible meeting increases our answer.

---

### `lastEndTime = meetings[i].second;`

Updates the boundary for checking future meetings.

---

# TRICKY CONDITION

```cpp
if(meetings[i].first > lastEndTime)
```

Notice the strict `>`.

Many interval problems use

```cpp
>=
```

This problem explicitly requires

```
start > previous end
```

Always read the problem statement carefully.

---

# WHY EACH MAJOR DECISION IS MADE

- Store meetings together to preserve `(start, end)` pairs.
- Sort by ending time because earlier finishing meetings maximize future opportunities.
- Always pick the first meeting since it finishes earliest.
- Skip overlapping meetings because they reduce the number of meetings we can schedule.
- Update `lastEndTime` only after selecting a meeting, since it should always represent the last scheduled meeting.

---

# EASY-TO-REMEMBER INTERVIEW SUMMARY

> **Activity Selection = Sort by Ending Time.**
>
> The meeting that finishes earliest always leaves the most room for future meetings. Therefore, sort all meetings by their end time and greedily select every meeting whose start time is strictly greater than the end time of the last selected meeting.
>
> This works because replacing a later-ending meeting with an earlier-ending compatible meeting never reduces the number of meetings that can be scheduled.
>
> **Time Complexity:** `O(N log N)`
>
> **Space Complexity:** `O(N)` (or `O(1)` extra if the input is stored directly as meeting pairs).
````
