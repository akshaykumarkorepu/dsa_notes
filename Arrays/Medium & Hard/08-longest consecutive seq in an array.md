# Longest Consecutive Subsequence

## PROBLEM:
Given an **unsorted array**, find the **length of the longest sequence of consecutive integers**.

- Consecutive numbers can appear **in any order**.
- We only need the **length**, not the actual sequence.
- This is a **subsequence**, **not a subarray**, so the elements do **not** need to be adjacent.

**Example**

```text
arr = [2,6,1,9,4,5,3]

Longest consecutive sequence:
1 2 3 4 5 6

Answer = 6
```

---

## PATTERN:

**Hashing + Sequence Start Detection**

**Trigger:**
> "Find the longest consecutive sequence in an unsorted array."

---

## WHY THIS PATTERN:

The array is **unsorted**.

In brute force, every time we want to know whether the next number exists, we scan the entire array.

Instead,

- Store all elements inside a **HashSet**.
- Checking whether a number exists becomes **O(1)**.
- Only start counting from the **beginning of a sequence**.

This removes repeated work.

---

## CORE IDEA:

Every consecutive sequence has exactly **one starting point**.

Example

```text
1 2 3 4 5 6
```

Only

```text
1
```

should start counting.

Why?

Because

```text
2
```

already has

```text
1
```

before it.

Similarly,

```text
3
```

already has

```text
2
```

before it.

So,

A number is the **start of a sequence** if

```cpp
num - 1
```

does **not** exist.

---

# BRUTE FORCE

## Idea

For every element,

assume it is the beginning of a sequence.

Keep checking whether

```text
current + 1
```

exists by scanning the entire array.

---

## Code

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& arr) {

        int n = arr.size();
        int ans = 1;

        for (int i = 0; i < n; i++) {

            int current = arr[i];
            int len = 1;

            while (find(arr.begin(), arr.end(), current + 1) != arr.end()) {
                current++;
                len++;
            }

            ans = max(ans, len);
        }

        return ans;
    }
};
```

---

## Dry Run

```text
arr = [1,2,3,4]
```

Start from

```text
1
```

Search

```text
2 ✓
3 ✓
4 ✓
5 ✗
```

Length = 4

Now start from

```text
2
```

Again search

```text
3 ✓
4 ✓
5 ✗
```

Same work repeated.

---

## Time Complexity

Outer loop = O(n)

Every `find()` = O(n)

Overall

```text
O(n²)
```

---

## Space Complexity

```text
O(1)
```

---

# OPTIMAL APPROACH

Store every element inside an **unordered_set**.

Now checking

```text
Does x exist?
```

takes

```text
O(1)
```

instead of

```text
O(n)
```

Then,

Only start counting if

```cpp
num - 1
```

does **not** exist.

---

# ALGORITHM

### Step 1

Store every element inside a HashSet.

```cpp
unordered_set<int> st;

for(int x : arr)
    st.insert(x);
```

---

### Step 2

Traverse every element.

---

### Step 3

Check

```cpp
num - 1
```

If previous number exists

→ Skip.

If previous number doesn't exist

→ This is the beginning of a sequence.

---

### Step 4

Start counting.

```cpp
current = num;
len = 1;
```

Keep checking

```cpp
current + 1
```

until it no longer exists.

---

### Step 5

Update answer.

---

# DRY RUN

Input

```text
arr = [2,6,1,9,4,5,3]
```

---

### Step 1

Create HashSet

```text
{
1
2
3
4
5
6
9
}
```

---

### Iteration 1

```text
num = 2
```

Check

```text
1 exists?
```

Yes.

Skip.

Reason:

Sequence should begin from 1.

---

### Iteration 2

```text
num = 6
```

Check

```text
5 exists?
```

Yes.

Skip.

---

### Iteration 3

```text
num = 1
```

Check

```text
0 exists?
```

No.

So this is the beginning of a sequence.

Initialize

```text
current = 1
len = 1
```

Check

```text
2 exists?
```

Yes

```text
current = 2
len = 2
```

---

Check

```text
3 exists?
```

Yes

```text
current = 3
len = 3
```

---

Check

```text
4 exists?
```

Yes

```text
current = 4
len = 4
```

---

Check

```text
5 exists?
```

Yes

```text
current = 5
len = 5
```

---

Check

```text
6 exists?
```

Yes

```text
current = 6
len = 6
```

---

Check

```text
7 exists?
```

No.

Stop.

```text
ans = 6
```

---

### Iteration 4

```text
num = 9
```

Check

```text
8 exists?
```

No.

Sequence

```text
9
```

Length

```text
1
```

Answer remains

```text
6
```

---

### Remaining Iterations

```text
4
5
3
```

All skipped because their previous numbers exist.

Final Answer

```text
6
```

---

# IMPORTANT CODE SNIPPETS

### Build HashSet

```cpp
unordered_set<int> st;

for(int x : arr)
    st.insert(x);
```

---

### Detect sequence start

```cpp
if(st.find(num - 1) == st.end())
```

Meaning

> Previous number doesn't exist.

So,

I am the first element.

---

### Expand sequence

```cpp
int current = num;
int len = 1;

while(st.find(current + 1) != st.end()){
    current++;
    len++;
}
```

---

### Update answer

```cpp
ans = max(ans, len);
```

---

# COMMON MISTAKES

### Mistake 1

Writing

```cpp
while(st.find(num + 1) != st.end())
```

instead of

```cpp
while(st.find(current + 1) != st.end())
```

`num` never changes.

`current` keeps moving.

---

### Mistake 2

Starting from every element.

This repeats work.

---

### Mistake 3

Checking

```cpp
num + 1
```

to identify the start.

Always check

```cpp
num - 1
```

---

### Mistake 4

Using `find()` on the vector instead of a HashSet.

Vector search = O(n)

HashSet search = O(1)

---

# WHY I MIGHT FORGET THIS

Because I think

> "Let's start from every number."

Wrong.

Always remember

```text
Previous number absent

↓

I am the first element

↓

Only I start counting.
```

---

# INTERVIEW FLOW

"I first thought of checking every element and repeatedly searching for the next consecutive number. Since searching an array takes O(n), that solution becomes O(n²).

To optimize, I store all elements inside an unordered_set so membership checking becomes O(1).

The key observation is that only the first element of a sequence should start counting. A number is the beginning of a sequence if its previous number (num - 1) does not exist.

From there, I keep extending the sequence using current + 1 and update the maximum length."

---

# TIME COMPLEXITY

## Brute Force

Outer loop

```text
O(n)
```

Each `find()`

```text
O(n)
```

Overall

```text
O(n²)
```

---

## Optimal

### Build HashSet

```text
O(n)
```

---

### Traverse array

```text
O(n)
```

---

### While Loop

Although there is a while loop,

every number is expanded only once because only sequence starts enter the loop.

Example

```text
1 2 3 4 5
```

Only

```text
1
```

expands.

`2,3,4,5` are skipped.

Therefore

Total work done by all while loops together =

```text
O(n)
```

Overall

```text
O(n)
```

---

# SPACE COMPLEXITY

## Brute Force

Only variables used.

```text
O(1)
```

---

## Optimal

HashSet stores every unique element.

```text
O(n)
```

---

# EDGE CASES

### Single Element

```text
[5]

Answer = 1
```

---

### All Duplicates

```text
2 2 2

Answer = 1
```

---

### Already Consecutive

```text
1 2 3 4 5

Answer = 5
```

---

### No Consecutive Numbers

```text
10 30 50

Answer = 1
```

---

### Duplicates Inside Sequence

```text
1 2 2 3

Answer = 3
```

HashSet automatically removes duplicates.

---

# PATTERN RECOGNITION

Whenever you see

- Consecutive numbers
- Unsorted array
- Need O(n)
- Fast existence checking
- Longest consecutive sequence

Think

```text
HashSet

↓

Find sequence starts

↓

Expand only once
```

---

# CLEAN C++ CODE

```cpp
class Solution {
  public:
    int longestConsecutive(vector<int>& arr) {
        
        unordered_set<int> st;
        
        int ans = 0;
        
        for(int x : arr){
            st.insert(x);
        }
        
        for(int num : arr){
            
            // Start only if previous number is absent
            if(st.find(num - 1) == st.end()){
                
                int current = num;
                int len = 1;
                
                while(st.find(current + 1) != st.end()){
                    current++;
                    len++;
                }
                
                ans = max(ans, len);
            }
        }
        
        return ans;
    }
};
```

---

# INTUITION BEHIND EVERY IMPORTANT LINE

### Create HashSet

```cpp
unordered_set<int> st;
```

Need fast O(1) lookup.

---

### Insert elements

```cpp
for(int x : arr){
    st.insert(x);
}
```

Store every element in the HashSet.

---

### Traverse array

```cpp
for(int num : arr)
```

Treat every number as a possible sequence start.

---

### Check sequence start

```cpp
if(st.find(num - 1) == st.end())
```

If the previous number doesn't exist,

I am the beginning of a sequence.

---

### Start counting

```cpp
int current = num;
int len = 1;
```

`current` walks through the sequence.

`len` stores its length.

---

### Expand sequence

```cpp
while(st.find(current + 1) != st.end())
```

As long as the next number exists,

keep moving.

---

### Move forward

```cpp
current++;
```

Walk to the next consecutive number.

---

### Increase length

```cpp
len++;
```

One more consecutive number found.

---

### Update answer

```cpp
ans = max(ans, len);
```

Store the maximum sequence length.

---

# EASY-TO-REMEMBER SUMMARY

- Store every number in a **HashSet**.
- **Never** start from every element.
- Start **only if `num - 1` doesn't exist**.
- `num` is the **starting point**.
- `current` is the **walking pointer**.
- Every sequence is explored exactly once.

## Memory Trick

> **"No previous number? I'm the start. Keep walking with `current` until the next number disappears."**
