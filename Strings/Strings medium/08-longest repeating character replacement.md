

# PROBLEM:

Longest Repeating Character Replacement

# PATTERN:

Sliding Window + Frequency Count

# WHY THIS PATTERN:

We need:

* longest substring
* continuous portion of string
* under a condition:

  * substring should be convertible into all same characters
  * using at most `k` replacements

Whenever we see:

* longest/shortest substring
* continuous window
* at most `k` operations/modifications/replacements

Think:

```text
Sliding Window
```

---

# CORE IDEA:

For every window:

```text
[left ... right]
```

we calculate:

```text
How many characters need replacement?
```

The BEST strategy is:

```text
Keep the most frequent character
Replace all remaining characters
```

So:

```text
replacements needed = window size - maxFreq
```

Where:

* `window size = right - left + 1`
* `maxFreq = frequency of most common character`

If:

```text
(window size - maxFreq) <= k
```

then the window is VALID.

Else:

```text
shrink from left
```

We keep track of the largest valid window.

---

# MOST IMPORTANT INTUITION

Suppose current window is:

```text
AABAB
```

Frequency:

| Character | Count |
| --------- | ----- |
| A         | 3     |
| B         | 2     |

Best choice:

```text
convert everything into A
```

Why?

Because A already appears most.

So we only replace:

```text
2 B's
```

Formula:

```text
window size - maxFreq
= 5 - 3
= 2
```

Meaning:

```text
2 replacements needed
```

That single idea is the ENTIRE problem.

---

# BRUTE FORCE (ONLY FOR INTERVIEW FLOW)

## Idea:

Generate all substrings.

For every substring:

* count frequencies
* find max frequency
* check:

```text
substring length - maxFreq <= k
```

Update answer.

---

# BRUTE FORCE CODE

```cpp
class Solution {
public:

    int longestSubstr(string& s, int k) {

        int n = s.size();
        int ans = 0;

        for(int i = 0; i < n; i++) {

            vector<int> freq(26, 0);

            for(int j = i; j < n; j++) {

                freq[s[j] - 'A']++;

                int maxFreq = 0;

                for(int x = 0; x < 26; x++) {
                    maxFreq = max(maxFreq, freq[x]);
                }

                int len = j - i + 1;

                if(len - maxFreq <= k) {
                    ans = max(ans, len);
                }
            }
        }

        return ans;
    }
};
```

---

# BRUTE FORCE TC:

Outer loop:

```text
O(n)
```

Inner loop:

```text
O(n)
```

Finding max frequency:

```text
O(26)
```

Total:

```text
O(n²)
```

---

# OPTIMIZATION THOUGHT PROCESS

In brute force:

* we recompute frequencies repeatedly
* overlapping substrings waste work

Observation:

```text
When moving from one substring to another,
most characters remain same.
```

So instead of rebuilding every substring:

```text
Use Sliding Window
```

Maintain frequencies dynamically.

---

# OPTIMAL APPROACH

Use:

* left pointer
* right pointer
* frequency array
* maxFreq

Expand window using `right`.

If window becomes invalid:

```text
(window size - maxFreq) > k
```

shrink from left.

Store maximum valid window size.

---

# IMPORTANT CODE SNIPPETS

## 1. Frequency Count

```cpp
freq[s[right] - 'A']++;
```

### Explanation:

Characters are converted into indices:

| Character | Index |
| --------- | ----- |
| A         | 0     |
| B         | 1     |
| C         | 2     |

So:

```cpp
'A' - 'A' = 0
'B' - 'A' = 1
```

This stores character frequencies.

---

## 2. Updating maxFreq

```cpp
maxFreq = max(maxFreq, freq[s[right] - 'A']);
```

Tracks highest frequency character inside current window.

---

## 3. Valid Window Check

```cpp
(right - left + 1) - maxFreq > k
```

Meaning:

```text
Too many replacements needed
```

So shrink the window.

---

## 4. Shrinking Window

```cpp
freq[s[left] - 'A']--;
left++;
```

Remove leftmost character.

---

## 5. Updating Answer

```cpp
ans = max(ans, right - left + 1);
```

Meaning:

```text
Among all valid windows,
store the largest one.
```

---

# OPTIMAL CODE

```cpp
class Solution {
public:

    int longestSubstr(string& s, int k) {

        vector<int> freq(26, 0);

        int left = 0;
        int maxFreq = 0;
        int ans = 0;

        for(int right = 0; right < s.size(); right++) {

            // include current character
            freq[s[right] - 'A']++;

            // update highest frequency
            maxFreq = max(maxFreq, freq[s[right] - 'A']);

            // shrink if invalid
            while((right - left + 1) - maxFreq > k) {

                freq[s[left] - 'A']--;
                left++;
            }

            // update answer
            ans = max(ans, right - left + 1);
        }

        return ans;
    }
};
```

---

# DRY RUN 1

## Example:

```text
s = "AABAB"
k = 1
```

---

## right = 0

Window:

```text
"A"
```

Frequency:

| A | 1 |

Window size:

```text
1
```

maxFreq:

```text
1
```

Replacements needed:

```text
1 - 1 = 0
```

VALID

```text
ans = 1
```

---

## right = 1

Window:

```text
"AA"
```

Frequency:

| A | 2 |

Replacements:

```text
2 - 2 = 0
```

VALID

```text
ans = 2
```

---

## right = 2

Window:

```text
"AAB"
```

Frequency:

| A | 2 |
| B | 1 |

Best strategy:

```text
Keep A's
Replace B
```

Replacements needed:

```text
3 - 2 = 1
```

VALID

```text
ans = 3
```

---

## right = 3

Window:

```text
"AABA"
```

Frequency:

| A | 3 |
| B | 1 |

Best strategy:

```text
Replace B -> A
```

Replacements:

```text
4 - 3 = 1
```

VALID

```text
ans = 4
```

---

## right = 4

Window:

```text
"AABAB"
```

Frequency:

| A | 3 |
| B | 2 |

Best strategy:

```text
Keep A's
Replace both B's
```

Replacements:

```text
5 - 3 = 2
```

INVALID because:

```text
2 > k
```

Shrink window.

Remove leftmost A.

New window:

```text
"ABAB"
```

Window size:

```text
4
```

VALID again.

Final answer remains:

```text
4
```

---

# DRY RUN 2

## Example:

```text
s = "ABBB"
k = 2
```

---

## right = 0

Window:

```text
"A"
```

Need:

```text
1 - 1 = 0
```

VALID

```text
ans = 1
```

---

## right = 1

Window:

```text
"AB"
```

Frequency:

| A | 1 |
| B | 1 |

Need:

```text
2 - 1 = 1
```

VALID

```text
ans = 2
```

---

## right = 2

Window:

```text
"ABB"
```

Frequency:

| A | 1 |
| B | 2 |

Best strategy:

```text
Replace A -> B
```

Need:

```text
3 - 2 = 1
```

VALID

```text
ans = 3
```

---

## right = 3

Window:

```text
"ABBB"
```

Frequency:

| A | 1 |
| B | 3 |

Need:

```text
4 - 3 = 1
```

VALID

```text
ans = 4
```

Final answer:

```text
4
```

---

# WHY WE DO NOT DECREASE maxFreq

Very important interview point.

Even when shrinking:

```cpp
freq[s[left] - 'A']--;
```

we do NOT recompute `maxFreq`.

Reason:

* recomputing every time would increase complexity
* keeping historical maxFreq still works correctly
* invalid windows eventually shrink anyway

This helps maintain:

```text
O(n)
```

time complexity.

---

# INTERVIEW EXPLANATION

## Short Version

> We use Sliding Window to maintain the longest valid substring.
>
> For every window, we track character frequencies.
>
> To make all characters same, we keep the most frequent character and replace the remaining ones.
>
> Replacements needed are:
>
> `window size - maxFreq`
>
> If replacements exceed `k`, we shrink the window.
>
> We continuously store the maximum valid window length.

---

# EDGE CASES

## 1. Single Character

```text
"A"
```

Answer:

```text
1
```

---

## 2. All Same Characters

```text
"AAAA"
```

Need:

```text
0 replacements
```

Answer:

```text
4
```

---

## 3. All Different Characters

```text
"ABCD"
k = 1
```

Maximum valid window:

```text
2
```

because only one replacement allowed.

---

# WHY I GOT STUCK / MIGHT FORGET

* I forget WHY:

```text
window size - maxFreq
```

gives replacements needed.

Remember:

```text
Keep majority character.
Replace everything else.
```

* I confuse:

```cpp
freq[s[right]]
```

with:

```cpp
freq[s[right] - 'A']
```

Always convert character → index.

* I forget that:

```text
ans stores largest VALID window only
```

* I sometimes think:

```text
maxFreq should decrease while shrinking
```

But historical maxFreq still works correctly.

---

# TIME COMPLEXITY

## Optimal Solution

Each character:

* enters window once
* leaves window once

So:

```text
O(n)
```

---

# SPACE COMPLEXITY

Frequency array size:

```text
26
```

So:

```text
O(26) = O(1)
```

constant extra space.

---

# PATTERN RECOGNITION

Whenever you see:

* longest substring
* at most k changes
* flips/replacements allowed
* continuous window

Think:

```text
Sliding Window + Frequency Count
```

Similar problems:

* Max Consecutive Ones III
* Fruit Into Baskets
* Longest Substring with At Most K Distinct Characters
* Binary Subarrays with constraints
