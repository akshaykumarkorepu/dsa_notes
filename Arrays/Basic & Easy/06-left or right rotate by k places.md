
## PROBLEM

Given an array `arr[]`, rotate it **to the left (counter-clockwise)** by `d` positions **in-place**.

**Example**

```text
Input:
arr = [1,2,3,4,5]
d = 2

Output:
[3,4,5,1,2]
```

---

# PATTERN

**Array Reversal Pattern (Three-Reversal Algorithm)**

---

# WHY THIS PATTERN

Whenever a question says:

- Rotate an array
- Do it **in-place**
- Use **constant extra space**

Immediately think of the **Three-Reversal Technique**.

Instead of shifting elements one by one, we rearrange the array by reversing different parts.

---

# CORE IDEA

Suppose the array is divided into two parts.

```text
A = First d elements
B = Remaining elements

Original

[A | B]

Need

[B | A]
```

Instead of moving elements individually:

1. Reverse A
2. Reverse B
3. Reverse the entire array

This automatically becomes

```text
[B | A]
```

---

# BRUTE FORCE

## Intuition

Store the first `d` elements temporarily.

Shift the remaining elements to the left.

Finally place the stored elements at the end.

This solution is intuitive and is often explained before moving to the optimal solution.

---

## Algorithm

1. Store first `d` elements in a temporary vector.
2. Shift elements from index `d` onward to the front.
3. Copy the stored elements at the end.

---

## Dry Run

```text
arr = [1,2,3,4,5]
d = 2

Store
temp = [1,2]

Shift remaining left

[3,4,5,_,_]

Copy temp

[3,4,5,1,2]
```

---

## Brute Force Code

```cpp
class Solution {
public:
    void rotateArr(vector<int>& arr, int d) {

        int n = arr.size();

        d = d % n;

        vector<int> temp;

        // Store first d elements
        for(int i = 0; i < d; i++)
            temp.push_back(arr[i]);

        // Shift remaining elements left
        for(int i = d; i < n; i++)
            arr[i - d] = arr[i];

        // Copy stored elements
        for(int i = 0; i < d; i++)
            arr[n - d + i] = temp[i];
    }
};
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(d)
```

---

# OPTIMAL APPROACH

## Three-Reversal Algorithm

Instead of using extra memory, rearrange the array by reversing portions of it.

---

# ALGORITHM

Suppose

```text
arr = [1 2 | 3 4 5]

A = [1 2]
B = [3 4 5]
```

### Step 1

Reverse first d elements.

```text
2 1 | 3 4 5
```

---

### Step 2

Reverse remaining elements.

```text
2 1 | 5 4 3
```

---

### Step 3

Reverse the entire array.

```text
3 4 5 1 2
```

Done.

---

# DRY RUN

Example

```text
arr = [1,2,3,4,5]
d = 2
```

Initially

```text
1 2 | 3 4 5
```

Reverse first d

```text
2 1 | 3 4 5
```

Reverse remaining

```text
2 1 | 5 4 3
```

Reverse whole array

```text
3 4 5 1 2
```

Answer obtained.

---

Another Example

```text
arr = [7,3,9,1]
d = 9

n = 4

d = 9 % 4 = 1
```

Reverse first part

```text
7 | 3 9 1
```

Reverse second part

```text
7 | 1 9 3
```

Reverse whole

```text
3 9 1 7
```

Correct.

---

# IMPORTANT OBSERVATIONS

- Rotating by `n` positions gives the same array.
- Rotating by `n+k` is equivalent to rotating by `k`.
- Therefore always write

```cpp
d %= n;
```

before doing anything.

---

# IMPORTANT CODE SNIPPETS

### Reduce unnecessary rotations

```cpp
d %= n;
```

---

### Reverse first block

```cpp
reverse(arr.begin(), arr.begin() + d);
```

---

### Reverse second block

```cpp
reverse(arr.begin() + d, arr.end());
```

---

### Reverse whole array

```cpp
reverse(arr.begin(), arr.end());
```

---

# COMMON MISTAKES

### 1. Forgetting

```cpp
d %= n;
```

When `d > n`, indices become incorrect.

---

### 2. Wrong reverse range

Wrong

```cpp
reverse(arr.begin(), arr.begin()+d-1);
```

Correct

```cpp
reverse(arr.begin(), arr.begin()+d);
```

`reverse()` excludes the ending iterator automatically.

---

### 3. Mixing Left Rotation and Right Rotation

The order of reversals changes.

---

### 4. Forgetting edge cases

- d = 0
- d = n
- n = 1

---

# WHY I MIGHT FORGET THIS

The three reversals don't look obvious.

Remember only this:

```text
Need

A B

↓

B A

Formula

Reverse A

Reverse B

Reverse Whole
```

Just memorize

> Reverse → Reverse → Reverse

---

# INTERVIEW FLOW

**If the interviewer asks this question:**

"I need to rotate an array left by d positions in-place."

Explain like this:

> First, I'll normalize the number of rotations using `d %= n` because rotating more than the array length repeats the same pattern.

> A straightforward solution is to store the first `d` elements, shift the remaining elements left, and copy the stored elements to the end. This takes O(n) time and O(d) extra space.

> Since the question asks for an in-place solution, I'll use the Three-Reversal Algorithm. I reverse the first `d` elements, reverse the remaining elements, and finally reverse the entire array. This gives the required rotation in O(n) time and O(1) extra space.

---

# TIME COMPLEXITY

## Brute Force

Store elements

```
O(d)
```

Shift remaining

```
O(n-d)
```

Copy back

```
O(d)
```

Overall

```
O(n)
```

---

## Optimal

Three reversals together visit each element only a constant number of times.

```
O(n)
```

---

# SPACE COMPLEXITY

## Brute Force

Temporary vector

```
O(d)
```

---

## Optimal

Only a few variables are used.

```
O(1)
```

---

# EDGE CASES

### d > n

```cpp
d %= n;
```

---

### d == 0

No rotation.

---

### d == n

Same array.

---

### n == 1

No change.

---

### Empty array (if allowed)

Check before taking modulo.

---

# PATTERN RECOGNITION

Whenever you see:

- Rotate array
- Circular movement
- In-place rotation
- Constant extra space
- Left rotation
- Right rotation

Immediately think

> **Three-Reversal Pattern**

---

# CLEAN C++ CODE (Optimal)

```cpp
class Solution {
public:
    void rotateArr(vector<int>& arr, int d) {

        int n = arr.size();

        d = d % n;

        reverse(arr.begin(), arr.begin() + d);

        reverse(arr.begin() + d, arr.end());

        reverse(arr.begin(), arr.end());
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Find array size

```cpp
int n = arr.size();
```

Needed for indexing and modulo.

---

### Normalize rotations

```cpp
d %= n;
```

Example

```
Rotate 12 times
Array size = 5

12 % 5 = 2
```

So rotating 12 times is exactly the same as rotating 2 times.

---

### Reverse first block

```cpp
reverse(arr.begin(), arr.begin()+d);
```

Reverse block **A**.

---

### Reverse second block

```cpp
reverse(arr.begin()+d, arr.end());
```

Reverse block **B**.

---

### Reverse whole array

```cpp
reverse(arr.begin(), arr.end());
```

Transforms

```text
A' B'

↓

B A
```

which is exactly the desired left rotation.

---

# MODIFYING THIS FOR RIGHT ROTATION

Suppose

```text
1 2 3 4 5
```

Right rotate by

```
d = 2
```

Expected

```text
4 5 1 2 3
```

Instead of reversing the first `d` elements, reverse the first `n-d` elements.

---

## Right Rotation Algorithm

```
1. d %= n

2. Reverse first (n-d) elements

3. Reverse last d elements

4. Reverse entire array
```

---

## Dry Run

```text
1 2 3 | 4 5
```

Reverse first part

```text
3 2 1 | 4 5
```

Reverse second part

```text
3 2 1 | 5 4
```

Reverse whole array

```text
4 5 1 2 3
```

Correct.

---

# RIGHT ROTATION CODE

```cpp
class Solution {
public:
    void rotateArr(vector<int>& arr, int d) {

        int n = arr.size();

        d %= n;

        reverse(arr.begin(), arr.begin() + (n - d));

        reverse(arr.begin() + (n - d), arr.end());

        reverse(arr.begin(), arr.end());
    }
};
```

---

# BRUTE FORCE FOR RIGHT ROTATION

## Idea

- Store last `d` elements.
- Shift remaining elements right.
- Copy stored elements to the beginning.

---

## Code

```cpp
class Solution {
public:
    void rotateArr(vector<int>& arr, int d) {

        int n = arr.size();

        d %= n;

        vector<int> temp;

        // Store last d elements
        for(int i = n - d; i < n; i++)
            temp.push_back(arr[i]);

        // Shift remaining elements right
        for(int i = n - d - 1; i >= 0; i--)
            arr[i + d] = arr[i];

        // Copy stored elements
        for(int i = 0; i < d; i++)
            arr[i] = temp[i];
    }
};
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(d)
```

---

# EASY-TO-REMEMBER SUMMARY

## Left Rotation

```
A | B

↓

Reverse A

↓

Reverse B

↓

Reverse Whole

↓

B | A
```

Code

```cpp
d %= n;

reverse(begin, begin+d);

reverse(begin+d, end);

reverse(begin, end);
```

---

## Right Rotation

```
A | B

(A = first n-d)

(B = last d)

↓

Reverse A

↓

Reverse B

↓

Reverse Whole

↓

B | A
```

Code

```cpp
d %= n;

reverse(begin, begin+n-d);

reverse(begin+n-d, end);

reverse(begin, end);
```

---

# ONE-LINE INTERVIEW MEMORY TRICK

**Left Rotation**

> Reverse First d → Reverse Rest → Reverse Whole

**Right Rotation**

> Reverse First n-d → Reverse Last d → Reverse Whole

Both run in:

- **Time:** `O(n)`
- **Space:** `O(1)`

using the **Three-Reversal Pattern**.
````
