

## PROBLEM:
You are given an array of package weights.

- Packages **must be shipped in the given order**.
- You have exactly **D days**.
- Each day, the total weight loaded **cannot exceed the boat capacity**.
- Find the **minimum capacity** required to finish shipping within **D days**.

---

# PATTERN:

**Binary Search on Answer**

(Search Space = Possible Boat Capacities)

---

# WHY THIS PATTERN:

We are **not searching inside the array**.

Instead, we are searching for the **minimum capacity**.

Notice something important:

| Capacity | Required Days |
|----------|---------------|
| Small | More days |
| Bigger | Fewer days |
| Very Big | Even fewer days |

As capacity increases,

**Required Days always decreases (or stays the same).**

This is a **monotonic property**, making Binary Search applicable.

Think of it like this:

```text
Capacity

5   -> 8 days ❌
6   -> 7 days ❌
7   -> 6 days ❌
8   -> 5 days ❌
9   -> 4 days ❌
10  -> 3 days ✅
11  -> 3 days ✅
12  -> 2 days ✅
13  -> 2 days ✅
```

There is a clear transition:

```text
False False False False True True True
```

Whenever you see

> Find the minimum value that satisfies a condition

and the answers look like

```text
FFFFFTTTTT
```

→ Binary Search on Answer.

---

# CORE IDEA:

For every capacity,

simulate shipping.

If it finishes within D days,

this capacity works.

Now try a **smaller** capacity.

Otherwise,

increase capacity.

---

# BRUTE FORCE:

## Idea

Try **every possible capacity** from

```text
Maximum package weight
```

to

```text
Total sum of all weights.
```

For each capacity,

calculate how many days are required.

The first capacity that works is the answer.

---

## Why start from Maximum Element?

Capacity cannot be smaller than the heaviest package.

Example:

```text
[3,5,8]
```

Capacity = 6

Cannot even carry 8.

Impossible.

So,

```cpp
low = max(arr)
```

---

## Why end at Sum?

If capacity equals total sum,

everything fits in one day.

Largest possible answer.

```cpp
high = sum(arr)
```

---

## Brute Force Code

```cpp
class Solution {
public:

    int findDays(vector<int>& arr, int capacity){

        int day = 1;
        int load = 0;

        for(int weight : arr){

            if(load + weight <= capacity){
                load += weight;
            }
            else{
                day++;
                load = weight;
            }

        }

        return day;
    }

    int leastWeightCapacity(vector<int>& arr, int D) {

        int low = *max_element(arr.begin(), arr.end());
        int high = accumulate(arr.begin(), arr.end(), 0);

        for(int capacity = low; capacity <= high; capacity++){

            int requiredDays = findDays(arr, capacity);

            if(requiredDays <= D)
                return capacity;
        }

        return -1;
    }
};
```

---

## Dry Run (Brute Force)

Example

```text
arr = [1,2,3,1,1]
D = 4
```

```text
low = 3
high = 8
```

Try

Capacity = 3

```text
Day1 : 1+2 =3

Day2 :3

Day3 :1+1

Required =3 days

3<=4

Answer =3
```

Done.

---

## Time Complexity

Let

```text
S = sum(arr)

M = max(arr)
```

Possible capacities

```text
S-M+1
```

For each capacity,

we scan entire array.

```text
O((S-M+1) × N)

≈ O(N × (S-M))
```

Worst case

```text
Very large.
```

---

## Space Complexity

```text
O(1)
```

---

# OPTIMAL APPROACH:

Instead of checking

```text
3
4
5
6
7
8
...
```

Use Binary Search.

Since

```text
Capacity ↑

Days ↓
```

we can eliminate half of the search space every iteration.

---

# ALGORITHM:

### Step 1

Lowest possible answer

```cpp
low = max(arr)
```

---

### Step 2

Highest possible answer

```cpp
high = sum(arr)
```

---

### Step 3

Binary Search

```text
mid = possible capacity
```

---

### Step 4

Find

```text
requiredDays(mid)
```

---

### Step 5

If

```text
requiredDays <= D
```

Capacity works.

Store answer.

Try smaller capacity.

```cpp
high = mid-1
```

---

### Step 6

Else

Need larger capacity.

```cpp
low = mid+1
```

---

### Step 7

Return answer.

---

# DRY RUN:

Example

```text
arr = [1,2,3,1,1]

D =4
```

---

### Initial

```text
low =3

high =8

ans=-1
```

---

### Iteration 1

```text
mid=(3+8)/2

=5
```

findDays(5)

```text
Day1

1+2=3

+3 exceeds

Day1=3

Day2=3+1+1=5

Required=2
```

```text
2<=4

Works
```

Store

```text
ans=5

high=4
```

---

### Iteration 2

```text
low=3

high=4

mid=3
```

findDays(3)

```text
Day1

1+2=3

Day2

3

Day3

1+1

Required=3
```

```text
3<=4

Works
```

```text
ans=3

high=2
```

Loop stops.

Answer

```text
3
```

---

# IMPORTANT CODE SNIPPETS:

### Lower Bound

```cpp
int low = *max_element(arr.begin(), arr.end());
```

Because capacity cannot be smaller than the largest package.

---

### Upper Bound

```cpp
long long high = accumulate(arr.begin(), arr.end(), 0LL);
```

Everything can be shipped in one day.

---

### Days Calculation

```cpp
if(load + weight <= capacity)
{
    load += weight;
}
else
{
    day++;
    load = weight;
}
```

If current package doesn't fit,

start a new day and place it there.

---

### Binary Search Decision

```cpp
if(requiredDays <= D)
{
    ans = mid;
    high = mid - 1;
}
else
{
    low = mid + 1;
}
```

If a capacity works,

search left because we want the **minimum** working capacity.

---

# COMMON MISTAKES:

### 1. Starting low from 0

Wrong.

Must start from

```cpp
max(arr)
```

---

### 2. Starting new day without loading current package

Wrong

```cpp
day++;
```

Correct

```cpp
day++;
load = weight;
```

Current package belongs to the new day.

---

### 3. Using `< D`

Wrong.

Need

```cpp
requiredDays <= D
```

We can always leave some days unused.

---

### 4. Forgetting answer

Need

```cpp
ans = mid;
```

before searching left.

---

### 5. Binary Searching on array

We're binary searching **capacity**, not array indices.

---

# WHY I MIGHT FORGET THIS:

Because the array is **never sorted**.

Binary Search is happening over the **answer range**.

Remember:

```text
Array → Simulation

Capacity → Binary Search
```

---

# INTERVIEW FLOW:

> We need the minimum boat capacity. The minimum possible capacity is the maximum package weight because every package must fit individually. The maximum possible capacity is the sum of all weights because then everything can be shipped in one day. As capacity increases, the number of days required never increases—it only decreases or stays the same. This monotonic property gives us a binary-searchable answer space. For each candidate capacity, we simulate loading packages in order and count the required days. If we can finish within D days, we try a smaller capacity; otherwise, we increase the capacity. The first feasible minimum capacity is the answer.

---

# TIME COMPLEXITY:

### Brute Force

```text
O((Sum-Max+1) × N)

≈ O(N × (Sum-Max))
```

**Reason:**
- There are `(Sum - Max + 1)` possible capacities to try.
- For each capacity, `findDays()` scans all `N` packages once.

---

### Optimal

```text
O(N × log(Sum-Max))
```

**Reason:**
- Binary Search performs `log(Sum - Max + 1)` iterations over the answer range.
- Each iteration calls `findDays()`, which scans the array once (`O(N)`).

---

# SPACE COMPLEXITY:

### Brute Force

```text
O(1)
```

### Optimal

```text
O(1)
```

Only a few integer variables are used.

---

# EDGE CASES:

### D = 1

Answer

```text
sum(arr)
```

---

### D = N

Answer

```text
max(arr)
```

---

### Single package

```text
[10]
```

Answer

```text
10
```

---

### Equal weights

```text
[5,5,5,5]
```

Still works.

---

### Very large weights

Use

```cpp
long long high = accumulate(arr.begin(), arr.end(), 0LL);
```

since the sum of weights can exceed the range of an `int`.

---

# PATTERN RECOGNITION:

Whenever you see:

- Minimum/Maximum possible answer
- Answer lies in a range
- Need to satisfy a condition
- Condition becomes only easier or only harder as answer changes
- Can write a checking function

Think:

```text
Binary Search on Answer
```

Typical examples:

- Koko Eating Bananas
- Minimum Days to Make M Bouquets
- Capacity to Ship Packages
- Split Array Largest Sum
- Allocate Books
- Painter's Partition

---

# Clean C++ Code

```cpp
class Solution {
public:
    int findDays(vector<int>& arr, int capacity) {

        int day = 1;
        int load = 0;

        for (int weight : arr) {

            if (load + weight <= capacity) {
                load += weight;
            }
            else {
                day++;
                load = weight;
            }
        }

        return day;
    }

    int leastWeightCapacity(vector<int>& arr, int D) {

        int low = *max_element(arr.begin(), arr.end());
        long long high = accumulate(arr.begin(), arr.end(), 0LL);

        int ans = -1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int requiredDays = findDays(arr, mid);

            if (requiredDays <= D) {
                ans = mid;
                high = mid - 1;
            }
            else {
                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int low = *max_element(arr.begin(), arr.end());
```

The boat must at least carry the heaviest package.

---

```cpp
long long high = accumulate(arr.begin(), arr.end(), 0LL);
```

The boat can always carry all packages in one day.

---

```cpp
int mid = low + (high - low) / 2;
```

Test the middle capacity while avoiding integer overflow.

---

```cpp
int requiredDays = findDays(arr, mid);
```

Simulate shipping with the chosen capacity.

---

```cpp
if(requiredDays <= D)
```

This capacity is sufficient.

Try finding a smaller one.

---

```cpp
ans = mid;
```

Store the current valid answer before searching left.

---

```cpp
high = mid - 1;
```

A smaller valid capacity might exist.

---

```cpp
low = mid + 1;
```

The current capacity is too small, so increase it.

---

```cpp
load = weight;
```

When starting a new day, the current package is the first package loaded on that day.

---

# Easy-to-Remember Summary

1. **Search Space**
   - `low = max(arr)`
   - `high = sum(arr)`

2. **Check Function**
   - Simulate loading packages.
   - Count required days.

3. **Decision**
   - `requiredDays <= D` → Valid → Search Left.
   - `requiredDays > D` → Invalid → Search Right.

4. **Pattern**
   - Increasing capacity never increases required days.
   - Monotonic (`FFFFTTTT`) ⇒ **Binary Search on Answer**.
