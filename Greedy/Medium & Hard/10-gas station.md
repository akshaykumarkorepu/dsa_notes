# Gas Station — Greedy Interview Notes

---

## PROBLEM

You are given:

- `gas[i]` → Amount of fuel available at station `i`.
- `cost[i]` → Fuel required to travel from station `i` to `(i + 1)`.

The stations form a **circular route**.

Rules:

- Start with an **empty tank**.
- Choose any station as the starting station.
- The tank should never become negative.
- Return the starting station index if one complete circular tour is possible.
- Otherwise, return `-1`.

**Important:** If a solution exists, it is guaranteed to be unique.

---

# PATTERN

**Greedy Elimination + Running Balance (Prefix Sum Greedy)**

Maintain a running fuel balance while traversing the stations once.

Whenever the balance becomes negative, eliminate the entire failed segment and start from the next station.

---

# WHY THIS PATTERN

This problem has two important properties:

1. The order of stations is fixed because they form a circular route.
2. A failure gives useful information:
   - If one starting station fails, multiple starting stations become impossible at once.

Instead of testing every station independently, Greedy eliminates an entire range of candidates.

---

# CORE IDEA

Instead of thinking separately about

```text
Collect Gas
↓

Spend Cost
```

Think in terms of **Net Gain**.

```cpp
gain = gas[i] - cost[i];
```

- Positive gain → Fuel increases.
- Negative gain → Fuel decreases.

Now the problem becomes:

> Find a starting station such that the running sum of gains never becomes negative.

---

# GREEDY OBSERVATION

Suppose the current candidate is `start`.

While traveling, the tank becomes negative for the first time at station `i`.

Then:

- Starting from `start` fails.
- Every station between `start` and `i` also fails.

Therefore, we can safely skip all those stations and directly try `i + 1`.

This avoids repeated work.

---

# WHY GREEDY WORKS

## Greedy Choice Property

If starting from station `S` causes the tank to become negative for the first time at station `F`, then every station between `S` and `F` is also an invalid starting point.

### Why?

Suppose starting from `S` reaches `F` with negative fuel.

Now imagine starting from some station `K` where

```text
S < K ≤ F
```

Starting from `K` skips the gas collected between `S` and `K - 1`.

So it reaches `F` with even less fuel.

Hence it must also fail.

Therefore, eliminating the entire failed segment never removes the correct answer.

---

# GREEDY CHOICE

Whenever

```cpp
tank < 0
```

reset the journey:

```cpp
start = i + 1;
tank = 0;
```

This means:

- Discard every station from the previous `start` to `i`.
- Make the next station the new candidate.

---

# WHY THIS CHOICE IS SAFE

If the current candidate cannot reach station `i + 1`, then every station inside that failed segment will also fail before reaching there.

So skipping them never removes the optimal answer.

Every eliminated station has already been proven impossible.

---

# SORTING

**Sorting is NOT required.**

Reason:

- The stations form a fixed circular route.
- Their order cannot be changed.
- Sorting would completely change the problem.

Hence, we must process the stations in their original order.

---

# INVARIANT

After processing every station:

1. Every station before `start` has already been proven impossible.
2. `tank` represents the fuel assuming we started from `start`.
3. `tank` is always non-negative after any reset.
4. `start` is the only remaining valid candidate seen so far.

These properties remain true throughout the algorithm.

---

# BRUTE FORCE

## Intuition

The simplest idea is:

- Assume every station is the starting station.
- Simulate one complete circular journey.
- If the tank becomes negative at any point, that starting station fails.
- Otherwise, if all `n` stations are visited successfully, return that station.

Although simple, this repeatedly simulates the same paths.

---

## Algorithm

For every station `start`:

Initialize

```cpp
tank = 0;
```

Travel exactly `n` stations.

For each step:

```cpp
curr = (start + step) % n;
```

Collect fuel:

```cpp
tank += gas[curr];
```

Travel:

```cpp
tank -= cost[curr];
```

If

```cpp
tank < 0
```

Current starting station fails.

Break immediately.

If all stations are completed successfully,

return `start`.

If every station fails,

return

```text
-1
```

---

# CLEAN C++ CODE

## Brute Force Solution (O(n²))

```cpp
class Solution {
public:
    int startStation(vector<int> &gas, vector<int> &cost) {

        int n = gas.size();

        // Try every station as the starting point
        for (int start = 0; start < n; start++) {

            int tank = 0;
            bool possible = true;

            // Travel exactly n stations
            for (int step = 0; step < n; step++) {

                int curr = (start + step) % n;

                tank += gas[curr];
                tank -= cost[curr];

                // Journey fails
                if (tank < 0) {
                    possible = false;
                    break;
                }
            }

            // Successfully completed one full circle
            if (possible)
                return start;
        }

        // No valid starting station exists
        return -1;
    }
};
```

### Time Complexity

- We try every station as the starting point (`n` choices).
- For each starting point, we may travel all `n` stations.

**Time:** `O(n²)`

### Space Complexity

Only a few variables are used.

**Space:** `O(1)`

---

## Brute Force Dry Run

Example:

```text
gas  = [4,5,7,4]
cost = [6,6,3,5]
```

---

### Try Start = 0

Initial Tank = 0

| Station | Tank Before | +Gas | -Cost | Tank After |
|---------|------------:|-----:|------:|-----------:|
|0|0|+4|-6|-2 ❌|

Tank becomes negative.

Start = 0 fails.

---

### Try Start = 1

Initial Tank = 0

| Station | Tank Before | +Gas | -Cost | Tank After |
|---------|------------:|-----:|------:|-----------:|
|1|0|+5|-6|-1 ❌|

Tank becomes negative.

Start = 1 fails.

---

### Try Start = 2

Initial Tank = 0

| Station | Tank Before | +Gas | -Cost | Tank After |
|---------|------------:|-----:|------:|-----------:|
|2|0|+7|-3|4|
|3|4|+4|-5|3|
|0|3|+4|-6|1|
|1|1|+5|-6|0|

Tank never becomes negative.

Return:

```text
2
```

---

### Why is Brute Force Slow?

Notice:

After Start = 0 fails,

we again visit

```text
1 → 2 → 3
```

Then for Start = 1,

we again visit

```text
2 → 3
```

The same stations are simulated repeatedly.

This repeated work leads to **O(n²)** time complexity.

---

# OPTIMAL APPROACH

## Key Observation

Suppose we start from station `S`.

While traveling, the tank becomes negative for the first time at station `F`.

Can any station between `S` and `F` be the answer?

**No.**

Why?

Starting from `S`, we collected gas from every station before reaching `F`.

Even after collecting all that gas, we still failed.

Now suppose we start from some station between `S` and `F`.

We skip some of the gas collected earlier.

Therefore, we reach `F` with **even less fuel**.

Hence every station between `S` and `F` must also fail.

So instead of checking them one by one,

we eliminate the entire segment and directly try

```text
F + 1
```

---

## Variables Maintained

### 1. `start`

Current candidate starting station.

---

### 2. `tank`

Current fuel assuming we started from `start`.

```cpp
tank += gas[i] - cost[i];
```

Whenever

```cpp
tank < 0
```

the current candidate becomes impossible.

---

### 3. `totalBalance`

Stores

```cpp
Σ(gas[i] - cost[i])
```

This checks whether a solution exists.

If

```cpp
totalBalance < 0
```

then

```text
Total Gas < Total Cost
```

No solution is possible.

---

## Algorithm

Initialize

```cpp
start = 0;
tank = 0;
totalBalance = 0;
```

Traverse every station once.

For every station,

compute

```cpp
gain = gas[i] - cost[i];
```

Update

```cpp
tank += gain;
totalBalance += gain;
```

If

```cpp
tank < 0
```

then

```cpp
start = i + 1;
tank = 0;
```

After traversal,

If

```cpp
totalBalance < 0
```

return

```cpp
-1;
```

Otherwise,

return

```cpp
start;
```

---

## Optimal Dry Run

Example:

```text
gas  = [4,5,7,4]
cost = [6,6,3,5]
```

Gain:

```text
[-2,-1,+4,-1]
```

Initial values:

```text
start = 0
tank = 0
totalBalance = 0
```

| i | Gain | Tank | Total Balance | Action | Start |
|---|-----:|-----:|--------------:|--------|------:|
|0|-2|-2|-2|Tank < 0 → Reset|1|
|1|-1|-1|-3|Tank < 0 → Reset|2|
|2|+4|4|1|Continue|2|
|3|-1|3|0|Continue|2|

Traversal finishes.

Final values:

```text
start = 2
totalBalance = 0
```

Since

```text
totalBalance >= 0
```

Return

```text
2
```

---

## Why Don't We Simulate Again?

We already know:

- Every station before `start` has been eliminated.
- From `start` to the end, the tank never became negative.
- `totalBalance >= 0`, so the remaining fuel is enough to cover the skipped prefix.

Therefore, the final candidate is guaranteed to complete the entire circle.

No second simulation is required.

---

# COMMON MISTAKES

### 1. Forgetting `totalBalance`

Only checking `tank` is incorrect.

Always verify

```cpp
totalBalance >= 0
```

---

### 2. Writing

```cpp
start = i;
```

Incorrect.

Correct:

```cpp
start = i + 1;
```

---

### 3. Forgetting

```cpp
tank = 0;
```

A new journey always starts with an empty tank.

---

### 4. Restarting the traversal

Never restart.

Continue scanning because the failed segment has already been eliminated.

---

### 5. Thinking another simulation is needed

Not required.

The greedy proof guarantees the final candidate is correct.

---

# INTERVIEW FLOW

1. Explain the brute force simulation.
2. Mention its `O(n²)` complexity.
3. Show that many stations are visited repeatedly.
4. Ask:
   - Can one failure eliminate multiple candidates?
5. Derive the Greedy observation.
6. Reset whenever `tank < 0`.
7. Maintain:
   - `start`
   - `tank`
   - `totalBalance`
8. At the end:
   - `totalBalance < 0` → `-1`
   - Otherwise → `start`

---

# TIME COMPLEXITY

### Brute Force

- Try every starting station.
- Simulate all stations.

**Time:** `O(n²)`

---

### Optimal

Single traversal.

Every station is processed exactly once.

**Time:** `O(n)`

---

# SPACE COMPLEXITY

### Brute Force

`O(1)`

### Optimal

`O(1)`

Only three variables are maintained.

---

# EDGE CASES

### Single Station

```text
gas >= cost
```

Return `0`.

Otherwise return `-1`.

---

### Total Gas < Total Cost

Impossible.

Return `-1`.

---

### Tank Becomes Negative

Reset

```cpp
start = i + 1;
tank = 0;
```

---

### Tank Equals Zero

Continue.

Zero fuel is allowed.

---

### Last Station Causes Failure

`start` becomes `n`.

Since `totalBalance < 0`, return `-1`.

---

# PATTERN RECOGNITION

Think of this Greedy pattern when:

- You need to find a valid starting point.
- The order cannot be changed.
- The problem involves a circular traversal.
- A failure eliminates an entire range of candidates.
- You maintain a running balance (prefix sum).
- Revisiting failed candidates would be redundant.

This is the **Greedy Elimination Pattern**.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int startStation(vector<int> &gas, vector<int> &cost) {

        int start = 0;
        int tank = 0;
        int totalBalance = 0;

        for (int i = 0; i < gas.size(); i++) {

            int gain = gas[i] - cost[i];

            tank += gain;
            totalBalance += gain;

            // Current candidate cannot continue.
            // Eliminate the failed segment.
            if (tank < 0) {
                start = i + 1;
                tank = 0;
            }
        }

        if (totalBalance < 0)
            return -1;

        return start;
    }
};
```

---

# INTUITION BEHIND IMPORTANT LINES

### `gain = gas[i] - cost[i]`

Convert the problem into a running balance problem.

---

### `tank += gain`

Track the current fuel assuming we started from the current candidate.

---

### `totalBalance += gain`

Check whether completing the entire circuit is possible at all.

---

### `if (tank < 0)`

The current candidate has failed.

---

### `start = i + 1`

Skip the entire failed segment and choose the next station.

---

### `tank = 0`

A new journey always starts with an empty tank.

---

### `if (totalBalance < 0)`

If total gas is less than total cost, no solution exists.

---

# INTERVIEW SUMMARY

- Convert the problem into **Net Gain** (`gas[i] - cost[i]`).
- Maintain a running balance (`tank`) for the current candidate.
- If the tank becomes negative, eliminate the entire failed segment.
- Use `totalBalance` to check if a solution exists.
- If `totalBalance < 0`, return `-1`.
- Otherwise, the final `start` is guaranteed to be the unique answer.

## One-Line Memory Trick

> **Negative tank → Skip the failed segment. Negative total → No solution. Otherwise, the final candidate is the answer.**
