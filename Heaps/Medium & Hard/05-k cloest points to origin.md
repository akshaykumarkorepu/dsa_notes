
## PROBLEM:

Given `n` points on a 2D plane and an integer `k`, return the **k closest points to the origin (0,0)**.

Distance of a point `(x,y)` from origin is:

\[
\sqrt{x^2+y^2}
\]

Since square root is monotonic (it preserves order), we can simply compare:

\[
x^2+y^2
\]

instead of actually computing the square root.

---

# PATTERN:

**Top K Elements using a Fixed Size Heap**

---

# WHY THIS PATTERN:

The question is asking for:

- Not the closest point
- Not all points sorted

Instead, it asks for only the **K closest**.

Whenever you hear:

- Top K
- K largest
- K smallest
- K closest
- K most frequent

Immediately think:

> **Can I maintain only K useful elements instead of sorting everything?**

That is exactly what a **fixed-size heap** does.

---

# CORE IDEA:

Each point has a distance from the origin.

Instead of sorting all distances,

we only keep the **K closest points seen so far**.

To do this efficiently:

- Store the current K closest points inside a **Max Heap**.
- The largest distance among these K points stays on the top.
- Whenever a new point is closer than the farthest point currently stored,
  remove the farthest one and insert the new one.

At the end,

the heap contains exactly the K closest points.

---

# WHY A HEAP?

Suppose

```
k = 3
```

Current closest points:

```
2
5
7
```

The farthest among them is

```
7
```

Now a new point comes with distance

```
4
```

We should remove

```
7
```

and insert

```
4
```

To remove the largest quickly,

we use a **Max Heap**.

A Max Heap always gives the largest element in

```
O(1)
```

and insertion/deletion both take

```
O(log K)
```

which is much faster than sorting every time.

---

# MIN HEAP OR MAX HEAP?

We need to keep the **K smallest distances**.

So among those K distances,

the one that should be easiest to remove is

the **largest**.

Therefore,

we use a **Max Heap**.

Top of heap = Current farthest among the K closest.

---

# WHAT IS STORED IN THE HEAP?

Each heap element stores

```cpp
(distanceSquared, point)
```

Example:

```
(13, [2,3])
(5, [1,2])
(25,[3,4])
```

Distance is stored first because the priority queue compares the first element.

---

# WHY ARE ELEMENTS PUSHED?

Every point could potentially belong to the answer.

So every point is inserted once.

```cpp
pq.push({distance, point});
```

---

# WHY ARE ELEMENTS POPPED?

If heap size becomes

```
k+1
```

then one point must be removed.

Which one?

The farthest.

Since this is a Max Heap,

that point is on top.

```cpp
pq.pop();
```

---

# WHAT PROPERTY (INVARIANT) DOES THE HEAP MAINTAIN?

After processing every point,

the heap always satisfies:

- Heap size ≤ K
- Heap contains the K closest points seen so far.
- Top = farthest among those K points.

This property remains true after every iteration.

---

# WHY IS MAINTAINING ONLY K ELEMENTS SUFFICIENT?

Suppose

```
k = 3
```

Current heap

```
2
5
7
```

New distance

```
20
```

Obviously,

20 can never belong to the 3 closest.

So we discard it immediately.

Now suppose

```
New distance = 4
```

Then

```
2
4
5
```

becomes the new answer.

The old

```
7
```

gets removed.

We never need to store more than K useful elements.

---

# BRUTE FORCE:

## Idea

Calculate distance of every point.

Store all distances.

Sort them.

Take the first K.

---

## Code

```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {

        vector<pair<long long, vector<int>>> arr;

        for(auto &point : points){

            long long x = point[0];
            long long y = point[1];

            long long dist = x*x + y*y;

            arr.push_back({dist, point});
        }

        sort(arr.begin(), arr.end());

        vector<vector<int>> ans;

        for(int i=0;i<k;i++)
            ans.push_back(arr[i].second);

        return ans;
    }
};
```

---

## Dry Run

Input

```
points

(1,3)
(-2,2)
(5,8)
(0,1)

k=2
```

Distances

```
10
8
89
1
```

Store

```
(10,(1,3))
(8,(-2,2))
(89,(5,8))
(1,(0,1))
```

Sort

```
1
8
10
89
```

Take first two

```
(0,1)

(-2,2)
```

Answer

```
[(0,1),(-2,2)]
```

---

## Time Complexity

### Computing distances

```
O(n)
```

---

### Sorting

Sorting all N elements

```
O(n log n)
```

---

### Taking first K

```
O(k)
```

---

### Total

```
O(n log n)
```

---

## Space Complexity

Extra array stores all points

```
O(n)
```

---

# WHY BRUTE FORCE IS NOT OPTIMAL

Suppose

```
n = 100000

k = 10
```

Brute force still sorts

```
100000
```

elements.

But we only needed

```
10
```

answers.

Most of the sorting work is unnecessary.

This is exactly why we optimize using a heap.

---

# OPTIMAL APPROACH:

Use a **fixed-size Max Heap**.

---

## Code

```cpp
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {

        priority_queue<
            pair<long long, vector<int>>,
            vector<pair<long long, vector<int>>>,
            less<pair<long long, vector<int>>>
        > pq;

        for(auto &point : points){

            long long x = point[0];
            long long y = point[1];

            long long dist = x*x + y*y;

            pq.push({dist, point});

            if(pq.size() > k)
                pq.pop();
        }

        vector<vector<int>> ans;

        while(!pq.empty()){
            ans.push_back(pq.top().second);
            pq.pop();
        }

        return ans;
    }
};
```

---

# ALGORITHM:

1. Create a Max Heap.
2. Traverse every point.
3. Compute squared distance.
4. Push `(distance, point)` into heap.
5. If heap size exceeds `k`, remove the largest distance.
6. After processing all points, heap contains exactly K closest points.
7. Pop everything into answer.

---

# DRY RUN:

Input

```
points

(1,3)
(-2,2)
(5,8)
(0,1)

k=2
```

---

### Insert (1,3)

distance

```
10
```

Heap

```
10
```

---

### Insert (-2,2)

distance

```
8
```

Heap

```
10
8
```

Top

```
10
```

---

### Insert (5,8)

distance

```
89
```

Heap

```
89
10
8
```

Size becomes 3.

Remove top.

Heap

```
10
8
```

Point (5,8) gets discarded because it is too far.

---

### Insert (0,1)

distance

```
1
```

Heap

```
10
8
1
```

Again size becomes 3.

Remove top.

Heap

```
8
1
```

Remaining points

```
(-2,2)

(0,1)
```

Answer.

---

# IMPORTANT CODE SNIPPETS:

## Distance

```cpp
long long dist = x*x + y*y;
```

---

## Max Heap

```cpp
priority_queue<
pair<long long, vector<int>>,
vector<pair<long long, vector<int>>>,
less<pair<long long, vector<int>>>
> pq;
```

---

## Push

```cpp
pq.push({dist, point});
```

---

## Maintain K elements

```cpp
if(pq.size()>k)
    pq.pop();
```

---

# COMMON MISTAKES:

### Using sqrt()

Unnecessary.

Compare squared distances.

---

### Using int

Coordinates can be

```
30000
```

Square becomes

```
900000000
```

Use

```cpp
long long
```

to safely handle calculations.

---

### Using Min Heap

Wrong.

We want to remove the farthest point quickly.

That requires a Max Heap.

---

### Sorting unnecessarily

Sorting all elements wastes time when K is much smaller than N.

---

### Forgetting to pop when heap exceeds K

Then heap will contain every point and lose the optimization.

---

# WHY I MIGHT FORGET THIS:

Because it feels like a sorting problem.

But the keyword is:

> **Return only K elements.**

Whenever only K answers are needed,

think

**Fixed Size Heap**.

---

# INTERVIEW FLOW:

**Interviewer:** How would you solve this?

"I can compute the distance of every point from the origin."

"One approach is to sort all points based on distance and return the first K."

"This takes O(n log n)."

"But we don't actually need all points in sorted order."

"We only need K closest."

"So I'll maintain a Max Heap of size K."

"For every point:

- compute its distance,
- push it,
- if heap exceeds K, remove the farthest point."

"Finally the heap contains exactly the K closest points."

---

# TIME COMPLEXITY:

## Brute Force

### Distance Calculation

Every point processed once.

```
O(n)
```

---

### Sorting

Sorting all N distances.

```
O(n log n)
```

---

### Taking first K

```
O(k)
```

---

### Total

```
O(n log n)
```

---

## Optimal

For every point:

Push into heap

```
O(log K)
```

Sometimes pop

```
O(log K)
```

Each iteration performs at most one push and one pop.

Each heap operation costs

```
O(log K)
```

For all N points:

```
O(n log K)
```

Since

```
K << N
```

for most interview problems,

```
log K << log N
```

making this significantly faster than sorting.

Example:

```
n = 100000
k = 10

Brute:
100000 × log₂(100000) ≈ 100000 × 17

Optimal:
100000 × log₂(10) ≈ 100000 × 3.3
```

So the heap performs far fewer comparisons than sorting the entire array.

---

# SPACE COMPLEXITY:

## Brute Force

Stores every point with its distance.

```
O(n)
```

---

## Optimal

Heap stores at most

```
K
```

elements.

Answer vector stores K points.

Ignoring the output array (which is usually not counted as auxiliary space), the extra space is:

```
O(k)
```

If the output array is included, the total memory used is still proportional to `k`, so it remains:

```
O(k)
```

---

# EDGE CASES:

- k = 1
- k = n
- Origin itself exists `(0,0)`
- Negative coordinates
- Large coordinates (use `long long`)
- Duplicate distances (problem guarantees unique answer)

---

# PATTERN RECOGNITION:

If the question says:

- K Closest
- K Smallest
- K Largest
- Top K
- K Best
- K Nearest

Ask yourself:

> Do I really need every element sorted?

If **No**, use a **fixed-size heap**.

### Heap Selection Rule

- Need **K smallest** → **Max Heap**
- Need **K largest** → **Min Heap**

Reason:

The heap always keeps the **worst candidate among the current K answers** at the top so it can be removed immediately when a better candidate arrives.

---

# LINE-BY-LINE INTUITION (Optimal Code)

### Create the Max Heap

```cpp
priority_queue<
    pair<long long, vector<int>>,
    vector<pair<long long, vector<int>>>,
    less<pair<long long, vector<int>>>
> pq;
```

- `less<>` makes it a **Max Heap**.
- Each element stores `(distanceSquared, point)`.
- The point with the **largest distance** stays on top.

---

### Compute squared distance

```cpp
long long dist = x * x + y * y;
```

- No need for `sqrt()`.
- Squared distances preserve the same ordering.

---

### Push every point

```cpp
pq.push({dist, point});
```

Every point is a potential answer until proven otherwise.

---

### Keep heap size fixed

```cpp
if (pq.size() > k)
    pq.pop();
```

- If we have more than `k` points, remove the farthest one.
- This maintains the invariant that the heap always contains the `k` closest points seen so far.

---

### Build the answer

```cpp
while (!pq.empty()) {
    ans.push_back(pq.top().second);
    pq.pop();
}
```

Extract all remaining points from the heap. The order doesn't matter because the problem accepts any order.

---

# EASY-TO-REMEMBER SUMMARY

- Compare **squared distances**, not `sqrt`.
- We need **K closest**, not a full sort.
- Keep only **K** useful candidates.
- Use a **Max Heap** because we want to discard the farthest among the current K.
- Heap stores `(distanceSquared, point)`.
- Push every point.
- If size exceeds `k`, pop the top (farthest).
- Final complexity:
  - **Brute:** `O(n log n)`, `O(n)`
  - **Optimal:** `O(n log k)`, `O(k)`
- Golden Rule:
  - **K smallest → Max Heap**
  - **K largest → Min Heap**
````
