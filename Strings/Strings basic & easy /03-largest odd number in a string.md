
# Note

## PROBLEM: Largest Odd Number in String

### PATTERN:
Greedy + String Traversal

---

## WHY THIS PATTERN:

We need the:

```txt
Largest-valued odd substring
```

Key observation:

```txt
A number is odd only if its last digit is odd.
```

To maximize value:

```txt
We should keep as many digits as possible.
```

So we need:

```txt
Longest prefix ending at an odd digit.
```

That is why we traverse:

```txt
RIGHT → LEFT
```

and stop at the first odd digit.

---

# CORE IDEA

Find the:

```txt
RIGHTMOST odd digit
```

Then return:

```txt
Substring from index 0 till that digit.
```

Because:

```txt
Longer prefix = larger number
```

---

# INTERVIEW EXPLANATION FLOW

## Step 1 — Observation

A number is odd if:

```txt
last digit is odd
```

Odd digits:

```txt
1 3 5 7 9
```

---

## Step 2 — Main Insight

To get the largest-valued substring:

```txt
We should keep maximum digits from the left.
```

So instead of generating all substrings:

```txt
Find the longest prefix that ends at an odd digit.
```

---

## Step 3 — Approach

Traverse from the end:

- If digit is even:
  - ignore it
- If digit is odd:
  - return prefix till that index

---

## Step 4 — Complexity

```txt
Time Complexity  = O(N)
Space Complexity = O(1)
```

---

# WHY THIS WORKS

Suppose:

```txt
s = "354268"
```

Removing from back:

```txt
354268 ❌
35426  ❌
3542   ❌
354    ❌
35     ✅
```

Largest odd substring:

```txt
"35"
```

We always want:

```txt
Longest valid prefix
```

---

# OPTIMAL CODE (C++)

```cpp
class Solution {
  public:
    string maxOdd(string s) {
        
        for(int i = s.size() - 1; i >= 0; i--) {
            
            int digit = s[i] - '0';
            
            if(digit % 2 == 1) {
                return s.substr(0, i + 1);
            }
        }
        
        return "";
    }
};
```

---

# LINE BY LINE EXPLANATION

---

## LOOP

```cpp
for(int i = s.size() - 1; i >= 0; i--)
```

### Meaning

Traverse from:

```txt
RIGHT → LEFT
```

Why?

Because we need:

```txt
Rightmost odd digit
```

---

# Example

```txt
s = "35427"
```

Indexes:

```txt
0 1 2 3 4
3 5 4 2 7
```

Initial:

```txt
i = 4
```

---

# DIGIT CONVERSION

```cpp
int digit = s[i] - '0';
```

### Why?

Inside string:

```txt
'7'
```

is a character.

We convert it into integer:

```txt
7
```

---

# Example

```cpp
'7' - '0'
```

ASCII:

```txt
55 - 48 = 7
```

So:

```txt
digit = 7
```

---

# ODD CHECK

```cpp
if(digit % 2 == 1)
```

Odd numbers leave remainder:

```txt
1
```

when divided by:

```txt
2
```

Examples:

```txt
7 % 2 = 1
5 % 2 = 1
```

Even numbers:

```txt
8 % 2 = 0
4 % 2 = 0
```

---

# RETURN STATEMENT

```cpp
return s.substr(0, i + 1);
```

---

# WHAT DOES substr() DO?

Syntax:

```cpp
substr(start, length)
```

Meaning:

```txt
Take substring starting from "start"
having "length" characters
```

---

# HERE

```cpp
s.substr(0, i + 1)
```

means:

```txt
Start from index 0
Take i+1 characters
```

---

# WHY i + 1 ?

Indexes start from:

```txt
0
```

So if:

```txt
i = 4
```

characters count is:

```txt
5
```

Indexes:

```txt
0 1 2 3 4
```

Hence:

```cpp
i + 1
```

---

# COMPLETE DRY RUN

# Example 1

```txt
s = "504"
```

Indexes:

```txt
0 1 2
5 0 4
```

---

## ITERATION 1

```txt
i = 2
```

Character:

```txt
'4'
```

Convert:

```txt
digit = 4
```

Check:

```txt
4 % 2 = 0
```

Even.

Move left.

---

## ITERATION 2

```txt
i = 1
```

Character:

```txt
'0'
```

Convert:

```txt
digit = 0
```

Check:

```txt
0 % 2 = 0
```

Even.

Move left.

---

## ITERATION 3

```txt
i = 0
```

Character:

```txt
'5'
```

Convert:

```txt
digit = 5
```

Check:

```txt
5 % 2 = 1
```

Odd found.

Return:

```cpp
s.substr(0, 1)
```

Result:

```txt
"5"
```

---

# Example 2

```txt
s = "35427"
```

Indexes:

```txt
0 1 2 3 4
3 5 4 2 7
```

---

## ITERATION 1

```txt
i = 4
```

Character:

```txt
'7'
```

Convert:

```txt
digit = 7
```

Check:

```txt
7 % 2 = 1
```

Odd found immediately.

Return:

```cpp
s.substr(0, 5)
```

Result:

```txt
"35427"
```

---

# Example 3

```txt
s = "2042"
```

Traversal:

```txt
2 → even
4 → even
0 → even
2 → even
```

No odd digit found.

Return:

```txt
""
```

---

# IMPORTANT CODE SNIPPETS

## Check if digit is odd

```cpp
(s[i] - '0') % 2 == 1
```

---

## Convert character to integer

```cpp
s[i] - '0'
```

---

## Return prefix till index i

```cpp
s.substr(0, i + 1)
```

Meaning:

```txt
start index = 0
length = i + 1
```

---

# EDGE CASES

## No odd digit

```txt
"2468"
```

Output:

```txt
""
```

---

## Entire string already odd

```txt
"13579"
```

Output:

```txt
"13579"
```

---

## Single digit odd

```txt
"7"
```

Output:

```txt
"7"
```

---

## Single digit even

```txt
"8"
```

Output:

```txt
""
```

---

# TIME COMPLEXITY

## O(N)

Why?

In worst case:

```txt
We traverse entire string once.
```

Example:

```txt
"2222222222"
```

We check every character once.

---

# SPACE COMPLEXITY

## O(1)

Why?

We only use:

```txt
1 variable → digit
1 loop variable → i
```

No extra array/hashmap/stack used.

---

# WHY I MIGHT FORGET THIS QUESTION

I might unnecessarily think about:

```txt
Generating all substrings
```

But the entire problem depends on ONE observation:

```txt
Largest odd substring =
Longest prefix ending at an odd digit.
```

So the moment I see:

```txt
Largest odd number in string
```

I should instantly think:

```txt
Scan from right and find first odd digit.
```

---

# FINAL TAKEAWAY

This is a pure observation-based greedy problem.

We do NOT need:

```txt
Nested loops
All substrings
Brute force generation
```

Just:

```txt
Traverse from back
Find first odd digit
Return prefix till there
```
````
