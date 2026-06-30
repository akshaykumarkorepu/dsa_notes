## Question: Contains Duplicate (Sorted vs Unsorted)

### PATTERN: Hashing / Adjacent Comparison / Two Pointers (depends on the input)
→ **Trigger:** "When I see duplicates, I first ask: **Is the array sorted? Do I need to preserve order? Is an in-place solution required?**"

---

### BRUTE FORCE

#### General Brute Force (Works for Any Array)

→ **Idea:** For every element, check all remaining elements. If any two elements are equal, a duplicate exists.

→ **Why:** This requires no extra data structure but repeatedly compares the same elements, making it inefficient.

→ **TC / SC:** **O(N²) / O(1)**

---

#### Better Approach (Optimal for Unsorted Arrays)

→ **Idea:** Traverse once while maintaining a **HashSet** of visited elements. If the current element already exists in the set, return `true`; otherwise insert it.

→ **Why:** Since duplicates can appear anywhere in an unsorted array, hashing provides constant-time lookup and avoids repeated comparisons.

→ **TC / SC:** **O(N) / O(N)**

---

#### Better Approach (Sorted Arrays)

→ **Idea:** Compare each element with its adjacent element (`arr[i]` and `arr[i-1]`). If they are equal, a duplicate exists.

→ **Why:** Sorting guarantees that all duplicates are consecutive, so adjacent comparison is sufficient.

→ **TC / SC:** **O(N) / O(1)**

---

### OPTIMAL

→ **First instinct:** "I immediately check whether the array is sorted. If it's unsorted, I use a HashSet. If it's sorted, I avoid hashing because the sorted property already gives me the answer efficiently."

→ **Core idea:**

- **Contains Duplicate (Sorted):** Keep comparing adjacent elements while traversing. If two neighbouring elements are equal, a duplicate exists.
- **Remove Duplicates In-place (Follow-up Problem):** Maintain two pointers:
  - **`i` = Read Pointer** → visits every element exactly once.
  - **`j` = Write Pointer** → points to the last unique element written.
  When `arr[i] != arr[j]`, we've found a new unique element, so increment `j` and copy `arr[i]` to `arr[j]`. This keeps the prefix `arr[0...j]` compressed with only unique elements while using **O(1)** extra space.

→ **TC / SC:**
- **Unsorted:** **O(N) / O(N)**
- **Sorted (Contains Duplicate):** **O(N) / O(1)**
- **Sorted (Remove Duplicates In-place):** **O(N) / O(1)**

---

### WATCH OUT FOR

→ **Don't blindly use a HashSet. First check if the array is sorted—recognizing the sorted property lets you eliminate unnecessary extra space and arrive at the optimal solution.**

---

### INTERVIEW FLOW (what I say out loud, in order)

1. "Before choosing the algorithm, I'd like to check whether the array is sorted."
2. "If it's unsorted, duplicates can appear anywhere, so I'd use a HashSet for O(N) lookup."
3. "If it's sorted, duplicates are adjacent, so hashing is unnecessary—I can simply compare neighbouring elements."
4. "If the follow-up asks me to remove duplicates in-place, I'll switch to Two Pointers: `i` reads every element, while `j` marks the last unique position and only moves when a new unique element is found."
5. "The final complexity is O(N), with O(N) space for unsorted arrays and O(1) extra space for sorted arrays."
```
