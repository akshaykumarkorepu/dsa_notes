

**Question:** Longest Subarray with Sum K

---

## PATTERN: Prefix Sum + HashMap (First Occurrence)
**→ Trigger:** *"When I see a contiguous subarray with a target sum, especially when the array contains negative numbers."*

---

## BRUTE FORCE
**→ Idea:** Start every subarray, extend it while maintaining a running sum, and update the maximum length whenever the sum becomes `k`.

**Crucial Snippet**
```cpp
sum += arr[j];

if(sum == k)
    ans = max(ans, j - i + 1);
```

**→ TC / SC:** `O(N²)` / `O(1)`

---

## OPTIMAL

**→ First instinct:** *"I immediately think of Prefix Sum + HashMap because negative numbers make Sliding Window invalid."*

**→ Core idea:** Maintain a running `prefix` sum and a HashMap storing **prefix sum → first occurrence index**. At every index, check whether `prefix - k` already exists. If it does, a valid subarray is found, and its length is `i - storedIndex`. Also handle the case where `prefix == k`, and store each prefix only the first time it appears.

**Crucial Snippets**
```cpp
prefix += arr[i];
```

```cpp
if(prefix == k)
    ans = i + 1;
```

```cpp
if(mp.find(prefix - k) != mp.end())
    ans = max(ans, i - mp[prefix - k]);
```

```cpp
if(mp.find(prefix) == mp.end())
    mp[prefix] = i;
```

**→ TC / SC:** `O(N)` / `O(N)`

---

## WATCH OUT FOR

**→** Store **only the first occurrence** of every prefix sum; overwriting it can reduce the maximum subarray length.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every subarray in `O(N²)`.
2. Observation: subarray sum = current prefix − previous prefix.
3. Need to find `(prefix - k)` efficiently using a HashMap.
4. Store **prefix sum → first occurrence index**.
5. Update answer using `i - storedIndex`, handling `prefix == k` separately.
````
