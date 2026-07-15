

## Question: Search X in Sorted Array

### PATTERN: Binary Search
→ **Trigger:** "when I see a sorted array and need to search/find an element (or any monotonic search space)."

---

### BRUTE FORCE
→ **Idea:** Traverse the array linearly until the target is found.
→ **TC / SC:** **O(n)** / **O(1)**

---

### OPTIMAL
→ **First instinct:** "I immediately think Binary Search because the array is sorted."

→ **Core idea:** Maintain two pointers `low` and `high` representing the current search space. Compute `mid`, compare `arr[mid]` with the target, and discard the impossible half by updating either `low = mid + 1` or `high = mid - 1`. Continue until the target is found or the search space becomes empty.

**Crucial Snippets:**

```cpp
while (low <= high)
```

```cpp
int mid = low + (high - low) / 2;
```

```cpp
if (arr[mid] == x)
    return mid;
```

```cpp
if (arr[mid] < x)
    low = mid + 1;
else
    high = mid - 1;
```

→ **TC / SC:** **O(log n)** / **O(1)**

---

### WATCH OUT FOR
→ Using `low = mid` or `high = mid` instead of `mid ± 1`, which can cause an infinite loop.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so Binary Search is applicable.
2. I'll maintain `low` and `high` as the current search space.
3. Compute the middle safely and compare it with the target.
4. Eliminate one half by updating `low` or `high`.
5. Repeat until found; otherwise return `-1`.
````
