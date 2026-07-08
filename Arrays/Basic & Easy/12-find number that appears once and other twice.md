
## PROBLEM:
Given an **unsorted array** where **every element appears exactly twice except one element which appears only once**, find the unique element.

Example:
```
Input : [2, 30, 2, 15, 20, 30, 15]
Output: 20
```

---

## PATTERN:
**Bit Manipulation → XOR (Pair Cancellation Pattern)**

---

## WHY THIS PATTERN:

This problem has a very strong interview trigger:

- Every element appears **exactly twice**
- Only **one element appears once**
- Need **O(n)** time and **O(1)** extra space

Whenever you see this combination, think of **XOR**.

Reason:
- Duplicate numbers cancel themselves.
- Only the unique number survives.

---

## CORE IDEA:

The XOR operator has three important properties:

```
a ^ a = 0
a ^ 0 = a
XOR is commutative and associative
```

Therefore,

```
1 ^ 2 ^ 1 ^ 5 ^ 5

=
(1 ^ 1) ^ (5 ^ 5) ^ 2

=
0 ^ 0 ^ 2

=
2
```

Every duplicate disappears automatically.

---

## BRUTE FORCE:

### Idea

For every element,
count how many times it appears.

If frequency becomes 1,
return it.

### Code

```cpp
int findUnique(vector<int>& arr) {

    int n = arr.size();

    for(int i = 0; i < n; i++) {

        int count = 0;

        for(int j = 0; j < n; j++) {

            if(arr[i] == arr[j])
                count++;
        }

        if(count == 1)
            return arr[i];
    }

    return -1;
}
```

### Dry Run

```
arr = [1,2,1,5,5]

Check 1
Frequency = 2

Check 2
Frequency = 1

Return 2
```

### Time Complexity

```
O(n²)
```

Reason:
For every element, we scan the whole array.

### Space Complexity

```
O(1)
```

---

## OPTIMAL APPROACH:

Use **XOR** of all elements.

Every duplicate pair becomes zero.

Only the unique element remains.

---

## ALGORITHM:

1. Initialize `ans = 0`.
2. Traverse the array.
3. XOR every element with `ans`.
4. Return `ans`.

---

## DRY RUN:

Example:

```
arr = [2,30,2,15,20,30,15]
```

Initially

```
ans = 0
```

### Step 1

```
ans = 0 ^ 2

= 2
```

---

### Step 2

```
ans = 2 ^ 30

= 28
```

---

### Step 3

```
ans = 28 ^ 2

= 30
```

The two **2's** have cancelled.

---

### Step 4

```
ans = 30 ^ 15

= 17
```

---

### Step 5

```
ans = 17 ^ 20

= 5
```

---

### Step 6

```
ans = 5 ^ 30

= 27
```

The two **30's** have cancelled.

---

### Step 7

```
ans = 27 ^ 15

= 20
```

The two **15's** have cancelled.

Final answer

```
20
```

---

## IMPORTANT CODE SNIPPETS:

### XOR Properties

```
a ^ a = 0

a ^ 0 = a

XOR is commutative

a ^ b = b ^ a

XOR is associative

(a ^ b) ^ c

=

a ^ (b ^ c)
```

---

### Optimal Logic

```cpp
int ans = 0;

for(int num : arr)
    ans ^= num;

return ans;
```

---

## COMMON MISTAKES:

### Mistake 1

Using OR instead of XOR.

Wrong

```cpp
ans |= num;
```

Correct

```cpp
ans ^= num;
```

---

### Mistake 2

Using HashMap immediately.

Works,

but interviewer expects

```
O(1)
```

space.

---

### Mistake 3

Sorting first.

Sorting works but becomes

```
O(n log n)
```

which is unnecessary.

---

### Mistake 4

Thinking intermediate XOR values should make sense.

Example

```
2 ^ 30 = 28
```

This intermediate value has no direct meaning.

Only the final XOR matters.

---

## WHY I MIGHT FORGET THIS:

Because XOR looks like a mathematical trick.

Remember this one sentence:

> **Whenever every element appears twice except one, XOR removes duplicate pairs automatically.**

That is enough to identify this pattern.

---

## INTERVIEW FLOW:

**Brute Force**

- Count frequency of every element.
- Return the one with frequency 1.
- Time: O(n²)

↓

**Better**

- Use HashMap to store frequencies.
- Time: O(n)
- Space: O(n)

↓

**Optimal**

- Every duplicate appears exactly twice.
- XOR cancels duplicate numbers because

```
a ^ a = 0
```

- Traverse once while maintaining running XOR.
- Return final XOR.

---

## TIME COMPLEXITY:

### Brute Force

```
O(n²)
```

Reason:

Nested loops.

---

### Optimal

```
O(n)
```

Reason:

Single traversal.

Each XOR operation takes constant time.

---

## SPACE COMPLEXITY:

### Brute Force

```
O(1)
```

---

### Optimal

```
O(1)
```

Reason:

Only one integer variable is used.

No extra data structure.

---

## EDGE CASES:

### Single element

```
[7]

Answer = 7
```

---

### Unique at beginning

```
[9,2,2,3,3]

Answer = 9
```

---

### Unique at end

```
[4,4,8]

Answer = 8
```

---

### Unique element is zero

```
[0,1,1]

Answer = 0
```

Works because

```
0 ^ 0 = 0
```

---

### Large numbers

```
10^9
```

Still works because XOR is a bitwise operation.

---

## PATTERN RECOGNITION:

Immediately think of XOR whenever you read:

- Every element appears exactly twice except one.
- Find the single/unique number.
- Remove duplicate pairs.
- Expected O(n) time.
- Expected O(1) extra space.
- Bit Manipulation tag.

Similar Interview Problems:

- Single Number
- Missing Number (XOR Variation)
- Find Odd Occurring Element
- Unique Number

---

# Clean C++ Code

```cpp
class Solution {
public:
    int findUnique(vector<int>& arr) {

        int ans = 0;

        for(int num : arr) {
            ans ^= num;
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### Line 1

```cpp
int ans = 0;
```

Start with zero because

```
0 ^ x = x
```

Zero acts as the identity element for XOR.

---

### Line 2

```cpp
for(int num : arr)
```

Visit every element exactly once.

---

### Line 3

```cpp
ans ^= num;
```

Maintain the XOR of all elements seen so far.

- First occurrence of a number stores it.
- Second occurrence cancels it because

```
a ^ a = 0
```

---

### Line 4

```cpp
return ans;
```

After every duplicate has cancelled itself,

only the unique element remains.

---

# Easy-to-Remember Summary

### Pattern

**Bit Manipulation → XOR Pair Cancellation**

### Trigger

> Every element appears twice except one.

### Key Property

```
a ^ a = 0
a ^ 0 = a
```

### Algorithm

```
ans = 0

For every number

    ans ^= number

Return ans
```

### Complexity

```
Time  : O(n)

Space : O(1)
```

### Memory Trick

> **Pairs vanish, loner survives.**
