
## PROBLEM:
Given an integer array `arr[]` and an integer `target`, determine whether there exist **two distinct indices** `i` and `j` such that:

```
arr[i] + arr[j] = target
```

Return:
- `true` if such a pair exists.
- `false` otherwise.

---

## PATTERN:
**Hashing (Complement Lookup using Hash Set)**

---

## WHY THIS PATTERN:

The question asks whether **a pair exists** whose sum equals a target.

For every element, there is **only one value** that can pair with it:

```
needed = target - current
```

Instead of comparing the current element with every other element, we only need to know:

> **"Has the required complement already appeared?"**

Hashing allows us to answer this question in **O(1)** average time.

---

## CORE IDEA:

For every element:

1. Compute the number required to complete the target.

```
needed = target - current
```

2. Check whether `needed` has already been seen.

- If yes → Pair found.
- If no → Store the current element and continue.

Instead of searching for a pair, we search for the **complement**.

---

## BRUTE FORCE:

### Intuition

The simplest idea is to check every possible pair.

Fix one element and compare it with every element after it.

### Code

```cpp
bool twoSum(vector<int>& arr, int target) {

    int n = arr.size();

    for(int i = 0; i < n; i++) {

        for(int j = i + 1; j < n; j++) {

            if(arr[i] + arr[j] == target)
                return true;
        }
    }

    return false;
}
```

### Dry Run

```
arr = [0,-1,2,-3,1]
target = -2

i = 0
0 + (-1)
0 + 2
0 + (-3)
0 + 1

No pair

i = 1
-1 + 2
-1 + (-3)
-1 + 1

No pair

i = 2
2 + (-3)
2 + 1

No pair

i = 3
-3 + 1 = -2

Found
```

### Time Complexity

- Two nested loops.
- Every pair is checked once.

**TC = O(n²)**

### Space Complexity

- No extra data structure is used.

**SC = O(1)**

---

## OPTIMAL APPROACH:

### Observation

Brute force compares the current element with every other element.

But for any current element, there is only **one** number that can complete the target.

Example:

```
target = 10

current = 7

needed = 3
```

We don't care about every previous number.

We only care whether **3 exists**.

To check this efficiently, we store previously seen elements in a **Hash Set**.

For every element:

- Compute the complement.
- Check whether it exists in the set.
- If yes, return `true`.
- Otherwise, insert the current element.

---

## ALGORITHM:

1. Create an empty `unordered_set`.
2. Traverse the array.
3. For every element:
   - Compute `needed = target - current`.
   - If `needed` exists in the set, return `true`.
   - Otherwise, insert the current element into the set.
4. If traversal finishes, return `false`.

---

## DRY RUN:

```
arr = [0,-1,2,-3,1]
target = -2

Hash Set = {}
```

### Iteration 1

```
Current = 0

Needed = -2

Present?

No

Insert 0

Set = {0}
```

---

### Iteration 2

```
Current = -1

Needed = -1

Present?

No

Insert

Set = {0,-1}
```

---

### Iteration 3

```
Current = 2

Needed = -4

Present?

No

Insert

Set = {0,-1,2}
```

---

### Iteration 4

```
Current = -3

Needed = 1

Present?

No

Insert

Set = {0,-1,2,-3}
```

---

### Iteration 5

```
Current = 1

Needed = -3

Present?

Yes

1 + (-3) = -2

Return true
```

---

## IMPORTANT CODE SNIPPETS:

### Compute complement

```cpp
int needed = target - num;
```

### Check if complement exists

```cpp
if(st.find(needed) != st.end())
    return true;
```

### Store current element

```cpp
st.insert(num);
```

### Complete traversal

```cpp
unordered_set<int> st;

for(int num : arr){

    int needed = target - num;

    if(st.find(needed) != st.end())
        return true;

    st.insert(num);
}

return false;
```

---

## COMMON MISTAKES:

### 1. Inserting before checking

Wrong:

```cpp
st.insert(num);

if(st.count(target-num))
```

Example:

```
arr = [5]
target = 10
```

After inserting, the set already contains `5`, so you incorrectly use the same element twice.

Always:

> **Check first → Insert later**

---

### 2. Forgetting that indices must be different

A number cannot pair with itself unless another occurrence exists.

---

### 3. Using a vector for lookup

Searching a vector takes **O(n)**.

Hash Set provides **O(1)** average lookup.

---

### 4. Using unordered_map unnecessarily

Only existence matters.

Hash Set is sufficient.

---

## WHY I MIGHT FORGET THIS:

Because I start thinking:

> **"Which two numbers make the target?"**

Instead, always think:

> **"Current number is fixed. What number do I need?"**

Once I compute the required complement, the problem becomes a simple lookup.

---

## INTERVIEW FLOW:

### Step 1

The brute force solution is to check every pair using two nested loops.

Time Complexity:

```
O(n²)
```

---

### Step 2

Observation:

For every element,

I don't need to compare it with every other element.

I only need one specific value:

```
needed = target - current
```

---

### Step 3

Now the problem becomes:

> **"Can I quickly check whether the required complement already exists?"**

Searching in an array still takes **O(n)**.

So I use a **Hash Set**, which supports average **O(1)** lookup.

---

### Step 4

Traverse the array once.

For every element:

- Compute complement.
- Check Hash Set.
- If found → return `true`.
- Otherwise insert current element.

---

### Step 5

Check before inserting,

otherwise we may use the same element twice.

---

## TIME COMPLEXITY:

### Brute Force

Two nested loops.

```
O(n²)
```

---

### Optimal

The array is traversed once.

For every element:

- Lookup in Hash Set → O(1) average
- Insertion → O(1) average

Overall:

```
O(n × (1 + 1))
= O(n)
```

**Reason:** We perform one lookup and one insertion for each of the `n` elements.

---

## SPACE COMPLEXITY:

### Brute Force

No extra space.

```
O(1)
```

---

### Optimal

The Hash Set may store every element.

```
O(n)
```

**Reason:** In the worst case, no pair is found, so all `n` elements are stored.

---

## EDGE CASES:

### Single element

```
[5]

false
```

---

### Duplicate elements

```
[3,3]

target = 6

true
```

---

### Negative numbers

```
[-3,1]

target = -2

true
```

---

### Target = 0

```
[-2,2]

true
```

---

### No valid pair

```
[1,2,3]

target = 10

false
```

---

## PATTERN RECOGNITION:

Think of **Hashing (Complement Lookup)** whenever you see:

- Two Sum
- Pair with given sum
- Check if pair exists
- Pair equals target
- Count pairs with sum K
- Find complement
- "Does another element satisfy this condition?"

Always ask yourself:

> **"If I fix one number, can I compute exactly what I need?"**

If yes,

the solution is usually:

> **Compute Complement → Lookup using Hashing**

---

# CLEAN C++ CODE

```cpp
class Solution {
public:
    bool twoSum(vector<int>& arr, int target) {

        unordered_set<int> st;

        for (int num : arr) {

            int needed = target - num;

            if (st.find(needed) != st.end())
                return true;

            st.insert(num);
        }

        return false;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

```cpp
unordered_set<int> st;
```

Stores all previously seen numbers.
Provides **O(1)** average insertion and lookup.

---

```cpp
for(int num : arr)
```

Traverse every element exactly once.

---

```cpp
int needed = target - num;
```

Compute the only value that can pair with the current number.

---

```cpp
if(st.find(needed) != st.end())
```

Check whether we've already seen the required complement.

---

```cpp
return true;
```

A valid pair is found, so stop immediately.

---

```cpp
st.insert(num);
```

Store the current number **after checking**, so the same element is never used twice.

---

```cpp
return false;
```

No valid pair exists if the loop finishes.

---

# EASY-TO-REMEMBER SUMMARY

### Pattern

**Hashing + Complement Lookup**

### Formula

```
needed = target - current
```

### Steps

1. Traverse the array once.
2. Compute the complement.
3. Check whether the complement exists in the Hash Set.
4. If yes → return `true`.
5. Otherwise insert the current element.
6. If traversal finishes → return `false`.

### Memory Trick

> **Fix one number → Compute what you need → Ask the Hash Set if you've already seen it.**
