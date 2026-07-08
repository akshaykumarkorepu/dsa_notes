
## Question: Max Consecutive Bit

### PATTERN: Linear Traversal + Consecutive Count (Run Length Counting)
→ **Trigger:** "when I see longest consecutive / maximum streak / continuous block / longest run of identical adjacent elements"

---

### BRUTE FORCE
→ **Idea:** Start from every index, count how long the same bit continues, and keep the maximum streak.

**Crucial Snippet**
```cpp
for(int i=0;i<n;i++){
    int count=1;
    for(int j=i+1;j<n;j++){
        if(arr[j]==arr[i]) count++;
        else break;
    }
    maxCount=max(maxCount,count);
}
```

→ **TC / SC:** **O(N²)** / **O(1)**

---

### OPTIMAL

→ **First instinct:** "I immediately compare every element with its previous element and maintain the current streak."

→ **Core idea:** Maintain two variables:
- `currentCount` → length of the current consecutive streak.
- `maxCount` → longest streak seen so far.

Traverse from index `1`. If `arr[i] == arr[i-1]`, extend the current streak (`currentCount++`); otherwise, reset it to `1` since the current element starts a new streak. Update `maxCount` after every iteration.

**Crucial Snippet**
```cpp
if(arr[i]==arr[i-1])
    currentCount++;
else
    currentCount=1;

maxCount=max(maxCount,currentCount);
```

→ **TC / SC:** **O(N)** / **O(1)**

---

### WATCH OUT FOR

→ Reset `currentCount` to **1**, not **0**, because the current element itself starts a new streak.

---

### INTERVIEW FLOW (what I say out loud, in order)

1. Brute force counts consecutive bits starting from every index, causing repeated work.
2. I can process each streak only once using a single traversal.
3. I'll maintain `currentCount` for the current streak and `maxCount` for the answer.
4. If the current bit matches the previous one, extend the streak; otherwise, reset it to 1.
5. Update the maximum after every iteration and return it.
````
