
**Question:** Unique Number I

---

## PATTERN: XOR (Pair Cancellation)
→ **Trigger:** *"when I see every element appears exactly twice except one, and O(1) extra space is expected."*

---

## BRUTE FORCE

→ **First instinct:** "I immediately count the frequency of every element."

→ **Idea:** For each element, traverse the entire array to count its occurrences. If its frequency is 1, return it. This helps derive the optimal solution by recognizing we're repeatedly counting duplicates.

**Crucial Snippet**
```cpp
for(int i = 0; i < n; i++) {
    int count = 0;
    for(int j = 0; j < n; j++)
        if(arr[i] == arr[j]) count++;

    if(count == 1) return arr[i];
}
```

→ **TC / SC:** **O(n²) / O(1)**

---

## OPTIMAL

→ **First instinct:** "I immediately use XOR because duplicate pairs cancel each other."

→ **Core idea:** Maintain a running XOR variable `ans` initialized to 0. XOR every array element with `ans`. Since `a ^ a = 0` and `0 ^ a = a`, every duplicate cancels out, leaving only the unique element in `ans`.

**Crucial Snippet**
```cpp
int ans = 0;

for(int num : arr)
    ans ^= num;

return ans;
```

→ **TC / SC:** **O(n) / O(1)**

---

## WATCH OUT FOR

→ Don't check frequency inside the inner loop in the brute-force solution; complete counting first, then check `count == 1`.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force is counting frequency for every element.
2. Since every duplicate appears exactly twice, XOR is a better choice.
3. Maintain one running XOR variable `ans`.
4. Duplicate pairs cancel because `a ^ a = 0`.
5. Final `ans` is the unique element.
````
