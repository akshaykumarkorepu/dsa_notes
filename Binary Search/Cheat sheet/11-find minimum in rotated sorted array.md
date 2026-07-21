
# Find Minimum in Rotated Sorted Array

## PATTERN: Binary Search on Rotated Sorted Array
→ **Trigger:** *"When I see a sorted array that has been rotated and I need to find the minimum (or rotation point) in O(log N)."*

---

## BRUTE FORCE
→ **Idea:** Traverse the entire array and keep updating the minimum element.

**Important Code Snippet**

```cpp
int mini = arr[0];

for (int x : arr)
    mini = min(mini, x);

return mini;
```

→ **TC / SC:** **O(N)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** *"I immediately use Binary Search and determine whether `mid` belongs to the left or right sorted half by comparing it with `arr[high]`."*

→ **Core idea:**
Maintain two pointers `low` and `high`. In every iteration, compute `mid` and compare `arr[mid]` with `arr[high]`.

- If `arr[mid] > arr[high]`, `mid` lies in the **left rotated half**, so the minimum must be on the right → `low = mid + 1`.
- Otherwise, `mid` lies in the **right sorted half**, and since it could itself be the minimum, keep it → `high = mid`.

Continue until `low == high`. That index stores the minimum element.

### Important Code Snippets

**Binary Search Loop**

```cpp
while (low < high)
```

**Find Middle**

```cpp
int mid = low + (high - low) / 2;
```

**Left Half**

```cpp
if (arr[mid] > arr[high])
    low = mid + 1;
```

**Right Half**

```cpp
else
    high = mid;
```

**Answer**

```cpp
return arr[low];
```

→ **TC / SC:** **O(log N)** / **O(1)**

---

## WATCH OUT FOR

→ Don't write `high = mid - 1`; `mid` itself may be the minimum, so always use `high = mid`.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The rotated array still consists of two sorted halves.
2. I'll use Binary Search by comparing `arr[mid]` with `arr[high]`.
3. If `arr[mid] > arr[high]`, the minimum lies on the right, so move `low`.
4. Otherwise, `mid` may be the minimum, so keep it by moving `high = mid`.
5. Continue until `low == high`; that index contains the minimum.
````
