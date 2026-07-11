
# Majority Element (Boyer-Moore Voting Algorithm)

**PATTERN:** Boyer-Moore Voting Algorithm (Voting / Pairwise Cancellation)  
→ **Trigger:** "when I see an element occurring **more than n/2 times** and the expected solution is **O(n) time with O(1) space**"

---

## BRUTE FORCE
→ **Idea:** For every element, count its frequency by scanning the entire array. Return the element if its frequency is greater than n/2.

**Snippet**
```cpp
for(int i=0;i<n;i++){
    int cnt=0;
    for(int j=0;j<n;j++)
        if(arr[i]==arr[j]) cnt++;

    if(cnt>n/2) return arr[i];
}
```

→ **TC / SC:** `O(n²) / O(1)`

---

## BETTER
→ **Idea:** Store the frequency of every element using a HashMap, then find the element whose frequency is greater than n/2.

**Snippet**
```cpp
unordered_map<int,int> mp;

for(int num : arr)
    mp[num]++;

for(auto it : mp)
    if(it.second > n/2)
        return it.first;
```

→ **TC / SC:** `O(n) / O(n)`

---

## OPTIMAL

→ **First instinct:** "I immediately think of Boyer-Moore Voting because the majority element survives pairwise cancellation."

→ **Core idea:** Maintain two variables: `candidate` and `count`. If `count == 0`, make the current element the new candidate. If the current element matches the candidate, increment `count`; otherwise decrement it. This simulates pairwise cancellation. Finally, verify the candidate by counting its frequency since the problem does not guarantee a majority element.

**Key Snippets**
```cpp
if(count == 0){
    candidate = num;
    count = 1;
}
else if(num == candidate)
    count++;
else
    count--;
```

```cpp
int freq = 0;

for(int num : arr)
    if(num == candidate)
        freq++;

if(freq > n/2)
    return candidate;

return -1;
```

→ **TC / SC:** `O(n) / O(1)`

---

## WATCH OUT FOR

→ Forgetting the **verification pass**. Boyer-Moore only finds a **candidate**; if the problem doesn't guarantee a majority element, you **must** count its frequency before returning it.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force is checking the frequency of every element (`O(n²)`).
2. Better approach is using a HashMap to store frequencies (`O(n)` space).
3. Observe that a majority element survives pairwise cancellation.
4. Apply Boyer-Moore Voting to find a candidate in one pass.
5. Verify the candidate's frequency and return it if it's greater than `n/2`.
````
