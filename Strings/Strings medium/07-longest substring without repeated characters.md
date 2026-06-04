

# PROBLEM:

Longest Substring Without Repeating Characters

# PATTERN:

Sliding Window

# WHY THIS PATTERN:

We need a:

- longest substring
- continuous portion of string
- under a condition:
  - all characters must be unique

Whenever we see:

- longest/shortest substring
- continuous subarray
- at most/exactly k
- unique/distinct characters

we should immediately think:

# Sliding Window

---

# PROBLEM UNDERSTANDING

We are given a string.

We need to find:

> Length of the longest substring where every character is distinct.

---

# IMPORTANT UNDERSTANDING

Substring means:

```text
continuous part of string
```

Example:

```text
"abc" → valid substring
"ace" → NOT a substring
```

because it is not continuous.

---

# EXAMPLES

## Example 1

```text
Input:
s = "abcabcbb"

Output:
3
```

Explanation:

```text
"abc"
```

is the longest substring without repeating characters.

---

## Example 2

```text
Input:
s = "bbbbb"

Output:
1
```

Explanation:

Only:

```text
"b"
```

is valid.

---

## Example 3

```text
Input:
s = "pwwkew"

Output:
3
```

Explanation:

```text
"wke"
```

is longest.

---

# HOW TO THINK ABOUT THE PROBLEM

The ONLY problem occurs when:

```text
duplicate character enters the substring
```

Example:

```text
"abc" → valid
"abca" → invalid
```

because `'a'` repeats.

So our task becomes:

```text
Maintain a window where all characters remain unique.
```

Whenever duplicate appears:

```text
Shrink the window from left.
```

This becomes Sliding Window.

---

# HOW TO ANSWER THIS IN AN INTERVIEW

# Step 1 — Start With Brute Force

Say:

> “The straightforward approach is to generate all substrings and check whether every substring contains distinct characters.”

---

# Step 2 — Explain How

Say:

> “I can use two loops:
>
> - outer loop chooses starting index
> - inner loop chooses ending index
>
> Then for each substring,
> I can use a hash set to check uniqueness.”

---

# Step 3 — Explain Complexity

Say:

> “Generating all substrings takes O(n²),
> and checking uniqueness takes O(n),
> so overall complexity becomes O(n³).”

---

# Step 4 — Explain Why It Is Slow

Say:

> “This is inefficient because we repeatedly re-check characters.
>
> Example:
>
> ```text
> "a"
> "ab"
> "abc"
> "abca"
> ```
>
> Characters are checked again and again.”

---

# Step 5 — Transition to Optimal

Say:

> “Instead of recomputing uniqueness for every substring,
> we can maintain a sliding window that always contains unique characters only.”

---

# Step 6 — Explain Sliding Window Intuition

Say:

> “I expand the right pointer to include new characters.
>
> If duplicate appears,
> I shrink the window from the left until the window becomes valid again.”

---

# Step 7 — Explain Why Complexity Improves

Say:

> “Each character enters the window once and leaves the window once.
>
> Therefore total work becomes linear.”

---

# INTERVIEW FLOW SUMMARY

```text
Brute Force
→ Why it is slow
→ Observation about duplicates
→ Sliding Window
→ Shrink when duplicate appears
→ O(n) optimization
```

This flow is EXTREMELY important in interviews.

---

# BRUTE FORCE (ONLY FOR INTERVIEW PROGRESSION)

## Idea

Generate every substring.

Check if substring has all unique characters.

Update maximum length.

---

# Brute Force Code

```cpp
class Solution {
  public:

    bool isUnique(string temp) {

        unordered_set<char> st;

        for(char ch : temp) {

            if(st.count(ch)) {
                return false;
            }

            st.insert(ch);
        }

        return true;
    }

    int longestUniqueSubstr(string &s) {

        int n = s.size();

        int maxi = 0;

        for(int i = 0; i < n; i++) {

            string temp = "";

            for(int j = i; j < n; j++) {

                temp += s[j];

                if(isUnique(temp)) {

                    maxi = max(maxi, (int)temp.size());
                }
            }
        }

        return maxi;
    }
};
```

---

# WHY temp IS CREATED INSIDE OUTER LOOP

```cpp
for(int i = 0; i < n; i++) {

    string temp = "";
```

Because:

```text
Every new starting index needs a fresh substring.
```

Example:

For:

```text
"abcd"
```

When:

```text
i = 0
```

we generate:

```text
"a"
"ab"
"abc"
"abcd"
```

Then when:

```text
i = 1
```

we should generate:

```text
"b"
"bc"
"bcd"
```

So substring construction must restart.

---

# BRUTE FORCE TIME COMPLEXITY

## Step 1 — Generating Substrings

Two loops generate all substrings:

```text
O(n²)
```

because number of substrings:

```text
n(n+1)/2
```

---

## Step 2 — Checking Uniqueness

For every substring:

```text
isUnique()
```

may scan entire substring.

Worst case:

```text
O(n)
```

---

# FINAL TC

```text
O(n² × n)
= O(n³)
```

---

# WHY O(n³) FEELS SLOW

We repeatedly re-check old characters.

Example:

```text
"a"
"ab"
"abc"
"abca"
```

Notice:

```text
'a'
```

gets checked again and again.

This repeated work causes inefficiency.

---

# BRUTE FORCE SPACE COMPLEXITY

Set stores unique characters.

General formula:

```text
O(min(n, charset))
```

---

# WHY min(n, charset)?

The set size depends on TWO limits:

## Limit 1 — String Length

If:

```text
n = 5
```

then set cannot store more than 5 characters.

---

## Limit 2 — Total Possible Unique Characters

If only lowercase letters are allowed:

```text
charset = 26
```

then set cannot store more than 26 unique characters.

---

# Therefore

Maximum possible set size is:

```text
minimum of:
- string length
- total possible unique characters
```

Hence:

```text
O(min(n, charset))
```

---

# Examples

## Example 1

```text
n = 3
charset = 26
```

Set can store at most:

```text
3 characters
```

because string itself has only 3 characters.

So:

```text
min(3,26) = 3
```

---

## Example 2

```text
n = 100000
charset = 26
```

Even though string is huge,
only 26 lowercase letters exist.

So set can store at most:

```text
26 characters
```

Thus:

```text
min(100000,26) = 26
```

which becomes:

```text
O(1)
```

---

# OPTIMAL APPROACH — SLIDING WINDOW

# CORE IDEA

Maintain a window:

```text
[left .... right]
```

such that:

```text
all characters inside window are unique
```

---

# WHAT HAPPENS?

## Case 1 — Character is unique

Expand window.

---

## Case 2 — Duplicate appears

Shrink from left until duplicate disappears.

---

# IMPORTANT UNDERSTANDING

The set:

```text
does NOT store substring order
```

It only stores:

```text
which characters currently exist
```

Actual window is determined ONLY by:

```text
left and right pointers
```

---

# OPTIMAL CODE

```cpp
class Solution {
  public:

    int longestUniqueSubstr(string &s) {

        unordered_set<char> st;

        int left = 0;
        int maxi = 0;

        for(int right = 0; right < s.size(); right++) {

            // duplicate found
            while(st.count(s[right])) {

                st.erase(s[left]);
                left++;
            }

            // insert current character
            st.insert(s[right]);

            // update answer
            maxi = max(maxi, right - left + 1);
        }

        return maxi;
    }
};
```

---

# LINE-BY-LINE EXPLANATION

## Create Set

```cpp
unordered_set<char> st;
```

Stores characters currently inside window.

Purpose:

```text
fast duplicate checking
```

---

# Left Pointer

```cpp
int left = 0;
```

Represents left boundary of sliding window.

---

# Maximum Answer

```cpp
int maxi = 0;
```

Stores longest valid substring length.

---

# Right Pointer Expands Window

```cpp
for(int right = 0; right < s.size(); right++)
```

Each iteration:

```text
try adding s[right]
```

into window.

---

# Duplicate Check

```cpp
while(st.count(s[right]))
```

Meaning:

```text
current character already exists in window
```

So window becomes invalid.

---

# Shrink Window

```cpp
st.erase(s[left]);
left++;
```

Remove characters from left side until duplicate disappears.

---

# Insert Character

```cpp
st.insert(s[right]);
```

Now current character safely enters window.

---

# Update Answer

```cpp
maxi = max(maxi, right - left + 1);
```

Window length formula:

```text
right - left + 1
```

---

# DETAILED DRY RUN

# Example

```text
s = "abcabcbb"
```

---

# Initial State

```text
left = 0
maxi = 0
set = {}
```

---

# right = 0

Character:

```text
'a'
```

Duplicate?

```text
NO
```

Insert:

```text
set = {'a'}
```

Window:

```text
"a"
```

Length:

```text
1
```

Update:

```text
maxi = 1
```

---

# right = 1

Character:

```text
'b'
```

Duplicate?

```text
NO
```

Insert:

```text
{'a','b'}
```

Window:

```text
"ab"
```

Length:

```text
2
```

Update:

```text
maxi = 2
```

---

# right = 2

Character:

```text
'c'
```

Insert:

```text
{'a','b','c'}
```

Window:

```text
"abc"
```

Length:

```text
3
```

Update:

```text
maxi = 3
```

---

# right = 3

Character:

```text
'a'
```

Duplicate found.

---

# Remove from left

Remove:

```text
s[left] = 'a'
```

Now:

```text
left = 1
set = {'b','c'}
```

Duplicate gone.

Insert new `'a'`.

Now:

```text
set = {'a','b','c'}
```

Current window:

```text
s[1...3] = "bca"
```

Length:

```text
3
```

maxi remains:

```text
3
```

---

# IMPORTANT THING TO REMEMBER

Set does NOT preserve order.

This:

```text
{'a','b','c'}
```

does NOT mean:

```text
"abc"
```

Actual substring is ALWAYS:

```text
s[left...right]
```

---

# Continue Similarly

Windows formed:

```text
"cab"
"abc"
"cb"
"b"
```

Final answer:

```text
3
```

---

# OPTIMAL TIME COMPLEXITY

# WHY O(n)?

Each character:

- enters set once
- removed from set once

So total operations:

```text
2n
```

which simplifies to:

```text
O(n)
```

---

# VERY IMPORTANT INTUITION

Even though there is a nested while loop:

```cpp
while(st.count(s[right]))
```

left pointer never moves backward.

Across the ENTIRE algorithm:

```text
left moves at most n times
right moves exactly n times
```

So total work remains linear.

---

# OPTIMAL SPACE COMPLEXITY

Set stores unique characters currently inside window.

General formula:

```text
O(min(n, charset))
```

---

# WHY min(n, charset)?

The set size depends on TWO limits:

## Limit 1 — String Length

If string length is:

```text
n = 5
```

then set cannot store more than 5 unique characters.

---

## Limit 2 — Total Possible Unique Characters

If lowercase english letters are allowed:

```text
charset = 26
```

then set cannot store more than 26 unique characters.

---

# Therefore

Maximum set size becomes:

```text
minimum of:
- n
- charset
```

Hence:

```text
O(min(n, charset))
```

---

# In This Problem

Only lowercase english letters exist.

So:

```text
charset = 26
```

Therefore:

```text
O(min(n,26))
```

which simplifies to:

```text
O(1)
```

because 26 is constant.

---

# EDGE CASES

## Empty String

Answer:

```text
0
```

---

## Single Character

```text
"a"
```

Answer:

```text
1
```

---

## All Same Characters

```text
"aaaa"
```

Answer:

```text
1
```

---

## All Unique Characters

```text
"abcdef"
```

Answer:

```text
6
```

---

# WHY I GOT STUCK / MIGHT FORGET

- I confuse set contents with actual substring order.
- I forget that:

```text
window = s[left...right]
```

NOT the set.

- I forget why we shrink the window.
- I forget that duplicate removal may require MULTIPLE removals.
- I sometimes forget why `temp` is reset inside outer loop in brute force.
- I forget that repeated re-checking causes O(n³) in brute force.
- I forget why space complexity is written as:

```text
O(min(n, charset))
```

instead of just O(n).

---

# KEY INTERVIEW POINTS

## What interviewer wants to hear

- Why brute force is slow
- Why sliding window fits naturally
- Why shrinking window works
- Why complexity becomes O(n)
- Why each character is processed at most twice
- Why space complexity depends on charset

---

# GOLDEN LINE FOR INTERVIEW

> “The window always maintains unique characters only.
> Whenever a duplicate enters,
> I shrink the window from the left until it becomes valid again.”

---

# MOST IMPORTANT PATTERN RECOGNITION

Whenever you see:

- longest substring
- shortest substring
- continuous subarray
- at most k
- exactly k
- distinct characters

Think:

# Sliding Window
````
