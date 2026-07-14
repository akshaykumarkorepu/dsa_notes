
## Question: Subarrays with Sum K

### PATTERN: Prefix Sum + HashMap (Frequency Map)
→ **Trigger:** "when I see **count subarrays with sum = K**, especially when the array contains **negative numbers**."

---

### BRUTE FORCE
→ **Idea:** Try every starting index, extend the ending index while maintaining the running sum, and increment the count whenever the sum becomes `k`.

**Crucial Snippet**
```cpp
for(int i=0;i<n;i++){
    int sum = 0;
    for(int j=i;j<n;j++){
        sum += arr[j];
        if(sum == k) count++;
    }
}
```

→ **TC / SC:** `O(N²)` / `O(1)`

---

### OPTIMAL

→ **First instinct:** "I immediately think of **Prefix Sum + HashMap** because negative numbers make Sliding Window invalid."

→ **Core idea:** Maintain a running `prefixSum` and a HashMap storing `prefixSum -> frequency`. At each index, check whether `(prefixSum - k)` already exists in the map. Every occurrence represents one valid subarray ending at the current index. Then store the current `prefixSum` for future indices.

**Crucial Snippets**
```cpp
unordered_map<int,int> mp;
mp[0] = 1;
```

```cpp
prefixSum += arr[i];
```

```cpp
if(mp.find(prefixSum-k) != mp.end())
    count += mp[prefixSum-k];
```

```cpp
mp[prefixSum]++;
```

→ **TC / SC:** `O(N)` (average) / `O(N)`

---

### WATCH OUT FOR

→ **Don't forget `mp[0] = 1`; otherwise you'll miss subarrays starting from index `0`.**

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every subarray in `O(N²)`.
2. Negative numbers eliminate Sliding Window.
3. Maintain a running `prefixSum` and store its frequency in a HashMap.
4. At each index, look for `prefixSum - k`; every occurrence gives one valid subarray.
5. Add its frequency to the answer, then store the current `prefixSum`.
````
