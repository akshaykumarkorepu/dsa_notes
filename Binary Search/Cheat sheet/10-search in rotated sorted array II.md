
# Search in Rotated Sorted Array II

**PATTERN:** Modified Binary Search (Rotated Sorted Array + Duplicates)  
→ **Trigger:** "when I see a rotated sorted array, need to search an element, and duplicates are allowed"

---

## BRUTE FORCE

→ **Idea:** Linearly scan the array and return true if the target is found.
→ **TC / SC:** **O(n)** / **O(1)**

---

## OPTIMAL

→ **First instinct:**  
"I immediately use Modified Binary Search, but before identifying the sorted half, I check if duplicates are creating ambiguity."

→ **Core idea:**  
Maintain two pointers: `low` and `high`. Compute `mid` every iteration.
- If `arr[mid] == target`, return true.
- If `arr[low] == arr[mid] && arr[mid] == arr[high]`, duplicates hide the pivot, so shrink both ends (`low++`, `high--`).
- Otherwise, identify the sorted half exactly like Search in Rotated Sorted Array-I and discard the other half.

**Crucial Snippets**

```cpp
if(arr[mid] == target)
    return true;
```

```cpp
if(arr[low] == arr[mid] &&
   arr[mid] == arr[high]) {
    low++;
    high--;
}
```

```cpp
else if(arr[low] <= arr[mid])
```

```cpp
if(arr[low] <= target &&
   target < arr[mid])
```

```cpp
if(arr[mid] < target &&
   target <= arr[high])
```

→ **TC / SC:**  
Average: **O(log n)**  
Worst: **O(n)** (many duplicates)  
Space: **O(1)**

---

## WATCH OUT FOR

→ Handling duplicates **after** checking the sorted half. Always handle

```cpp
arr[low] == arr[mid] && arr[mid] == arr[high]
```

first, otherwise you may choose the wrong half or get stuck.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. It's a rotated sorted array, so I'll use Modified Binary Search.
2. Since duplicates are allowed, they may hide the pivot.
3. If `low == mid == high`, I'll shrink both ends to remove ambiguity.
4. Otherwise, identify the sorted half exactly like Rotated Array-I.
5. Keep narrowing the search space until the target is found or the search space becomes empty.
````
