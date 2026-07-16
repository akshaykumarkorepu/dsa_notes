
## PROBLEM:

Given a **sorted array of distinct integers**, return:
- the index if `k` exists.
- otherwise, return the index where it should be inserted to maintain the sorted order.

---

## PATTERN:

**Lower Bound Binary Search**

**Trigger:** *Find the first index where `arr[i] >= k`.*

---

## WHY THIS PATTERN:

The answer to this problem is exactly the **Lower Bound**.

There are only two possibilities:

- If `arr[lowerBound] == k`
  - The element already exists, so return its index.

- Otherwise
  - The lower bound itself is the correct insertion position.

Since the array is sorted, Binary Search lets us find this position in **O(log N)** time.

---

## CORE IDEA:

Don't think:

> **"Find k."**

Instead think:

> **"Find the first element that is greater than or equal to k."**

That index is always the answer.

Example:

```
arr = [1,3,5,6]
k = 2

First element >=2
      ↓
[1,3,5,6]
   ^
index = 1
```

---

## BRUTE FORCE:

### Idea

Traverse the array from left to right.

- If `arr[i] == k`
  - Return `i`.

- If `arr[i] > k`
  - Return `i`.

If no such element exists, return `n`.

### Code

```cpp
int searchInsert(vector<int>& arr, int k) {

    for(int i = 0; i < arr.size(); i++) {

        if(arr[i] >= k)
            return i;
    }

    return arr.size();
}
```

### Dry Run

```
arr = [1,3,5,6]
k = 2

1 < 2

3 >= 2

Answer = 1
```

### Time Complexity

**O(N)**

### Space Complexity

**O(1)**

---

## OPTIMAL APPROACH:

Use **Lower Bound Binary Search**.

Maintain

```cpp
ans = n;
```

Whenever

```cpp
arr[mid] >= k
```

- `mid` is a possible answer.
- Store it.
- Move left because an earlier valid position may exist.

Whenever

```cpp
arr[mid] < k
```

Move right.

Finally, `ans` will be the first index where

```
arr[index] >= k
```

which is exactly the required answer.

---

## ALGORITHM:

```
ans = n

low = 0
high = n-1

while(low <= high)

    mid

    if(arr[mid] >= k)

        ans = mid

        high = mid - 1

    else

        low = mid + 1

return ans
```

---

## DRY RUN:

### Example 1

```
arr = [1,3,5,6]
k = 2
```

Initial State

```
low = 0
high = 3
ans = 4
```

### Iteration 1

```
mid = 1

arr[1] = 3

3 >= 2
```

Possible answer

```
ans = 1

high = 0
```

---

### Iteration 2

```
low = 0
high = 0

mid = 0

arr[0] = 1

1 < 2
```

Move right

```
low = 1
```

Loop ends.

Return

```
1
```

---

### Example 2

```
arr = [1,3,5,6]
k = 5
```

Iteration 1

```
mid = 1

3 < 5

low = 2
```

Iteration 2

```
mid = 2

5 >= 5

ans = 2

high = 1
```

Loop ends.

Return

```
2
```

---

### Example 3

```
arr = [2,6,7,10,14]
k = 15
```

Every element is smaller.

```
ans never changes.

ans = n = 5
```

Return

```
5
```

Insert at the end.

---

## IMPORTANT CODE SNIPPETS:

### Lower Bound Update

```cpp
if(arr[mid] >= k){
    ans = mid;
    high = mid - 1;
}
```

---

### Search Right

```cpp
else{
    low = mid + 1;
}
```

---

### Safe Mid Calculation

```cpp
int mid = low + (high - low) / 2;
```

---

## COMMON MISTAKES:

### Mistake 1

Searching only for equality.

Wrong.

Need insertion position as well.

---

### Mistake 2

Using

```cpp
arr[mid] > k
```

instead of

```cpp
arr[mid] >= k
```

That becomes **Upper Bound**, giving wrong answers when `k` exists.

---

### Mistake 3

After finding a valid answer,

doing

```cpp
low = mid + 1;
```

Wrong.

Need to search left for an earlier valid position.

---

### Mistake 4

Returning `mid`.

The correct answer may exist further left.

Always return `ans` (or `low` after the loop if using the standard lower-bound implementation).

---

## WHY I MIGHT FORGET THIS:

Because I think:

> **"Find k."**

instead of

> **"Find the first element ≥ k."**

This is **not a search problem.**

It is a **Lower Bound problem.**

---

## INTERVIEW FLOW:

> The array is sorted, so Binary Search is applicable.

> We need either the position of `k` or the position where it should be inserted.

> Both are handled by finding the **Lower Bound**, i.e., the first element greater than or equal to `k`.

> Whenever `arr[mid] >= k`, I store it as a possible answer and continue searching left.

> Otherwise, I search the right half.

> The final stored answer is the insertion index, and if `k` exists, it is exactly its position.

---

## TIME COMPLEXITY:

**O(log N)**

### Reason

Every iteration removes half of the remaining search space.

Maximum iterations are approximately

```
log₂N
```

---

## SPACE COMPLEXITY:

**O(1)**

Only constant extra variables are used.

---

## EDGE CASES:

### Smaller than every element

```
arr = [3,5,7]

k = 1

Answer = 0
```

---

### Larger than every element

```
arr = [2,4,6]

k = 10

Answer = 3
```

---

### Element already exists

```
arr = [1,3,5]

k = 3

Answer = 1
```

---

### Single Element

```
arr = [5]

k = 5

Answer = 0
```

```
arr = [5]

k = 2

Answer = 0
```

```
arr = [5]

k = 10

Answer = 1
```

---

## PATTERN RECOGNITION:

Immediately think **Lower Bound Binary Search** whenever you see:

- Sorted array
- Insert position
- First element ≥ X
- First valid index
- Smallest index satisfying a condition
- Maintain sorted order
- Lower Bound
- First occurrence (with slight modifications)

### Trigger Sentence

> **Find the first index where the value becomes greater than or equal to the target.**

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    int searchInsert(vector<int>& arr, int k) {

        int low = 0;
        int high = arr.size() - 1;
        int ans = arr.size();

        while(low <= high){

            int mid = low + (high - low) / 2;

            if(arr[mid] >= k){
                ans = mid;
                high = mid - 1;
            }
            else{
                low = mid + 1;
            }
        }

        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int ans = arr.size();
```

Assume the element should be inserted at the end.

---

```cpp
int mid = low + (high - low) / 2;
```

Safely computes the middle index without overflow.

---

```cpp
if(arr[mid] >= k)
```

Current index is a valid insertion position.

Try finding an earlier one.

---

```cpp
ans = mid;
```

Store the current best answer.

---

```cpp
high = mid - 1;
```

Search left for a smaller valid index.

---

```cpp
else{
    low = mid + 1;
}
```

Current value is too small.

Search the right half.

---

```cpp
return ans;
```

Returns the first index where `arr[index] >= k`.

If no such index exists, `ans` remains `n`, meaning insert at the end.

---

# EASY-TO-REMEMBER SUMMARY

- **Pattern:** Lower Bound Binary Search.
- Think:

  > **"Find the first element ≥ target."**

- If the target exists, that's its index.
- Otherwise, that's its insertion position.

### Rule to Remember

```
arr[mid] >= target
        ↓
Store answer
Go Left
```

```
arr[mid] < target
        ↓
Go Right
```

Never think **"Find target."**

Always think **"Find first ≥ target."**
