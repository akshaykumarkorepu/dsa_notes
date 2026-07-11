
# Sort 0s, 1s and 2s

## PATTERN: Dutch National Flag (Three-Pointer Partitioning)
→ **Trigger:** "when I see an array with exactly 3 distinct values, asked to group/sort them in one pass with O(1) extra space."

---

## BRUTE FORCE
→ **Idea:** Count the number of 0s, 1s, and 2s, then overwrite the array with those counts.
→ **Crucial Snippet:**
```cpp
while(count0--) arr[idx++] = 0;
while(count1--) arr[idx++] = 1;
while(count2--) arr[idx++] = 2;
```
→ **TC / SC:** `O(N)` (2 passes) / `O(1)`

---

## OPTIMAL

→ **First instinct:** "I immediately think of Dutch National Flag with three pointers: low, mid, and high."

→ **Core idea:**
Maintain four regions:
- `0 ... low-1` → all 0s
- `low ... mid-1` → all 1s
- `mid ... high` → unknown
- `high+1 ... n-1` → all 2s

Process only `arr[mid]`:
- If `0` → swap with `low`, then `low++`, `mid++`
- If `1` → just `mid++`
- If `2` → swap with `high`, then `high--` (**don't move `mid`** because the swapped element is still unprocessed).

→ **Crucial Snippets:**
```cpp
while(mid <= high)
```

```cpp
if(arr[mid] == 0){
    swap(arr[low], arr[mid]);
    low++;
    mid++;
}
```

```cpp
else if(arr[mid] == 1){
    mid++;
}
```

```cpp
else{
    swap(arr[mid], arr[high]);
    high--;
}
```

→ **TC / SC:** `O(N)` / `O(1)`

---

## WATCH OUT FOR

→ **Never increment `mid` after swapping with `high`; the new element at `mid` has not been processed yet.**

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force is counting frequencies and rewriting the array in two passes.
2. Since the follow-up asks for one pass, I'll use the Dutch National Flag algorithm.
3. Maintain three pointers: `low`, `mid`, and `high` to create four regions.
4. Process `arr[mid]`: 0 → front, 1 → skip, 2 → end.
5. After swapping with `high`, don't increment `mid` because the swapped element is still unknown.
````
