

# PROBLEM:
Sort by Frequency

# PATTERN:
Hashing + Custom Sorting

# WHY THIS PATTERN:
The question depends on:

1. Counting frequencies efficiently
2. Sorting using custom conditions

We use:
- HashMap → frequency counting
- Vector + Comparator → custom sorting

---

# PROBLEM UNDERSTANDING

Given a string:

```cpp
s = "geeksforgeeks"
```

We must arrange characters according to:

## Rule 1
Smaller frequency comes first.

## Rule 2
If frequencies are same:
- lexicographically smaller character comes first.

---

# EXAMPLE

Input:

```cpp
"geeksforgeeks"
```

Frequency Table:

| Character | Frequency |
|---|---|
| f | 1 |
| o | 1 |
| r | 1 |
| g | 2 |
| k | 2 |
| s | 2 |
| e | 4 |

Now sort:

- Frequency 1 → f o r
- Frequency 2 → g g k k s s
- Frequency 4 → e e e e

Final Answer:

```cpp
"forggkksseeee"
```

---

# CORE IDEA

## Step 1
Count frequencies using hashmap.

## Step 2
Store `(character, frequency)` pairs in vector.

## Step 3
Sort vector using custom comparator.

## Step 4
Build final string.

---

# WHY BRUTE FORCE IS NOT NECESSARY

Brute force would:
- repeatedly count frequencies
- manually arrange characters

This is inefficient.

Optimal approach:
- HashMap gives frequency in O(1)
- Sorting only unique characters is efficient

Since only lowercase letters exist:
- maximum unique characters = 26

So optimal approach is enough.

---

# INTERVIEW EXPLANATION FLOW

If interviewer asks:

---

## Step 1 — Explain intuition

Say:

> “The question depends on frequencies, so first I’ll count frequency of every character using a hashmap.”

---

## Step 2 — Explain sorting need

Say:

> “Now I need custom sorting:
> - smaller frequency first
> - if same frequency, lexicographically smaller first.”

---

## Step 3 — Explain vector usage

Say:

> “unordered_map cannot be sorted directly, so I’ll store all pairs inside a vector.”

---

## Step 4 — Explain comparator

Say:

> “Comparator checks:
> - same frequency → alphabetical order
> - otherwise → smaller frequency first.”

---

## Step 5 — Explain answer construction

Say:

> “Finally, I’ll append every character frequency number of times into the answer string.”

---

# IMPORTANT CONCEPTS USED

| Concept | Meaning |
|---|---|
| unordered_map | frequency counting |
| pair<char,int> | stores character + frequency |
| vector<pair<char,int>> | stores all pairs |
| custom comparator | custom sorting rules |
| string(n,ch) | repeat character n times |

---

# FULL OPTIMAL CODE

```cpp
class Solution {
  public:
  
    string frequencySort(string &s) {
        
        // Step 1: Count frequencies
        unordered_map<char,int> freq;
        
        for(char ch : s){
            freq[ch]++;
        }
        
        
        // Step 2: Store character-frequency pairs
        vector<pair<char,int>> arr;
        
        for(auto it : freq){
            arr.push_back(it);
        }
        
        
        // Step 3: Sort vector
        sort(arr.begin(), arr.end(),
        
            [](pair<char,int> &a, pair<char,int> &b){
                 
                // Same frequency
                if(a.second == b.second)
                    return a.first < b.first;
            
                // Smaller frequency first
                return a.second < b.second;
            }
        );
        
        
        // Step 4: Build final answer
        string ans = "";
    
        for(auto it : arr){
            
            // Repeat character frequency number of times
            ans += string(it.second, it.first);
        }
    
    
        // Step 5: Return answer
        return ans;
    }
};
```

---

# STEP-BY-STEP DETAILED EXPLANATION

---

# STEP 1 — Frequency Counting

```cpp
unordered_map<char,int> freq;
```

Stores:

```text
character -> frequency
```

---

# Loop

```cpp
for(char ch : s){
    freq[ch]++;
}
```

---

# Dry Run

Input:

```cpp
s = "tree"
```

---

## Iteration 1

```cpp
ch = 't'
```

Map:

```text
t -> 1
```

---

## Iteration 2

```cpp
ch = 'r'
```

Map:

```text
t -> 1
r -> 1
```

---

## Iteration 3

```cpp
ch = 'e'
```

Map:

```text
t -> 1
r -> 1
e -> 1
```

---

## Iteration 4

```cpp
ch = 'e'
```

Map:

```text
t -> 1
r -> 1
e -> 2
```

Final Map:

| Character | Frequency |
|---|---|
| t | 1 |
| r | 1 |
| e | 2 |

---

# STEP 2 — Store Pairs into Vector

```cpp
vector<pair<char,int>> arr;
```

Each pair stores:

```text
(character, frequency)
```

---

# Loop

```cpp
for(auto it : freq){
    arr.push_back(it);
}
```

---

# Why Needed?

Because:

```text
unordered_map cannot be sorted directly
```

So we move data into vector.

---

# Vector After Loop

```cpp
[
 ('t',1),
 ('r',1),
 ('e',2)
]
```

---

# STEP 3 — Sorting

```cpp
sort(arr.begin(), arr.end(),
```

---

# Meaning

Sort entire vector.

---

# Comparator

```cpp
[](pair<char,int> &a, pair<char,int> &b)
```

means:

```text
Compare two pairs at a time
```

---

# VERY IMPORTANT

## If comparator returns TRUE

```text
a should come before b
```

## If comparator returns FALSE

```text
a should NOT come before b
```

---

# Comparator Logic

```cpp
if(a.second == b.second)
    return a.first < b.first;

return a.second < b.second;
```

---

# Meaning of `.first` and `.second`

| Expression | Meaning |
|---|---|
| a.first | character |
| a.second | frequency |

---

# CASE 1 — Same Frequency

```cpp
if(a.second == b.second)
```

Meaning:

```text
If frequencies are equal
```

Then:

```cpp
return a.first < b.first;
```

Meaning:

```text
Alphabetically smaller character first
```

---

# Example

Compare:

```cpp
('b',1) and ('a',1)
```

Check:

```cpp
1 == 1
```

TRUE.

Now:

```cpp
'b' < 'a'
```

FALSE.

Meaning:

```text
('a',1) comes before ('b',1)
```

---

# CASE 2 — Different Frequencies

```cpp
return a.second < b.second;
```

Meaning:

```text
Smaller frequency comes first
```

---

# Example

Compare:

```cpp
('x',1) and ('y',3)
```

Check:

```cpp
1 < 3
```

TRUE.

Meaning:

```text
('x',1) comes before ('y',3)
```

---

# COMPLETE SORT DRY RUN

Current Vector:

```cpp
[
 ('t',1),
 ('r',1),
 ('e',2)
]
```

---

# Compare ('t',1) and ('r',1)

Check:

```cpp
1 == 1
```

TRUE.

Now:

```cpp
't' < 'r'
```

FALSE.

Meaning:

```text
r comes before t
```

---

# Compare ('e',2) and ('r',1)

Check:

```cpp
2 < 1
```

FALSE.

Meaning:

```text
r comes before e
```

---

# Final Sorted Vector

```cpp
[
 ('r',1),
 ('t',1),
 ('e',2)
]
```

---

# STEP 4 — Build Final Answer

```cpp
string ans = "";
```

Initially:

```cpp
ans = ""
```

---

# Loop

```cpp
for(auto it : arr)
```

Iterates through sorted vector.

---

# MOST IMPORTANT LINE

```cpp
ans += string(it.second, it.first);
```

---

# Understanding This

```cpp
string(count, character)
```

creates:

```text
character repeated count times
```

---

# Example

```cpp
string(4,'e')
```

creates:

```cpp
"eeee"
```

---

# COMPLETE DRY RUN

Current Vector:

```cpp
[
 ('r',1),
 ('t',1),
 ('e',2)
]
```

---

# Iteration 1

```cpp
it = ('r',1)
```

This:

```cpp
string(1,'r')
```

creates:

```cpp
"r"
```

Now:

```cpp
ans = "r"
```

---

# Iteration 2

```cpp
it = ('t',1)
```

Creates:

```cpp
"t"
```

Now:

```cpp
ans = "rt"
```

---

# Iteration 3

```cpp
it = ('e',2)
```

Creates:

```cpp
"ee"
```

Now:

```cpp
ans = "rtee"
```

---

# FINAL ANSWER

```cpp
return ans;
```

returns:

```cpp
"rtee"
```

---

# COMMON MISTAKES

---

# Mistake 1

```cpp
for(auto it : arr)
```

instead of:

```cpp
for(auto it : freq)
```

Remember:

```text
freq = source
arr = destination
```

---

# Mistake 2

Wrong sort closing:

```cpp
};
```

Correct:

```cpp
);
```

Because:
- `}` closes lambda
- `)` closes sort
- `;` ends statement

---

# Mistake 3

Confusing `.first` and `.second`

Remember:

```text
.first = character
.second = frequency
```

---

# WHY I MIGHT FORGET THIS QUESTION

1. Forgetting unordered_map cannot be sorted
2. Confusing comparator logic
3. Forgetting sort syntax
4. Forgetting meaning of:

```cpp
string(count, character)
```

---

# EASY MEMORY TRICK

```text
.first  -> character
.second -> frequency
```

Comparator says:

```text
Same frequency:
    smaller alphabet first

Different frequency:
    smaller frequency first
```

---

# TIME COMPLEXITY

---

# Frequency Counting

```cpp
O(n)
```

because we traverse string once.

---

# Sorting

At most 26 lowercase letters.

```cpp
O(26 log 26)
```

which is constant.

---

# Final Time Complexity

```cpp
O(n)
```

---

# SPACE COMPLEXITY

HashMap stores at most 26 characters.

Vector stores at most 26 pairs.

So:

```cpp
O(26)
```

which becomes:

```cpp
O(1)
```

constant space.
````
