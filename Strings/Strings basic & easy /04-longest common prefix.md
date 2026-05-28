
# Note

## PROBLEM: Longest Common Prefix of Strings

## PATTERN: String Traversal / Vertical Scanning

---

## WHY THIS PATTERN:

We compare characters COLUMN BY COLUMN across all strings.

The moment:
- characters mismatch
OR
- any string ends

the common prefix stops.

---

## CORE IDEA:

Take the first string as a reference.

For every index `i`:
- take character `first[i]`
- compare it with the same index in all other strings

If all match:
- add character to answer

If mismatch happens:
- return answer immediately

---

## WHY THIS WORKS:

A prefix means:

```text
starting common portion
```

So all strings MUST have:
- same character
- at same index

Example:

```text
geeksforgeeks
geeks
geek
geezer
```

Check vertically:

```text
g g g g ✅
e e e e ✅
e e e e ✅
k k k z ❌
```

Mismatch occurs at index 3.

So answer = `"gee"`

---

## WHY I GOT STUCK / MIGHT FORGET:

I confuse:
- `arr[j][i]`
- why we use `j < n`
- why mismatch instantly stops answer
- why we compare vertically

Main thing to remember:

```text
arr[j][i]

↓

j = string number
i = character index
```

We are checking characters vertically like a matrix.

---

## EDGE CASES:

### 1. No common prefix

```cpp
["hello", "world"]
```

Answer:

```cpp
""
```

---

### 2. One string fully matches

```cpp
["flow", "flower"]
```

Answer:

```cpp
"flow"
```

---

### 3. All strings same

```cpp
["abc", "abc", "abc"]
```

Answer:

```cpp
"abc"
```

---

### 4. Single string

```cpp
["coding"]
```

Answer:

```cpp
"coding"
```

---

# BRUTE FORCE (ONLY FOR UNDERSTANDING)

Generate all prefixes of first string.

For every prefix:
- check whether all strings contain it.

This works but becomes unnecessary and inefficient.

Better approach:
- directly compare characters column-wise.

---

# OPTIMAL APPROACH

## STEP 1:
Take first string as reference.

## STEP 2:
Traverse every character of first string.

## STEP 3:
For each character:
- compare with all remaining strings

## STEP 4:
If:
- index goes out of bounds
OR
- mismatch occurs

return answer immediately.

## STEP 5:
Otherwise append character into answer.

---

# OPTIMAL CODE (C++)

```cpp
class Solution {
public:

    string longestCommonPrefix(vector<string> arr) {

        int n = arr.size();

        string first = arr[0];
        string ans = "";

        // Traverse characters of first string
        for(int i = 0; i < first.length(); i++) {

            char ch = first[i];

            // Compare with all remaining strings
            for(int j = 1; j < n; j++) {

                // String ended OR mismatch found
                if(i >= arr[j].length() || arr[j][i] != ch) {
                    return ans;
                }
            }

            ans += ch;
        }

        return ans;
    }
};
```

---

# MOST IMPORTANT CODE SNIPPET

```cpp
if(i >= arr[j].length() || arr[j][i] != ch)
```

Meaning:

Either:
- current string ended
OR
- character mismatch happened

So prefix must stop.

---

# UNDERSTANDING arr[j][i]

This is the MOST IMPORTANT PART.

Example:

```cpp
arr = ["geeks", "geek", "geezer"]
```

---

## `arr[j]`

means:

```text
choose the j-th string
```

Example:

```cpp
arr[2] = "geezer"
```

---

## `arr[j][i]`

means:

```text
choose character i
inside string j
```

Example:

```cpp
arr[2][3]
```

means:

```text
index 3 of "geezer"

g e e z e r
0 1 2 3
```

So:

```cpp
arr[2][3] = 'z'
```

---

# VERY IMPORTANT VISUALIZATION

Think of strings like rows in a table.

```text
          0 1 2 3 4
arr[0] =  g e e k s ...
arr[1] =  g e e k s
arr[2] =  g e e k
arr[3] =  g e e z e r
```

We scan COLUMN BY COLUMN.

---

# WHY DO WE USE `j < n` ?

Suppose:

```cpp
n = 4
```

Valid indices:

```text
0 1 2 3
```

Loop:

```cpp
for(int j = 1; j < n; j++)
```

means:

```text
j = 1, 2, 3
```

So last index (`n-1`) IS INCLUDED.

IMPORTANT:

```cpp
j < n
```

does NOT mean:
"go till n"

It means:
"continue while j is smaller than n"

So last valid value becomes:

```text
n-1
```

---

# WHY NOT `j < n-1` ?

Because that would skip the last string.

Example:

```cpp
n = 4
```

```cpp
j < n-1
```

becomes:

```cpp
j < 3
```

So:

```text
j = 1, 2
```

Index 3 never gets checked.

Wrong.

---

# FULL DETAILED DRY RUN

## INPUT:

```cpp
arr = ["geeksforgeeks", "geeks", "geek", "geezer"]
```

---

# INITIALIZATION

```cpp
n = 4

first = "geeksforgeeks"

ans = ""
```

---

# OUTER LOOP

```cpp
i = 0
```

---

# CURRENT CHARACTER

```cpp
ch = 'g'
```

---

# INNER LOOP

## j = 1

```cpp
arr[1] = "geeks"
```

Check:

```cpp
if(0 >= 5 || 'g' != 'g')
```

FALSE

---

## j = 2

```cpp
arr[2] = "geek"
```

Check:

```cpp
if(0 >= 4 || 'g' != 'g')
```

FALSE

---

## j = 3

```cpp
arr[3] = "geezer"
```

Check:

```cpp
if(0 >= 6 || 'g' != 'g')
```

FALSE

---

# ALL MATCHED

```cpp
ans += 'g'
```

Now:

```cpp
ans = "g"
```

---

# NEXT ITERATION

```cpp
i = 1
ch = 'e'
```

All strings have `'e'`.

```cpp
ans = "ge"
```

---

# NEXT ITERATION

```cpp
i = 2
ch = 'e'
```

All strings have `'e'`.

```cpp
ans = "gee"
```

---

# NEXT ITERATION

```cpp
i = 3
ch = 'k'
```

---

## j = 1

```text
"geeks"[3] = k
```

Match ✅

---

## j = 2

```text
"geek"[3] = k
```

Match ✅

---

## j = 3

```text
"geezer"[3] = z
```

Mismatch ❌

---

Condition becomes true:

```cpp
arr[j][i] != ch
```

So:

```cpp
return ans;
```

Current answer:

```cpp
"gee"
```

---

# FINAL OUTPUT

```cpp
"gee"
```

---

# TIME COMPLEXITY

Let:

```text
N = number of strings
M = length of smallest string
```

For every character,
we compare across all strings.

So:

```text
TC = O(N × M)
```

Which is same as:

```text
O(n * min(|arr[i]|))
```

---

## WHY?

Suppose:

```text
N = 4 strings
M = 3 characters checked
```

Then comparisons roughly become:

```text
4 × 3
```

---

# SPACE COMPLEXITY

There are TWO ways to say this.

---

## 1. Ignoring output string

We only use:
- variables
- loops
- one character

No extra data structure.

So:

```text
SC = O(1)
```

---

## 2. Including output string

Answer string can grow up to:

```text
length of smallest string
```

So:

```text
SC = O(M)
```

Which is same as:

```text
O(min(|arr[i]|))
```

---

# BEST INTERVIEW ANSWER

```text
Time Complexity:
O(N × M)

where:
N = number of strings
M = smallest string length

--------------------------------------------------

Auxiliary Space:
O(1)

If output string is included,
space becomes O(M).
```

---

# INTERVIEW EXPLANATION FLOW

“I use the first string as a reference and compare characters index-by-index across all other strings.

For every index:
- if any string becomes shorter
OR
- characters mismatch

I stop immediately because common prefix cannot continue further.

Otherwise I keep adding characters into the answer.”

---

# MOST IMPORTANT TAKEAWAY

This problem is NOT about substrings.

It is about:

```text
VERTICAL CHARACTER MATCHING
```

across all strings.
````
