

## PROBLEM:
Print Anagrams Together

Given an array of strings, group together all strings that are anagrams.

Example:

Input:

```text
act, god, cat, dog, tac
```

Output:

```text
[
 [act, cat, tac],
 [god, dog]
]
```

---

# PATTERN:
HashMap + String Transformation

---

# WHY THIS PATTERN:

We need to:

- identify related strings
- group them efficiently
- avoid repeated comparisons

The key observation is:

All anagrams become identical after sorting.

Example:

```text
act → act
cat → act
tac → act
```

Since all become same after sorting,
we can use the sorted string as a GROUP KEY.

HashMap is perfect because we need:

```text
key → group of strings
```

---

# CORE IDEA

Convert every string into a standard form.

That standard form is:

```text
SORTED STRING
```

Example:

```text
act → act
cat → act
tac → act
```

All belong to same group because sorted version is same.

So:

1. Sort every string
2. Use sorted string as hashmap key
3. Store original string inside that group

---

# INTERVIEW FLOW (VERY IMPORTANT)

## STEP 1 — Explain Initial Thinking

Say:

> “Initially I thought of comparing every string with every other string to check whether they are anagrams.”

---

# BRUTE FORCE

For every pair:

- sort both strings
- compare sorted versions
- if equal → same group

Example:

```text
act vs cat
```

Sorting:

```text
act → act
cat → act
```

Same → anagrams.

---

# WHY BRUTE FORCE IS BAD

Suppose:

```text
N strings
```

We compare every pair:

```text
O(N²)
```

For every comparison:

sorting costs:

```text
O(K log K)
```

Total:

```text
O(N² × K log K)
```

Very expensive.

---

# OPTIMIZATION THINKING

Ask:

> “Can I avoid pair comparisons completely?”

This is the key optimization step.

---

# BIG OBSERVATION

All anagrams become identical after sorting.

Example:

```text
act → act
cat → act
tac → act
```

So instead of:

```text
checking pairs manually
```

we directly use:

```text
sorted string as identity/group key
```

---

# DATA STRUCTURE

We need:

```text
key → group of strings
```

Example:

```text
act → [act, cat, tac]
```

This is perfect HashMap usage.

So use:

```cpp
unordered_map<string, vector<string>>
```

---

# OPTIMAL APPROACH

For every string:

1. Create copy
2. Sort copy
3. Use sorted copy as hashmap key
4. Store original string inside that group

Finally:

Return all hashmap groups.

---

# OPTIMAL CODE

```cpp
class Solution {
public:

    vector<vector<string>> anagrams(vector<string>& arr) {

        unordered_map<string, vector<string>> mp;

        for(string s : arr) {

            // Create copy
            string temp = s;

            // Sort copy
            sort(temp.begin(), temp.end());

            // Group original string
            mp[temp].push_back(s);
        }

        vector<vector<string>> ans;

        // Store all groups
        for(auto it : mp) {
            ans.push_back(it.second);
        }

        return ans;
    }
};
```

---

# CODE EXPLANATION

## Step 1 — Create HashMap

```cpp
unordered_map<string, vector<string>> mp;
```

We store:

```text
sorted_string → group_of_original_strings
```

Example:

```text
act → [act, cat, tac]
dgo → [god, dog]
```

---

## Step 2 — Traverse Array

```cpp
for(string s : arr)
```

Visit every string one by one.

Example traversal:

```text
act
god
cat
dog
tac
```

---

## Step 3 — Create Copy

```cpp
string temp = s;
```

We create copy because:

we want original string unchanged.

Example:

```text
s = cat
temp = cat
```

---

## Step 4 — Sort Copy

```cpp
sort(temp.begin(), temp.end());
```

Example:

```text
cat → act
```

This creates the STANDARD FORM.

---

## Step 5 — Group Using HashMap

```cpp
mp[temp].push_back(s);
```

Example:

```cpp
mp["act"].push_back("cat");
```

HashMap becomes:

```text
act → [act, cat]
```

This is the MOST IMPORTANT LINE.

---

## Step 6 — Store Final Groups

```cpp
for(auto it : mp) {
    ans.push_back(it.second);
}
```

Take all hashmap groups and store in answer.

---

# MOST IMPORTANT CODE SNIPPETS

## 1. Creating Standard Form

```cpp
sort(temp.begin(), temp.end());
```

This converts:

```text
cat → act
```

This is the CORE transformation.

---

## 2. Grouping Step

```cpp
mp[temp].push_back(s);
```

Meaning:

Store original string inside correct anagram group.

Example:

```cpp
mp["act"].push_back("cat");
```

HashMap becomes:

```text
act → [act, cat]
```

---

## 3. Extracting Final Answer

```cpp
for(auto it : mp) {
    ans.push_back(it.second);
}
```

Store all groups into final answer.

---

# DETAILED DRY RUN

Input:

```text
act, god, cat, dog, tac
```

---

# ITERATION 1

Current string:

```text
act
```

Copy:

```text
temp = act
```

Sort:

```text
temp = act
```

Store:

```text
act → [act]
```

HashMap:

```text
act → [act]
```

---

# ITERATION 2

Current string:

```text
god
```

Sort:

```text
god → dgo
```

Store:

```text
dgo → [god]
```

HashMap:

```text
act → [act]
dgo → [god]
```

---

# ITERATION 3

Current string:

```text
cat
```

Sort:

```text
cat → act
```

Store:

```text
act → [act, cat]
```

HashMap:

```text
act → [act, cat]
dgo → [god]
```

---

# ITERATION 4

Current string:

```text
dog
```

Sort:

```text
dog → dgo
```

Store:

```text
dgo → [god, dog]
```

HashMap:

```text
act → [act, cat]
dgo → [god, dog]
```

---

# ITERATION 5

Current string:

```text
tac
```

Sort:

```text
tac → act
```

Store:

```text
act → [act, cat, tac]
```

Final HashMap:

```text
act → [act, cat, tac]
dgo → [god, dog]
```

---

# FINAL ANSWER

```text
[
 [act, cat, tac],
 [god, dog]
]
```

---

# WHY THIS WORKS

Because:

All anagrams become identical after sorting.

Example:

```text
act → act
cat → act
tac → act
```

Same sorted form
⇒ same group.

---

# TIME COMPLEXITY

Let:

```text
N = number of strings
K = maximum string length
```

For every string:

Sorting costs:

```text
O(K log K)
```

Done for N strings:

```text
O(N × K log K)
```

---

# SPACE COMPLEXITY

HashMap stores all strings.

Total:

```text
O(N × K)
```

---

# EDGE CASES

- Single string
- No anagrams
- All strings same
- Empty input
- Duplicate strings

---

# ADVANCED OPTIMIZATION (OPTIONAL)

Instead of sorting strings,
we can use character frequency as key.

Example:

```text
act
```

Frequency:

```text
a1c1t1
```

cat also produces:

```text
a1c1t1
```

This removes sorting.

Complexity improves from:

```text
O(N × K log K)
```

to:

```text
O(N × K)
```

BUT:

For most on-campus interviews,
sorting + hashmap solution is more than enough.

Mention this optimization ONLY IF:

- interviewer asks for optimization
- constraints are huge
- you finish early and want to discuss improvements

---

# WHY I MIGHT FORGET THIS

I may forget:

- why sorting helps
- why hashmap is needed
- why we create copy before sorting
- that brute force compares every pair

---

# MEMORY TRICK

> “Anagrams become identical after sorting.”

This single observation gives the entire approach instantly.

---

# INTERVIEW EXPLANATION

> “I first thought of comparing every pair of strings and checking whether they are anagrams by sorting. But pair comparisons lead to O(N²) complexity. Then I observed that all anagrams become identical after sorting, so instead of comparing pairs, I used the sorted string itself as a hashmap key to directly group strings together.”
````
