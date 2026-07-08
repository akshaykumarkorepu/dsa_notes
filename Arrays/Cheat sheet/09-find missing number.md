

**Question:** Missing in Array

---

## PATTERN: XOR / Missing Number Pattern
→ **Trigger:** "When I see numbers from **1 to n**, exactly **one missing**, all elements are **distinct**, and O(1) extra space is expected."

---

## BRUTE FORCE

### Idea:
Check every number from **1 to n** and linearly search for it in the array. The first number not found is the answer.

**Crucial Snippet**

```cpp
for(int i=1;i<=n;i++){
    bool found = false;

    for(int num : arr){
        if(num == i){
            found = true;
            break;
        }
    }

    if(!found)
        return i;
}
```

→ **TC / SC:**

```
O(n²) / O(1)
```

---

## BETTER (Hashing)

### Idea:
Store which numbers are present using a boolean array. Traverse once to mark presence, then find the first missing index.

**Crucial Snippet**

```cpp
vector<bool> visited(n+1,false);

for(int num : arr)
    visited[num] = true;

for(int i=1;i<=n;i++)
    if(!visited[i])
        return i;
```

→ **TC / SC:**

```
O(n) / O(n)
```

---

## BETTER (Sum Formula)

### Idea:
Compute the expected sum using **n(n+1)/2**, compute the actual array sum, and subtract them.

**Crucial Snippet**

```cpp
long long expected = 1LL * n * (n + 1) / 2;

long long actual = 0;

for(int num : arr)
    actual += num;

return expected - actual;
```

→ **TC / SC:**

```
O(n) / O(1)
```

---

## OPTIMAL

→ **First instinct:**
"I immediately think of XOR because every number except one appears exactly once."

→ **Core idea:**
XOR all numbers from **1 to n**, then XOR every array element. Every common number cancels itself (`a ^ a = 0`), leaving only the missing number. Maintain a single variable `xorAns` throughout the traversal.

**Crucial Snippet**

```cpp
int xorAns = 0;

for(int i=1;i<=n;i++)
    xorAns ^= i;

for(int num : arr)
    xorAns ^= num;

return xorAns;
```

→ **TC / SC:**

```
O(n) / O(1)
```

---

## WATCH OUT FOR

→ Using `n = arr.size()` instead of **`n = arr.size() + 1`**, which makes the algorithm miss the last number in the range.

---

## INTERVIEW FLOW

1. Brute force: Check every number using linear search.
2. Improve using hashing to avoid repeated searches.
3. Mention the mathematical sum approach and its overflow limitation.
4. Use XOR since duplicates cancel (`a ^ a = 0`).
5. XOR complete range with array and return the remaining value.
