

**Question:** Union of Two Sorted Arrays

### PATTERN: Two Pointers (Merge Pattern)
→ **Trigger:** "When I see **two sorted arrays** and need a **sorted union/intersection/merge** in linear time."

---

### BRUTE FORCE
→ **Idea:** Insert all elements into a `set`, then convert the set to a vector (set automatically removes duplicates and keeps elements sorted).

**Crucial Snippet**
```cpp
set<int> st;

for(int x : a) st.insert(x);
for(int x : b) st.insert(x);

vector<int> ans(st.begin(), st.end());
```

→ **TC / SC:**
- **TC:** `O((n+m) log(n+m))`
- **SC:** `O(n+m)`

---

### OPTIMAL

→ **First instinct:**  
**"I immediately think of Merge Sort's two-pointer technique since both arrays are already sorted."**

→ **Core idea:**  
Maintain two pointers `i` and `j`. Compare `a[i]` and `b[j]`. Insert the smaller value (or one copy if equal) **only if it's different from the last inserted element**, then move the appropriate pointer(s). After one array finishes, process the remaining unique elements of the other array.

**Crucial Snippets**

**Duplicate Check**
```cpp
if(ans.empty() || ans.back() != current)
    ans.push_back(current);
```

**Three Cases**
```cpp
if(a[i] < b[j])      // push a[i], i++
else if(a[i] > b[j]) // push b[j], j++
else                 // push once, i++, j++
```

**Equal Case**
```cpp
// a[i] == b[j]
// Can push either a[i] or b[j]
i++;
j++;
```

→ **TC / SC:**
- **TC:** `O(n+m)`
- **SC:** `O(1)` *(excluding output array)*

---

### WATCH OUT FOR

→ Forgetting the duplicate check before every insertion. Always insert only if:

```cpp
ans.empty() || ans.back() != current
```

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Since both arrays are sorted, I'll use the Merge Sort two-pointer technique.
2. I'll compare `a[i]` and `b[j]` and insert the smaller unique element.
3. If both elements are equal, I'll insert only one copy and move both pointers.
4. After one array ends, I'll process the remaining unique elements of the other array.
5. This gives `O(n+m)` time with `O(1)` extra space.
```**````
`````
