
# Move All Zeroes to End

## PATTERN: Two Pointers (Read Pointer + Write Pointer / Stable Compaction)
→ **Trigger:** "when I see *move specific elements to one side while preserving relative order and doing it in-place*"

---

## BRUTE FORCE

→ **Idea:** Store all non-zero elements in a temporary array, append zeros, then copy back.

**Crucial Snippets**

```cpp
if(arr[i] != 0){
    temp.push_back(arr[i]);
}
```

```cpp
while(temp.size() < n){
    temp.push_back(0);
}
```

```cpp
arr = temp;
```

→ **TC / SC:** `O(n)` / `O(n)`

---

## OPTIMAL

→ **First instinct:** "I immediately use two pointers—`i` scans the array and `j` marks where the next non-zero should be placed."

→ **Core idea:** Maintain `j` as the next position for a non-zero element. Traverse using `i`. Whenever `arr[i]` is non-zero, swap it with `arr[j]` and increment `j`. Since `i` visits elements from left to right, the relative order of non-zero elements is preserved while zeros naturally move to the end.

**Crucial Snippets**

```cpp
int j = 0;
```

```cpp
if(arr[i] != 0){
    swap(arr[i], arr[j]);
    j++;
}
```

→ **TC / SC:** `O(n)` / `O(1)`

---

## WATCH OUT FOR

→ **Don't think `j` points to a zero. It points to the next position where a non-zero element should be placed. Increment `j` only after placing a non-zero.**

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force is to collect all non-zero elements into a temporary array and append zeros.
2. To optimize, I'll avoid the extra array and use two pointers.
3. `i` scans every element while `j` tracks the next position for a non-zero.
4. Whenever I find a non-zero, I swap it with `arr[j]` and increment `j`.
5. This preserves the order of non-zero elements and moves all zeros to the end in `O(n)` time and `O(1)` space.
````
