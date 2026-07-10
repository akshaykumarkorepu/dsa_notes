

## Question: Two Sum - Pair with Given Sum

### PATTERN: Hashing (Complement Lookup)
→ **Trigger:** "when I see pair/two numbers whose sum equals a target and only existence or indices are required"

---

### BRUTE FORCE
→ **Idea:** Check every possible pair using two nested loops and return true if any pair sums to the target.
→ **TC / SC:** **O(n²)** / **O(1)**

---

### OPTIMAL
→ **First instinct:** "I immediately compute the complement (`target - current`) and check whether I've already seen it using a Hash Set."

→ **Core idea:** Traverse the array once while maintaining an `unordered_set<int> st` of previously seen elements. For every `num`, compute `needed = target - num`. If `needed` exists in `st`, a valid pair is found. Otherwise, insert `num` into the set and continue.

**Crucial Snippets:**

```cpp
int needed = target - num;
```

```cpp
if(st.find(needed) != st.end())
    return true;
```

```cpp
st.insert(num);
```

→ **TC / SC:** **O(n)** / **O(n)**

---

### WATCH OUT FOR
→ **Always check the complement before inserting the current element**, otherwise you may incorrectly use the same element twice (e.g., `[5]`, target = `10`).

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every pair using two nested loops → **O(n²)**.
2. Observation: for every element, I only need its complement (`target - current`).
3. Store previously seen elements in a Hash Set for **O(1)** average lookup.
4. For each element, check if the complement exists; if yes, return true; otherwise insert the current element.
5. Single traversal gives **O(n)** time with **O(n)** extra space.
````
