
# Rotate Array by One (Clockwise)

---

## PROBLEM

Given an array, rotate it by **one position clockwise**.

### Example

```text
Input : [1, 2, 3, 4, 5]
Output: [5, 1, 2, 3, 4]
```

**Observation**

- The last element moves to the front.
- Every other element shifts one position to the right.

---

## PATTERN

**Array Element Shifting (In-place Rotation)**

### Trigger

> "Move elements left/right by a fixed amount while preserving their relative order."

---

## WHY THIS PATTERN?

Only **one rotation** is required.

Since every element simply shifts one position:

- No reversal algorithm is needed.
- No cyclic replacement is needed.
- No extra array is required.

The simplest approach is:

1. Save the last element.
2. Shift all elements one position to the right.
3. Place the saved element at index `0`.

---

## CORE IDEA

Before shifting:

- Save the last element because it will be overwritten.

During shifting:

- Traverse **from right to left**.
- Copy each element into its next position.

After shifting:

- Place the saved last element at the beginning.

---

# BRUTE FORCE

### Is brute force necessary?

**No.**

The in-place solution is already the simplest and most efficient.

Using another array would only increase space complexity without simplifying the logic.

---

# OPTIMAL APPROACH

### Idea

1. Store the last element.
2. Shift every element one position to the right.
3. Place the stored element at index `0`.

---

# ALGORITHM

```text
Store last = arr[n-1]

For i = n-1 down to 1
    arr[i] = arr[i-1]

arr[0] = last
```

---

# DRY RUN

### Example

```text
arr = [1, 2, 3, 4, 5]
```

### Step 1

Save the last element.

```text
last = 5
```

Current array

```text
[1, 2, 3, 4, 5]
```

---

### Step 2

Shift elements from right to left.

#### i = 4

```text
arr[4] = arr[3]

Before:
[1, 2, 3, 4, 5]

After:
[1, 2, 3, 4, 4]
```

---

#### i = 3

```text
arr[3] = arr[2]

Before:
[1, 2, 3, 4, 4]

After:
[1, 2, 3, 3, 4]
```

---

#### i = 2

```text
arr[2] = arr[1]

Before:
[1, 2, 3, 3, 4]

After:
[1, 2, 2, 3, 4]
```

---

#### i = 1

```text
arr[1] = arr[0]

Before:
[1, 2, 2, 3, 4]

After:
[1, 1, 2, 3, 4]
```

---

### Step 3

Place the saved last element at the front.

```text
arr[0] = last

Before:
[1, 1, 2, 3, 4]

After:
[5, 1, 2, 3, 4]
```

---

### Final Output

```text
[5, 1, 2, 3, 4]
```

---

# IMPORTANT CODE SNIPPETS

### Save the last element

```cpp
int last = arr[n - 1];
```

---

### Shift elements from right to left

```cpp
for (int i = n - 1; i > 0; i--)
{
    arr[i] = arr[i - 1];
}
```

---

### Place the saved element at the beginning

```cpp
arr[0] = last;
```

---

# COMMON MISTAKES

## 1. Traversing from left to right

❌ Wrong

```cpp
for (int i = 0; i < n - 1; i++)
{
    arr[i + 1] = arr[i];
}
```

Why?

Values get overwritten before they can be copied.

---

## 2. Forgetting to save the last element

If you don't save it first:

```text
[1,2,3,4,5]

↓

Shift

↓

[1,1,2,3,4]
```

The original `5` is permanently lost.

---

## 3. Wrong loop condition

✅ Correct

```cpp
i > 0
```

❌ Wrong

```cpp
i >= 0
```

Because

```cpp
arr[i - 1]
```

becomes

```cpp
arr[-1]
```

which is invalid.

---

# WHY I MIGHT FORGET THIS

Many people naturally begin shifting from the front.

Remember this rule:

> **Whenever shifting right, always traverse from right to left.**

Otherwise you'll overwrite values that haven't been copied yet.

---

# INTERVIEW FLOW

> We need to rotate the array by one position clockwise. The last element should become the first while every other element shifts one position to the right. I first save the last element because it will otherwise be overwritten during shifting. Then I traverse from right to left and shift every element one position ahead. Finally, I place the saved last element at index 0. This performs the rotation in-place using constant extra space.

---

# TIME COMPLEXITY

### O(n)

### Reason

- The loop runs `n - 1` times.
- Every element is moved exactly once.

---

# SPACE COMPLEXITY

### O(1)

### Reason

Only one extra variable (`last`) is used.

---

# EDGE CASES

### Single Element

```text
Input : [5]
Output: [5]
```

---

### Two Elements

```text
Input : [1,2]
Output: [2,1]
```

---

### All Elements Same

```text
Input : [7,7,7]
Output: [7,7,7]
```

---

### Large Array

Still runs in

```text
O(n)
```

---

# PATTERN RECOGNITION

Whenever you see:

- Rotate array by one
- Shift elements right
- Shift elements left
- Move last to front
- Move first to end

Immediately think:

> **Save the boundary element → Shift in the correct direction → Restore the saved element**

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    void rotate(vector<int> &arr) {
        int n = arr.size();

        int last = arr[n - 1];

        for (int i = n - 1; i > 0; i--) {
            arr[i] = arr[i - 1];
        }

        arr[0] = last;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Store the array size

```cpp
int n = arr.size();
```

- Saves the array length.
- Avoids calling `size()` repeatedly.

---

### Save the last element

```cpp
int last = arr[n - 1];
```

- The last element will be overwritten during shifting.
- Save it before starting.

---

### Traverse from right to left

```cpp
for (int i = n - 1; i > 0; i--)
```

- Moving backwards prevents overwriting values that are still needed.

---

### Shift every element

```cpp
arr[i] = arr[i - 1];
```

- Copy the previous element into the current position.
- This shifts the array one position to the right.

---

### Restore the saved element

```cpp
arr[0] = last;
```

- Places the original last element at the beginning.
- Rotation is complete.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Array Element Shifting
- **Trigger:** Rotate or shift an array by one position.
- **Core Idea:** Save last → Shift right (back to front) → Put saved element at the front.
- **Traversal Rule:** Right shift ⇒ Traverse **right to left**.
- **Time Complexity:** **O(n)**
- **Space Complexity:** **O(1)**

### Golden Rule

> **Always save the element that will be overwritten before you start shifting.**
````
