

# PROBLEM:

You are given two arrays:

- `s[i]` → Start time of the `i-th` meeting.
- `f[i]` → Finish time of the `i-th` meeting.

There is only **one meeting room**.

Select the **maximum number of non-overlapping meetings** and return their **1-based indices in increasing order**.

**Important Note:**

Two meetings are considered overlapping if one starts **at or before** the previous meeting finishes.

So a meeting can only be selected if:

```cpp
start > lastEndTime
```

---

# PATTERN:

**Greedy → Interval Scheduling (Sort + Select)**

Flow:

```
Sort meetings by finish time
            ↓
Pick the earliest finishing meeting
            ↓
Skip overlapping meetings
            ↓
Continue selecting compatible meetings
```

This is one of the most common greedy interview patterns.

---

# WHY THIS PATTERN:

Every selected meeting occupies the room until it finishes.

The earlier a meeting finishes,

- the earlier the room becomes free.
- the more meetings can potentially fit afterwards.

So instead of choosing

- earliest starting meeting ❌
- shortest meeting ❌
- longest meeting ❌

we always choose

> **the meeting that finishes first.**

---

# CORE IDEA:

Think of the meeting room as a shared resource.

Every meeting blocks the room.

So we always want the room to become available **as early as possible**.

Earlier finish
→ more remaining free time
→ higher chance of scheduling more meetings.

---

# GREEDY OBSERVATION:

Suppose we have:

```
Meeting A : (1,10)

Meeting B : (1,3)

Meeting C : (4,5)
```

If we choose A

```
1-----------10
```

No more meetings fit.

Answer = 1

If we choose B

```
1---3

    4---5
```

Answer = 2

Choosing the meeting that finishes earlier leaves more opportunities.

---

# WHY GREEDY WORKS:

Suppose an optimal solution chooses a meeting that finishes later.

Replace it with another meeting that

- starts no later
- finishes earlier.

Nothing becomes worse.

Instead,

the room becomes free earlier,

so every future meeting that was previously possible is **still possible**, and maybe even more.

Therefore,

choosing the earliest finishing meeting never decreases the answer.

This is called the **Greedy Choice Property**.

---

# GREEDY CHOICE:

At every step,

choose the meeting that

- finishes earliest
- does not overlap with the previously selected meeting.

---

# WHY THIS CHOICE IS SAFE:

Suppose

```
Meeting A : ends at 8

Meeting B : ends at 5
```

Both are available.

Choosing Meeting B cannot reduce future choices because

after finishing at 5,

everything that could start after 8 can also start after 5.

Earlier finish

→ more free time

→ more future possibilities.

Hence this greedy choice is always safe.

---

# SORTING:

Sorting **is required**.

Sort meetings by

```
Finish Time (Ascending)
```

If finish times are equal,

sort by

```
Original Index
```

because the problem asks us to prefer the smaller indexed meeting.

Sorting allows us to process meetings in the correct greedy order.

---

# INVARIANT:

After every iteration,

`lastEndTime`

always stores the finish time of the **last selected meeting**.

Also,

the selected meetings

- never overlap
- are the maximum possible among all meetings processed so far.

This property always remains true.

---

# BRUTE FORCE:

## Is brute force necessary?

No.

This is a classic greedy scheduling problem.

Interviewers usually expect the greedy solution directly.

Brute force only demonstrates why greedy is needed.

## Idea

Generate every subset.

For each subset,

- check if meetings overlap.
- keep the maximum valid subset.

Total subsets:

```
2^N
```

Impossible for

```
N = 100000
```

### Time Complexity

```
O(2^N × N)
```

### Space Complexity

```
O(N)
```

Hence brute force is not practical.

---

# OPTIMAL APPROACH:

### Step 1

Store every meeting as

```
(start, finish, originalIndex)
```

---

### Step 2

Sort by finish time.

---

### Step 3

Always select the first meeting.

```
lastEndTime = finish
```

---

### Step 4

Traverse remaining meetings.

If

```cpp
start > lastEndTime
```

select it.

Update

```cpp
lastEndTime = finish
```

---

### Step 5

The meetings were selected in finish-time order.

The problem wants indices in increasing order.

So,

sort the answer before returning.

---

# ALGORITHM:

```
Create list:

(start, finish, index)

↓

Sort by finish time

↓

Take first meeting

↓

Store its finish time

↓

For every remaining meeting

    if(start > lastEndTime)

          select meeting

          update lastEndTime

↓

Sort selected indices

↓

Return answer
```

---

# DRY RUN:

Input

```
Start

1 3 0 5 8 5

Finish

2 4 6 7 9 9
```

Store

| Start | Finish | Index |
|-------|--------|-------|
|1|2|1|
|3|4|2|
|0|6|3|
|5|7|4|
|8|9|5|
|5|9|6|

Already sorted by finish time.

---

### Pick first meeting

```
(1,2)

Answer = [1]

lastEndTime = 2
```

---

### Meeting 2

```
(3,4)

3 > 2

Take
```

Answer

```
[1,2]
```

lastEndTime

```
4
```

---

### Meeting 3

```
(0,6)

0 > 4 ?

No

Skip
```

---

### Meeting 4

```
(5,7)

5 > 4

Take
```

Answer

```
[1,2,4]
```

lastEndTime

```
7
```

---

### Meeting 5

```
(8,9)

8 > 7

Take
```

Answer

```
[1,2,4,5]
```

lastEndTime

```
9
```

---

### Meeting 6

```
(5,9)

5 > 9 ?

No

Skip
```

Sort answer

```
[1,2,4,5]
```

Final Answer.

---

# COMMON MISTAKES:

### 1. Sorting by start time

Wrong.

We must sort by finish time.

---

### 2. Using

```cpp
start >= lastEndTime
```

Wrong.

The problem clearly states meetings sharing an endpoint overlap.

Correct condition is

```cpp
start > lastEndTime
```

---

### 3. Forgetting original indices

After sorting,

the original order is lost.

Always store

```cpp
index = i + 1;
```

---

### 4. Forgetting to sort the answer

Meetings are selected by finish time.

The problem asks for indices in increasing order.

Hence

```cpp
sort(ans.begin(), ans.end());
```

---

### 5. Incorrect tie-breaking

If finish times are equal,

choose the smaller original index.

---

# INTERVIEW FLOW:

Interviewer:

"How would you solve this problem?"

Answer:

> Every selected meeting blocks the room until it finishes. To maximize the number of meetings, we should free the room as early as possible. Therefore, we sort all meetings by finish time and always select the earliest finishing meeting that doesn't overlap with the previously selected one. Finishing earlier never reduces future scheduling options—it only increases them. This satisfies the greedy choice property and gives the optimal solution.

---

# TIME COMPLEXITY:

Creating meeting list

```
O(N)
```

Sorting meetings

```
O(N log N)
```

Greedy traversal

```
O(N)
```

Sorting answer

```
O(N log N)
```

Overall

```
O(N log N)
```

Sorting dominates the complexity.

---

# SPACE COMPLEXITY:

Meeting list

```
O(N)
```

Answer vector

```
O(N)
```

Overall

```
O(N)
```

---

# EDGE CASES:

### Single meeting

```
Start

3

Finish

7

Answer

[1]
```

---

### All meetings overlap

Only one meeting can be selected.

---

### No meetings overlap

Every meeting gets selected.

---

### Same finish times

Choose the meeting with the smaller index.

---

### Empty input

Return an empty vector.

---

# PATTERN RECOGNITION:

Immediately think of **Interval Scheduling (Sort + Select)** whenever you see:

- Maximum number of meetings/events/jobs.
- One room / one machine / one resource.
- Meetings cannot overlap.
- Goal is to maximize the number of intervals selected.
- Selecting one interval affects future choices.

### Pattern Summary

**Pattern**

```
Interval Greedy (Sort + Select)
```

**Sort By**

```
Finish Time (Ascending)

If equal → Original Index
```

**Greedy Choice**

```
Always select the earliest finishing compatible meeting.
```

**Why Local Optimum Becomes Global Optimum**

```
Earlier finish leaves the room free sooner.

This never decreases future scheduling possibilities.
```

**Invariant**

```
Selected meetings are always non-overlapping.

lastEndTime always stores the finish time of the last selected meeting.
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    vector<int> maxMeetings(vector<int> &s, vector<int> &f) {

        int n = s.size();

        if (n == 0)
            return {};

        // Store {start, finish, original index}
        vector<vector<int>> meetings;

        for (int i = 0; i < n; i++) {
            meetings.push_back({s[i], f[i], i + 1});
        }

        // Sort by finish time.
        // If finish times are equal,
        // choose the meeting with the smaller index.
        sort(meetings.begin(), meetings.end(),
             [](vector<int> &a, vector<int> &b) {

                 if (a[1] == b[1])
                     return a[2] < b[2];

                 return a[1] < b[1];
             });

        vector<int> ans;

        // Always take the earliest finishing meeting.
        ans.push_back(meetings[0][2]);

        int lastEndTime = meetings[0][1];

        // Check remaining meetings.
        for (int i = 1; i < n; i++) {

            // Strictly greater because equal times overlap.
            if (meetings[i][0] > lastEndTime) {

                ans.push_back(meetings[i][2]);

                lastEndTime = meetings[i][1];
            }
        }

        // Return indices in increasing order.
        sort(ans.begin(), ans.end());

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Store `(start, finish, index)`

```cpp
meetings.push_back({s[i], f[i], i + 1});
```

Sorting changes the order of meetings.

The original index is needed for the final answer.

---

### Sort by finish time

```cpp
sort(...)
```

Meetings that finish earlier free the room earlier.

This maximizes future scheduling opportunities.

---

### Tie-break by index

```cpp
if (a[1] == b[1])
    return a[2] < b[2];
```

If two meetings finish together,

choose the smaller indexed meeting.

---

### Pick the first meeting

```cpp
ans.push_back(meetings[0][2]);
```

The earliest finishing meeting is always safe to take because nothing has been selected yet.

---

### Track room availability

```cpp
lastEndTime = meetings[0][1];
```

This tells us when the room becomes free again.

---

### Compatibility check

```cpp
if (meetings[i][0] > lastEndTime)
```

Only meetings that start strictly after the previous meeting ends are valid.

---

### Update finish time

```cpp
lastEndTime = meetings[i][1];
```

The newly selected meeting now occupies the room until its finish time.

---

### Sort answer

```cpp
sort(ans.begin(), ans.end());
```

Meetings were selected in finish-time order.

The problem requires indices in increasing order.

---

# WHY EACH MAJOR DECISION IS MADE

- Sort by finish time because earlier finishing meetings maximize future opportunities.
- Always choose the first compatible meeting because it frees the room earliest.
- Skip overlapping meetings because they cannot coexist with the last selected meeting.
- Store original indices because sorting changes the original order.
- Sort the final answer because the output requires increasing indices.

---

# EASY-TO-REMEMBER INTERVIEW SUMMARY

> This is the classic **Interval Scheduling Greedy** problem.
>
> Sort all meetings by finish time, always choose the earliest finishing compatible meeting, update the last finish time, and continue.
>
> **Finishing earlier leaves the room free sooner, which can never reduce future scheduling possibilities.**
>
> That is why the greedy choice is always optimal.
````
