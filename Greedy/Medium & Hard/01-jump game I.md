

## PROBLEM:

You are given an array `arr` where `arr[i]` represents the maximum jump length from index `i`.

Return **true** if you can reach the last index starting from index `0`, otherwise return **false**.

---

# PATTERN:

**Greedy - Farthest Reach (Greedy Reachability)**

This is one of the most common Greedy patterns where we continuously maintain the **farthest index we can reach**.

---

# WHY THIS PATTERN:

At every position, we don't care **how** we reached it.

We only care about one thing:

> **What is the farthest position I can reach so far?**

If the current index is reachable, then it can potentially extend our reachable range.

So instead of exploring every possible jump (like DFS/BFS), we greedily keep updating the maximum reachable index.

---

# CORE IDEA:

Imagine your reachable area as an interval.

Initially,

```
Reachable = [0]
```

Whenever you stand at a reachable index `i`, your jump extends the interval to

```
i + arr[i]
```

Keep expanding this reachable interval.

If at some point the current index lies **outside** the reachable interval,

then no future index can ever be reached.

---

# GREEDY OBSERVATION:

If index `i` is reachable,

then every jump from `i` only helps by increasing the farthest reachable position.

So the only information worth keeping is

```
farthestReach = max(farthestReach, i + arr[i])
```

If we ever reach

```
farthestReach >= n-1
```

we are guaranteed to reach the last index.

---

# WHY GREEDY WORKS:

Greedy works because reaching an index earlier never reduces future possibilities.

Instead, it only increases (or keeps) the maximum reachable distance.

Once an index becomes unreachable,

every index after it is also unreachable.

So there is no reason to consider multiple paths.

Maintaining only the farthest reachable index is sufficient.

---

# GREEDY CHOICE:

Whenever the current index is reachable,

update

```
farthestReach = max(farthestReach, i + arr[i])
```

---

# WHY THIS CHOICE IS SAFE:

Suppose multiple ways exist to reach index `i`.

It doesn't matter which path was used.

Once we are standing at `i`, the only useful information is

```
How far can I now go?
```

Keeping the maximum reachable position can never hurt.

It always preserves every future possibility.

---

# SORTING:

**Not Required**

Reason:

The position of every element matters.

Sorting would destroy the jump relationships.

---

# INVARIANT:

After processing index `i`,

```
farthestReach
```

always stores

> **the farthest index reachable using any sequence of jumps from indices [0...i].**

This invariant remains true throughout the algorithm.

---

# BRUTE FORCE:

### Is brute force necessary?

**No.**

In interviews, brute force is usually **not necessary** for this problem.

Reason:

The greedy solution is very natural once you observe that only the farthest reachable index matters.

A recursive DFS would try every possible jump,

leading to exponential complexity.

Since there is no meaningful optimization transition needed,

interviewers usually expect the greedy solution directly.

---

# OPTIMAL APPROACH:

Maintain one variable:

```
farthestReach
```

Traverse the array.

For every index:

- If current index is beyond `farthestReach`,
  we cannot even stand here.

  Return false.

Otherwise,

update

```
farthestReach = max(farthestReach, i + arr[i])
```

If at any point

```
farthestReach >= lastIndex
```

return true.

---

# ALGORITHM:

1. Initialize

```
farthestReach = 0
```

2. Traverse array.

3. If

```
i > farthestReach
```

return false.

4. Update

```
farthestReach = max(farthestReach, i + arr[i])
```

5. If

```
farthestReach >= n-1
```

return true.

6. Finish traversal.

7. Return true.

---

# DRY RUN:

### Example 1

```
arr = [1,2,0,3,0,0]
```

| Index | Jump | Farthest Reach |
|--------|------|----------------|
|0|1|max(0,1)=1|
|1|2|max(1,3)=3|
|2|0|max(3,2)=3|
|3|3|max(3,6)=6|

Now

```
6 >= last index (5)
```

Answer = True

---

### Example 2

```
arr = [1,0,2]
```

Initially

```
farthest = 0
```

Index 0

```
farthest = 1
```

Index 1

```
jump = 0

farthest remains 1
```

Index 2

```
2 > farthest (1)
```

Cannot even reach index 2.

Answer = False.

---

# COMMON MISTAKES:

### Mistake 1

Using

```
if(i >= farthestReach)
```

Wrong.

Correct:

```
if(i > farthestReach)
```

Because

```
i == farthestReach
```

means the current index is still reachable.

---

### Mistake 2

Thinking we must always jump the maximum distance.

We never actually simulate jumps.

We only track the maximum reachable position.

---

### Mistake 3

Stopping only after finishing traversal.

As soon as

```
farthestReach >= last index
```

we can immediately return true.

---

### Mistake 4

Confusing this with **Jump Game II**.

This problem only asks

```
Can we reach?
```

Not

```
Minimum jumps?
```

---

# INTERVIEW FLOW:

> We only need to know how far we've been able to reach so far.
>
> If the current index is beyond that distance, it means this position is unreachable, so the answer is false.
>
> Otherwise, since this index is reachable, we extend our reachable range using
>
> ```
> i + arr[i]
> ```
>
> We keep updating the farthest reachable position.
>
> If we can eventually reach or cross the last index, the answer is true.

---

# TIME COMPLEXITY:

### O(n)

Reason:

Each index is processed exactly once.

Each iteration performs only constant-time operations.

---

# SPACE COMPLEXITY:

### O(1)

Reason:

Only one variable

```
farthestReach
```

is maintained.

No extra data structures are used.

---

# EDGE CASES:

### 1. Single element

```
[0]
```

Already at destination.

Answer:

```
true
```

---

### 2. Starting element is 0

```
[0,2,3]
```

Cannot move anywhere.

Answer:

```
false
```

---

### 3. Zero in the middle but can be crossed

```
[2,0,1]
```

Jump directly over the zero.

Answer:

```
true
```

---

### 4. Zero blocks the path

```
[1,0,2]
```

Reach index 1.

Cannot move further.

Answer:

```
false
```

---

### 5. Multiple consecutive zeros

```
[3,0,0,0,2]
```

If first jump reaches beyond them,

Answer:

```
true
```

Otherwise,

```
false
```

---

### 6. Large jump at beginning

```
[10,0,0,0]
```

Immediately reaches end.

Answer:

```
true
```

---

### 7. Every element is 1

```
[1,1,1,1]
```

Simply move one step each time.

Answer:

```
true
```

---

### 8. Last element is 0

```
[2,3,1,1,0]
```

Perfectly fine.

We only need to **reach** it, not move after reaching it.

Answer:

```
true
```

---

# PATTERN RECOGNITION:

Look for this Greedy pattern whenever:

- Need to determine **whether a destination is reachable**.
- Need to maintain the **maximum reachable boundary**.
- Choices only expand future possibilities.
- The path itself is irrelevant.
- Only the current reachable range matters.
- No backtracking is needed.

Typical problems:

- Jump Game
- Minimum Taps to Water Garden
- Video Stitching
- Jump Game II (variation)

---

# C++ Code

```cpp
class Solution {
public:
    bool canReach(vector<int>& arr) {
        int farthestReach = 0;
        int n = arr.size();

        for (int i = 0; i < n; i++) {

            // Current index is unreachable
            if (i > farthestReach)
                return false;

            // Extend the farthest reachable position
            farthestReach = max(farthestReach, i + arr[i]);

            // Already able to reach the last index
            if (farthestReach >= n - 1)
                return true;
        }

        return true;
    }
};
```

---

# Line-by-Line Intuition

### `int farthestReach = 0;`

Initially, we can only stand at index `0`.

---

### `if (i > farthestReach)`

If the current index itself cannot be reached,

then no future index can be reached either.

Immediately return `false`.

---

### `farthestReach = max(farthestReach, i + arr[i]);`

Standing at index `i`, we can jump up to `i + arr[i]`.

Update the maximum reachable position seen so far.

---

### `if (farthestReach >= n - 1)`

The moment we can reach (or cross) the last index,

there is no need to continue.

Return `true`.

---

### `return true;`

If the loop completes without encountering an unreachable index,

then the last index is reachable.

---

# Tricky Condition Explained

### Why `i > farthestReach` and NOT `i >= farthestReach`?

Suppose

```
arr = [1,0]
```

Initially

```
farthestReach = 1
```

At

```
i = 1
```

```
i == farthestReach
```

Index 1 is still reachable.

If we used

```
>=
```

we would incorrectly return false.

Hence the correct condition is:

```cpp
if (i > farthestReach)
```

---

# Easy Interview Summary

- Track only the **farthest reachable index**.
- If the current index is beyond that range, return **false**.
- Otherwise, extend the reachable range using `i + arr[i]`.
- The first time the reachable range reaches or crosses the last index, return **true**.
- **Time:** `O(n)`
- **Space:** `O(1)`
- **Pattern:** Greedy - Farthest Reach / Reachability
````
