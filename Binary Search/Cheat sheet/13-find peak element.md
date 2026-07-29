
# Peak Element

## PATTERN: Binary Search on Slope (Local Maximum)
→ **Trigger:** "when I see an unsorted array where comparing adjacent elements tells me which side must contain the answer"

---

## BRUTE FORCE

→ **Idea:** Scan every element and return the first element greater than both its neighbours.

### Crucial Snippet

```cpp
if(n == 1) return 0;

if(arr[0] > arr[1]) return 0;

for(int i = 1; i < n - 1; i++){
    if(arr[i] > arr[i-1] && arr[i] > arr[i+1])
        return i;
}

if(arr[n-1] > arr[n-2]) return n-1;
```

→ **TC / SC:** **O(n)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately Binary Search on the slope by comparing `arr[mid]` with `arr[mid+1]`."

→ **Core idea:** Maintain `low` and `high`. At every step compare `arr[mid]` and `arr[mid+1]`. If `arr[mid] < arr[mid+1]`, we're on an increasing slope, so the peak must be on the right (`low = mid + 1`). Otherwise we're on a decreasing slope, so the peak is at `mid` or to its left (`high = mid`). Continue until `low == high`; that index is the peak.

### Crucial Snippets

**Binary Search Loop**

```cpp
while(low < high)
```

**Increasing Slope**

```cpp
if(arr[mid] < arr[mid+1])
    low = mid + 1;
```

**Decreasing Slope**

```cpp
else
    high = mid;
```

**Answer**

```cpp
return low;
```

→ **TC / SC:** **O(log n)** / **O(1)**

---

## WATCH OUT FOR

→ Never write **`high = mid - 1`**. `mid` itself can be the peak, so always use **`high = mid`**.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force is to scan every element and check whether it's greater than both neighbours.
2. We can optimize because comparing adjacent elements tells us the slope direction.
3. If `arr[mid] < arr[mid+1]`, the peak must lie on the right; otherwise it's on the left including `mid`.
4. Perform Binary Search until `low == high`.
5. Return `low` (or `high`), which is the peak index.
````
