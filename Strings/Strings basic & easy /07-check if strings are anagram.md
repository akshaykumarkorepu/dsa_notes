
# Note

## PROBLEM: Valid Anagram

## PATTERN: Frequency Counting / Hashing

---

# WHY THIS PATTERN

In anagrams:
- order does NOT matter
- frequencies matter

We only care whether:
- both strings contain same characters
- with same counts

So frequency counting is the best approach.

---

# CORE IDEA

1. If lengths differ → impossible to be anagrams
2. Count frequencies using first string
3. Remove frequencies using second string
4. If:
   - character missing
   - frequency becomes negative

   → not an anagram

5. If everything balances perfectly → anagram

---

# MOST IMPORTANT INTUITION

```text
First string ADDS frequencies.
Second string REMOVES frequencies.
```

If everything balances perfectly:

```text
all frequencies become zero.
```

That means:

```text
same characters
+
same frequencies
=
anagram
```

---

# EDGE CASES

- Different lengths
- Extra character in second string
- Missing character
- Same strings
- Single character strings

---

# WHY I GOT STUCK / MIGHT FORGET

- Forgetting why we decrement frequencies
- Forgetting why negative frequency means invalid
- Forgetting why arrays are used instead of hashmap
- Forgetting that order does NOT matter
- Forgetting the purpose of:

```cpp
if(freq.find(ch) == freq.end())
```

---

# MOST IMPORTANT MEMORY

```text
This problem is NOT about order.
It is ONLY about frequency balancing.
```

---

# DO WE NEED BRUTE FORCE?

Not really.

The optimal solution is already intuitive.

But interviewer may expect progression from:

```text
sorting → hashing
```

So sorting approach can be mentioned briefly.

---

# SORTING APPROACH

## IDEA

Sort both strings.

If sorted strings become equal,
they are anagrams.

---

## CODE

```cpp
class Solution {
  public:
    bool areAnagrams(string& s1, string& s2) {

        sort(s1.begin(), s1.end());
        sort(s2.begin(), s2.end());

        return s1 == s2;
    }
};
```

---

## EXAMPLE

```text
listen -> eilnst
silent -> eilnst
```

Both become same.

---

## TIME COMPLEXITY

```text
O(n log n)
```

because sorting is used.

---

## SPACE COMPLEXITY

```text
O(1)
```

(depends on sorting implementation)

---

# WHY OPTIMIZE FURTHER?

We can solve in linear time using frequency counting.

---

# HASHMAP APPROACH

## WHY HASHMAP?

Hashmap stores:

```text
character -> frequency
```

Useful when:
- character range is unknown
- Unicode exists
- uppercase/lowercase mixed
- arbitrary characters are present

---

# HASHMAP CODE

```cpp
class Solution {
  public:
    bool areAnagrams(string& s1, string& s2) {

        // Different lengths means impossible
        if(s1.size() != s2.size())
            return false;

        unordered_map<char,int> freq;

        // Count frequencies from first string
        for(char ch : s1){
            freq[ch]++;
        }

        // Remove frequencies using second string
        for(char ch : s2){

            // Character not present
            if(freq.find(ch) == freq.end())
                return false;

            freq[ch]--;

            // Extra occurrences found
            if(freq[ch] < 0)
                return false;
        }

        return true;
    }
};
```

---

# HASHMAP CODE EXPLANATION

## PART 1 — Length Check

```cpp
if(s1.size() != s2.size())
    return false;
```

If lengths differ,
they can never be anagrams.

### Example

```text
abc
abcc
```

Different lengths.
Impossible.

---

## PART 2 — Create HashMap

```cpp
unordered_map<char,int> freq;
```

Stores:

```text
character -> count
```

### Example

```text
aabcc
```

Map:

```text
a -> 2
b -> 1
c -> 2
```

---

## PART 3 — Count Frequencies

```cpp
for(char ch : s1){
    freq[ch]++;
}
```

### Example

```text
s1 = "geeks"
```

Map becomes:

```text
g -> 1
e -> 2
k -> 1
s -> 1
```

---

## MOST IMPORTANT

```text
First string ADDS frequencies.
```

---

## PART 4 — Traverse Second String

```cpp
for(char ch : s2)
```

Now second string REMOVES frequencies.

---

## PART 5 — VERY IMPORTANT CHECK

```cpp
if(freq.find(ch) == freq.end())
    return false;
```

### WHAT DOES THIS MEAN?

It checks:

```text
"Does this character exist in hashmap?"
```

If NOT:

Second string contains character not present in first string.

---

## EXAMPLE

```text
s1 = "abc"
s2 = "abd"
```

Map contains:

```text
a,b,c
```

But:

```text
d
```

does not exist.

So return false.

---

## VERY IMPORTANT INTERVIEW POINT

```cpp
freq.find(ch) == freq.end()
```

means:

```text
character not found
```

---

## PART 6 — Remove Frequencies

```cpp
freq[ch]--;
```

This cancels frequencies.

### Example

Before:

```text
g -> 1
e -> 2
k -> 1
s -> 1
```

Processing:

```text
k
```

After:

```text
k -> 0
```

---

## MOST IMPORTANT

```text
Second string REMOVES frequencies.
```

---

## PART 7 — Negative Frequency Check

```cpp
if(freq[ch] < 0)
    return false;
```

Negative means:

```text
Second string contains extra occurrences.
```

---

## EXAMPLE

```text
s1 = "aab"
s2 = "abb"
```

Map initially:

```text
a -> 2
b -> 1
```

Processing second `b`:

```text
b -> -1
```

Negative.

So return false.

---

## PART 8 — Return True

```cpp
return true;
```

If all checks pass:
- no missing characters
- no extra frequencies
- lengths same

Therefore:
strings are anagrams.

---

# FULL HASHMAP DRY RUN

## INPUT

```text
s1 = "geeks"
s2 = "kseeg"
```

---

## STEP 1

Length check:

```text
5 == 5
```

Continue.

---

## STEP 2

Build map from s1:

```text
g -> 1
e -> 2
k -> 1
s -> 1
```

---

## STEP 3

Traverse s2.

### Character

```text
k
```

Map:

```text
k -> 0
```

### Character

```text
s
```

Map:

```text
s -> 0
```

### Character

```text
e
```

Map:

```text
e -> 1
```

### Character

```text
e
```

Map:

```text
e -> 0
```

### Character

```text
g
```

Map:

```text
g -> 0
```

Everything balanced.

Return:

```text
true
```

---

# ARRAY OPTIMIZATION

## IMPORTANT OBSERVATION

Problem says:

```text
only lowercase English letters
```

That means:
only:

```text
a to z
```

exist.

So instead of hashmap,
we can use array of size 26.

---

# WHY ARRAY WORKS

## MAPPING

```text
0 -> a
1 -> b
2 -> c
...
25 -> z
```

Using:

```cpp
ch - 'a'
```

---

## EXAMPLE

```cpp
'c' - 'a'
```

ASCII:

```text
99 - 97 = 2
```

So:

```text
'c' maps to index 2
```

---

# MOST IMPORTANT IDEA

```text
Character gets converted into array index.
```

---

# ARRAY CODE (OPTIMAL)

```cpp
class Solution {
  public:
    bool areAnagrams(string& s1, string& s2) {

        // Different lengths means impossible
        if(s1.size() != s2.size())
            return false;

        int freq[26] = {0};

        // Add frequencies from first string
        for(char ch : s1){
            freq[ch - 'a']++;
        }

        // Remove frequencies using second string
        for(char ch : s2){

            freq[ch - 'a']--;

            // Extra occurrences found
            if(freq[ch - 'a'] < 0)
                return false;
        }

        return true;
    }
};
```

---

# ARRAY CODE EXPLANATION

## PART 1 — Create Frequency Array

```cpp
int freq[26] = {0};
```

Initially:

```text
[0,0,0,0,0....]
```

Every index represents one character.

---

## MAPPING

```text
index 0 -> a
index 1 -> b
index 2 -> c
...
index 25 -> z
```

---

## PART 2 — Add Frequencies

```cpp
freq[ch - 'a']++;
```

### Example

```text
'a' - 'a' = 0
'b' - 'a' = 1
'c' - 'a' = 2
```

So:

```cpp
freq[0]++
```

means frequency of `a` increases.

---

## EXAMPLE

```text
s1 = "aabc"
```

Iterations:

```text
a -> freq[0]++
a -> freq[0]++
b -> freq[1]++
c -> freq[2]++
```

Final array:

```text
[2,1,1,0,0...]
```

Meaning:

```text
a -> 2
b -> 1
c -> 1
```

---

## MOST IMPORTANT

```text
First string ADDS frequencies.
```

---

## PART 3 — Remove Frequencies

```cpp
freq[ch - 'a']--;
```

Second string REMOVES frequencies.

If perfectly balanced:

```text
all values become zero.
```

---

## PART 4 — Why No find() Needed?

VERY IMPORTANT DIFFERENCE.

HASHMAP:

Keys are created dynamically.

So we need:

```cpp
if(freq.find(ch) == freq.end())
```

to check whether character exists.

---

BUT ARRAY IS DIFFERENT.

In array:

```cpp
int freq[26]
```

ALL 26 positions already exist.

Meaning:

```text
freq[0]
freq[1]
...
freq[25]
```

always exist.

There is NO concept of:

```text
"key not found"
```

---

## EXAMPLE

```text
'd' - 'a' = 3
```

So:

```cpp
freq[3]
```

ALWAYS exists.

That is why array approach does NOT need:

```cpp
find()
```

---

# THEN HOW DOES ARRAY DETECT INVALID CHARACTERS?

Using:

```cpp
if(freq[ch - 'a'] < 0)
```

Negative frequency means:
second string contains extra occurrences.

---

## EXAMPLE

```text
s1 = "abc"
s2 = "abd"
```

After processing s1:

```text
a -> 1
b -> 1
c -> 1
d -> 0
```

Now processing s2:

```text
a -> 0
b -> 0
d -> -1
```

NEGATIVE!

Meaning:

```text
second string has extra d
```

So return false.

---

# MOST IMPORTANT DIFFERENCE

## HASHMAP

```cpp
if(freq.find(ch) == freq.end())
```

checks:

```text
"Does character exist?"
```

---

## ARRAY

```cpp
if(freq[ch - 'a'] < 0)
```

checks:

```text
"Did this character appear extra times?"
```

---

# WHY ARRAY IS BETTER HERE

Because:
- fixed character range
- only 26 lowercase letters exist

Array gives:
- direct indexing
- no hashing
- less memory
- faster execution

---

# HASHMAP VS ARRAY

## HASHMAP

```cpp
unordered_map<char,int>
```

### Pros
- flexible
- works for all characters

### Cons
- extra hashing overhead
- slower than array

---

## ARRAY

```cpp
int freq[26]
```

### Pros
- faster
- constant space
- optimized

### Cons
- only works for fixed ranges

---

# TIME COMPLEXITY

## HASHMAP

```text
O(n)
```

Why?

```text
First traversal -> O(n)
Second traversal -> O(n)
```

Total:

```text
O(2n) -> O(n)
```

---

## ARRAY

```text
O(n)
```

Same logic.

BUT:

```text
Array is faster practically due to direct indexing.
```

---

# SPACE COMPLEXITY

## HASHMAP

```text
O(k)
```

where:

```text
k = unique characters
```

---

## ARRAY

```text
O(26)
```

which becomes:

```text
O(1)
```

constant space.

---

# WHAT INTERVIEWERS EXPECT

Strong candidates usually say:

> “Hashmap works well for frequency counting.
> But since the problem states only lowercase English letters,
> we can optimize further using a fixed-size array of 26.”

This shows:
- observation
- optimization skills
- interview maturity

---

# INTERVIEW EXPLANATION

> “In anagrams, order does not matter — frequencies matter.
>
> So first I check whether lengths differ because then they can never be anagrams.
>
> Then I count frequencies using the first string.
>
> While traversing the second string, I reduce frequencies.
>
> If a character is missing or frequency becomes negative, the strings are not anagrams.
>
> Since the problem contains only lowercase English letters, I can optimize the hashmap solution using a frequency array of size 26.”
````
