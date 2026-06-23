

## PROBLEM:

You are given:
- `greed[i]` = minimum cookie size required by the `i-th` child.
- `cookie[j]` = size of the `j-th` cookie.

Each child can receive **at most one cookie**, and each cookie can be assigned to **at most one child**.

A child is satisfied if:

```
cookie[j] >= greed[i]
```

Return the **maximum number of satisfied children**.

---

## PATTERN:

**Greedy + Sorting + Two Pointers (Greedy Matching)**

---

## WHY THIS PATTERN:

We want to maximize the number of satisfied children.

If we waste a larger cookie on a child who could have been satisfied with a smaller cookie, we may not have enough large cookies left for greedier children.

So we should always:

> **Assign the smallest cookie that can satisfy the current least greedy child.**

Sorting makes this greedy decision possible, while two pointers allow us to process both arrays efficiently.

---

## CORE IDEA:

Sort both arrays.

Maintain:

- `left` → current child
- `right` → current cookie

There are only two cases:

- If the cookie can satisfy the child, assign it and move both pointers.
- Otherwise, discard the cookie and move only the cookie pointer.

The greedy choice is to **never waste a larger cookie when a smaller one is sufficient.**

---

## BRUTE FORCE:

For every child, iterate through all unused cookies and assign the smallest cookie that can satisfy that child.

To implement this, maintain a `used[]` array to keep track of cookies that have already been assigned.

For each child:
- Traverse every cookie.
- Ignore cookies that are already used.
- Among the remaining cookies, choose the smallest cookie that satisfies the child's greed.
- Mark that cookie as used.

This works correctly but is inefficient because we repeatedly scan the entire cookie array for every child.

**Time Complexity:** `O(n × m)`

**Space Complexity:** `O(m)` (used array)

### Is Brute Force Really Necessary?

**No.**

For this problem, the greedy solution is quite natural once you observe that a larger cookie should never be wasted on a less greedy child.

Unlike problems such as Activity Selection or Job Sequencing, the brute force does **not** help derive the greedy solution.

In interviews, simply mentioning the brute force approach along with its time and space complexity is usually sufficient.

Unless the interviewer specifically asks for it, **you do not need to write the brute force code or explain its dry run.**

---

## OPTIMAL APPROACH:

### Intuition

The goal is to satisfy the maximum number of children.

Suppose a child needs a cookie of size **3**.

Available cookies:

```
3 5 8
```

Giving cookie **3** is always better than giving cookie **8** because:

- Both satisfy the child.
- Cookie **8** may be needed later by a greedier child.
- Cookie **3** cannot satisfy children with larger greed values.

Therefore, always assign the **smallest possible cookie** that satisfies the current child.

---

### Why Greedy Works

The greedy choice is:

> **Assign the smallest available cookie that can satisfy the least greedy remaining child.**

This is always safe because it preserves larger cookies for greedier children.

Replacing the chosen cookie with a larger one can never increase the number of satisfied children.

---

### What is the Greedy Choice?

Assign the **smallest valid cookie** to the **least greedy remaining child**.

---

### Why is this Choice Safe?

Suppose a child needs:

```
3
```

Available cookies are:

```
3 5 8
```

Giving cookie **3** leaves **5** and **8** available for greedier children.

Giving cookie **8** instead wastes a valuable larger cookie without increasing the number of satisfied children.

Hence, the greedy decision is always optimal.

---

### Why Sorting?

Sorting both arrays allows us to process:

- children from least greedy to most greedy.
- cookies from smallest to largest.

Without sorting, we cannot guarantee that we're assigning the smallest valid cookie.

---

### Invariant Maintained

After every iteration:

- Every child before `left` has already been processed.
- Every cookie before `right` has either been assigned or discarded.
- No discarded cookie can satisfy the current child.
- Every assigned cookie is the smallest valid cookie for that child.

This invariant guarantees that no larger cookie has been wasted.

---

## ALGORITHM:

1. Sort the `greed` array.
2. Sort the `cookie` array.
3. Initialize:
   - `left = 0` (child pointer)
   - `right = 0` (cookie pointer)
4. While both pointers are within bounds:
   - If `cookie[right] >= greed[left]`
     - Assign the cookie.
     - Move both pointers.
   - Else
     - Discard the cookie.
     - Move only `right`.
5. Return `left`.

---

## DRY RUN:

**Input**

```
greed  = [1,10,3]
cookie = [1,2,3]
```

After sorting:

```
greed  = [1,3,10]
cookie = [1,2,3]
```

Initial:

```
left = 0
right = 0
```

### Iteration 1

Current child:

```
1
```

Current cookie:

```
1
```

```
1 >= 1 ✔
```

Assign cookie.

```
left = 1
right = 1
Satisfied = 1
```

---

### Iteration 2

Child needs:

```
3
```

Cookie:

```
2
```

```
2 >= 3 ✘
```

Discard cookie.

```
right = 2
```

The child still needs a cookie, so `left` does not move.

---

### Iteration 3

Child needs:

```
3
```

Cookie:

```
3
```

```
3 >= 3 ✔
```

Assign cookie.

```
left = 2
right = 3
Satisfied = 2
```

Cookies are exhausted.

Return:

```
2
```

---

## COMMON MISTAKES:

### 1. Not sorting the arrays

The greedy decision becomes invalid.

---

### 2. Giving the largest cookie first

This may waste large cookies that are needed later.

---

### 3. Incrementing both pointers when the cookie is too small

Wrong.

Only move the cookie pointer.

---

### 4. Moving the child pointer when the cookie cannot satisfy it

Wrong.

The child still needs a larger cookie.

---

## INTERVIEW FLOW:

"I first observe that we want to maximize the number of satisfied children.

A larger cookie should never be wasted on a child who could have been satisfied using a smaller cookie.

So I sort both arrays.

I maintain two pointers:

- `left` for children.
- `right` for cookies.

If the current cookie satisfies the current child, I assign it and move both pointers.

Otherwise, the cookie is too small and cannot satisfy any future child either, so I discard it by moving only the cookie pointer.

This greedy strategy always preserves larger cookies for greedier children and guarantees the maximum number of satisfied children."

---

## TIME COMPLEXITY:

Sorting `greed`:

```
O(n log n)
```

Sorting `cookie`:

```
O(m log m)
```

Two-pointer traversal:

```
O(n + m)
```

Overall:

```
O(n log n + m log m)
```

where:

- `n` = number of children
- `m` = number of cookies

---

## SPACE COMPLEXITY:

Auxiliary Space:

```
O(1)
```

Only two pointers are used.

> **Note:** If the interviewer includes the recursion stack of `std::sort`, the space complexity becomes `O(log n + log m)`. Otherwise, the expected interview answer is **O(1)** auxiliary space.

---

## EDGE CASES:

- No child can be satisfied.
- Every child can be satisfied.
- More cookies than children.
- More children than cookies.
- Duplicate greed values.
- Duplicate cookie sizes.
- Single child.
- Single cookie.

---

## PATTERN RECOGNITION:

Look for these clues:

- Maximize the number of successful assignments.
- Each resource can be used only once.
- Every request has a minimum requirement.
- Resources have different capacities.
- Matching the smallest feasible resource is beneficial.
- Sorting naturally orders both requests and resources.

Typical solution:

```
Sort both arrays
        ↓
Smallest request
        ↓
Smallest feasible resource
        ↓
If possible:
Move both pointers

Else:
Discard smaller resource
```

Whenever you see problems involving:

- Assign jobs to workers
- Cookies to children
- Tasks to machines
- Resources to requests

think:

> **Greedy Matching = Sort + Two Pointers**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int maxChildren(vector<int>& greed, vector<int>& cookie) {

        sort(greed.begin(), greed.end());
        sort(cookie.begin(), cookie.end());

        int left = 0;
        int right = 0;

        while (left < greed.size() && right < cookie.size()) {

            if (cookie[right] >= greed[left]) {
                left++;
                right++;
            }
            else {
                right++;
            }
        }

        return left;
    }
};
```

---

# Intuition Behind Every Important Line

### Sort both arrays

```cpp
sort(greed.begin(), greed.end());
sort(cookie.begin(), cookie.end());
```

Sorting allows us to always match the least greedy child with the smallest possible cookie.

---

### Initialize pointers

```cpp
int left = 0;
int right = 0;
```

- `left` points to the current child.
- `right` points to the current cookie.

---

### Traverse both arrays

```cpp
while (left < greed.size() && right < cookie.size())
```

Continue until either all children are processed or all cookies are exhausted.

---

### Cookie can satisfy the child

```cpp
if (cookie[right] >= greed[left])
```

The current cookie is large enough.

Assign it.

```cpp
left++;
right++;
```

Move both pointers because:

- the child has been satisfied.
- the cookie has been used.

---

### Cookie is too small

```cpp
else {
    right++;
}
```

The cookie cannot satisfy the current child.

Since future children are equally or more greedy, this cookie cannot satisfy them either.

Discard it and try the next cookie.

Do **not** move `left`, because the child is still waiting for a suitable cookie.

---

### Return answer

```cpp
return left;
```

`left` represents the number of satisfied children because it increases only when a child successfully receives a cookie.

---

# Easy-to-Remember Summary

- **Sort both arrays.**
- `left` → child, `right` → cookie.
- If the cookie satisfies the child, assign it and move both pointers.
- Otherwise, discard the cookie by moving only `right`.
- **Never waste a larger cookie when a smaller valid one is available.**
- This is a classic **Greedy Matching (Sort + Two Pointers)** problem.
````
