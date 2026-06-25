
# PROBLEM:

Given the arrival and departure times of trains, find the **minimum number of platforms** required so that **no train has to wait**.

A platform becomes free **only after a train departs**.

If a train arrives **at or before** another train departs (`arrival <= departure`), they **cannot** use the same platform.

---

# PATTERN:

**Greedy + Sorting + Two Pointers (Sweep Line Algorithm / Count Active Intervals)**

---

# WHY THIS PATTERN:

This problem is **not about selecting intervals**.

Instead, it asks:

> **How many trains are present at the station simultaneously?**

Every arrival increases the number of occupied platforms.

Every departure decreases the number of occupied platforms.

Rather than checking overlaps between every pair of trains, we process events in chronological order.

Sorting allows us to process these events efficiently.

---

# CORE IDEA:

Think of each train event as:

- Arrival → Need one more platform (`+1`)
- Departure → One platform becomes free (`-1`)

While moving through time, keep track of:

```
Current Platforms = Number of trains currently at the station
```

The maximum value reached is the answer.

---

# GREEDY OBSERVATION:

At every step, only one question matters:

> Which happens first?

- Next arrival
- Next departure

If the next event is an arrival:

- Allocate one more platform.

If the next event is a departure:

- Free one platform.

Past events never need to be reconsidered.

---

# WHY GREEDY WORKS:

After sorting, events are processed in chronological order.

Every event changes only one thing:

```
Number of occupied platforms
```

Future events cannot change how many platforms were needed at an earlier time.

Therefore, keeping track of the maximum occupied platforms automatically gives the optimal answer.

---

# GREEDY CHOICE:

Always compare:

```
Next Arrival
vs
Next Departure
```

If

```
arrival <= departure
```

Process the arrival first.

Otherwise,

Process the departure first.

---

# WHY THIS CHOICE IS SAFE:

Suppose

```
Arrival = 1000
Departure = 1000
```

The train departing at 1000 is still occupying the platform.

Therefore, the arriving train cannot use it immediately.

Hence,

```
arrival <= departure
```

must require another platform.

Using

```
<
```

instead of

```
<=
```

will produce incorrect answers.

This is the most common interview mistake.

---

# SORTING:

### Is sorting required?

**Yes.**

Without sorting, events are not processed in chronological order.

### What do we sort?

- Sort all arrival times.
- Sort all departure times.

Notice that we **do not** keep arrival and departure pairs together.

We only care about the chronological order of events.

---

# INVARIANT:

At every iteration:

```
currentPlatforms
```

always represents

> Number of trains currently occupying platforms.

and

```
maxPlatforms
```

always stores

> Maximum platforms required so far.

This property remains true throughout the algorithm.

---

# BRUTE FORCE:

## Is brute force necessary?

**Yes, but only to explain the optimization.**

Interviewers generally expect the optimal solution.

However, showing brute force demonstrates how we derive the greedy approach.

---

## Idea

For every train,

count how many other trains overlap with it.

The maximum overlap is the answer.

### Overlap Condition

Two trains overlap if

```
arr[i] <= dep[j]
AND
arr[j] <= dep[i]
```

---

## Brute Force Code

```cpp
class Solution {
public:
    int minPlatform(vector<int>& arr, vector<int>& dep) {

        int n = arr.size();
        int answer = 1;

        for(int i = 0; i < n; i++) {

            int platforms = 1;

            for(int j = i + 1; j < n; j++) {

                if(arr[i] <= dep[j] && arr[j] <= dep[i])
                    platforms++;
            }

            answer = max(answer, platforms);
        }

        return answer;
    }
};
```

---

## Dry Run (Brute Force)

```
Arrival     = [900, 940, 950]
Departure   = [910,1200,1120]
```

Train 1 overlaps with none

Platforms = 1

Train 2 overlaps with Train 3

Platforms = 2

Train 3 overlaps with Train 2

Platforms = 2

Answer = 2

---

### Why is brute force bad?

For every train, we compare it with every other train.

```
Time Complexity = O(n²)
```

For

```
n = 100000
```

it becomes

```
10^10 comparisons
```

which is too slow.

---

# OPTIMAL APPROACH:

Instead of comparing every pair of trains,

observe that

```
Arrival   → +1 platform

Departure → -1 platform
```

Sort arrivals.

Sort departures.

Traverse both arrays simultaneously.

Track the number of occupied platforms.

The maximum occupied platforms is the answer.

---

# ALGORITHM:

1. Sort arrival times.
2. Sort departure times.
3. Initialize two pointers:
   - i → arrivals
   - j → departures
4. Initialize:
   - currentPlatforms = 0
   - maxPlatforms = 0
5. While both pointers are valid:
   - If arrival <= departure
     - currentPlatforms++
     - update answer
     - i++
   - Else
     - currentPlatforms--
     - j++
6. Return maxPlatforms.

---

# DRY RUN:

```
Arrival

900
940
950
1100
1500
1800

Departure

910
1120
1130
1200
1900
2000
```

| Arrival | Departure | Action | Current Platforms | Maximum |
|----------|-----------|--------|------------------|---------|
|900|910|Arrival|1|1|
|940|910|Departure|0|1|
|940|1120|Arrival|1|1|
|950|1120|Arrival|2|2|
|1100|1120|Arrival|3|3|
|1500|1120|Departure|2|3|
|1500|1130|Departure|1|3|
|1500|1200|Departure|0|3|
|1500|1900|Arrival|1|3|
|1800|1900|Arrival|2|3|

Final Answer

```
3
```

---

# COMMON MISTAKES:

### 1. Using `<` instead of `<=`

Wrong

```cpp
if(arr[i] < dep[j])
```

Correct

```cpp
if(arr[i] <= dep[j])
```

Because arrivals at the exact departure time still require another platform.

---

### 2. Forgetting to sort

Without sorting,

events are processed incorrectly.

---

### 3. Pairing arrival and departure together

Not required.

Only chronological event order matters.

---

### 4. Updating answer after departures

Update the answer only after increasing the number of occupied platforms.

---

### 5. Thinking this is Interval Scheduling

It is not.

This problem counts overlaps.

No train is removed.

---

# INTERVIEW FLOW:

> Brute Force

A straightforward solution is to compare every train with every other train and count overlapping intervals.

This takes O(n²), which is too slow for n = 100000.

---

> Key Observation

Every arrival increases the number of occupied platforms.

Every departure decreases the number of occupied platforms.

Instead of comparing intervals, we only need to process these events in chronological order.

---

> Greedy Idea

Sort arrivals separately.

Sort departures separately.

Maintain two pointers.

Compare the next arrival with the next departure.

- If the next event is an arrival, allocate a platform.
- Otherwise, free a platform.

Track the maximum number of occupied platforms.

That maximum is the answer.

---

> Why Greedy Works

After sorting, events are processed in time order.

At every step,

```
currentPlatforms
```

is exactly the number of trains currently at the station.

The maximum value reached is therefore the minimum number of platforms required.

---

# TIME COMPLEXITY:

Sorting arrivals

```
O(n log n)
```

Sorting departures

```
O(n log n)
```

Two-pointer traversal

```
O(n)
```

Overall

```
O(n log n)
```

---

# SPACE COMPLEXITY:

Ignoring sorting implementation,

```
O(1)
```

If sorting uses recursion,

```
O(log n)
```

stack space may be required.

---

# EDGE CASES:

### Single train

```
Answer = 1
```

---

### No overlapping trains

Only one platform is needed.

---

### All trains overlap

Answer = n

---

### Arrival equals departure

```
arrival <= departure
```

requires another platform.

---

### Already sorted input

Algorithm works correctly.

---

### Unsorted input

Sorting handles it automatically.

---

# PATTERN RECOGNITION:

Think of this pattern whenever you see:

- Maximum overlapping intervals
- Minimum number of rooms/platforms/resources
- Meeting Rooms II
- Airplanes in the Sky
- Conference Room Allocation
- Arrival-Departure style problems
- Count active intervals

The giveaway is:

> **Find the maximum number of intervals active at the same time.**

Whenever you need the maximum simultaneous intervals,

think:

```
Sort Events

Arrival → +1

Departure → -1

Two Pointers

Track Maximum Active Intervals
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int minPlatform(vector<int>& arr, vector<int>& dep) {

        int n = arr.size();

        // Sort arrival and departure times
        sort(arr.begin(), arr.end());
        sort(dep.begin(), dep.end());

        int i = 0;                  // Arrival pointer
        int j = 0;                  // Departure pointer

        int currentPlatforms = 0;
        int maxPlatforms = 0;

        while(i < n && j < n) {

            // Next event is an arrival
            if(arr[i] <= dep[j]) {

                currentPlatforms++;

                maxPlatforms = max(maxPlatforms, currentPlatforms);

                i++;
            }

            // Next event is a departure
            else {

                currentPlatforms--;

                j++;
            }
        }

        return maxPlatforms;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Sort arrivals

```cpp
sort(arr.begin(), arr.end());
```

Processes arrivals in chronological order.

---

### Sort departures

```cpp
sort(dep.begin(), dep.end());
```

Processes departures in chronological order.

---

### Two pointers

```cpp
int i = 0;
int j = 0;
```

- i tracks the next arrival.
- j tracks the next departure.

---

### Compare arrival and departure

```cpp
if(arr[i] <= dep[j])
```

If the next event is an arrival,

we need another platform.

---

### Increase occupied platforms

```cpp
currentPlatforms++;
```

One more train is currently at the station.

---

### Update answer

```cpp
maxPlatforms = max(maxPlatforms, currentPlatforms);
```

Stores the maximum occupied platforms seen so far.

---

### Departure case

```cpp
currentPlatforms--;
```

A train leaves.

One platform becomes free.

---

### Move the correct pointer

```cpp
i++;
```

or

```cpp
j++;
```

Advance only the event that was processed.

---

# WHY THE TRICKY CONDITION IS `<=`

Suppose

```
Arrival = 1000

Departure = 1000
```

At time 1000,

the departing train is still occupying the platform.

Therefore,

the arriving train cannot use it immediately.

Hence,

```cpp
if(arr[i] <= dep[j])
```

is correct.

Using

```cpp
<
```

would undercount the required platforms.

---

# EASY-TO-REMEMBER INTERVIEW SUMMARY

**Pattern**

Greedy + Sorting + Two Pointers (Sweep Line)

**Core Idea**

Treat arrivals as `+1` and departures as `-1`.

**Greedy Choice**

Always process whichever event happens first.

**Invariant**

`currentPlatforms` always equals the number of trains currently occupying platforms.

**Why Greedy Works**

Processing events in chronological order accurately tracks the number of active trains at every moment.

The maximum active trains equals the minimum platforms required.

**Sorting**

Required to process events chronologically.

**Complexity**

- Time: **O(n log n)**
- Space: **O(1)** (excluding sorting stack)
````
