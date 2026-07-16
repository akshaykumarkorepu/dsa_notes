
# Number of Occurrences in Sorted Array

**PATTERN:** Binary Search on Boundaries (First & Last Occurrence)  
→ **Trigger:** *"when I see a sorted array with duplicates and need the frequency/count of a target."*

---

## BRUTE FORCE

→ **Idea:** Traverse the entire array and count every occurrence of the target.

→ **TC / SC:** `O(n)` / `O(1)`

---

## OPTIMAL

→ **First instinct:** *"I immediately find the first occurrence and last occurrence using Binary Search, then compute `last - first + 1`."*

→ **Core idea:** Perform Binary Search twice. In the first search, maintain `ans` and whenever `arr[mid] == target`, update `ans = mid` and continue searching left (`high = mid - 1`). In the second search, again maintain `ans`, but continue searching right (`low = mid + 1`). If the first occurrence is `-1`, return `0`; otherwise return `last - first + 1`.

**Crucial Snippets**

```cpp
// First Occurrence
if(arr[mid] == target){
    ans = mid;
    high = mid - 1;
}
```

```cpp
// Last Occurrence
if(arr[mid] == target){
    ans = mid;
    low = mid + 1;
}
```

```cpp
if(first == -1) return 0;
return last - first + 1;
```

→ **TC / SC:** `O(log n)` / `O(1)`

---

## WATCH OUT FOR

→ Forgetting to continue searching after finding the target—**first occurrence moves left (`high = mid - 1`), last occurrence moves right (`low = mid + 1`)**.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. The array is sorted, so Binary Search is applicable.
2. I'll find the first and last occurrence separately.
3. After finding the target, I continue searching toward the required boundary.
4. If the first occurrence doesn't exist, return `0`.
5. Otherwise, answer is `last - first + 1`.
````
