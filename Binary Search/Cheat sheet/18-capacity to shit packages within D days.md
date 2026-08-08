
# PATTERN: Binary Search on Answer
→ **Trigger:** "When I need the **minimum capacity/value** that satisfies a condition, and increasing the answer makes the condition easier (monotonic: FFFFFTTTT)."

---

# BRUTE FORCE

→ **Idea:** Try every capacity from `max(arr)` to `sum(arr)` and simulate shipping until you find the first capacity that finishes within `D` days.

**Crucial Snippets**
```cpp
int low = *max_element(arr.begin(), arr.end());
int high = accumulate(arr.begin(), arr.end(), 0);

for (int capacity = low; capacity <= high; capacity++) {
    if (findDays(arr, capacity) <= D)
        return capacity;
}
```

```cpp
if (load + weight <= capacity)
    load += weight;
else {
    day++;
    load = weight;
}
```

→ **TC / SC:**
- **Time:** `O((Sum - Max + 1) × N)`
- **Space:** `O(1)`

---

# OPTIMAL

→ **First instinct:** "I immediately Binary Search on the answer (capacity) from `max(arr)` to `sum(arr)`."

→ **Core idea:** Maintain the search space `[low, high]`, where `low = max(arr)` and `high = sum(arr)`. For every `mid` capacity, simulate shipping using `findDays()`, maintaining `load` and `day`. If `requiredDays <= D`, store `mid` as a possible answer and search left (`high = mid - 1`); otherwise search right (`low = mid + 1`). This works because required days decrease monotonically as capacity increases.

**Crucial Snippets**
```cpp
int low = *max_element(arr.begin(), arr.end());
long long high = accumulate(arr.begin(), arr.end(), 0LL);
```

```cpp
while (low <= high) {
    int mid = low + (high - low) / 2;
```

```cpp
int requiredDays = findDays(arr, mid);

if (requiredDays <= D) {
    ans = mid;
    high = mid - 1;
}
else {
    low = mid + 1;
}
```

```cpp
if (load + weight <= capacity)
    load += weight;
else {
    day++;
    load = weight;
}
```

→ **TC / SC:**
- **Time:** `O(N × log(Sum - Max))`
- **Space:** `O(1)`

---

# WATCH OUT FOR

→ Start the search from `max(arr)` (not `0`) because every individual package must fit inside the boat, and when starting a new day don't forget `load = weight`.

---

# INTERVIEW FLOW (what I say out loud, in order)

1. Search space is `max(arr)` to `sum(arr)`.
2. Capacity vs required days is monotonic, so Binary Search on Answer.
3. Simulate shipping using `findDays(capacity)`.
4. If `requiredDays <= D`, store answer and search left.
5. Otherwise search right until the minimum valid capacity is found.
````
