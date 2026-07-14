
## PROBLEM:
Given an array (can contain **positive, negative, and zero**) and an integer `k`, count the **number of contiguous subarrays** whose sum is exactly `k`.

The goal is to return the **count**, not the subarrays themselves.

---

## PATTERN:
**Prefix Sum + HashMap (Frequency Map)**

---

## WHY THIS PATTERN:

Normally, to find a subarray sum, we'd try every starting index (O(N²)).

Instead of repeatedly calculating subarray sums, we store **prefix sums**.

The key observation is:

```text
Subarray Sum = Current Prefix Sum - Previous Prefix Sum
```

If we need

```text
Subarray Sum = k
```

then

```text
Current Prefix - Previous Prefix = k
```

Rearranging,

```text
Previous Prefix = Current Prefix - k
```

So while traversing the array, we only need to ask:

> **"Have I seen a prefix sum equal to (currentPrefix - k) before?"**

A HashMap answers this in **O(1)** average time.

---

## CORE IDEA:

As we move through the array:

- Keep calculating the running (prefix) sum.
- Store every prefix sum in a HashMap.
- For the current prefix sum:
  - Check whether `(prefixSum - k)` already exists.
  - If yes, every occurrence represents one valid subarray ending at the current index.
- Add that frequency to the answer.
- Store the current prefix sum for future indices.

---

## BRUTE FORCE:

### Intuition

For every starting index, keep extending the ending index while maintaining the running sum.

If the running sum becomes `k`, increase the answer.

### Code

```cpp
int count = 0;

for (int i = 0; i < n; i++) {

    int sum = 0;

    for (int j = i; j < n; j++) {

        sum += arr[j];

        if (sum == k)
            count++;
    }
}

return count;
```

### Dry Run

```
arr = [10,2,-2,-20,10]
k = -10
```

Start = 0

```
10
12
10
-10 ✓
0
```

Start = 1

```
2
0
-20
-10 ✓
```

Start = 2

```
-2
-22
-12
```

Start = 3

```
-20
-10 ✓
```

Answer = 3

### Time Complexity

```
O(N²)
```

### Space Complexity

```
O(1)
```

---

## OPTIMAL APPROACH:

Instead of checking every possible starting index, store the running prefix sums.

At every index:

- Calculate the current prefix sum.
- Ask:
  > **"Have I seen a prefix sum equal to currentPrefix - k?"**
- If yes, every occurrence forms one valid subarray ending at the current index.
- Store the current prefix sum.

HashMap stores

```
Prefix Sum -> Frequency
```

---

## ALGORITHM:

```
Create HashMap

mp[0] = 1

prefixSum = 0

count = 0

For every element:

    prefixSum += element

    if (prefixSum-k exists)

        count += frequency

    store current prefix

Return count
```

---

## DRY RUN:

### Input

```
arr = [10,2,-2,-20,10]
k = -10
```

Initially

```
prefixSum = 0

count = 0

Map

0 → 1
```

---

### Index 0

Element

```
10
```

Current Prefix

```
10
```

Need

```
10-(-10)=20
```

20 not present.

Store

```
10→1
```

Map

| Prefix | Frequency |
|--------|----------:|
| 0 | 1 |
| 10 | 1 |

---

### Index 1

Current Prefix

```
12
```

Need

```
22
```

Not present.

Store

```
12→1
```

Map

| Prefix | Frequency |
|--------|----------:|
| 0 | 1 |
| 10 | 1 |
| 12 | 1 |

---

### Index 2

Current Prefix

```
10
```

Need

```
20
```

Not present.

Store

```
10→2
```

Map

| Prefix | Frequency |
|--------|----------:|
| 0 | 1 |
| 10 | 2 |
| 12 | 1 |

Notice prefix sum **10** appeared twice.

---

### Index 3

Current Prefix

```
-10
```

Need

```
0
```

Map contains

```
0 → 1
```

So

```
count += 1
```

Count becomes

```
1
```

This corresponds to subarray

```
0...3
```

Store

```
-10→1
```

---

### Index 4

Current Prefix

```
0
```

Need

```
10
```

Map contains

```
10 → 2
```

So

```
count += 2
```

Count becomes

```
3
```

These correspond to

```
1...4

3...4
```

Final Answer

```
3
```

---

## IMPORTANT CODE SNIPPETS:

### Initialize map

```cpp
unordered_map<int,int> mp;
mp[0]=1;
```

### Update prefix sum

```cpp
prefixSum += arr[i];
```

### Find required prefix sum

```cpp
if(mp.find(prefixSum-k)!=mp.end())
    count += mp[prefixSum-k];
```

### Store current prefix

```cpp
mp[prefixSum]++;
```

---

## COMMON MISTAKES:

### 1. Forgetting

```cpp
mp[0]=1;
```

Then subarrays starting from index `0` are never counted.

---

### 2. Updating the map before checking

Wrong

```cpp
mp[prefixSum]++;

count += mp[prefixSum-k];
```

Correct

```cpp
count += mp[prefixSum-k];

mp[prefixSum]++;
```

The current prefix sum should only be available for **future** subarrays.

---

### 3. Using Sliding Window

Sliding Window works only when all numbers are positive.

This array contains negative numbers.

---

### 4. Using unordered_set

Wrong.

Need

```cpp
unordered_map<int,int>
```

because multiple identical prefix sums represent multiple valid starting points.

---

### 5. Forgetting that HashMap stores **frequency**, not indices

We don't need the starting index.

We only need **how many times** a prefix sum has occurred.

---

## WHY I MIGHT FORGET THIS:

Because the equation feels confusing.

Instead remember the story:

> I know my current prefix sum.

I ask:

> **"Which previous prefix sum should exist so that the remaining part equals k?"**

That required prefix is

```
Current Prefix - k
```

The HashMap simply tells me whether I've already seen it.

---

## INTERVIEW FLOW:

> The brute force solution checks every possible subarray in O(N²).

> We can optimize by storing prefix sums.

> Since

```
Subarray Sum = Current Prefix - Previous Prefix
```

we need

```
Previous Prefix = Current Prefix - k
```

While traversing the array, we store every prefix sum and its frequency in a HashMap.

For each index, if `(currentPrefix - k)` exists, every occurrence represents one valid subarray ending at the current index.

This gives an O(N) solution.

---

## TIME COMPLEXITY:

### Average Case

```
O(N)
```

Reason:

- Traverse array once → `O(N)`
- HashMap lookup → `O(1)` average
- HashMap insertion → `O(1)` average

Overall

```
O(N)
```

---

## SPACE COMPLEXITY:

```
O(N)
```

Worst case:

Every prefix sum is unique.

The HashMap stores all prefix sums.

---

## EDGE CASES:

### Subarray starts from index 0

```
arr = [2,3]

k = 5
```

Handled because

```cpp
mp[0]=1;
```

---

### Multiple zeros

```
arr = [0,0,0]

k = 0
```

Answer

```
6
```

Need frequency map.

---

### Negative numbers

```
[-1,-1,1]
```

Sliding Window fails.

Prefix Sum works.

---

### No valid subarray

```
[1,2,3]

k = 100
```

Answer

```
0
```

---

### Duplicate prefix sums

```
Prefix =

10

12

10

0
```

Frequency is essential because one current prefix can match multiple previous prefixes.

---

## PATTERN RECOGNITION:

Whenever you see:

- Contiguous **subarray**
- Sum equals **K**
- Array contains **negative numbers**
- Count subarrays
- Longest subarray with sum K
- Subarray sum divisible by K
- Equal 0s and 1s (after transformation)

Immediately think:

> **Prefix Sum + HashMap**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int cntSubarrays(vector<int> &arr, int k) {

        unordered_map<int, int> mp;

        mp[0] = 1;

        int prefixSum = 0;
        int count = 0;

        for (int num : arr) {

            prefixSum += num;

            if (mp.find(prefixSum - k) != mp.end())
                count += mp[prefixSum - k];

            mp[prefixSum]++;
        }

        return count;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
unordered_map<int,int> mp;
```

Stores:

```
Prefix Sum → Frequency
```

---

```cpp
mp[0]=1;
```

Before processing the array, the prefix sum is `0` once. This helps count subarrays starting from index `0`.

---

```cpp
prefixSum += num;
```

Continuously builds the running (prefix) sum.

---

```cpp
if(mp.find(prefixSum-k)!=mp.end())
```

Ask:

> **"Have I already seen the prefix sum I need?"**

The required prefix sum is:

```
Current Prefix - k
```

---

```cpp
count += mp[prefixSum-k];
```

Every occurrence of that prefix sum forms a different valid subarray ending at the current index.

---

```cpp
mp[prefixSum]++;
```

Store the current prefix sum so future indices can use it.

---

# Easy-to-Remember Summary

- **Pattern:** Prefix Sum + HashMap
- **Golden Equation:**

```
Subarray Sum = Current Prefix − Previous Prefix
```

- Rearrange:

```
Previous Prefix = Current Prefix − K
```

- Store **Prefix Sum → Frequency** in a HashMap.
- Initialize:

```cpp
mp[0] = 1;
```

- At every index:
  1. Update prefix sum.
  2. Check if `prefixSum - k` exists.
  3. Add its frequency to the answer.
  4. Store the current prefix sum.

- **Time:** `O(N)` (average)
- **Space:** `O(N)`
