
## PROBLEM:
Given a binary array `nums[]` containing only `0`s and `1`s, return the **maximum number of consecutive `1`s** present in the array.

Example:

```text
Input : [1,1,0,0,1,1,1,0]

Output : 3
```

Explanation:

```text
1 1 0 0 1 1 1 0
        └─────┘
    3 consecutive 1's
```

---

# PATTERN:

**Linear Traversal + Running Count**

---

# WHY THIS PATTERN:

The question asks for the **longest continuous occurrence of one specific value (1)**.

Instead of comparing every element with every other element, we only need to know:

> **"Is the current element a 1?"**

- If yes → continue counting.
- If no → the streak ends.

Since each element is processed only once, a single traversal is sufficient.

---

# CORE IDEA:

Maintain

- `currentCount` → current consecutive 1's
- `maxCount` → maximum consecutive 1's seen so far

Whenever we encounter

- `1` → extend the streak.
- `0` → reset the streak.

Update the maximum after every element.

---

# BRUTE FORCE:

## Approach

The brute force idea is:

For every index,

- if it contains `1`,
- count how many consecutive `1`s continue from that position,
- keep the maximum.

### Example

```text
1 1 0 0 1 1 1 0
```

Start counting from

```text
Index 0 → 2

Index 1 → 1

Index 4 → 3

Index 5 → 2

Index 6 → 1
```

Maximum = **3**

### Brute Force Code

```cpp
int maxCount = 0;

for(int i=0;i<n;i++)
{
    if(nums[i]==1)
    {
        int count=0;

        for(int j=i;j<n && nums[j]==1;j++)
            count++;

        maxCount=max(maxCount,count);
    }
}

return maxCount;
```

### Dry Run

Input

```text
1 1 0 0 1 1 1 0
```

### i = 0

Count

```text
1

↓

2

↓

Stop at 0
```

Maximum

```text
2
```

---

### i = 4

Count

```text
1

↓

2

↓

3

↓

Stop at 0
```

Maximum

```text
3
```

Remaining indices never exceed 3.

Answer

```text
3
```

### Time Complexity

```text
O(N²)
```

Worst case:

```text
1 1 1 1 1 1
```

Every starting index scans almost the remaining array.

### Space Complexity

```text
O(1)
```

---

# OPTIMAL APPROACH:

## Approach

The brute force repeatedly counts the same streak.

Example

```text
1 1 1 1
```

Starting from

```text
Index 0

↓

Index 1

↓

Index 2
```

we repeatedly count the same consecutive 1's.

Instead,

scan the array only once.

At every element ask

> **Is the current element equal to 1?**

If yes

→ continue the streak.

If no

→ streak ends.

Maintain the maximum streak throughout the traversal.

---

# ALGORITHM:

1. Initialize

```text
currentCount = 0

maxCount = 0
```

2. Traverse the array.

3. If

```text
nums[i] == 1
```

increase the current streak.

4. Otherwise,

reset the streak to 0.

5. Update the maximum after every iteration.

6. Return the answer.

---

# DRY RUN:

Input

```text
nums = [1,1,0,0,1,1,1,0]
```

Initially

```text
currentCount = 0

maxCount = 0
```

---

### i = 0

Current

```text
1
```

Increment

```text
currentCount = 1
```

Update

```text
maxCount = 1
```

---

### i = 1

Current

```text
1
```

Increment

```text
currentCount = 2
```

Update

```text
maxCount = 2
```

---

### i = 2

Current

```text
0
```

Reset

```text
currentCount = 0
```

Maximum remains

```text
2
```

---

### i = 3

Current

```text
0
```

Reset

```text
currentCount = 0
```

---

### i = 4

Current

```text
1
```

Increment

```text
currentCount = 1
```

---

### i = 5

Increment

```text
currentCount = 2
```

---

### i = 6

Increment

```text
currentCount = 3
```

Update

```text
maxCount = 3
```

---

### i = 7

Current

```text
0
```

Reset

```text
currentCount = 0
```

Answer

```text
3
```

---

# IMPORTANT CODE SNIPPETS:

### Extend the streak

```cpp
if(num == 1)
    currentCount++;
```

---

### Reset the streak

```cpp
else
    currentCount = 0;
```

---

### Update answer

```cpp
maxCount = max(maxCount,currentCount);
```

---

# COMMON MISTAKES:

### Mistake 1

Initializing

```cpp
currentCount = 1;
```

Wrong.

If the array starts with

```text
0
```

the answer becomes incorrect.

Correct initialization

```cpp
currentCount = 0;
maxCount = 0;
```

---

### Mistake 2

Forgetting to update

```cpp
maxCount
```

after incrementing the streak.

---

### Mistake 3

Comparing

```cpp
nums[i] == nums[i-1]
```

This is a different problem.

Here we only care about

```cpp
nums[i] == 1
```

---

# WHY I MIGHT FORGET THIS:

This problem looks very similar to

**Maximum Consecutive Bit**, but the condition is different.

There,

we compare with the previous element.

Here,

we compare with the target value

```cpp
== 1
```

because only 1's contribute to the answer.

---

# INTERVIEW FLOW:

> "A brute force solution counts consecutive 1's starting from every index containing a 1, but this repeatedly scans the same streaks. Instead, I traverse the array once while maintaining the current streak of 1's. If I encounter a 1, I increment the streak; otherwise, I reset it to 0. I continuously update the maximum streak. This gives an O(N) solution with O(1) extra space."

---

# TIME COMPLEXITY:

### Brute Force

```text
O(N²)
```

Reason:

Each starting position may scan almost the remaining array.

---

### Optimal

```text
O(N)
```

Reason:

Each element is visited exactly once.

---

# SPACE COMPLEXITY:

### Brute Force

```text
O(1)
```

---

### Optimal

```text
O(1)
```

Only two variables are maintained.

---

# EDGE CASES:

### All zeros

```text
0 0 0 0

Answer = 0
```

---

### All ones

```text
1 1 1 1

Answer = 4
```

---

### Single element

```text
[1]

Answer = 1
```

```text
[0]

Answer = 0
```

---

### Alternating bits

```text
1 0 1 0 1

Answer = 1
```

---

### Long streak at end

```text
0 0 1 1 1

Answer = 3
```

---

### Long streak at beginning

```text
1 1 1 0 0

Answer = 3
```

---

# PATTERN RECOGNITION:

Whenever the question contains

- maximum consecutive 1's
- longest run of 1's
- continuous occurrence of one value
- longest streak of a target element

immediately think

> **Linear Traversal + Running Count**

General template

```cpp
current = 0;
maximum = 0;

for(int num : nums)
{
    if(num == target)
        current++;
    else
        current = 0;

    maximum = max(maximum,current);
}
```

This same pattern applies to

- Maximum consecutive 0's
- Longest consecutive vowels
- Longest consecutive even numbers
- Longest consecutive positive numbers
- Longest streak of any specific target value

---

# Clean C++ Code

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {

        int currentCount = 0;
        int maxCount = 0;

        for(int num : nums)
        {
            if(num == 1)
            {
                currentCount++;
            }
            else
            {
                currentCount = 0;
            }

            maxCount = max(maxCount, currentCount);
        }

        return maxCount;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int currentCount = 0;
```

Initially, no consecutive 1's have been seen.

---

```cpp
int maxCount = 0;
```

If the array contains no 1's, the correct answer should remain 0.

---

```cpp
for(int num : nums)
```

Traverse every element exactly once.

---

```cpp
if(num == 1)
```

Only 1's contribute to the streak.

---

```cpp
currentCount++;
```

Extend the current streak of 1's.

---

```cpp
currentCount = 0;
```

A 0 breaks the streak completely.

---

```cpp
maxCount = max(maxCount,currentCount);
```

Store the longest streak seen so far.

---

```cpp
return maxCount;
```

Return the maximum consecutive 1's.

---

# EASY-TO-REMEMBER SUMMARY

Think of carrying a **counter of consecutive 1's** while walking through the array.

- **1** → Extend the streak.
- **0** → Break the streak and reset to 0.
- After every element → Update the maximum.

### One-line Memory Trick

> **"Target found → Increase count. Target missing → Reset count. Keep the maximum."**
