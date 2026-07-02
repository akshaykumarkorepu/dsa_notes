
## PROBLEM:

Given **two sorted arrays** (which may contain duplicates), return the **union** of both arrays in **sorted order** containing **only distinct elements**.

### Example

```text
a = [1,2,2,3,5]
b = [2,3,4,6]

Output:
[1,2,3,4,5,6]
```

---

# PATTERN:

**Two Pointers (Merge Pattern)**

---

# WHY THIS PATTERN:

The biggest clue in the question is:

> **Both arrays are already sorted.**

When two arrays are sorted, we don't need a set or sorting again.

Instead, we compare the current elements of both arrays exactly like **Merge Sort**.

This allows us to:

- Keep the output sorted.
- Remove duplicates while traversing.
- Visit every element only once.

---

# CORE IDEA:

Think exactly like **Merge Sort**.

Keep two pointers.

```text
i → First array

j → Second array
```

Always compare

```text
a[i]

b[j]
```

There are only **three cases**:

1. `a[i] < b[j]`
   - Insert `a[i]`
   - Move `i`

2. `a[i] > b[j]`
   - Insert `b[j]`
   - Move `j`

3. `a[i] == b[j]`
   - Insert only one copy
   - Move both pointers

Before inserting anything, always check:

```cpp
if(ans.empty() || ans.back() != current)
```

This removes duplicates.

---

# BRUTE FORCE:

## Intuition

The question requires:

- Unique elements
- Sorted order

A **set** already provides both.

So,

- Insert every element into a set.
- Convert the set into a vector.

---

## Approach

```text
Create empty set

Insert all elements of first array

Insert all elements of second array

Copy set → vector

Return vector
```

---

## Code

```cpp
vector<int> findUnion(vector<int>& a, vector<int>& b)
{
    set<int> st;

    for(int x : a)
        st.insert(x);

    for(int x : b)
        st.insert(x);

    vector<int> ans(st.begin(), st.end());

    return ans;
}
```

---

## Dry Run

```text
a = [2,2,3,5]
b = [1,2,4]
```

Initially

```text
st = {}
```

Insert first array

```text
Insert 2

st = {2}

Insert 2

Already exists

st = {2}

Insert 3

st = {2,3}

Insert 5

st = {2,3,5}
```

Insert second array

```text
Insert 1

st = {1,2,3,5}

Insert 2

Already exists

st = {1,2,3,5}

Insert 4

st = {1,2,3,4,5}
```

Copy into vector

```text
ans = [1,2,3,4,5]
```

Return answer.

---

## Why Optimize?

Although correct,

the set completely ignores the fact that the arrays are already sorted.

We're doing unnecessary work.

Instead of

```text
O((n+m)log(n+m))
```

we can achieve

```text
O(n+m)
```

using Two Pointers.

---

# OPTIMAL APPROACH:

## Intuition

Since both arrays are sorted,

there is no need to use a set.

Simply keep two pointers.

Compare

```text
a[i]

b[j]
```

The smaller element must appear first in the final answer.

If both are equal,

insert only one copy.

---

# ALGORITHM:

```text
Create answer vector.

i = 0
j = 0

While both arrays have elements

    if a[i] < b[j]

        Insert a[i] if unique
        Move i

    else if a[i] > b[j]

        Insert b[j] if unique
        Move j

    else

        Insert one copy
        Move both pointers

Copy remaining unique elements from first array.

Copy remaining unique elements from second array.

Return answer.
```

---

# DRY RUN:

Input

```text
a = [1,2,2,3,5]
b = [2,3,4,6]
```

Initially

```text
i = 0
j = 0

ans = []
```

---

### Step 1

Compare

```text
1

2
```

1 is smaller.

Insert

```text
1
```

Answer

```text
[1]
```

Move

```text
i++
```

---

### Step 2

Compare

```text
2

2
```

Equal.

Insert one copy.

```text
[1,2]
```

Move

```text
i++
j++
```

---

### Step 3

Compare

```text
2

3
```

2 is smaller.

Duplicate?

Last inserted element

```text
2
```

Same.

Do not insert.

Move

```text
i++
```

---

### Step 4

Compare

```text
3

3
```

Equal.

Insert

```text
3
```

Answer

```text
[1,2,3]
```

Move both.

---

### Step 5

Compare

```text
5

4
```

Insert

```text
4
```

Answer

```text
[1,2,3,4]
```

Move

```text
j++
```

---

### Step 6

Compare

```text
5

6
```

Insert

```text
5
```

Answer

```text
[1,2,3,4,5]
```

Move

```text
i++
```

First array finishes.

Remaining element

```text
6
```

Insert

```text
[1,2,3,4,5,6]
```

Done.

---

# IMPORTANT CODE SNIPPETS:

## Duplicate Check

```cpp
if(ans.empty() || ans.back()!=current)
    ans.push_back(current);
```

---

## First Array Smaller

```cpp
if(a[i] < b[j])
{
    if(ans.empty() || ans.back()!=a[i])
        ans.push_back(a[i]);

    i++;
}
```

---

## Second Array Smaller

```cpp
else if(a[i] > b[j])
{
    if(ans.empty() || ans.back()!=b[j])
        ans.push_back(b[j]);

    j++;
}
```

---

## Equal Case

```cpp
else
{
    if(ans.empty() || ans.back()!=a[i])
        ans.push_back(a[i]);

    i++;
    j++;
}
```

---

## ⭐ IMPORTANT INTERVIEW NOTE: Equal Case (`a[i] == b[j]`)

A very common interview question is:

> **"Why are you using `a[i]`? Why not `b[j]`?"**

### Explanation

We reach the `else` block only after

```cpp
if(a[i] < b[j])
```

and

```cpp
else if(a[i] > b[j])
```

both fail.

Therefore,

```cpp
a[i] == b[j]
```

So these two statements are completely equivalent:

```cpp
ans.push_back(a[i]);
```

OR

```cpp
ans.push_back(b[j]);
```

because both contain exactly the same value.

### Example

```text
a = [1,2,3,5]
         ^
         i

b = [2,3,4,6]
      ^
      j

a[i] = 3
b[j] = 3
```

Both

```cpp
ans.push_back(a[i]);
```

and

```cpp
ans.push_back(b[j]);
```

insert

```text
3
```

### Why do we move BOTH pointers?

Since

```cpp
a[i] == b[j]
```

both current elements have already been processed.

The union requires only **one copy**.

Therefore,

```cpp
i++;
j++;
```

move both pointers to the next unprocessed elements.

---

## ⭐ GOLDEN RULE

Whenever you write

```cpp
ans.push_back(X);
```

your duplicate check should always compare against the same value:

```cpp
if(ans.empty() || ans.back() != X)
```

Examples

```cpp
ans.push_back(a[i]);
```

↓

```cpp
ans.back() != a[i]
```

---

```cpp
ans.push_back(b[j]);
```

↓

```cpp
ans.back() != b[j]
```

---

Equal case

```cpp
a[i] == b[j]
```

so either

```cpp
ans.push_back(a[i]);
```

or

```cpp
ans.push_back(b[j]);
```

is perfectly correct.

---

## ⭐ EASY MEMORY TRICK

Whenever you write

```cpp
ans.push_back(X);
```

immediately think

> **"The duplicate check should compare `ans.back()` with the same value `X` that I'm pushing."**

Never think about arrays.

Always think about **the value you're inserting**.

---

## Remaining Elements

```cpp
while(i<n)
{
    if(ans.empty() || ans.back()!=a[i])
        ans.push_back(a[i]);

    i++;
}
```

```cpp
while(j<m)
{
    if(ans.empty() || ans.back()!=b[j])
        ans.push_back(b[j]);

    j++;
}
```

---

# COMMON MISTAKES:

### 1. Forgetting duplicate check

Wrong

```cpp
ans.push_back(a[i]);
```

Correct

```cpp
if(ans.empty() || ans.back()!=a[i)
```

---

### 2. Moving only one pointer in equal case

Wrong

```cpp
i++;
```

Correct

```cpp
i++;
j++;
```

Both elements have already been processed.

---

### 3. Forgetting remaining elements

Always process

```cpp
while(i<n)

while(j<m)
```

---

### 4. Using a set in the optimal solution

Works,

but wastes the advantage of sorted arrays.

---

# WHY I MIGHT FORGET THIS:

Because I focus on comparing the arrays and forget the duplicate check.

The entire question boils down to remembering one line:

```cpp
if(ans.empty() || ans.back()!=current)
```

Everything else is simply Merge Sort.

---

# INTERVIEW FLOW:

> Since both arrays are already sorted, I don't need a set. I use two pointers like Merge Sort. At each step, I compare the current elements. I insert the smaller one if it hasn't already been inserted. If both elements are equal, I insert one copy and move both pointers. After one array finishes, I process the remaining unique elements from the other array. This gives an O(n+m) solution.

---

# TIME COMPLEXITY:

## Brute Force

```text
Insert into set

↓

O((n+m)log(n+m))

Copy set → vector

↓

O(n+m)

Overall

O((n+m)log(n+m))
```

### Reason

- First loop runs `n` times.
- Second loop runs `m` times.
- Each insertion into a set takes `O(log(n+m))`.
- Copying the set takes `O(n+m)`.
- The insertion cost dominates.

---

## Optimal

```text
O(n+m)
```

### Reason

Each pointer moves only forward.

```text
i

0 →1 →2 →...→ n
```

```text
j

0 →1 →2 →...→ m
```

Neither pointer ever moves backward.

Every element is processed exactly once.

---

# SPACE COMPLEXITY:

## Brute Force

```text
O(n+m)
```

Reason

- Set stores unique elements.
- Answer vector stores the final union.

---

## Optimal

Extra Space

```text
O(1)
```

Reason

Only two pointers and a few variables are used.

(Output array is generally **not counted** as extra space.)

---

# EDGE CASES:

### All duplicates

```text
a = [1,1,1]
b = [1,1]

Output

[1]
```

---

### No common elements

```text
a = [1,2]
b = [3,4]

Output

[1,2,3,4]
```

---

### One array completely smaller

```text
a = [1,2]
b = [5,6]

Output

[1,2,5,6]
```

---

### One array empty (if allowed)

```text
a = []
b = [1,2]

Output

[1,2]
```

---

### Negative numbers

```text
a = [-5,-2]
b = [-3,4]

Output

[-5,-3,-2,4]
```

---

# PATTERN RECOGNITION:

Use the **Two Pointer (Merge Pattern)** whenever you see:

- Two sorted arrays.
- Need sorted output.
- Merge two sorted sequences.
- Union.
- Intersection.
- Linear traversal of two sorted arrays.

### Trigger Sentence

> **Whenever I see two sorted arrays and need a sorted result, I should immediately think of the Merge Pattern using Two Pointers.**

---

# Clean C++ Code

```cpp
class Solution {
public:
    vector<int> findUnion(vector<int>& a, vector<int>& b) {

        int n = a.size();
        int m = b.size();

        int i = 0;
        int j = 0;

        vector<int> ans;

        while(i < n && j < m){

            if(a[i] < b[j]){

                if(ans.empty() || ans.back() != a[i])
                    ans.push_back(a[i]);

                i++;
            }

            else if(a[i] > b[j]){

                if(ans.empty() || ans.back() != b[j])
                    ans.push_back(b[j]);

                j++;
            }

            else{

                if(ans.empty() || ans.back() != a[i])
                    ans.push_back(a[i]);

                i++;
                j++;
            }
        }

        while(i < n){

            if(ans.empty() || ans.back() != a[i])
                ans.push_back(a[i]);

            i++;
        }

        while(j < m){

            if(ans.empty() || ans.back() != b[j])
                ans.push_back(b[j]);

            j++;
        }

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### Two pointers

```cpp
int i = 0;
int j = 0;
```

One pointer tracks each sorted array.

---

### Compare current elements

```cpp
if(a[i] < b[j])
```

The smaller value must appear first in the sorted union.

---

### Duplicate check

```cpp
if(ans.empty() || ans.back() != current)
```

Insert only if this value is different from the last inserted value.

---

### Equal case

```cpp
i++;
j++;
```

Both pointers are pointing to the same value.

Insert it once and move both because both elements have already been processed.

---

### Remaining loops

```cpp
while(i<n)

while(j<m)
```

If one array finishes first, process the remaining unique elements from the other array.

---

# Easy-to-Remember Summary

- **Pattern:** Two Pointers (Merge)
- **Brute:** Use a `set` → unique + sorted → `O((n+m)log(n+m))`
- **Optimal:** Merge two sorted arrays using two pointers.
- **Rule 1:** Smaller value → insert if unique → move that pointer.
- **Rule 2:** Equal value → insert once → move both pointers.
- **Rule 3:** Before every insertion, always write:

```cpp
if(ans.empty() || ans.back() != current)
```

### ⭐ One-Line Memory Trick

> **"Merge like Merge Sort, but never push the same value twice."**
````
