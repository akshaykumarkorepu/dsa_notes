
# Note

## PROBLEM: Isomorphic Strings

---

# PATTERN:

Hashing / Character Mapping

---

# WHY THIS PATTERN:

This question is about maintaining a:

```text
One-to-One Mapping
```

between characters of two strings.

We need to ensure:

1. Same character always maps consistently
2. Two different characters do NOT map to same character

This is NOT a frequency problem.

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

For every mapping:

We must check:

## 1. Was this character already mapped before?

If YES:

- current mapping MUST match old mapping

Example:

```text
a -> x
a -> x
```

VALID.

But:

```text
a -> x
a -> y
```

INVALID.

---

## 2. If character is new:

Then target character must NOT already be occupied.

Example:

```text
a -> x
b -> x
```

INVALID.

Because two characters cannot map to same character.

---

# WHY WE USE HASHMAP AND HASHSET

---

# WHY HASHMAP?

```cpp
unordered_map<char, char> mp;
```

We use a:

```text
HashMap
```

because we need to store:

```text
relationship / mapping
```

between characters.

Specifically:

```text
character from s1 -> character from s2
```

Example:

```text
a -> x
b -> y
```

---

# WHAT DOES HASHMAP HELP US ANSWER?

It helps answer:

```text
What was this character mapped to earlier?
```

Example:

Suppose earlier:

```text
a -> x
```

was stored.

Now later if we see:

```text
a -> y
```

we can check:

```cpp
mp['a']
```

which gives:

```text
x
```

Then we compare:

```text
x != y
```

So mapping became inconsistent.

INVALID.

---

# VERY IMPORTANT UNDERSTANDING

HashMap is needed because:

```text
we need BOTH:
- key
- value
```

We are storing:

| Key | Value |
|---|---|
| a | x |
| b | y |

A set CANNOT do this.

Because set only stores values.

It cannot store relationships.

---

# SO HASHMAP IS USED FOR:

```text
Checking consistency
```

Meaning:

```text
Did this character map the same way earlier?
```

---

# WHY HASHSET?

```cpp
unordered_set<char> used;
```

We use a:

```text
HashSet
```

because we ONLY care about:

```text
whether a character is already occupied or not
```

We do NOT care:

```text
WHO mapped to it
```

We only care:

```text
Is this target character already taken?
```

---

# EXAMPLE

Suppose:

```text
a -> x
```

already exists.

Now:

```text
x
```

is occupied.

So we store:

```text
used = {x}
```

---

# NOW LATER

Suppose we try:

```text
b -> x
```

Before creating mapping:

we check:

```cpp
used.count('x')
```

If TRUE:

then:

```text
x already occupied
```

So:

```text
INVALID
```

---

# IMPORTANT UNDERSTANDING

For uniqueness checking:

we ONLY need:

```text
existence checking
```

We do NOT need mapping.

We only care:

```text
Is x already used?
```

YES or NO.

That is exactly what Set is best for.

---

# SIMPLE INTUITION

## HashMap

```text
Stores relationships
```

Example:

```text
a -> x
```

---

## HashSet

```text
Stores occupied characters
```

Example:

```text
x already taken
```

---

# MEMORY TRICK

Think:

## HashMap

```text
Who maps to whom?
```

---

## HashSet

```text
Which characters are already occupied?
```

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

# CASE 1

## Character already mapped before

```cpp
if(mp.count(c1))
```

This asks:

```text
Did we already map c1 earlier?
```

If YES:

verify mapping consistency.

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

# CASE 2

## Character appearing first time

```cpp
else
```

Now we try to create new mapping.

Before creating mapping:

check whether target character already occupied.

```cpp
if(used.count(c2))
    return false;
```

This asks:

```text
Is c2 already occupied?
```

If YES:

```text
Another character already maps to c2
```

INVALID.

---

# OTHERWISE

Create new mapping.

```cpp
mp[c1] = c2;
used.insert(c2);
```

Meaning:

```text
Store mapping
+
Mark target character as occupied
```

---

# VERY IMPORTANT INTUITION

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

# DRY RUN 1 (VALID CASE)

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

### Check:

```text
Was a already mapped?
```

NO.

### Check:

```text
Is x already occupied?
```

NO.

### Create mapping

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

### Check:

```text
Was a already mapped?
```

YES.

Earlier:

```text
a -> x
```

Current:

```text
a -> x
```

Same mapping.

VALID.

Nothing changes.

---

# ITERATION 3

Current mapping:

```text
b -> y
```

### Check:

```text
Was b already mapped?
```

NO.

### Check:

```text
Is y occupied?
```

NO.

### Create mapping

```text
mp = {a:x, b:y}
used = {x,y}
```

---

# LOOP FINISHES

No conflicts found.

Return:

```text
true
```

---

# DRY RUN 2 (FAILURE CASE)

## INPUT

```text
s1 = "aab"
s2 = "xyz"
```

---

# ITERATION 1

```text
a -> x
```

Store mapping.

```text
mp = {a:x}
used = {x}
```

---

# ITERATION 2

Current mapping:

```text
a -> y
```

But earlier:

```text
a -> x
```

Now:

```cpp
if(mp[c1] != c2)
```

becomes:

```text
x != y
```

TRUE.

Return:

```text
false
```

---

# WHY DID IT FAIL?

Because:

```text
Same character mapped differently
```

---

# DRY RUN 3 (FAILURE CASE)

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
mp = {a:x}
used = {x}
```

---

# ITERATION 2

Current mapping:

```text
b -> x
```

Check:

```text
Is x already occupied?
```

YES.

Because earlier:

```text
a -> x
```

already existed.

So:

```cpp
if(used.count(c2))
```

becomes TRUE.

Return:

```text
false
```

---

# WHY DID IT FAIL?

Because:

```text
Two characters mapped to same character
```

---

# INTERVIEW EXPLANATION FLOW

If interviewer asks:

```text
Explain your approach.
```

Say this:

---

“We need to maintain a one-to-one mapping between characters of both strings.

So while traversing both strings together:

- if a character from s1 was already mapped earlier, then its current mapping must remain consistent
- if it is appearing for first time, then the target character in s2 must not already be occupied by another character

To implement this:

- I use a hashmap to store mappings
- and a hashset to track already occupied target characters

This guarantees both consistency and uniqueness of mapping.”

---

# IMPORTANT CODE SNIPPETS

## CHECK IF CHARACTER ALREADY MAPPED

```cpp
if(mp.count(c1))
```

Meaning:

```text
Did this character already get mapped earlier?
```

---

# VERIFY CONSISTENCY

```cpp
if(mp[c1] != c2)
    return false;
```

Meaning:

```text
Earlier mapping and current mapping mismatch
```

---

# CHECK IF TARGET ALREADY OCCUPIED

```cpp
if(used.count(c2))
    return false;
```

Meaning:

```text
Another character already mapped here
```

---

# CREATE NEW MAPPING

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

# FULL CODE

```cpp
class Solution {
  public:

    bool areIsomorphic(string s1, string s2) {

        unordered_map<char, char> mp;
        unordered_set<char> used;

        for(int i = 0; i < s1.size(); i++) {

            char c1 = s1[i];
            char c2 = s2[i];

            if(mp.count(c1)) {

                if(mp[c1] != c2)
                    return false;
            }
            else {

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

# TIME COMPLEXITY

## TC: O(n)

We traverse the strings once.

Loop runs:

```text
n times
```

Inside every iteration:

- hashmap lookup → O(1)
- hashmap insertion → O(1)
- set lookup → O(1)
- set insertion → O(1)

So total:

```text
O(n)
```

---

# SPACE COMPLEXITY

## SC: O(1)

Why?

Only lowercase English letters exist.

Maximum mappings possible:

```text
26
```

So hashmap/set size never grows beyond constant size.

Technically:

```text
O(26) = O(1)
```

---

# BRUTE FORCE?

NOT REQUIRED.

Reason:

The hashmap solution is already direct and intuitive.

There is no meaningful brute-force progression here.

Most interviewers expect the optimal mapping approach immediately.

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

## 3. Single character strings

```text
a -> z
```

VALID.

---

## 4. Same strings

```text
abc -> abc
```

VALID.

Characters can map to themselves.

---

# WHY I MIGHT FORGET THIS QUESTION

- forgetting reverse uniqueness condition
- only checking consistency
- forgetting that:

```text
b -> x
```

must fail if:

```text
a -> x
```

already exists

- confusing this with frequency matching problems

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
HashMap + HashSet
```
````
