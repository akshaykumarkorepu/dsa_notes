

## PROBLEM:

You are given the start and end times of `n` meetings.

Return the **minimum number of meeting rooms** required so that all meetings can take place.

> **Important:** If one meeting ends at time `t` and another starts at time `t`, **the same room can be reused**.

---

# PATTERN:

**Greedy → Two Sorted Arrays + Two Pointers (Sweep Line / Overlapping Intervals)**

This is the **exact same pattern** as **Minimum Platforms**.

---

# WHY THIS PATTERN:

A new room is required **only when a meeting starts before any currently occupied room becomes free**.

Instead of checking overlaps with every meeting, process meetings in chronological order.

So we:

- Sort all start times.
- Sort all end times.
- Move through both arrays simultaneously.

This lets us know exactly when:

- a room becomes occupied
- a room becomes free

without explicitly assigning meetings to rooms.

---

# CORE IDEA:

Imagine walking along the timeline.

Whenever:

- a meeting starts → occupy one room
- a meeting ends → free one room

The **maximum number of rooms occupied simultaneously** is the answer.

---

# GREEDY OBSERVATION:

At every meeting start:

- If no room has become free yet,
  → allocate a new room.

- Otherwise,
  → immediately reuse the earliest room that became free.

Always reusing the earliest available room is optimal.

---

# WHY GREEDY WORKS:

Only the **earliest ending meeting** can possibly free a room first.

If even that room isn't free,

then no other room can be free.

So:

- earliest end unavailable
  → new room required

- earliest end available
  → reuse it immediately

No future decision can reduce the number of rooms already needed.

---

# GREEDY CHOICE:

Whenever possible,

**reuse the room whose meeting finishes earliest.**

Otherwise,

allocate one more room.

---

# WHY THIS CHOICE IS SAFE:

Suppose the earliest ending room cannot be reused.

Every other room finishes even later.

Therefore,

none of them can be reused either.

So opening a new room is unavoidable.

Hence the greedy choice is always optimal.

---

# SORTING:

Yes.

Sort

- start times
- end times

independently.

Sorting creates the chronological order of

- meeting arrivals
- meeting completions

which allows processing events from left to right.

---

# INVARIANT:

After processing every event,

`currentRooms`

always equals

> number of meetings currently running.

`maxRooms`

always stores

> maximum simultaneous meetings seen so far.

This invariant remains true throughout the algorithm.

---

# BRUTE FORCE:

**Brute force is unnecessary.**

A naive solution would compare every meeting with every other meeting or simulate room assignments.

This takes **O(n²)** or worse.

Since the problem is fundamentally about counting overlapping intervals,

sorting and processing events directly leads naturally to the optimal **O(n log n)** solution.

---

# OPTIMAL APPROACH:

1. Sort start array.
2. Sort end array.
3. Use two pointers.

Pointer `i`

→ next meeting starting.

Pointer `j`

→ next meeting ending.

If

```cpp
start[i] < end[j]
```

No room is free yet.

Need another room.

Otherwise

```cpp
start[i] >= end[j]
```

A meeting has ended.

Free one room.

Continue until every meeting has been processed.

---

# ALGORITHM:

1. Sort start array.
2. Sort end array.
3. Initialize

```cpp
currentRooms = 0;
maxRooms = 0;
i = 0;
j = 0;
```

4. While both pointers are valid

If

```cpp
start[i] < end[j]
```

```cpp
currentRooms++;
maxRooms = max(maxRooms, currentRooms);
i++;
```

Else

```cpp
currentRooms--;
j++;
```

5. Return `maxRooms`.

---

# DRY RUN:

```cpp
start = [2,6,9]
end   = [4,10,12]
```

Initially

```cpp
currentRooms = 0
maxRooms = 0
```

### Meeting starts at 2

```cpp
2 < 4

currentRooms = 1
maxRooms = 1
```

---

### Meeting ends at 4

```cpp
6 >= 4

currentRooms = 0
```

Room becomes free.

---

### Meeting starts at 6

```cpp
6 < 10

currentRooms = 1
maxRooms = 1
```

---

### Meeting starts at 9

```cpp
9 < 10

currentRooms = 2
maxRooms = 2
```

Need another room.

---

### Meeting ends at 10

```cpp
currentRooms = 1
```

---

### Meeting ends at 12

```cpp
currentRooms = 0
```

Answer

```cpp
2
```

---

# COMMON MISTAKES:

### Mistake 1

Using

```cpp
if(start[i] <= end[j])
```

This is **wrong**.

The problem says

> start == end

means the same room can be reused.

Correct condition

```cpp
if(start[i] < end[j])
```

---

### Mistake 2

Not sorting both arrays.

Without sorting,

events are no longer processed chronologically.

---

### Mistake 3

Returning `currentRooms`.

The answer is

> maximum rooms ever occupied

not

> rooms occupied at the end.

---

### Mistake 4

Trying to explicitly assign meetings to rooms.

Not needed.

Only the count matters.

---

# INTERVIEW FLOW:

Interviewer:

**"How would you solve it?"**

You:

> We only care about how many meetings overlap at any moment.

> Instead of assigning rooms, I'll process all meeting starts and ends in chronological order.

> I'll sort both arrays separately.

> If the next meeting starts before the earliest meeting ends, I need another room.

> Otherwise, one room becomes free and I reuse it.

> The maximum simultaneous rooms occupied is the answer.

---

# TIME COMPLEXITY:

Sorting

```cpp
O(n log n)
```

Two-pointer traversal

```cpp
O(n)
```

Overall

```cpp
O(n log n)
```

---

# SPACE COMPLEXITY:

Ignoring sorting implementation

```cpp
O(1)
```

If sorting uses recursion

```cpp
O(log n)
```

---

# EDGE CASES:

### One meeting

```cpp
start = [5]
end = [10]

Answer = 1
```

---

### No overlaps

```cpp
1-2
2-3
3-4

Answer = 1
```

---

### All overlap

```cpp
1-10
2-9
3-8
4-7

Answer = 4
```

---

### Equal start and end

```cpp
1-3
3-5

Answer = 1
```

This is the most important edge case.

---

# PATTERN RECOGNITION:

Whenever you see

- Minimum Meeting Rooms
- Minimum Platforms
- Minimum Conference Rooms
- Number of Concurrent Users
- CPU Scheduling
- Maximum Simultaneous Events
- Overlapping Intervals

think

> "Count the maximum number of intervals active at the same time."

Then immediately think

> Sort starts + Sort ends + Two Pointers.

---

# DIFFERENCE FROM MINIMUM PLATFORMS ⭐

These two problems use the **same algorithm**, but differ in **one comparison operator**.

| Minimum Platforms | Meeting Rooms II |
|-------------------|------------------|
| Train arrival at the exact departure time **still needs another platform** | Meeting starting exactly when another ends **can reuse the same room** |
| Condition: `arrival <= departure` | Condition: `start < end` |
| Equality (`==`) counts as overlap | Equality (`==`) does **not** count as overlap |

### Minimum Platforms

```cpp
if(arrival[i] <= departure[j])
```

If

```text
Arrival = 1000
Departure = 1000
```

the arriving train still needs a **new platform**.

---

### Meeting Rooms II

```cpp
if(start[i] < end[j])
```

If

```text
Meeting 1 : 1 - 5
Meeting 2 : 5 - 8
```

the same room is immediately reused.

---

## Easy way to remember

```text
Minimum Platforms
-----------------
arrival == departure
Need NEW platform

Use <=


Meeting Rooms II
----------------
start == end
Reuse SAME room

Use <
```

This single comparison operator is the **only logical difference** between the two problems.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int minMeetingRooms(vector<int> &start, vector<int> &end) {

        int n = start.size();

        // Sort all meeting start times
        sort(start.begin(), start.end());

        // Sort all meeting end times
        sort(end.begin(), end.end());

        // Pointer for next meeting start
        int i = 0;

        // Pointer for next meeting end
        int j = 0;

        // Rooms currently occupied
        int currentRooms = 0;

        // Maximum rooms ever occupied
        int maxRooms = 0;

        while (i < n && j < n) {

            // Next meeting starts before the earliest one ends.
            // Need one more room.
            if (start[i] < end[j]) {

                currentRooms++;

                maxRooms = max(maxRooms, currentRooms);

                i++;
            }
            else {

                // A meeting has finished.
                // Free one room.
                currentRooms--;

                j++;
            }
        }

        return maxRooms;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Sort start times

```cpp
sort(start.begin(), start.end());
```

Process meetings in chronological order of arrival.

---

### Sort end times

```cpp
sort(end.begin(), end.end());
```

Always know which meeting finishes first.

---

### Two pointers

```cpp
i
```

Next meeting beginning.

```cpp
j
```

Earliest meeting ending.

---

### Core comparison

```cpp
if(start[i] < end[j])
```

The earliest room is still occupied.

Need another room.

---

```cpp
else
```

A meeting has already finished.

Reuse that room.

---

### Current rooms

```cpp
currentRooms++;
```

Another room becomes occupied.

---

```cpp
currentRooms--;
```

A room becomes free.

---

### Maximum rooms

```cpp
maxRooms = max(maxRooms, currentRooms);
```

Track the peak number of simultaneous meetings.

That peak is exactly the minimum number of rooms required.

---

# EASY-TO-REMEMBER INTERVIEW SUMMARY

- Think of meetings as **start** and **end** events on a timeline.
- Sort both arrays independently.
- Use two pointers to process events chronologically.
- If the next meeting starts **before** the earliest one ends, allocate a new room.
- Otherwise, reuse the room that just became free.
- The maximum number of rooms occupied at any time is the answer.
- This is the same pattern as **Minimum Platforms**.
- The **only difference** is the comparison operator:
  - **Minimum Platforms:** `arrival <= departure`
  - **Meeting Rooms II:** `start < end`
````
