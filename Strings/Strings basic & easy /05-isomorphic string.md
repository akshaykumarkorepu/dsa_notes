
# Note

## PROBLEM: Isomorphic Strings

---

# PATTERN:

Hashing / Character Mapping

---

# WHY THIS PATTERN:

This problem is about maintaining a:

```text
One-to-One Mapping
```

between characters of two strings.

We need to ensure:

1. Same character always maps consistently
2. Two different characters do NOT map to same character

This is NOT a frequency matching problem.

This is a:

- mapping problem
- consistency problem
- uniqueness problem

Whenever you hear:

- "replace characters"
- "map characters"
- "consistent replacement"

immediately think:

```text
HashMap + uniqueness checking
```

---

# CORE IDEA

We traverse both strings together.

At every index:

```text
s1[i] -> s2[i]
```

represents the CURRENT mapping.

For every mapping we must check:

---

## RULE 1 — CONSISTENCY

Same character must always map the same way.

VALID:

```text
a -> x
a -> x
```

INVALID:

```text
a -> x
a -> y
```

---

## RULE 2 — UNIQUENESS

Two different characters cannot map to same character.

INVALID:

```text
a -> x
b -> x
```

---

# WHY WE USE HASHMAP + HASHSET

---

# HASHMAP

```cpp
unordered_map<char,char> mp;
```

Stores:

```text
s1 -> s2 mapping
```

Example:

```text
a -> x
b -> y
```

Purpose:

```text
Checks consistency
```

Meaning:

```text
If a mapped to x earlier,
it must STILL map to x.
```

---

# HASHSET

```cpp
unordered_set<char> used;
```

Stores:

```text
already occupied characters from s2
```

Example:

```text
used = {x,y}
```

Meaning:

```text
x and y are already taken
```

Purpose:

```text
Checks uniqueness
```

Meaning:

```text
No other character can map to x now.
```

---

# IMPORTANT INTUITION

This question is NOT asking:

```text
Do frequencies match?
```

It is asking:

```text
Is mapping valid, consistent and unique?
```

That is the MOST important understanding.

---

# STEP-BY-STEP ALGORITHM

For every index:

```cpp
char c1 = s1[i];
char c2 = s2[i];
```

Meaning:

```text
Current mapping:
c1 -> c2
```

---

# CASE 1 — CHARACTER ALREADY MAPPED

```cpp
if(mp.count(c1))
```

This asks:

```text
Did we already map c1 earlier?
```

If YES:

verify consistency.

```cpp
if(mp[c1] != c2)
    return false;
```

Meaning:

```text
Earlier mapping and current mapping mismatch
```

INVALID.

---

# CASE 2 — CHARACTER APPEARING FIRST TIME

```cpp
else
```

Now we try creating new mapping.

Before creating mapping:

check whether target character already occupied.

```cpp
if(used.count(c2))
    return false;
```

Meaning:

```text
Another character already mapped here
```

INVALID.

---

# OTHERWISE

Create mapping.

```cpp
mp[c1] = c2;
used.insert(c2);
```

Meaning:

```text
Store mapping
+
Mark target character occupied
```

---

# HASHMAP DRY RUN (VALID CASE)

## INPUT

```text
s1 = "aab"
s2 = "xxy"
```

---

# INITIAL STATE

```text
mp = {}
used = {}
```

---

# ITERATION 1

Current mapping:

```text
a -> x
```

- a not mapped before
- x not occupied

Create mapping.

```text
mp = {a:x}
used = {x}
```

---

# ITERATION 2

Current mapping:

```text
a -> x
```

- a already mapped
- old mapping = x
- current mapping = x

Consistent.

Continue.

---

# ITERATION 3

Current mapping:

```text
b -> y
```

- b not mapped
- y free

Create mapping.

```text
mp = {a:x,b:y}
used = {x,y}
```

Loop finishes.

Return:

```text
true
```

---

# FAILURE CASE 1

## INPUT

```text
s1 = "aab"
s2 = "xyz"
```

---

# FAILURE

Earlier:

```text
a -> x
```

Now:

```text
a -> y
```

This line detects inconsistency:

```cpp
if(mp[c1] != c2)
```

becomes:

```text
x != y
```

TRUE.

Return false.

---

# FAILURE CASE 2

## INPUT

```text
s1 = "abc"
s2 = "xxz"
```

---

# FAILURE

Earlier:

```text
a -> x
```

Now:

```text
b -> x
```

This line detects uniqueness violation:

```cpp
if(used.count(c2))
```

because:

```text
x already occupied
```

Return false.

---

# FULL HASHMAP + HASHSET CODE

```cpp
class Solution {
  public:
    bool areIsomorphic(string &s1, string &s2) {

        unordered_map<char,char> mp;
        unordered_set<char> used;

        for(int i=0;i<s1.size();i++){

            char c1 = s1[i];
            char c2 = s2[i];

            if(mp.count(c1)){

                if(mp[c1] != c2)
                    return false;
            }

            else{

                if(used.count(c2))
                    return false;

                mp[c1] = c2;
                used.insert(c2);
            }
        }

        return true;
    }
};
```

---

# ARRAY OPTIMIZATION

Since character range is fixed:

```text
26 lowercase letters
```

we can replace hashmap with arrays.

This removes hashing overhead and gives:

```text
direct indexing
```

---

# WHY ARRAYS WORK

Characters are internally numbers.

| Character | Index |
|---|---|
| a | 0 |
| b | 1 |
| c | 2 |
| x | 23 |

This line:

```cpp
int c1 = s1[i] - 'a';
```

converts character into index.

Example:

```text
'c' - 'a' = 2
```

because:

```text
c is 2 positions after a
```

---

# WHY TWO ARRAYS?

---

# map1

```cpp
map1[c1]
```

stores:

```text
s1 -> s2
```

Example:

```text
a -> x
```

---

# map2

```cpp
map2[c2]
```

stores:

```text
s2 -> s1
```

Example:

```text
x -> a
```

Purpose:

```text
Checks reverse uniqueness
```

---

# WHY map2 IS IMPORTANT

Suppose:

```text
a -> x
```

already exists.

Now later:

```text
b -> x
```

Without map2,
you cannot detect:

```text
x already occupied
```

---

# WHY +1 IS USED

Initially:

```cpp
int map1[26] = {0};
```

All values are:

```text
0
```

Meaning:

```text
UNMAPPED
```

---

# PROBLEM

Suppose:

```text
a -> a
```

Then:

```text
c2 = 0
```

If we directly stored:

```cpp
map1[c1] = c2;
```

then:

```cpp
map1[0] = 0;
```

But:

```text
0 already means unmapped
```

Problem.

---

# SO WE STORE

```text
c2 + 1
```

Example:

| Actual Index | Stored Value |
|---|---|
| 0 | 1 |
| 1 | 2 |
| 23 | 24 |

Now:

```text
0
```

safely means:

```text
UNMAPPED
```

---

# ARRAY CONDITIONS

---

# NEW MAPPING

```cpp
if(map1[c1] == 0 && map2[c2] == 0)
```

Meaning:

```text
Neither side mapped yet
```

So create mapping.

---

# INCONSISTENT MAPPING

```cpp
else if(map1[c1] != c2 + 1 || map2[c2] != c1 + 1)
```

Meaning:

```text
mapping inconsistent
```

Return false.

---

# ARRAY DRY RUN FAILURE CASE

## INPUT

```text
s1 = "abc"
s2 = "xxz"
```

---

# ITERATION 1

```text
a -> x
```

Store mapping.

```text
map1[0] = 24
map2[23] = 1
```

Meaning:

```text
a -> x
x -> a
```

---

# ITERATION 2

```text
b -> x
```

Now:

```cpp
map2[23] != 0
```

because:

```text
x already mapped by a
```

So mapping becomes inconsistent.

Return false.

---

# FULL ARRAY CODE

```cpp
class Solution {
  public:
    bool areIsomorphic(string &s1, string &s2) {

        int map1[26] = {0};
        int map2[26] = {0};

        for (int i = 0; i < s1.length(); i++) {

            int c1 = s1[i] - 'a';
            int c2 = s2[i] - 'a';

            if (map1[c1] == 0 && map2[c2] == 0) {

                map1[c1] = c2 + 1;
                map2[c2] = c1 + 1;
            }

            else if (map1[c1] != c2 + 1 || map2[c2] != c1 + 1) {

                return false;
            }
        }

        return true;
    }
};
```

---

# TIME COMPLEXITY

## TC: O(n)

We traverse strings once.

Every operation:

- hashmap lookup
- hashmap insertion
- array access
- set lookup

takes:

```text
O(1)
```

So overall:

```text
O(n)
```

---

# SPACE COMPLEXITY

## HASHMAP VERSION

Technically:

```text
O(26)
```

because only lowercase letters.

So:

```text
O(1)
```

---

# ARRAY VERSION

Arrays are fixed size:

```text
26
```

So:

```text
O(1)
```

---

# INTERVIEW FLOW

If interviewer asks:

```text
Explain your approach.
```

Say:

---

“We need to maintain a one-to-one mapping between characters of both strings.

So while traversing both strings together:

- if a character was already mapped earlier,
  its mapping must remain consistent

- if it is appearing for first time,
  then target character should not already be occupied

To implement this:

- I use hashmap for consistency
- and hashset for uniqueness

Since character range is fixed,
we can further optimize this using arrays.”

---

# WHEN TO USE HASHMAP

Use hashmap when:

- character range unknown
- Unicode
- strings
- objects

---

# WHEN TO USE ARRAYS

Use arrays when:

- lowercase letters
- ASCII chars
- fixed small range

---

# EDGE CASES

## 1. Same character mapping differently

```text
a -> x
a -> y
```

INVALID.

---

## 2. Different characters mapping same way

```text
a -> x
b -> x
```

INVALID.

---

## 3. Same strings

```text
abc -> abc
```

VALID.

Characters can map to themselves.

---

## 4. Single character strings

```text
a -> z
```

VALID.

---

# WHY I MIGHT FORGET THIS QUESTION

- forgetting reverse uniqueness
- only checking forward mapping
- confusing it with frequency matching
- forgetting why map2 exists
- forgetting why +1 is used in arrays

---

# MEMORY TRICK

Think:

```text
One-to-One Mapping
```

NOT:

```text
Frequency Matching
```

The moment you hear:

```text
consistent replacement
```

immediately think:

```text
HashMap + uniqueness checking
```

Then optimize to:

```text
Array mapping
```
````
