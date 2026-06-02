
## PROBLEM: Longest Palindromic Substring

PATTERN: Two Pointers (Expand Around Center)

WHY THIS PATTERN:  
A palindrome always mirrors around its center.

Instead of generating all substrings and checking whether each one is palindrome, we directly generate valid palindromes by expanding outward from every possible center.

This avoids repeated palindrome checking.

---

# CORE IDEA

For every index:

```cpp
expand(i, i)       // odd length palindrome
expand(i, i + 1)   // even length palindrome
```

Expand outward while:

```cpp
s[left] == s[right]
```

Keep track of the longest palindrome found.

---

# KEY OBSERVATION

Every palindrome expands symmetrically around its center.

Examples:

## Odd Length

```text
"aba"
```

Center:

```text
 b
```

Expansion starts from:

```cpp
(i, i)
```

---

## Even Length

```text
"abba"
```

Center:

```text
bb
```

Expansion starts from:

```cpp
(i, i+1)
```

---

# WHY BRUTE FORCE IS MENTIONED HERE

Brute force is useful because:

- optimal solution is not immediately obvious
- interviewer expects optimization thinking
- transition to center expansion becomes natural

BUT:

Normally in interviews:

```text
Explain brute force briefly
Code only optimal
```

---

# BRUTE FORCE (ONLY EXPLAIN)

## IDEA

Generate all substrings.

For every substring:

- check palindrome
- keep longest answer

---

# BRUTE FORCE TC

Generating substrings:

O(n²)

Palindrome check:

O(n)

# Total TC

O(n³)

# SC

O(1)

(auxiliary)

---

# INTERVIEW TRANSITION

Say this:

> "The expensive part is repeatedly checking whether substrings are palindromes.
>
> Since palindromes are symmetric, we can directly expand from centers instead."

This transition is VERY important.

---

# OPTIMAL APPROACH

## IDEA

Treat every index as a possible center.

For every index:

```cpp
expand(i, i)       // odd palindrome
expand(i, i+1)     // even palindrome
```

Expand outward while characters match.

Track longest palindrome.

---

# FULL OPTIMAL CODE

```cpp
class Solution {
public:

    string expand(string &s, int left, int right) {

        while(left >= 0 &&
              right < s.length() &&
              s[left] == s[right]) {

            left--;
            right++;
        }

        return s.substr(left + 1, right - left - 1);
    }

    string longestPalindrome(string s) {

        string ans = "";

        for(int i = 0; i < s.length(); i++) {

            // Odd length palindrome
            string odd = expand(s, i, i);

            // Even length palindrome
            string even = expand(s, i, i + 1);

            // Update answer
            if(odd.length() > ans.length()) {
                ans = odd;
            }

            if(even.length() > ans.length()) {
                ans = even;
            }
        }

        return ans;
    }
};
```

---

# MOST IMPORTANT CODE SNIPPET

This is the HEART of the problem:

```cpp
while(left >= 0 &&
      right < s.length() &&
      s[left] == s[right]) {

    left--;
    right++;
}
```

Memorize this template.

This exact logic appears in MANY palindrome problems.

---

# WHY:

```cpp
left + 1
```

AND:

```cpp
right - left - 1
```

work

Pointers overshoot by one step before loop stops.

Example:

```text
Valid palindrome:
0 ----- 2

Pointers after loop:
-1 ---- 3
```

Actual palindrome:

```text
(left+1) to (right-1)
```

Length formula:

```text
end - start + 1
```

becomes:

```text
(right-1) - (left+1) + 1
```

which simplifies to:

```text
right - left - 1
```

---

# DETAILED DRY RUN

# INPUT

```text
s = "babad"
```

Indexes:

```text
0 1 2 3 4
b a b a d
```

---

# INITIAL

```cpp
ans = ""
```

---

# i = 0

## Odd Expansion

```cpp
expand(s,0,0)
```

Palindrome:

```text
"b"
```

Update:

```text
ans = "b"
```

---

## Even Expansion

```cpp
expand(s,0,1)
```

```text
'b' != 'a'
```

Returns empty string.

---

# i = 1

## Odd Expansion

```cpp
expand(s,1,1)
```

Expansion:

```text
a == a
b == b
```

Palindrome:

```text
"bab"
```

Update:

```text
ans = "bab"
```

---

## Even Expansion

```text
a != b
```

No palindrome.

---

# i = 2

Odd palindrome:

```text
"aba"
```

Same length as:

```text
"bab"
```

Condition:

```cpp
>
```

NOT updated.

This preserves FIRST occurring palindrome.

---

# FINAL ANSWER

```text
"bab"
```

---

# EDGE CASES

## 1. Single Character

```text
"a"
```

Answer:

```text
"a"
```

---

## 2. All Same Characters

```text
"aaaa"
```

Answer:

```text
"aaaa"
```

Worst-case expansion.

---

## 3. No Large Palindrome

```text
"abc"
```

Answer:

```text
"a"
```

First single-character palindrome.

---

## 4. Even Length Palindrome

```text
"abba"
```

Need:

```cpp
expand(i,i+1)
```

Otherwise solution fails.

---

# WHY I GOT STUCK / MIGHT FORGET

## 1. Forgetting Even Length Palindromes

Need BOTH:

```cpp
expand(i,i)
expand(i,i+1)
```

---

## 2. Forgetting Why Return Formula Works

Remember:

```text
Pointers overshoot by one step
```

So actual palindrome becomes:

```cpp
left + 1
right - left - 1
```

---

## 3. Using `>=`

Wrong:

```cpp
>=
```

Correct:

```cpp
>
```

Otherwise first occurring palindrome condition breaks.

---

## 4. Forgetting The Main Observation

Remember ONLY this:

```text
Palindrome expands from center
```

Everything comes from this idea.

---

# HOW TO EXPLAIN IN INTERVIEW

Start with:

> "A brute force approach would generate all substrings and check whether each substring is palindrome, which takes O(n³)."

Then transition:

> "We can optimize this using the symmetry property of palindromes."

Then explain:

> "Every palindrome expands around its center.
>
> So for every index:
>
> - I expand from `(i,i)` for odd length palindromes
> - and `(i,i+1)` for even length palindromes.
>
> I keep expanding while characters match and track the longest palindrome found."

Then mention:

```text
TC = O(n²)
SC = O(1)
```

---

# INTERVIEW FLOW

## Step 1

Clarify question.

---

## Step 2

Mention brute force briefly.

---

## Step 3

Give key observation:

```text
Palindrome expands from center
```

---

## Step 4

Explain odd/even centers.

---

## Step 5

Code optimal solution.

---

## Step 6

Dry run:

```text
"babad"
```

---

## Step 7

Mention TC and SC.

---

# TIME COMPLEXITY

Number of centers:

```text
2n - 1
```

Each expansion:

```text
O(n)
```

# Total TC

```text
O(n²)
```

---

# SPACE COMPLEXITY

Ignoring substring creation:

```text
O(1)
```

---

# INSTANT MEMORY TRICK

Whenever you see:

```text
Palindrome substring
```

Immediately think:

```text
CENTER EXPANSION
```

Then:

```cpp
expand(i,i)
expand(i,i+1)
```

That is the entire problem.
````
