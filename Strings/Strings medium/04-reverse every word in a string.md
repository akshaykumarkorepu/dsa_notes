
# PROBLEM:
Reverse each word in a given string

---

# PATTERN:
String Traversal + Word Extraction

---

# WHY THIS PATTERN:

We need to process the string word by word.

For every word:
- extract the word
- reverse it
- add it into answer

We also need to handle:
- leading spaces
- trailing spaces
- multiple spaces

So manual traversal using an index is the best approach.

---

# CORE IDEA:

Traverse the string using index `i`.

For every iteration:

1. Skip all spaces
2. Extract one complete word
3. Reverse the word
4. Add it into answer
5. Add exactly one space between words

---

# IMPORTANT UNDERSTANDING

We are NOT reversing the whole string.

We are reversing EACH WORD individually.

Example:

Input:

```text
"hi ok"
```

Output:

```text
"ih ko"
```

NOT:

```text
"ko ih"
```

---

# BRUTE FORCE IDEA (Brief Mention)

A simpler/basic approach is using:

```cpp
stringstream
```

It automatically:
- skips extra spaces
- extracts words one by one

Then we:
- reverse each word
- add it into answer

This is acceptable, but interviewers usually prefer manual traversal because it shows stronger string handling skills.

---

# BRUTE FORCE CODE

```cpp
class Solution {
  public:
  
    string reverseWords(string s) {
        
        stringstream ss(s);
        
        string word;
        string ans = "";
        
        while(ss >> word) {
            
            reverse(word.begin(), word.end());
            
            if(ans.length() > 0) {
                ans += ' ';
            }
            
            ans += word;
        }
        
        return ans;
    }
};
```

---

# OPTIMAL APPROACH

Use manual traversal with index `i`.

This gives:
- better control
- proper understanding of spaces
- cleaner interview explanation

---

# OPTIMAL CODE

```cpp
class Solution {
  public:
    
    string reverseWords(string s) {
        
        string ans = "";
        
        int n = s.size();
        int i = 0;
        
        
        // Traverse entire string
        while(i < n) {
            
            
            // STEP 1 → Skip spaces
            while(i < n && s[i] == ' ') {
                i++;
            }
            
            
            // STEP 2 → Extract word
            string word = "";
            
            while(i < n && s[i] != ' ') {
                word += s[i];
                i++;
            }
            
            
            // STEP 3 → Reverse word
            reverse(word.begin(), word.end());
            
            
            // STEP 4 → Add into answer
            if(word.length() > 0) {
                
                // Add one space before next word
                if(ans.length() > 0) {
                    ans += ' ';
                }
                
                ans += word;
            }
        }
        
        return ans;
    }
};
```

---

# IMPORTANT CODE SNIPPETS

---

## 1. Skipping Spaces

```cpp
while(i < n && s[i] == ' ')
```

Meaning:

> Keep moving until non-space character appears.

Used for:
- leading spaces
- trailing spaces
- multiple spaces

---

## 2. Extracting One Complete Word

```cpp
while(i < n && s[i] != ' ')
```

Meaning:

> Keep taking characters until space appears.

This extracts exactly ONE complete word.

---

## 3. Reversing Word

```cpp
reverse(word.begin(), word.end());
```

Example:

```text
"hi" -> "ih"
```

---

## 4. Prevent Processing Empty Word

```cpp
if(word.length() > 0)
```

Meaning:

> Process only if a valid word exists.

Important for:
- trailing spaces
- strings containing only spaces

Example:

```text
"hello     "
```

After skipping trailing spaces:
- no word exists
- word becomes empty

This condition prevents useless processing.

---

## 5. Add Single Space Between Words

```cpp
if(ans.length() > 0)
```

Meaning:

> Add space only before second/third/fourth words.

This avoids:

```text
" ih ko"
```

Correct output:

```text
"ih ko"
```

---

# COMPLETE DRY RUN

# INPUT

```text
s = "hi ok"
```

---

# STRING VISUALIZATION

```text
Index : 0 1 2 3 4

Char  : h i ' ' o k
```

---

# INITIAL STATE

```text
ans = ""
i = 0
n = 5
```

---

# FIRST OUTER LOOP ITERATION

Condition:

```text
0 < 5
```

TRUE.

Enter loop.

---

# STEP 1 → Skip Spaces

Code:

```cpp
while(i < n && s[i] == ' ')
```

Current:

```text
s[0] = 'h'
```

Not a space.

Loop does NOT run.

---

# STEP 2 → Extract Word

Code:

```cpp
while(i < n && s[i] != ' ')
```

---

## Iteration 1

Take:

```text
h
```

Now:

```text
word = "h"
i = 1
```

---

## Iteration 2

Take:

```text
i
```

Now:

```text
word = "hi"
i = 2
```

---

# STOP CONDITION

Current:

```text
s[2] = ' '
```

Condition:

```cpp
s[i] != ' '
```

becomes FALSE.

Extraction stops.

---

# STEP 3 → Reverse

```text
"hi" -> "ih"
```

---

# STEP 4 → Add Into Answer

Check:

```cpp
if(word.length() > 0)
```

Current:

```text
word = "ih"
length = 2
```

TRUE.

---

# Check Space Condition

```cpp
if(ans.length() > 0)
```

Current:

```text
ans = ""
length = 0
```

FALSE.

No space added.

---

# Add Word

```cpp
ans += word;
```

Now:

```text
ans = "ih"
```

---

# FIRST WORD FINISHED

Current:

```text
i = 2
ans = "ih"
```

---

# SECOND OUTER LOOP ITERATION

Condition:

```text
2 < 5
```

TRUE.

This is how second word starts.

---

# STEP 1 → Skip Spaces

Current:

```text
s[2] = ' '
```

TRUE.

Execute:

```cpp
i++;
```

Now:

```text
i = 3
```

Current:

```text
s[3] = 'o'
```

Loop stops.

---

# STEP 2 → Extract Word

---

## Iteration 1

Take:

```text
o
```

Now:

```text
word = "o"
i = 4
```

---

## Iteration 2

Take:

```text
k
```

Now:

```text
word = "ok"
i = 5
```

---

# STOP CONDITION

Check:

```text
5 < 5
```

FALSE.

Extraction stops.

---

# STEP 3 → Reverse

```text
"ok" -> "ko"
```

---

# STEP 4 → Add Into Answer

Check:

```cpp
if(ans.length() > 0)
```

Current:

```text
ans = "ih"
length = 2
```

TRUE.

So:

```cpp
ans += ' ';
```

Now:

```text
ans = "ih "
```

Then:

```cpp
ans += word;
```

Now:

```text
ans = "ih ko"
```

---

# FINAL OUTER LOOP CHECK

Current:

```text
i = 5
n = 5
```

Condition:

```text
5 < 5
```

FALSE.

Loop ends.

---

# FINAL OUTPUT

```text
"ih ko"
```

---

# EDGE CASES

## 1. Leading Spaces

Input:

```text
"   hi ok"
```

Handled by:

```cpp
while(i < n && s[i] == ' ')
```

---

## 2. Multiple Spaces

Input:

```text
"hi     ok"
```

Extra spaces skipped automatically.

---

## 3. Trailing Spaces

Input:

```text
"hi ok     "
```

Trailing spaces ignored.

---

## 4. Single Word

Input:

```text
"hello"
```

Output:

```text
"olleh"
```

---

## 5. Only Spaces

Input:

```text
"     "
```

Output:

```text
""
```

Handled using:

```cpp
if(word.length() > 0)
```

---

# TIME COMPLEXITY

# O(N)

Why?

Every character is visited at most once.

- skipping spaces moves forward
- extraction moves forward

No unnecessary revisits happen.

---

# SPACE COMPLEXITY

# O(N)

Why?

We store:
- answer string
- temporary word string

Worst case:
answer stores the full reversed sentence.

---

# WHY I MIGHT FORGET / GET STUCK

## 1. Forgetting where extraction stops

This line controls stopping:

```cpp
while(i < n && s[i] != ' ')
```

It stops when:
- space appears
OR
- string ends

---

## 2. Forgetting why ans.length() > 0 exists

```cpp
if(ans.length() > 0)
```

Used to:
- add spaces only before later words
- avoid leading spaces

---

## 3. Forgetting why word.length() > 0 exists

```cpp
if(word.length() > 0)
```

Used to:
- avoid processing empty words
- handle trailing spaces safely

---

# INTERVIEW EXPLANATION

You can explain like this:

> “I traverse the string manually using an index.  
First I skip unnecessary spaces.  
Then I extract one complete word until a space appears.  
After extracting the word, I reverse it and append it to the final answer.  
Before appending a new word, I add exactly one space if the answer already contains words.  
This automatically handles leading, trailing, and multiple spaces while maintaining linear complexity.”
````
