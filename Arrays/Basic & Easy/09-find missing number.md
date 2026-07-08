
## PROBLEM:
You are given an array of size **n − 1** containing **distinct integers** from **1 to n**. Exactly **one number is missing**.

Return the missing number.

**Example:**

```text
Input : [1,2,3,5]
Output: 4
```

---

## PATTERN:

**Mathematical XOR (Missing Number Pattern)**

> Interview progression:
>
> **Brute Force → Hashing → Sum Formula → XOR**

---

## WHY THIS PATTERN:

There are three key observations:

1. Numbers belong to a fixed range **1 to n**.
2. Exactly **one number is missing**.
3. Every other number appears exactly once.

These properties allow us to replace searching with mathematical techniques.

The XOR approach is the most optimal because it provides:

- O(n) Time
- O(1) Space
- No overflow issues
- No extra memory

---

## CORE IDEA:

Think of the complete sequence and the given array.

```text
Complete

1 2 3 4 5

Array

1 2 3 5
```

Everything is the same except one number.

If every common number cancels itself,

only the missing number remains.

XOR provides exactly this cancellation.

```text
a ^ a = 0

0 ^ a = a
```

Therefore,

```text
(1^2^3^4^5)

^

(1^2^3^5)

=

4
```

---

# BRUTE FORCE

Since the optimal solution is not immediately obvious, interviewers usually expect progression.

---

## Approach 1 : Linear Search

### Intuition

Check every number from **1 to n**.

For each number, search the entire array.

If the number is not found, it is the missing number.

---

### Algorithm

```
For every number i from 1 to n

    Search the entire array

    If i is not found

        Return i
```

---

### Code

```cpp
int missingNum(vector<int>& arr) {

    int n = arr.size() + 1;

    for(int i=1;i<=n;i++){

        bool found=false;

        for(int num:arr){

            if(num==i){
                found=true;
                break;
            }

        }

        if(!found)
            return i;
    }

    return -1;
}
```

---

### Dry Run

```text
arr = [1,2,3,5]

n = 5

Check 1

Found

----------------

Check 2

Found

----------------

Check 3

Found

----------------

Check 4

Search

1

2

3

5

Not found

Return 4
```

---

### Time Complexity

Outer loop

```
O(n)
```

Inner loop

```
O(n)
```

Overall

```
O(n²)
```

---

### Space Complexity

```
O(1)
```

---

# Better Approach : Hashing

## Intuition

Instead of searching repeatedly,

remember which numbers exist.

Create a boolean array called

```
visited[]
```

Initially

```text
F F F F F
```

Traverse the array once and mark every number.

```text
arr

1 2 3 5

↓

visited

T T T F T
```

The remaining False index is the missing number.

---

### Algorithm

```
Create visited array

Traverse the array

Mark visited[arr[i]] = true

Traverse from 1 to n

Return the first index whose value is false
```

---

### Code

```cpp
int missingNum(vector<int>& arr) {

    int n = arr.size()+1;

    vector<bool> visited(n+1,false);

    for(int num:arr)
        visited[num]=true;

    for(int i=1;i<=n;i++){

        if(!visited[i])
            return i;
    }

    return -1;
}
```

---

### Dry Run

```text
visited

F F F F F

Read 1

T F F F F

Read 2

T T F F F

Read 3

T T T F F

Read 5

T T T F T

Only 4 is False

Return 4
```

---

### Time Complexity

```
O(n)
```

---

### Space Complexity

```
O(n)
```

---

# Better Mathematical Approach : Sum Formula

## Intuition

Instead of remembering numbers,

compare sums.

Expected numbers

```text
1 2 3 4 5
```

Expected Sum

```
15
```

Actual array

```text
1 2 3 5
```

Actual Sum

```
11
```

Difference

```
15 - 11 = 4
```

---

### Formula

```
Sum = n(n+1)/2
```

---

### Algorithm

```
Find Expected Sum

Find Actual Sum

Return

Expected Sum - Actual Sum
```

---

### Code

```cpp
int missingNum(vector<int>& arr) {

    int n = arr.size()+1;

    long long expected = 1LL*n*(n+1)/2;

    long long actual=0;

    for(int num:arr)
        actual+=num;

    return expected-actual;
}
```

---

### Dry Run

```text
n = 5

Expected

5×6/2

=

15

------------------

Actual

1+2+3+5

=

11

------------------

15-11

=

4
```

---

### Time Complexity

```
O(n)
```

---

### Space Complexity

```
O(1)
```

---

### Limitation

```
n*(n+1)
```

may overflow if stored in

```
int
```

Use

```cpp
long long
```

or

```cpp
1LL * n * (n+1)
```

This is why XOR is generally preferred.

---

# OPTIMAL APPROACH

## XOR Cancellation

Every duplicate disappears.

```
a ^ a = 0
```

Suppose

```text
Complete

1 2 3 4 5

Array

1 2 3 5
```

Take XOR

```text
1^2^3^4^5

^

1^2^3^5
```

Rearrange

```text
1^1

2^2

3^3

5^5

^4
```

Everything cancels.

Only

```
4
```

remains.

No overflow.

No extra memory.

---

## ALGORITHM

```
n = arr.size()+1

xorAns = 0

Step 1

XOR every number from 1 to n

Step 2

XOR every array element

Step 3

Return xorAns
```

---

## DRY RUN

Input

```text
arr

8 2 4 5 3 7 1
```

Original numbers

```text
1 2 3 4 5 6 7 8
```

Take XOR

```text
(1^2^3^4^5^6^7^8)

^

(8^2^4^5^3^7^1)
```

Rearrange

```text
1^1

2^2

3^3

4^4

5^5

7^7

8^8

^6
```

Everything cancels.

Remaining

```
6
```

Return

```
6
```

---

## IMPORTANT CODE SNIPPETS

### XOR Properties

```cpp
a ^ a = 0
```

```cpp
0 ^ a = a
```

---

### XOR Complete Range

```cpp
for(int i=1;i<=n;i++)
    xorAns ^= i;
```

---

### XOR Array

```cpp
for(int num:arr)
    xorAns ^= num;
```

---

### Return

```cpp
return xorAns;
```

---

## COMMON MISTAKES

### Mistake 1

```cpp
n = arr.size();
```

Correct

```cpp
n = arr.size()+1;
```

---

### Mistake 2

Loop

```cpp
i<n
```

instead of

```cpp
i<=n
```

---

### Mistake 3

Using

```cpp
int
```

for the Sum Formula.

Use

```cpp
long long
```

---

### Mistake 4

Thinking XOR means addition.

Remember

```
XOR ≠ Addition
```

It only cancels equal numbers.

---

## WHY I MIGHT FORGET THIS

Don't memorize "Use XOR."

Understand why it works.

```text
Complete

1 2 3 4 5

Array

1 2 3 5

↓

Pair every common number

↓

Everything disappears

↓

Only 4 remains.
```

Think:

> **XOR is an eraser. Every duplicate gets erased.**

---

## INTERVIEW FLOW

Start with

### Brute Force

Search every number.

```
O(n²)
```

↓

### Hashing

Store which numbers exist.

```
O(n)

Space O(n)
```

↓

### Sum Formula

Compare expected and actual sums.

```
O(n)

Space O(1)
```

Mention overflow.

↓

### XOR (Optimal)

Use cancellation.

```
a ^ a = 0
```

No overflow.

No extra memory.

```
O(n)

O(1)
```

---

## TIME COMPLEXITY

XOR Complete Range

```
O(n)
```

XOR Array

```
O(n)
```

Overall

```
O(n)
```

---

## SPACE COMPLEXITY

Only

```
n

xorAns
```

No extra array.

Therefore

```
O(1)
```

---

## EDGE CASES

### Single Element

```text
arr

1

Answer

2
```

---

### Missing First Number

```text
2 3 4 5

Answer

1
```

---

### Missing Last Number

```text
1 2 3 4

Answer

5
```

---

### Large n

XOR still works.

No overflow.

---

## PATTERN RECOGNITION

Whenever you notice:

- Numbers belong to a fixed range **1 to n**
- Exactly one element is missing
- All elements are distinct
- O(1) extra space is expected

Think in this order:

1. Brute Force
2. Hashing
3. Sum Formula
4. XOR (Optimal)

Similar XOR pattern appears in:

- Single Number
- Missing and Repeating Number
- Odd Occurring Element
- Two Unique Numbers

---

# Clean C++ Code

```cpp
class Solution {
public:
    int missingNum(vector<int>& arr) {

        int n = arr.size() + 1;
        int xorAns = 0;

        for (int i = 1; i <= n; i++)
            xorAns ^= i;

        for (int num : arr)
            xorAns ^= num;

        return xorAns;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int n = arr.size() + 1;
```

The original array contained `n` numbers, but one is missing.

---

```cpp
int xorAns = 0;
```

Start from `0` because `0 ^ x = x`.

---

```cpp
for(int i=1;i<=n;i++)
    xorAns ^= i;
```

XOR all numbers that should exist.

---

```cpp
for(int num:arr)
    xorAns ^= num;
```

Remove every number that actually exists.

Matching numbers cancel.

---

```cpp
return xorAns;
```

After cancellation, only the missing number remains.

---

# Easy-to-Remember Summary

```
Brute Force
↓
Search every number
O(n²)

↓

Hashing
↓
Remember which numbers exist
O(n)
Space O(n)

↓

Sum Formula
↓
Expected Sum − Actual Sum
O(n)
Space O(1)

↓

XOR (Best)
↓
Complete XOR ^ Array XOR
O(n)
Space O(1)
No Overflow
```

## Memory Trick

> **Complete Set XOR Given Set = Missing Number**

or simply

> **Duplicates disappear, missing survives.**
