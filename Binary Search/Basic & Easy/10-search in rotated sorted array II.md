
## PROBLEM:

Given a **sorted array that has been rotated** and **may contain duplicate elements**, determine whether a given target exists in the array.

---

## PATTERN:

**Modified Binary Search (Rotated Sorted Array + Duplicates)**

**Trigger:**
> "Search in a rotated sorted array where duplicates are allowed."

---

## WHY THIS PATTERN:

In a rotated sorted array, **at least one half is normally sorted**, allowing Binary Search to discard half of the search space.

However, **duplicates can hide the pivot**, making it impossible to determine which half is sorted.

Example:

```text
1 1 1 1 1
L   M   R
```

Here,

```cpp
arr[low] == arr[mid] == arr[high]
```

Both halves look identical, so Binary Search cannot decide which side to discard.

To remove this ambiguity, we shrink both ends.

```cpp
low++;
high--;
```

Once ambiguity is removed, continue exactly like **Search in Rotated Sorted Array-I**.

---

# DIFFERENCE FROM SEARCH IN ROTATED SORTED ARRAY-I

| Search in Rotated Array-I | Search in Rotated Array-II |
|---------------------------|----------------------------|
| No duplicates | Duplicates allowed |
| One half is always identifiable | Duplicates may hide the sorted half |
| Pure Modified Binary Search | Modified Binary Search + Duplicate Handling |
| Worst Case: O(log n) | Worst Case: O(n) |
| No ambiguity | Must remove ambiguity first |

### The ONLY Extra Step

Before identifying the sorted half, check:

```cpp
if(arr[low] == arr[mid] &&
   arr[mid] == arr[high])
{
    low++;
    high--;
}
```

Everything else is identical to Search in Rotated Sorted Array-I.

---

# CORE IDEA:

Every iteration has only **3 possibilities**.

### Case 1

Target found.

```cpp
if(arr[mid] == target)
    return true;
```

---

### Case 2

Duplicates create ambiguity.

```cpp
arr[low] == arr[mid] &&
arr[mid] == arr[high]
```

Cannot determine the sorted half.

```cpp
low++;
high--;
```

---

### Case 3

No ambiguity.

One half is definitely sorted.

- Check whether the left half is sorted.
- Otherwise, the right half is sorted.
- Decide where the target lies.

---

# BRUTE FORCE:

### Idea

Traverse the array and compare every element with the target.

### Code

```cpp
bool search(vector<int>& arr, int target) {
    for(int x : arr){
        if(x == target)
            return true;
    }
    return false;
}
```

### Dry Run

```
arr = [4,5,8,1,1,1,2]
target = 6

4 ❌
5 ❌
8 ❌
1 ❌
1 ❌
1 ❌
2 ❌

Return false.
```

### Time Complexity

```
O(n)
```

### Space Complexity

```
O(1)
```

---

# OPTIMAL APPROACH:

Use **Modified Binary Search**.

The algorithm is identical to Search in Rotated Array-I except for one additional condition.

Whenever

```cpp
arr[low] == arr[mid] &&
arr[mid] == arr[high]
```

we cannot determine the sorted half.

Therefore,

```cpp
low++;
high--;
```

After removing ambiguity, perform the normal rotated binary search.

---

# ALGORITHM:

```
Initialize low and high.

While(low <= high)

    Find mid.

    If arr[mid] == target
        return true

    If arr[low] == arr[mid] == arr[high]
        low++
        high--

    Else if(left half is sorted)

        If target lies in left half
            high = mid - 1
        Else
            low = mid + 1

    Else

        Right half is sorted.

        If target lies in right half
            low = mid + 1
        Else
            high = mid - 1

Return false.
```

---

# DRY RUN:

### Example

```
arr = [3,3,3,1,2,3]
target = 2
```

---

### Iteration 1

```
Index : 0 1 2 3 4 5
Array : 3 3 3 1 2 3

L           R
      M
```

```
low = 0
mid = 2
high = 5

arr[low] = 3
arr[mid] = 3
arr[high] = 3
```

Target?

```
3 == 2

No
```

Duplicate condition?

```
3 == 3 == 3

Yes
```

Cannot identify the sorted half.

```
low++
high--
```

Now

```
low = 1
high = 4
```

---

### Iteration 2

```
Index : 0 1 2 3 4 5
Array : 3 3 3 1 2 3

  L     R
    M
```

```
low = 1
mid = 2
high = 4

arr[low] = 3
arr[mid] = 3
arr[high] = 2
```

Duplicate condition?

```
3 == 3 == 2

No
```

Left sorted?

```
arr[low] <= arr[mid]

3 <= 3

Yes
```

Target inside left?

```
3 <= 2 < 3

False
```

Move right.

```
low = mid + 1

low = 3
```

---

### Iteration 3

```
Index : 0 1 2 3 4 5
Array : 3 3 3 1 2 3

      L R
      M
```

```
low = 3
mid = 3
high = 4

arr[mid] = 1
```

Target?

```
1 == 2

No
```

Left sorted?

```
1 <= 1

Yes
```

Target inside left?

```
1 <= 2 < 1

False
```

Move right.

```
low = 4
```

---

### Iteration 4

```
Index : 0 1 2 3 4 5
Array : 3 3 3 1 2 3

        M
```

```
arr[mid] = 2
```

Target found.

```
Return true
```

---

# IMPORTANT OBSERVATIONS:

- Duplicates are a problem **only when**
  ```cpp
  arr[low] == arr[mid] == arr[high]
  ```
- If even one of them is different, we can identify the sorted half.
- Duplicate handling **must happen before** checking the sorted half.
- Apart from duplicate handling, the algorithm is identical to Search in Rotated Array-I.

---

# IMPORTANT CODE SNIPPETS:

### Duplicate Handling

```cpp
if(arr[low] == arr[mid] &&
   arr[mid] == arr[high])
{
    low++;
    high--;
}
```

---

### Left Half Sorted

```cpp
else if(arr[low] <= arr[mid])
```

---

### Target Inside Left Half

```cpp
if(arr[low] <= target &&
   target < arr[mid])
```

---

### Target Inside Right Half

```cpp
if(arr[mid] < target &&
   target <= arr[high])
```

---

# COMMON MISTAKES:

- Forgetting the duplicate condition.
- Checking the sorted half before handling duplicates.
- Shrinking when only
  ```cpp
  arr[low] == arr[mid]
  ```
  (Wrong!)
- Incorrect inequalities.
- Forgetting that worst-case complexity becomes O(n).

---

# WHY I MIGHT FORGET THIS:

Because this problem is **almost identical** to Search in Rotated Array-I.

Remember:

> **Duplicates create ambiguity.**
>
> **Remove ambiguity first.**
>
> **Then perform the normal rotated binary search.**

---

# INTERVIEW FLOW:

1. Explain that the array is sorted and rotated.
2. Mention that Binary Search still works.
3. Explain how duplicates create ambiguity.
4. Introduce the duplicate handling condition.
5. Explain that after removing ambiguity, the remaining algorithm is exactly the same as Search in Rotated Array-I.
6. Mention why the worst-case complexity becomes O(n).

---

# TIME COMPLEXITY:

### Average Case

```
O(log n)
```

Reason:

Most iterations discard half of the search space.

---

### Worst Case

```
O(n)
```

Reason:

When many duplicates exist, e.g.

```
[1,1,1,1,1,1]
```

we repeatedly execute

```cpp
low++;
high--;
```

which shrinks the search space by only two elements instead of half.

---

# SPACE COMPLEXITY:

```
O(1)
```

Only constant extra variables are used.

---

# EDGE CASES:

- All elements are duplicates.
- Target not present.
- Single element.
- No rotation.
- Rotation at the last index.
- Pivot surrounded by duplicates.
- Target equals the duplicated value.

---

# PATTERN RECOGNITION:

Whenever you see

- Sorted Array
- Rotated Array
- Search Element
- Duplicates Allowed

Immediately think:

```
Modified Binary Search

↓

Is mid the answer?

↓

Yes → Return

↓

No

↓

Are low, mid and high equal?

↓

Yes

↓

Shrink both ends

↓

No

↓

Find the sorted half

↓

Does target lie in it?

↓

Yes → Search that half

↓

No → Search the other half
```

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    bool Search(vector<int>& arr, int target) {

        int low = 0;
        int high = arr.size() - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            // Target found
            if (arr[mid] == target)
                return true;

            // Duplicates create ambiguity
            if (arr[low] == arr[mid] &&
                arr[mid] == arr[high]) {

                low++;
                high--;
            }

            // Left half is sorted
            else if (arr[low] <= arr[mid]) {

                if (arr[low] <= target &&
                    target < arr[mid])

                    high = mid - 1;

                else

                    low = mid + 1;
            }

            // Right half is sorted
            else {

                if (arr[mid] < target &&
                    target <= arr[high])

                    low = mid + 1;

                else

                    high = mid - 1;
            }
        }

        return false;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
int low = 0;
int high = arr.size() - 1;
```

Start Binary Search on the entire array.

---

```cpp
while(low <= high)
```

Keep searching until the search space becomes empty.

---

```cpp
int mid = low + (high - low) / 2;
```

Safely compute the middle index without overflow.

---

```cpp
if(arr[mid] == target)
```

Always check the middle first.

---

```cpp
if(arr[low] == arr[mid] &&
   arr[mid] == arr[high])
```

Duplicates hide the pivot, so we cannot determine the sorted half.

---

```cpp
low++;
high--;
```

Discard duplicate boundary elements to remove ambiguity.

---

```cpp
else if(arr[low] <= arr[mid])
```

The left half is sorted.

---

```cpp
if(arr[low] <= target &&
   target < arr[mid])
```

Check whether the target lies inside the sorted left half.

---

```cpp
low = mid + 1;
```

If the target is not in the sorted left half, search the right half.

---

```cpp
if(arr[mid] < target &&
   target <= arr[high])
```

Check whether the target lies inside the sorted right half.

---

```cpp
return false;
```

The target does not exist in the array.

---

# EASY-TO-REMEMBER SUMMARY

> **Search in Rotated Array-II = Search in Rotated Array-I + Duplicate Handling**

```
mid == target ?

↓

Yes → Return

↓

No

↓

low == mid == high ?

↓

Yes

↓

low++
high--

↓

No

↓

Find the sorted half

↓

Check whether the target belongs to it

↓

Discard the other half
```

**Memory Trick:**

> **Duplicates = Ambiguity**
>
> **Ambiguity → Shrink**
>
> **No Ambiguity → Normal Rotated Binary Search**
