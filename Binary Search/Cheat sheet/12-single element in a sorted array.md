

**Question:** Single Among Doubles in a Sorted

---

## PATTERN: Binary Search on Index Parity (Even-Odd Pair Pattern)

→ **Trigger:** *"When I see a **sorted array**, every element appears **twice except one**, and the expected complexity is **O(log N)**."*

---

## BRUTE FORCE

→ **Idea:** Check every adjacent pair. The first broken pair is the answer; if none breaks, the last element is the answer.

**Crucial Snippet**

```cpp
for(int i = 0; i < n-1; i += 2)
    if(arr[i] != arr[i+1])
        return arr[i];

return arr[n-1];
```

→ **TC / SC:** **O(N)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** *"I immediately think about the pair alignment. Before the unique element, pairs start at even indices; after it, pairs start at odd indices, so Binary Search can find where this pattern changes."*

→ **Core idea:**

Always force `mid` to be **even** so it points to the **first element of a pair**. Compare `arr[mid]` with `arr[mid+1]`.

- If the pair is correct (`arr[mid] == arr[mid+1]`), the single element must be on the **right**, so move `low = mid + 2`.
- Otherwise, the pairing has already broken, so the answer is at `mid` or to its left, hence `high = mid`.

Continue until `low == high`.

**Crucial Snippets**

```cpp
if(mid % 2 == 1)
    mid--;
```

```cpp
if(arr[mid] == arr[mid+1])
    low = mid + 2;
else
    high = mid;
```

```cpp
return arr[low];
```

→ **TC / SC:** **O(log N)** / **O(1)**

---

## WATCH OUT FOR

→ Forgetting to make `mid` **even** before comparing `arr[mid]` and `arr[mid+1]`, which results in comparing the second element of one pair with the first element of the next pair.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so I'll look for a Binary Search property.
2. Before the unique element, pairs start at even indices; after it, they start at odd indices.
3. I'll always make `mid` even so it points to the first element of a pair.
4. Compare `arr[mid]` and `arr[mid+1]` to decide which half contains the answer.
5. When `low == high`, that index is the single element.
````
