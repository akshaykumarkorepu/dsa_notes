

## Question: Koko Eating Bananas

### PATTERN: Binary Search on Answer
→ **Trigger:** "When I see a question asking for the **minimum/maximum value** that satisfies a condition, and if one answer works then all larger (or smaller) answers also work (monotonic property)."

---

### BRUTE FORCE
→ **Idea:** Try every eating speed from `1` to `max(arr)`. For each speed, calculate the total hours needed using ceiling division. Return the first valid speed.

**Crucial Snippet:**
```cpp
int maxPile = *max_element(arr.begin(), arr.end());

for (int speed = 1; speed <= maxPile; speed++) {

    long long hours = 0;

    for (int pile : arr)
        hours += (pile + speed - 1) / speed;

    if (hours <= k)
        return speed;
}
```

→ **TC / SC:**
- **Time:** `O(n × maxPile)`
- **Space:** `O(1)`

---

### OPTIMAL

→ **First instinct:** "I immediately binary search on the eating speed because if one speed works, every larger speed also works."

→ **Core idea:** Binary search over the answer space `[1, max(arr)]`. For every candidate speed `mid`, compute the total hours using `hours += ceil(pile / mid)`. If `hours <= k`, store `mid` as the current answer and search the left half for a smaller valid speed; otherwise search the right half.

**Crucial Snippets:**

**Search Space**
```cpp
int low = 1;
int high = *max_element(arr.begin(), arr.end());
```

**Middle**
```cpp
int mid = low + (high - low) / 2;
```

**Feasibility Check**
```cpp
long long hours = 0;

for (int pile : arr)
    hours += (pile + mid - 1) / mid;
```

**Binary Search Decision**
```cpp
if (hours <= k) {
    ans = mid;
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

→ **TC / SC:**
- **Time:** `O(n log(max(arr)))`
- **Space:** `O(1)`

---

### WATCH OUT FOR

→ Forgetting **ceiling division**. Never use `pile / speed`; always use:

```cpp
(pile + speed - 1) / speed
```

---

### INTERVIEW FLOW (what I say out loud, in order)

1. "Brute force is to try every eating speed from 1 to the maximum pile."
2. "That is O(n × maxPile), which is too slow."
3. "The answer is monotonic—if one speed works, every larger speed also works."
4. "I'll binary search the answer and calculate the required hours for each candidate speed."
5. "If the hours fit within `k`, I store the answer and search left; otherwise I search right."
````
