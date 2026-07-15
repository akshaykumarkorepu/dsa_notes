
# TWO SUM

## PATTERN: Hashing (Complement Lookup) + Sorting & Two Pointers
→ **Trigger:** "when I see *find/check whether a pair sums to a target* in an unsorted array."

---

## BRUTE FORCE
→ **Idea:** Check every possible pair using two nested loops.

```cpp
for(int i=0;i<n;i++)
    for(int j=i+1;j<n;j++)
        if(arr[i]+arr[j]==target)
            return true;
```

→ **TC / SC:** **O(n²)** / **O(1)**

---

## OPTIMAL (Fastest Runtime – Hashing)

→ **First instinct:** "I immediately compute the complement (`target - current`) and check if I've already seen it."

→ **Core idea:**

Maintain an `unordered_set` containing all previously seen numbers.

For every element:

- Compute the required complement.
- If it already exists in the set, a valid pair is found.
- Otherwise, insert the current element and continue.

State maintained:

- `unordered_set<int> st` → stores previously seen numbers.
- `needed = target - num`

**Key Snippets**

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

→ **TC / SC:** **O(n)** average / **O(n)**

---

## ALTERNATIVE (Space Optimized – Sorting + Two Pointers)

→ **First instinct:** "If modifying the array is allowed, sort it and search from both ends."

→ **Core idea:**

Sort the array first.

Maintain two pointers:

- `left` → smallest element
- `right` → largest element

At every step:

- If current sum equals target → found.
- If sum is smaller → move `left++` to increase the sum.
- If sum is larger → move `right--` to decrease the sum.

State maintained:

- `left`
- `right`
- `sum`

**Key Snippets**

```cpp
sort(arr.begin(), arr.end());
```

```cpp
int left = 0;
int right = n - 1;
```

```cpp
int sum = arr[left] + arr[right];
```

```cpp
if(sum == target)
    return true;
else if(sum < target)
    left++;
else
    right--;
```

→ **TC / SC:** **O(n log n)** / **O(1)** *(assuming in-place sorting)*

---

## WATCH OUT FOR

**Hashing:** Always **check before inserting**, otherwise the same element may be used twice.

```cpp
// Correct
if(st.count(target-num))
    return true;

st.insert(num);
```

---

## INTERVIEW FLOW (what I say out loud)

1. Brute force checks every pair in **O(n²)**.
2. Observation: for every number, I only need its complement.
3. Use a Hash Set for **O(1)** average lookup, giving **O(n)** time.
4. Mention the alternative: sort + two pointers if **O(1)** extra space is preferred.
5. Check before inserting in the Hash Set to avoid using the same element twice.
````
