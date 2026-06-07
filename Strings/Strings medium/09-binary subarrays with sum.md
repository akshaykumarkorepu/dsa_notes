

## PROBLEM:
Binary Subarray Sum

---

## PATTERN:
Prefix Sum + HashMap

---

## WHY THIS PATTERN?

Because:

- We need:
  - count of subarrays
  - with exact sum = target

And for subarray sum problems:

```text
prefix sums
```

help us calculate subarray sums efficiently.

The hashmap is used because:

```text
we need fast lookup of previous prefix sums
```

---

# BRUTE FORCE (INTERVIEW PROGRESSION)

## WHY BRUTE FORCE FIRST?

This is a good interview progression problem because:

- optimal solution is NOT immediately obvious
- interviewer expects optimization thinking
- transition from O(N²) → O(N) is important

---

# BRUTE FORCE IDEA

Generate all subarrays and calculate sums.

```cpp
int count = 0;

for(int i = 0; i < n; i++) {

    int sum = 0;

    for(int j = i; j < n; j++) {

        sum += arr[j];

        if(sum == target) {
            count++;
        }
    }
}
```

---

# BRUTE FORCE TC & SC

## TIME COMPLEXITY

```text
O(N²)
```

WHY?

- outer loop → N
- inner loop → N

---

## SPACE COMPLEXITY

```text
O(1)
```

---

# OPTIMIZATION THOUGHT

In brute force:

```text
we repeatedly calculate sums
```

This repetition is unnecessary.

Instead:

> Can we reuse previous calculations?

YES → Prefix Sum.

---

# CORE IDEA

At every index:

```text
currentPrefix = total sum till current index
```

Now we ask:

> “How much previous sum should I remove
> so the remaining part becomes target?”

That removable amount becomes:

```text
need = currentPrefix - target
```

If that removable prefix existed before:

then remaining subarray sum = target.

---

# MAIN FORMULA

Subarray sum formula:

```text
subarraySum(i+1,j) = prefix[j] - prefix[i]
```

For target:

```text
prefix[j] - prefix[i] = target
```

Rearrange:

```text
prefix[i] = prefix[j] - target
```

This becomes:

```cpp
need = prefix - target;
```

---

# HASHMAP STORES

```text
prefixSum → frequency
```

Example:

```text
{
  0 : 1,
  1 : 2,
  2 : 1
}
```

Meaning:

- prefix 0 seen once
- prefix 1 seen twice
- prefix 2 seen once

---

# WHY FREQUENCY?

Because:

```text
same prefix can appear multiple times
```

And each occurrence creates another valid subarray.

---

# IMPORTANT INTUITION

We are NOT directly searching for subarrays.

We are searching for:

# “previous prefixes to remove”

so remaining sum becomes target.

---

# FLOW OF THE ALGORITHM

For every index:

---

## STEP 1 — Update Prefix Sum

```cpp
prefix += arr[i];
```

Meaning:

```text
calculate total sum till current index
```

---

## STEP 2 — Find Required Prefix

```cpp
need = prefix - target;
```

Meaning:

> “How much sum should I remove
> so remaining becomes target?”

---

## STEP 3 — Check HashMap

```cpp
if(mp.find(need) != mp.end()) {
    count += mp[need];
}
```

Meaning:

If removable prefix existed before:

```text
valid subarray exists
```

If frequency is 2:

```text
2 valid subarrays exist
```

---

## STEP 4 — Store Current Prefix

```cpp
mp[prefix]++;
```

Meaning:

```text
store current prefix for future indices
```

---

# IMPORTANT ORDER

Correct order:

```text
1. update prefix
2. check hashmap
3. store prefix
```

NOT:

```text
store first
```

Because:

```text
we only want PREVIOUS prefixes
```

---

# WHY `mp[0] = 1` ?

```cpp
mp[0] = 1;
```

This handles:

```text
subarrays starting from index 0
```

Example:

```text
arr = [1,1]
target = 2
```

At index 1:

```text
prefix = 2
need = 0
```

So:

```text
mp[0] must exist
```

---

# FINAL OPTIMAL CODE

```cpp
class Solution {
  public:
  
    int countSubarrays(vector<int>& arr, int target) {
        
        unordered_map<int,int> mp;

        int prefix = 0;
        int count = 0;

        mp[0] = 1;

        for(int i = 0; i < arr.size(); i++) {

            // STEP 1
            prefix += arr[i];

            // STEP 2
            int need = prefix - target;

            // STEP 3
            if(mp.find(need) != mp.end()) {
                count += mp[need];
            }

            // STEP 4
            mp[prefix]++;
        }

        return count;
    }
};
```

---

# DRY RUN

## EXAMPLE

```text
arr = [1,0,1,0,1]
target = 2
```

---

# INITIAL

```cpp
prefix = 0
count = 0

mp = {
  0 : 1
}
```

---

# INDEX 0

Value:

```text
1
```

---

## STEP 1

```cpp
prefix += 1
```

```text
prefix = 1
```

---

## STEP 2

```text
need = 1 - 2 = -1
```

---

## STEP 3

```text
-1 not found
```

No valid subarray.

---

## STEP 4

```cpp
mp[1]++
```

Map:

```text
{
 0:1,
 1:1
}
```

---

# INDEX 1

Value:

```text
0
```

---

## STEP 1

```text
prefix = 1
```

---

## STEP 2

```text
need = -1
```

---

## STEP 3

Not found.

---

## STEP 4

```cpp
mp[1]++
```

Map:

```text
{
 0:1,
 1:2
}
```

---

# INDEX 2

Value:

```text
1
```

---

## STEP 1

```text
prefix = 2
```

---

## STEP 2

```text
need = 0
```

---

## STEP 3

```text
mp[0] = 1
```

Meaning:

```text
one removable prefix exists
```

So:

```cpp
count += 1
```

Count:

```text
1
```

Valid subarray:

```text
[1,0,1]
```

---

## STEP 4

```cpp
mp[2]++
```

Map:

```text
{
 0:1,
 1:2,
 2:1
}
```

---

# INDEX 3

Value:

```text
0
```

---

## STEP 1

```text
prefix = 2
```

---

## STEP 2

```text
need = 0
```

---

## STEP 3

```text
mp[0] = 1
```

Another valid subarray.

```cpp
count += 1
```

Count:

```text
2
```

Valid subarray:

```text
[1,0,1,0]
```

---

## STEP 4

```cpp
mp[2]++
```

Map:

```text
{
 0:1,
 1:2,
 2:2
}
```

---

# INDEX 4

Value:

```text
1
```

---

## STEP 1

```text
prefix = 3
```

---

## STEP 2

```text
need = 1
```

---

## STEP 3

```text
mp[1] = 2
```

Meaning:

```text
two removable prefixes exist
```

So:

```cpp
count += 2
```

Count:

```text
4
```

Valid subarrays:

```text
[0,1,0,1]
[1,0,1]
```

---

## STEP 4

```cpp
mp[3]++
```

---

# FINAL ANSWER

```text
4
```

---

# INTERVIEW EXPLANATION FLOW

If interviewer says:

> “Explain your approach.”

Say:

---

> I first thought of brute force by generating all subarrays and checking sums, which takes O(N²).
>
> To optimize,
> I used prefix sums.
>
> At every index,
> I know the total sum till current position.
>
> Now I ask:
>
> “How much previous sum should I remove so remaining becomes target?”
>
> That removable amount is:
>
> currentPrefix - target
>
> So if I’ve seen that removable prefix before,
> then the remaining subarray has sum = target.
>
> I store prefix sums and their frequencies in a hashmap for O(1) lookup.

---

# TIME COMPLEXITY

## OPTIMAL TC

```text
O(N)
```

WHY?

- each element processed once
- hashmap lookup → average O(1)

---

# SPACE COMPLEXITY

```text
O(N)
```

WHY?

Worst case:

all prefix sums unique.

Example:

```text
[1,1,1,1]
```

prefixes:

```text
1,2,3,4
```

all stored in hashmap.

---

# EDGE CASES

- target larger than total sum
- all zeros
- subarrays starting from index 0
- repeated prefix sums
- target = 0 (important order issue)

---

# WHY I GOT STUCK / MIGHT FORGET

- I forget WHY:
  
```cpp
need = prefix - target
```

REAL intuition:

> We are trying to REMOVE some old prefix
> so remaining becomes target.

---

- I forget why frequency is needed.

Reason:

```text
same prefix may appear multiple times
```

Each occurrence creates another valid subarray.

---

- I forget why:

```cpp
mp[0] = 1
```

Reason:

```text
handles subarrays starting from index 0
```

---

- I forget order.

Correct:

```text
1. update prefix
2. check hashmap
3. insert prefix
```

Because we only want PREVIOUS prefixes.
````
