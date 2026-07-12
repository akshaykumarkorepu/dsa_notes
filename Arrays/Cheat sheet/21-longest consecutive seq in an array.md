
# Longest Consecutive Subsequence

**PATTERN:** Hashing + Sequence Start Detection  
→ **Trigger:** "when I see an unsorted array, consecutive numbers, and need O(n) time"

---

## BRUTE FORCE

→ **Idea:** For every element, assume it is the start of a sequence and repeatedly search the array for `current + 1` using linear search.

**Crucial Snippet**
```cpp
while(find(arr.begin(), arr.end(), current + 1) != arr.end()) {
    current++;
    len++;
}
```

→ **TC / SC:** **O(n²)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately store all elements in an `unordered_set` for O(1) lookups and only start counting from sequence beginnings."

→ **Core idea:** Insert all elements into a HashSet. Traverse every number, but only start expanding if `num - 1` is absent (meaning this is the first element of a sequence). Maintain `current` (walking pointer) and `len` (current sequence length), keep moving while `current + 1` exists, and update `ans`.

**Crucial Snippets**
```cpp
unordered_set<int> st;
for(int x : arr) st.insert(x);
```

```cpp
if(st.find(num - 1) == st.end())
```

```cpp
while(st.find(current + 1) != st.end()) {
    current++;
    len++;
}
```

```cpp
ans = max(ans, len);
```

→ **TC / SC:** **O(n)** (average case) / **O(n)**

---

## WATCH OUT FOR

→ Using `num + 1` instead of `current + 1` inside the `while` loop. `num` never changes; only `current` moves through the sequence.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force repeatedly searches the array for the next consecutive element, giving O(n²).
2. I'll store all elements in an `unordered_set` for O(1) existence checks.
3. I'll only start counting from numbers whose previous element (`num - 1`) doesn't exist.
4. From each starting point, I'll expand using `current + 1`, maintaining `current` and `len`.
5. Update the maximum sequence length and return the answer.
````
