
# Minimum Jumps — Greedy Interview Notes

---

# PROBLEM:

You are given an array `arr[]` where `arr[i]` represents the maximum number of steps you can jump forward from index `i`.

Find the **minimum number of jumps** required to reach the last index.

Return `-1` if it is impossible to reach the last index.

---

# PATTERN:

**Greedy + Range Expansion (Implicit BFS / Level-by-Level Traversal)**

Also known as:

- Greedy Boundary Expansion
- Implicit BFS on an Array
- Greedy Interval Expansion

---

# WHY THIS PATTERN:

Normally, when we hear "minimum jumps", we think:

> "Which index should I jump to next?"

But that is **not** the correct way to think about this problem.

One jump does **not** give us one destination.

It gives us an entire **reachable range**.

Example:

```text
arr = [2,3,1,1,4]

From index 0

You can reach:

1
2
```

Instead of immediately choosing between index `1` and `2`, we first explore **both** because both are reachable using the same number of jumps.

This is exactly how **Breadth First Search (BFS)** works.

Each jump represents one BFS level.

Instead of using a queue, we simply maintain the boundary of the current level.

---

# CORE IDEA:

Don't ask

> "Where should I jump?"

Instead ask

> "Using one more jump, how far can my current reachable range extend?"

Once the entire current range has been explored, we increase the jump count and expand the boundary.

---

# GREEDY OBSERVATION:

Suppose our current jump allows us to reach

```text
Indices

2 3 4
```

Instead of immediately jumping to index 2,

we inspect

```text
2

3

4
```

For every index, compute

```cpp
i + arr[i]
```

which tells us how far that position can reach.

The best future boundary is simply

```cpp
max(i + arr[i])
```

among all indices inside the current range.

Only after exploring the entire range do we commit to another jump.

---

# WHY GREEDY WORKS:

The important observation is:

Every index inside the current reachable range requires the **same number of jumps** to reach.

So before taking another jump, we should inspect **all** of them.

Among those indices,

the one that reaches the farthest into the future gives us the largest possible reachable boundary.

A larger boundary can never reduce our future choices.

Instead, it only gives us more possibilities.

Therefore,

the local optimum

> "Expand to the farthest possible boundary"

always leads to the global optimum

> "Minimum number of jumps."

---

# GREEDY CHOICE:

While scanning the current reachable range,

always maintain

```cpp
farthest = max(farthest, i + arr[i]);
```

When the current range ends,

increase the jump count and expand the boundary

```cpp
currentEnd = farthest;
```

---

# WHY THIS CHOICE IS SAFE:

Suppose after scanning the current range we have two possible future boundaries

```text
Boundary A -> index 10

Boundary B -> index 15
```

If we choose boundary B,

everything reachable from boundary A is **also reachable** from boundary B.

The larger boundary never removes any future options.

It only adds more.

Therefore expanding to the farthest possible boundary is always safe.

This is called the **Greedy Choice Property**.

---

# SORTING:

Sorting is **NOT required.**

Reason:

The order of the array represents valid jump positions.

Sorting changes the entire meaning of the problem because indices change.

We must process indices exactly in their original order.

---

# INVARIANT:

At every iteration,

### currentEnd

stores

> The farthest index reachable using exactly `jumps` jumps.

### farthest

stores

> The farthest index reachable using one additional jump from any index inside the current range.

This property remains true throughout the algorithm.

---

# BRUTE FORCE:

## Should we discuss brute force?

**Yes.**

Interviewers usually expect progression because the Greedy solution is not immediately obvious.

Showing the transition demonstrates good problem-solving ability.

---

## Intuition

At every index,

try every possible jump.

Return the minimum answer among all possibilities.

---

## Recursive State

```cpp
f(index, jumps)
```

Meaning

> Starting from `index`, after taking `jumps` jumps so far, what is the minimum total number of jumps required to reach the last index?

---

## Base Cases

### Reached destination

```cpp
if(index >= n-1)
    return jumps;
```

Already reached or crossed the last index.

Return total jumps taken.

---

### Cannot move

```cpp
if(arr[index] == 0)
    return INT_MAX;
```

This path is impossible.

---

## Recursive Choice

```cpp
for(int jump = 1; jump <= arr[index]; jump++)
{
    ans = min(ans,
              solve(index + jump, jumps + 1));
}
```

Try every possible jump.

Return the minimum answer.

---

## Brute Force Code

```cpp
class Solution {
public:

    int solve(int index, int jumps, vector<int>& arr)
    {
        int n = arr.size();

        if(index >= n-1)
            return jumps;

        if(arr[index] == 0)
            return INT_MAX;

        int ans = INT_MAX;

        for(int jump = 1; jump <= arr[index]; jump++)
        {
            int next = solve(index + jump,
                             jumps + 1,
                             arr);

            if(next != INT_MAX)
                ans = min(ans, next);
        }

        return ans;
    }

    int minJumps(vector<int>& arr)
    {
        int ans = solve(0,0,arr);

        return ans==INT_MAX ? -1 : ans;
    }
};
```

---

## Why Brute Force is Slow

The recursion explores every possible jump.

Example

```text
                f(0)

          /      |       \

      f(1)     f(2)     f(3)

        |         |

      f(2)      f(4)

        |

      f(3)
```

Notice

```text
f(2)

f(3)
```

are solved multiple times.

This repeated work causes exponential time.

Memoization removes repeated work but still takes **O(N²)** in the worst case, which is too slow for `N = 10⁵`.

---

# TRANSITION TO GREEDY

Instead of deciding

> "Should I jump to index 2 or index 3?"

ask

> "How far can every index in my current reachable range take me?"

Since every index in the current range costs the same number of jumps,

we should first inspect all of them,

find the farthest future reach,

and only then take another jump.

This leads directly to the Greedy solution.

---

# OPTIMAL APPROACH:

Maintain three variables.

### jumps

Number of jumps already taken.

---

### currentEnd

End of the current reachable range.

Meaning:

> Using the current number of jumps, we can reach only up to this index.

---

### farthest

While scanning the current range,

this stores the farthest index reachable using one additional jump.

---

Whenever the current range finishes,

take one jump

and expand the boundary.

---

# ALGORITHM:

1. If array size is 1

Return 0.

Already at destination.

---

2. If first element is 0

Return -1.

Cannot move anywhere.

---

3. Initialize

```cpp
jumps = 0

currentEnd = 0

farthest = 0
```

---

4. Traverse from index 0 to n-2.

---

5. Update

```cpp
farthest = max(farthest,
               i + arr[i]);
```

Keep extending the farthest reachable boundary.

---

6. If

```cpp
i == currentEnd
```

Current reachable range is fully explored.

Take one jump.

```cpp
jumps++;

currentEnd = farthest;
```

---

7. If

```cpp
currentEnd == i
```

Boundary did not move.

We are stuck.

Return -1.

---

8. If

```cpp
currentEnd >= n-1
```

Destination is already inside the reachable range.

Return jumps.

---

# DRY RUN:

Example

```text
arr = [2,3,1,1,4]
```

Initially

```text
jumps = 0

currentEnd = 0

farthest = 0
```

---

### i = 0

```text
Reach = 0 + 2 = 2

farthest = 2
```

Boundary finished.

Take one jump.

```text
jumps = 1

currentEnd = 2
```

Current reachable range

```text
1 2
```

---

### i = 1

```text
Reach = 1 + 3 = 4

farthest = 4
```

---

### i = 2

```text
Reach = 2 + 1 = 3

farthest = 4
```

Boundary finished.

Take another jump.

```text
jumps = 2

currentEnd = 4
```

Since

```text
currentEnd >= last index
```

Return

```text
2
```

---

# COMMON MISTAKES:

- Forgetting the case `n == 1`.
- Forgetting `arr[0] == 0`.
- Incrementing jumps at every index.
- Updating `currentEnd` before finishing the current range.
- Confusing `currentEnd` and `farthest`.
- Iterating until `n` instead of `n-1`.
- Forgetting to detect when movement becomes impossible.

---

# INTERVIEW FLOW:

### Step 1 — Explain the Brute Force

Say:

> "At every index, I can jump to multiple positions. The brute force solution recursively tries every possible jump and returns the minimum number of jumps."

---

### Step 2 — Explain the Drawback

Say:

> "The same indices are solved repeatedly, leading to exponential time. Memoization reduces repeated work but still takes O(N²), which is too slow for N = 10⁵."

---

### Step 3 — State the Key Observation

Say:

> "I don't actually need to decide the exact landing position immediately. One jump gives me an entire reachable range."

---

### Step 4 — Introduce the Greedy Insight

Say:

> "Every index inside the current reachable range requires the same number of jumps to reach. So before spending another jump, I should inspect every index in the current range and find which one extends my future reach the most."

---

### Step 5 — Explain the Variables

Say:

- `currentEnd` → End of the current reachable range.
- `farthest` → Best future boundary discovered so far.
- `jumps` → Number of jumps already taken.

---

### Step 6 — Explain the Greedy Decision

Say:

> "Whenever I finish scanning the current range, I have complete information about the best next range. I then take exactly one jump and expand my boundary to the farthest reachable position."

---

### Step 7 — Explain Why It Works

Say:

> "Choosing the farthest boundary can never hurt because it includes every smaller boundary and possibly more positions. Therefore, maximizing the boundary locally also minimizes the total number of jumps globally."

---

### Step 8 — Complexity

Say:

> "Each index is processed exactly once, so the algorithm runs in O(N) time using only three integer variables, giving O(1) extra space."

---

# TIME COMPLEXITY:

## Brute Force

### Time: Exponential (approximately O(k^N) in the worst case)

Reason:

- Every recursive call can branch into multiple recursive calls.
- Many subproblems are solved repeatedly.
- The recursion tree grows exponentially.

---

## Memoization / DP

### Time: O(N²)

Reason:

There are `N` states (one for each index).

For each state, we may try up to `arr[i]` jumps.

In the worst case,

```text
arr[i] = O(N)
```

So,

```text
N states × N transitions

= O(N²)
```

---

## Greedy

### Time: O(N)

Reason:

The loop runs from index `0` to `n-2`.

Every index is visited exactly once.

Each iteration performs only constant-time operations:

- one `max()`
- one comparison
- a few assignments

Therefore,

```text
Total Time = O(N)
```

---

# SPACE COMPLEXITY:

## Brute Force

### O(N)

Reason:

The recursion depth can become `N` in the worst case.

No additional data structures are used.

---

## Memoization

### O(N)

Reason:

One DP array of size `N` is maintained.

The recursion stack can also take up to `O(N)` space.

---

## Greedy

### O(1)

Reason:

Only three variables are maintained throughout the algorithm:

```cpp
jumps

currentEnd

farthest
```

Their size does not depend on `N`.

No recursion.

No queue.

No stack.

No extra array.

Hence,

```text
Space = O(1)
```

---

# EDGE CASES:

### Single element

```text
[0]
```

Already at destination.

Answer = 0

---

### Cannot move from first index

```text
[0,5]
```

Answer = -1

---

### Direct jump

```text
[5,0,0,0]
```

Answer = 1

---

### Stuck in the middle

```text
[3,2,1,0,4]
```

Boundary never expands.

Answer = -1

---

### Reach destination exactly

```text
[2,3,1,1,4]
```

Answer = 2

---

# PATTERN RECOGNITION:

This Greedy pattern usually appears when:

- We need the **minimum number of actions**.
- One action creates a **range of possibilities** instead of a single choice.
- Every state in the current range has the same cost.
- We can postpone the decision until we've explored the current range.
- The goal is to expand the reachable boundary as much as possible.

Think:

> **"Am I processing one reachable range at a time and expanding to the farthest possible boundary?"**

If yes,

this is the **Greedy Range Expansion (Implicit BFS)** pattern.

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int minJumps(vector<int>& arr) {

        int n = arr.size();

        // Already at the destination
        if (n == 1)
            return 0;

        // Cannot make the first move
        if (arr[0] == 0)
            return -1;

        int jumps = 0;       // Number of jumps taken
        int currentEnd = 0;  // End of current reachable range
        int farthest = 0;    // Farthest index reachable using one more jump

        // No need to process the last index
        for (int i = 0; i < n - 1; i++) {

            // Update the farthest index reachable from the current range
            farthest = max(farthest, i + arr[i]);

            // Finished exploring the current range
            if (i == currentEnd) {

                // Take one jump
                jumps++;

                // Expand to the next reachable range
                currentEnd = farthest;

                // Cannot move any further
                if (currentEnd == i)
                    return -1;

                // Destination is already reachable
                if (currentEnd >= n - 1)
                    return jumps;
            }
        }

        return -1;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### `farthest = max(farthest, i + arr[i]);`

Among every index in the current reachable range, keep track of the one that reaches the farthest.

---

### `if (i == currentEnd)`

This means we have finished exploring the current jump's reachable range.

Now we must take another jump.

---

### `jumps++;`

We are moving to the next BFS level (next jump).

---

### `currentEnd = farthest;`

Expand the reachable boundary to the best possible future boundary discovered while scanning the current range.

---

### `if (currentEnd == i)`

Even after taking a jump, the boundary did not move.

No further progress is possible.

Return `-1`.

---

### `if (currentEnd >= n - 1)`

The last index now lies inside the reachable boundary.

No need to continue scanning.

Return the answer immediately.

---

# EASY INTERVIEW SUMMARY

> **Brute Force:** At every index, try every possible jump recursively and return the minimum answer. This is exponential because many states are recomputed.

> **Greedy Insight:** Don't decide the landing position immediately. Each jump gives an entire reachable range. Scan every index in the current range, compute the farthest index that can be reached with one more jump, and only then spend a jump to expand the boundary.

> **Why It Works:** Every index in the current range costs the same number of jumps to reach. Expanding to the farthest boundary can never reduce future options, so the local greedy choice also produces the global optimum.

> **Complexity:** **O(N)** time because every index is visited once, and **O(1)** space because only three variables (`jumps`, `currentEnd`, and `farthest`) are maintained.
````
