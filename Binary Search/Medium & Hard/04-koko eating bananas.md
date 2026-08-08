# Koko Eating Bananas

## PROBLEM:

You are given `n` piles of bananas and `k` hours.

If Koko eats at speed `s` bananas/hour:

- A pile of size `pile` takes `ceil(pile / s)` hours.
- She can eat from **only one pile per hour**.

Find the **minimum eating speed** so that the total hours required is **≤ k**.

---

## PATTERN:

**Binary Search on Answer (Search Space Binary Search)**

---

## WHY THIS PATTERN:

We are **not searching inside the array**.

Instead, we are searching for the **minimum valid eating speed**.

Possible answers look like:

```
Speed = 1   -> Too slow
Speed = 2   -> Too slow
Speed = 3   -> Too slow
Speed = 4   -> Too slow
Speed = 5   -> Works
Speed = 6   -> Works
Speed = 7   -> Works
...
```

Notice the pattern:

```
False False False False True True True True
```

Once a speed works,

every larger speed will also work.

This is a **monotonic property**.

Whenever you see:

```
FFFFFTTTTT
```

or

```
TTTTFFFF
```

think:

> **Binary Search on Answer**

---

## CORE IDEA:

For every eating speed,

calculate

```
Total Hours = Σ ceil(pile / speed)
```

If

```
hours <= k
```

then this speed is possible.

Since we need the **minimum** valid speed,

search on the **left half**.

Otherwise,

search on the **right half**.

---

## BRUTE FORCE:

### Idea

Try every possible speed

```
1
2
3
...
maxPile
```

For each speed,

calculate total hours.

Return the first valid one.

### Code

```cpp
class Solution {
public:
    int kokoEat(vector<int>& arr, int k) {

        int maxPile = *max_element(arr.begin(), arr.end());

        for (int speed = 1; speed <= maxPile; speed++) {

            long long hours = 0;

            for (int pile : arr) {
                hours += (pile + speed - 1) / speed;
            }

            if (hours <= k)
                return speed;
        }

        return -1;
    }
};
```

### Dry Run

```
arr = [5,10,3]
k = 4
```

Speed = 1

```
5 + 10 + 3 = 18
```

Not possible.

Speed = 2

```
3 + 5 + 2 = 10
```

Not possible.

Speed = 3

```
2 + 4 + 1 = 7
```

Not possible.

Speed = 4

```
2 + 3 + 1 = 6
```

Not possible.

Speed = 5

```
1 + 2 + 1 = 4
```

Possible.

Answer = **5**

### Time Complexity

```
O(n × maxPile)
```

Reason:

```
maxPile possible speeds

For every speed

scan all n piles.
```

Worst case:

```
10^6 × 10^6 = 10^12
```

Too slow.

### Space Complexity

```
O(1)
```

---

## OPTIMAL APPROACH:

Use **Binary Search on Answer**.

Instead of checking every speed,

binary search between

```
1
...
maxPile
```

---

## ALGORITHM:

### Step 1

Initialize search space

```cpp
low = 1;
high = maxPile;
```

---

### Step 2

Find middle speed

```cpp
mid = low + (high - low) / 2;
```

---

### Step 3

Calculate total hours

```cpp
hours += ceil(pile / mid);
```

using integer arithmetic

```cpp
hours += (pile + mid - 1) / mid;
```

---

### Step 4

If

```
hours <= k
```

Current speed works.

Store it.

Search left.

```cpp
high = mid - 1;
```

---

### Step 5

Else,

speed is too slow.

Search right.

```cpp
low = mid + 1;
```

---

### Step 6

Return answer.

---

## DRY RUN:

```
arr = [5,10,3]
k = 4
```

Maximum pile

```
10
```

Search Space

```
1 ... 10
```

### Iteration 1

```
low = 1
high = 10

mid = 5
```

Hours

```
5  -> 1

10 -> 2

3  -> 1

Total = 4
```

Valid

```
ans = 5

high = 4
```

---

### Iteration 2

```
low = 1
high = 4

mid = 2
```

Hours

```
5 -> 3

10 -> 5

3 -> 2

Total = 10
```

Too slow

```
low = 3
```

---

### Iteration 3

```
low = 3
high = 4

mid = 3
```

Hours

```
2 + 4 + 1 = 7
```

Too slow

```
low = 4
```

---

### Iteration 4

```
low = 4
high = 4

mid = 4
```

Hours

```
2 + 3 + 1 = 6
```

Too slow

```
low = 5
```

Loop ends

```
low = 5
high = 4
```

Return

```
5
```

---

## IMPORTANT CODE SNIPPETS:

### Search Space

```cpp
int low = 1;
int high = *max_element(arr.begin(), arr.end());
```

---

### Safe Mid

```cpp
int mid = low + (high - low) / 2;
```

---

### Ceiling Division

```cpp
hours += (pile + mid - 1) / mid;
```

Remember:

```
ceil(a / b)

=

(a + b - 1) / b
```

---

### Valid Answer

```cpp
if (hours <= k) {
    ans = mid;
    high = mid - 1;
}
```

---

### Invalid Answer

```cpp
else {
    low = mid + 1;
}
```

---

## COMMON MISTAKES:

### 1. Using normal division

Wrong

```cpp
hours += pile / mid;
```

Correct

```cpp
hours += (pile + mid - 1) / mid;
```

---

### 2. Searching right after finding an answer

We need the **minimum** speed.

Always search left.

---

### 3. Using `int` for total hours

Wrong

```cpp
int hours;
```

Correct

```cpp
long long hours;
```

---

### 4. Starting search from 0

Wrong

```cpp
low = 0;
```

Division by zero.

Always

```cpp
low = 1;
```

---

### 5. Forgetting to store answer

Always write

```cpp
ans = mid;
```

before moving left.

---

## WHY I MIGHT FORGET THIS:

This problem looks like a simulation.

Many people immediately think:

```
Try every speed.
```

Instead ask:

> If one speed works, will every larger speed also work?

If the answer is **Yes**,

then use **Binary Search on Answer**.

---

## INTERVIEW FLOW:

> "The brute force solution is to try every eating speed from 1 to the maximum pile size. For each speed, calculate the total hours using ceiling division and return the first valid speed. This takes O(n × maxPile), which is too slow.
>
> I notice that the answer is monotonic. If a speed works, every larger speed will also work. Therefore, I can binary search on the answer.
>
> My search space is from 1 to the maximum pile size. For each candidate speed, I calculate the required hours. If the hours are within k, I store the answer and search the left half to find a smaller valid speed. Otherwise, I search the right half.
>
> This reduces the complexity to O(n log(maxPile))."

---

## TIME COMPLEXITY:

### Brute Force

```
O(n × maxPile)
```

Reason:

```
maxPile speeds

×

scan n piles
```

---

### Optimal

```
O(n log(maxPile))
```

Reason:

```
Binary Search

=

log(maxPile) iterations

Each iteration

=

O(n)
```

Overall

```
O(n log(maxPile))
```

---

## SPACE COMPLEXITY:

### Brute Force

```
O(1)
```

### Optimal

```
O(1)
```

Only a few variables are used.

---

## EDGE CASES:

### Single pile

```
arr = [10]

k = 1

Answer = 10
```

---

### k equals number of piles

```
arr = [5,10,3]

k = 3

Answer = 10
```

Every pile must finish in one hour.

---

### Very large k

```
arr = [3,6]

k = 100

Answer = 1
```

---

### All piles equal

```
arr = [8,8,8]
```

Works normally.

---

### Large input values

```
n = 10^6

arr[i] = 10^6
```

Always use

```cpp
long long hours;
```

---

## PATTERN RECOGNITION:

Whenever you see:

- Find the **minimum/maximum value** satisfying a condition.
- The answer lies in a numeric range.
- A feasibility function that is monotonic.
- If one answer works, all larger (or smaller) answers also work.

Think:

> **Binary Search on Answer**

Common questions using this pattern:

- Koko Eating Bananas
- Capacity to Ship Packages Within D Days
- Minimum Days to Make m Bouquets
- Smallest Divisor Given a Threshold
- Aggressive Cows
- Allocate Minimum Number of Pages
- Painter's Partition

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int kokoEat(vector<int>& arr, int k) {

        int low = 1;
        int high = *max_element(arr.begin(), arr.end());

        int ans = -1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            long long hours = 0;

            for (int pile : arr) {
                hours += (pile + mid - 1) / mid;
            }

            if (hours <= k) {
                ans = mid;
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int low = 1;
```

Minimum possible eating speed.

---

```cpp
int high = *max_element(arr.begin(), arr.end());
```

Maximum speed we ever need.

Eating faster than the largest pile gives no benefit.

---

```cpp
int ans = -1;
```

Stores the smallest valid speed found so far.

---

```cpp
while (low <= high)
```

Standard Binary Search loop.

---

```cpp
int mid = low + (high - low) / 2;
```

Current speed being tested.

Written this way to avoid overflow.

---

```cpp
long long hours = 0;
```

Total hours can exceed the range of `int`.

---

```cpp
hours += (pile + mid - 1) / mid;
```

Computes

```
ceil(pile / mid)
```

using integer arithmetic.

---

```cpp
if (hours <= k)
```

Current speed is valid.

---

```cpp
ans = mid;
```

Save the answer before trying to improve it.

---

```cpp
high = mid - 1;
```

Try finding a smaller valid speed.

---

```cpp
low = mid + 1;
```

Current speed is too slow.

Increase it.

---

```cpp
return ans;
```

Returns the minimum valid speed.

---

# EASY-TO-REMEMBER SUMMARY

- **Search Space:** `1 → max(arr)`
- **Check Function:** `Σ ceil(pile / speed)`
- If `hours <= k`
  - Save answer
  - Search Left
- Else
  - Search Right
- Ceiling Division:

```cpp
(pile + speed - 1) / speed
```

- Use `long long` for total hours.
- Pattern = **Binary Search on Answer**
- Time = **O(n log(maxPile))**
- Space = **O(1)**
