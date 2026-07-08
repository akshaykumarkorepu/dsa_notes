
## PROBLEM:
Given an array `arr[]` containing positive, negative, and zero values, and an integer `k`, find the **length of the longest contiguous subarray** whose sum is exactly equal to `k`.

If no such subarray exists, return `0`.

---

## PATTERN:
**Prefix Sum + HashMap (Store First Occurrence)**

---

## WHY THIS PATTERN:

This problem asks for:

- A **contiguous subarray**
- Sum equals a given target `k`
- Array contains **negative numbers**

Whenever negative numbers are present:

- ❌ Sliding Window **does not work** because increasing/decreasing the window does not guarantee the sum moves in one direction.
- ✅ Prefix Sum lets us convert the subarray sum problem into a prefix lookup problem.

This makes the solution possible in **O(N)**.

---

## CORE IDEA:

Let

```
Prefix[i] = Sum of elements from index 0 to i
```

Suppose the current prefix sum is

```
currentPrefix
```

We want a subarray whose sum is `k`.

We know

```
Subarray Sum

=

Current Prefix

-

Previous Prefix
```

So,

```
Current Prefix - Previous Prefix = k

Previous Prefix = Current Prefix - k
```

Therefore,

At every index:

- Calculate the current prefix sum.
- Check whether `(currentPrefix - k)` has appeared before.
- If yes, we found a valid subarray.

To perform this lookup efficiently, we store prefix sums in a HashMap.

---

# BRUTE FORCE:

### Intuition

Generate every possible subarray.

For every starting index:

- Keep extending the ending index.
- Maintain a running sum.
- Whenever the running sum equals `k`, update the answer.

This approach helps us understand why optimization is needed.

---

### Code

```cpp
int longestSubarray(vector<int>& arr, int k)
{
    int n = arr.size();
    int ans = 0;

    for(int i=0;i<n;i++)
    {
        int sum=0;

        for(int j=i;j<n;j++)
        {
            sum+=arr[j];

            if(sum==k)
            {
                ans=max(ans,j-i+1);
            }
        }
    }

    return ans;
}
```

---

### Dry Run

```
arr = [10,5,2,7,1,-10]

k = 15
```

Start with

```
i=0

sum=0
```

```
10
```

Not equal.

```
10+5=15
```

Answer becomes

```
2
```

```
10+5+2=17
```

No.

```
10+5+2+7=24
```

No.

```
10+5+2+7+1=25
```

No.

```
10+5+2+7+1-10=15
```

Length

```
6
```

Answer

```
6
```

Repeat for every starting index.

Maximum answer remains

```
6
```

---

### Time Complexity

```
O(N²)
```

Reason:

Outer loop chooses every starting index.

Inner loop extends every ending index.

---

### Space Complexity

```
O(1)
```

---

## OPTIMAL APPROACH:

Instead of calculating subarray sums repeatedly,

store

```
Prefix Sum
```

At every index

```
Current Prefix = Sum from index 0 to current index
```

Now ask

```
Has

Current Prefix - k

appeared before?
```

If yes,

then the elements after that previous prefix form a subarray whose sum is exactly `k`.

Use a HashMap

```
Prefix Sum → First Occurrence Index
```

to perform this lookup in

```
O(1)
```

---

## ALGORITHM:

1. Initialize

```
prefix = 0
```

2. Create a HashMap

```
Prefix Sum → First Index
```

3. Traverse the array

4. Update

```
prefix += arr[i]
```

5. If

```
prefix == k
```

then

```
answer = i+1
```

6. Check whether

```
prefix-k
```

exists inside the HashMap.

If yes

```
length

=

i - storedIndex
```

Update answer.

7. Store current prefix sum only if it has never appeared before.

8. Return answer.

---

## DRY RUN:

Input

```
arr=[10,5,2,7,1,-10]

k=15
```

Initially

```
prefix=0

answer=0

map={}
```

|Index|Element|Prefix|Need (Prefix-k)|Found?|Answer|Map|
|------|-------|------|---------------|------|------|---|
|0|10|10|-5|No|0|10→0|
|1|5|15|0|No|2 (prefix==k)|10→0,15→1|
|2|2|17|2|No|2|17→2|
|3|7|24|9|No|2|24→3|
|4|1|25|10|Yes (index 0)|4|25→4|
|5|-10|15|0|No|6 (prefix==k)|Do not overwrite 15|

Final Answer

```
6
```

---

## IMPORTANT CODE SNIPPETS:

### Update Prefix

```cpp
prefix += arr[i];
```

---

### Entire Prefix Equals k

```cpp
if(prefix==k)
{
    ans=i+1;
}
```

---

### Find Previous Prefix

```cpp
if(mp.find(prefix-k)!=mp.end())
{
    ans=max(ans,
            i-mp[prefix-k]);
}
```

---

### Store Only First Occurrence

```cpp
if(mp.find(prefix)==mp.end())
{
    mp[prefix]=i;
}
```

This is the most important line in the solution.

---

## COMMON MISTAKES:

### Mistake 1

Overwriting prefix sums.

Wrong

```cpp
mp[prefix]=i;
```

Correct

```cpp
if(mp.find(prefix)==mp.end())
{
    mp[prefix]=i;
}
```

---

### Mistake 2

Forgetting

```cpp
if(prefix==k)
```

Example

```
[5]

k=5
```

Answer should be

```
1
```

---

### Mistake 3

Using Sliding Window.

Sliding Window only works when all numbers are positive.

Negative numbers break the window property.

---

### Mistake 4

Writing

```
k-prefix
```

instead of

```
prefix-k
```

---

### Mistake 5

Using `int` prefix for larger constraints.

Safer implementation

```cpp
long long prefix=0;
```

---

## WHY I MIGHT FORGET THIS:

Usually people remember

```
Prefix Sum
```

but forget

```
Store ONLY the first occurrence.
```

Reason

Earlier index

↓

Longer subarray.

Also remember

```
Need

Prefix-k

NOT

k-Prefix
```

---

## INTERVIEW FLOW:

### Step 1

Start with brute force.

Generate every possible subarray.

Maintain a running sum.

Whenever the sum becomes `k`, update the answer.

Time Complexity

```
O(N²)
```

Space Complexity

```
O(1)
```

---

### Step 2

Observation

We're recomputing almost the same sums repeatedly.

Can we reuse previous calculations?

---

### Step 3

Introduce Prefix Sum.

Instead of calculating every subarray sum,

store

```
Sum from index 0 till current index.
```

---

### Step 4

Derive

```
Subarray Sum

=

Current Prefix

-

Previous Prefix
```

Therefore

```
Previous Prefix

=

Current Prefix-k
```

---

### Step 5

Need fast lookup.

Store

```
Prefix Sum → First Index
```

inside a HashMap.

---

### Step 6

Store only the first occurrence.

Earlier occurrence always produces the longest subarray.

---

### Step 7

Traverse once.

Update prefix.

Check

```
prefix==k
```

Check

```
prefix-k
```

Store prefix.

Return answer.

---

## TIME COMPLEXITY:

### Brute Force

```
O(N²)
```

Reason

Every starting index is paired with every possible ending index.

---

### Optimal

```
O(N)
```

Reason

Each element is processed exactly once.

HashMap insertion and lookup are

```
O(1)
```

on average.

---

## SPACE COMPLEXITY:

### Brute Force

```
O(1)
```

---

### Optimal

```
O(N)
```

Reason

In the worst case,

every prefix sum is unique,

so the HashMap stores

```
N
```

entries.

---

## EDGE CASES:

### Single Element

```
[5]

k=5
```

Answer

```
1
```

---

### No Valid Subarray

```
1 2 3

k=10
```

Answer

```
0
```

---

### Entire Array

```
10 5 2 7 1 -10

k=15
```

Answer

```
6
```

---

### Negative Numbers

Works correctly.

---

### Multiple Same Prefix Sums

Always keep the first occurrence.

---

### All Zeros

```
0 0 0

k=0
```

Answer

```
3
```

---

## PATTERN RECOGNITION:

Whenever you see:

- Longest Subarray with Sum K
- Count Subarrays with Sum K
- Equal number of 0s and 1s
- Longest Subarray with Equal 0s and 1s
- Longest Subarray with Given XOR
- Subarray problems involving negative numbers

Immediately think

```
Prefix Sum + HashMap
```

If the array contains only positive numbers,

also consider

```
Sliding Window
```

Otherwise,

Prefix Sum is the correct pattern.

---

# Clean C++ Code

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& arr, int k) {

        unordered_map<long long,int> mp;

        long long prefix = 0;

        int ans = 0;

        for(int i=0;i<arr.size();i++)
        {
            // Update current prefix sum
            prefix += arr[i];

            // Subarray starts from index 0
            if(prefix==k)
            {
                ans=i+1;
            }

            // Check if required prefix exists
            if(mp.find(prefix-k)!=mp.end())
            {
                ans=max(ans,
                        i-mp[prefix-k]);
            }

            // Store only first occurrence
            if(mp.find(prefix)==mp.end())
            {
                mp[prefix]=i;
            }
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### HashMap

```cpp
unordered_map<long long,int> mp;
```

Stores

```
Prefix Sum → First Occurrence Index
```

---

### Running Prefix

```cpp
long long prefix=0;
```

Maintains the cumulative sum from index `0` to the current index.

---

### Update Prefix

```cpp
prefix+=arr[i];
```

Add the current element to the running sum.

---

### Prefix Equals Target

```cpp
if(prefix==k)
```

If the entire prefix sums to `k`, the valid subarray starts from index `0`.

---

### Find Required Prefix

```cpp
mp.find(prefix-k)
```

Checks whether a previous prefix sum exists that can form a subarray with sum `k`.

---

### Update Maximum Length

```cpp
ans=max(ans,
        i-mp[prefix-k]);
```

Calculate the current subarray length and update the answer.

---

### Store First Occurrence

```cpp
if(mp.find(prefix)==mp.end())
```

Store only the first occurrence because it always produces the longest subarray.

---

# Easy-to-Remember Summary

✅ Array contains negative numbers?

→ Think **Prefix Sum + HashMap**

Remember these four steps:

1. Update Prefix Sum.
2. If `prefix == k`, update answer.
3. Look for `prefix - k`.
4. Store the prefix only if it's the first occurrence.

### One-Line Memory Trick

> **Current Prefix → Need (Prefix − K) → First Occurrence gives the Longest Subarray.**
