

## PROBLEM: Reverse Words (Words Separated By Dots)

## PATTERN:
String Traversal + Word Extraction + Reverse

---

# WHY THIS PATTERN

This problem requires:

- parsing a string
- extracting valid words
- handling delimiters carefully
- reversing word order
- rebuilding the final string

This is a classic:

- String Parsing problem
- Delimiter-Based Extraction problem

The delimiter here is:

```txt
'.'
```

---

# CORE IDEA

Traverse the string character-by-character.

Build the current word using a temporary string.

Whenever:

```txt
'.'
```

appears:

- current word ends
- push it into vector
- reset temp

After extracting all words:

- reverse the vector
- rebuild final answer using single dots

---

# MOST IMPORTANT CONCEPT

```txt
Keep adding characters into temp
until delimiter appears.
```

Delimiter acts like:

```txt
WORD FINISHED SIGNAL
```

---

# VERY IMPORTANT DIFFERENCE

We are reversing:

```txt
order of words
```

NOT:

```txt
characters inside words
```

### WRONG

```txt
skeeg.rof
```

### CORRECT

```txt
for.geeks
```

---

# EDGE CASES

### Leading dots

```txt
"..geeks"
```

### Trailing dots

```txt
"geeks.."
```

### Multiple dots

```txt
"geeks....for"
```

### Single word

```txt
"..home....."
```

### Multiple consecutive delimiters

---

# WHY I GOT STUCK / MIGHT FORGET

- Forgetting to push the LAST word
- Forgetting to ignore empty strings
- Accidentally adding trailing dot
- Confusing reversing words vs reversing characters

---

# MOST COMMON BUG

Forgetting:

```cpp
if(temp != "") {
    words.push_back(temp);
}
```

after loop ends.

---

# BRUTE FORCE

Brute force progression is NOT very necessary here because:

- the optimal approach is already straightforward
- O(n) traversal solution is immediately visible
- no major optimization leap exists

So directly writing the optimal solution is perfectly fine in interviews.

---

# INTERVIEW FLOW

## STEP 1 — Clarify The Problem

Start by saying:

> “We need to reverse the order of words separated by dots.
>
> We also need to ignore extra dots and ensure only single dots exist between words in the final answer.”

This shows:

- clarity
- communication
- understanding

---

## STEP 2 — Mention Edge Cases Early

Say:

> “I should also handle:
>
> - leading dots
> - trailing dots
> - multiple consecutive dots
> - single-word strings”

Example:

```txt
"..geeks....for..."
```

This gives interviewer confidence immediately.

---

## STEP 3 — Explain Main Idea Before Coding

Say:

> “I’ll traverse the string character-by-character.
>
> I’ll build the current word using a temporary string.
>
> Whenever I encounter a dot, I’ll treat it as the end of a word.
>
> If temp is non-empty, I’ll push it into a vector.
>
> After extracting all valid words, I’ll reverse the vector and rebuild the final answer using single dots.”

This is the PERFECT interview explanation.

---

# OPTIMAL SOLUTION

## FULL CODE

```cpp
class Solution {
  public:

    string reverseWords(string s) {

        // Vector to store all extracted words
        vector<string> words;

        // Temporary string to build one word
        string temp = "";



        // Traverse every character
        for(char ch : s) {

            // If current character is NOT dot
            if(ch != '.') {

                // Add character into temp
                temp += ch;
            }

            // If current character IS dot
            else {

                // Store only valid words
                if(temp != "") {

                    // Push word into vector
                    words.push_back(temp);

                    // Reset temp
                    temp = "";
                }
            }
        }



        // VERY IMPORTANT:
        // Push last word if still present
        if(temp != "") {
            words.push_back(temp);
        }



        // Reverse all words
        reverse(words.begin(), words.end());



        // Build final answer
        string ans = "";

        for(int i = 0; i < words.size(); i++) {

            // Add current word
            ans += words[i];

            // Add dot only if not last word
            if(i != words.size() - 1) {
                ans += ".";
            }
        }

        return ans;
    }
};
```

---

# PROPER FLOW OF THE SOLUTION

---

## STEP 1 — Create Vector To Store Words

### CODE

```cpp
vector<string> words;
```

### WHY?

We need to store all extracted words separately.

Eventually:

```txt
["geeks", "for", "geeks"]
```

---

## STEP 2 — Create temp String

### CODE

```cpp
string temp = "";
```

### WHY?

We read characters one-by-one.

So temp helps us build one word character-by-character.

Think of temp as:

```txt
current word builder
```

---

## STEP 3 — Traverse Entire String

### CODE

```cpp
for(char ch : s)
```

### WHY?

The loop visits every character individually.

Example:

```txt
'.'
'.'
'g'
'e'
'e'
'k'
's'
'.'
'f'
'o'
'r'
```

IMPORTANT:

The loop NEVER sees complete words.

It only sees:

```txt
single characters
```

---

## STEP 4 — Check If Character Is NOT Dot

### CODE

```cpp
if(ch != '.')
```

### WHY?

If current character is not dot,
then it belongs to current word.

---

## STEP 5 — Build Current Word

### CODE

```cpp
temp += ch;
```

### DRY RUN

Initially:

```txt
temp = ""
```

Read:

```txt
'g'
```

Now:

```txt
temp = "g"
```

Read:

```txt
'e'
```

Now:

```txt
temp = "ge"
```

Eventually:

```txt
temp = "geeks"
```

---

## STEP 6 — Dot Means Word Finished

Now:

```txt
ch = '.'
```

So:

```cpp
else
```

runs.

Meaning:

```txt
Current word has ended
```

---

## STEP 7 — Ignore Empty Words

### CODE

```cpp
if(temp != "")
```

### WHY IMPORTANT?

Input may contain:

```txt
"..geeks....for..."
```

Without this condition:

```cpp
words.push_back(temp);
```

would insert:

```txt
["", "", "geeks"]
```

WRONG.

We only insert actual words.

---

## STEP 8 — Store Completed Word

### CODE

```cpp
words.push_back(temp);
```

### EXAMPLE

Before:

```txt
temp  = "geeks"
words = []
```

After:

```txt
words = ["geeks"]
```

---

## STEP 9 — Reset temp

### CODE

```cpp
temp = "";
```

### WHY?

Previous word is complete.

Now we prepare to build next word.

---

## STEP 10 — VERY IMPORTANT LAST WORD HANDLING

### CODE

```cpp
if(temp != "") {
    words.push_back(temp);
}
```

### WHY IMPORTANT?

Inside loop,
words are inserted ONLY when dot appears.

But last word may not have dot after it.

Example:

```txt
"geeks.for"
```

Loop ends with:

```txt
temp = "for"
```

But:

```txt
words = ["geeks"]
```

So we manually push last word.

THIS IS ONE OF THE MOST COMMON MISTAKES.

---

## STEP 11 — Reverse Word Order

### CODE

```cpp
reverse(words.begin(), words.end());
```

### EXAMPLE

Before:

```txt
["i", "like", "coding"]
```

After:

```txt
["coding", "like", "i"]
```

---

## STEP 12 — Create Final Answer String

### CODE

```cpp
string ans = "";
```

### WHY?

This stores the final output.

---

## STEP 13 — Traverse Reversed Words

### CODE

```cpp
for(int i = 0; i < words.size(); i++)
```

### WHY?

We visit every reversed word one-by-one.

---

## STEP 14 — Add Current Word Into Answer

### CODE

```cpp
ans += words[i];
```

### EXAMPLE

Initially:

```txt
ans = ""
```

After adding:

```txt
"coding"
```

Now:

```txt
ans = "coding"
```

---

## STEP 15 — Add Dot Carefully

### CODE

```cpp
if(i != words.size() - 1)
```

### WHY IMPORTANT?

We want:

```txt
coding.like.i
```

NOT:

```txt
coding.like.i.
```

No trailing dot should exist.

---

## STEP 16 — Add Dot

### CODE

```cpp
ans += ".";
```

---

# FULL DRY RUN

## INPUT

```txt
"..geeks..for.geeks."
```

---

## INITIAL STATE

```txt
words = []
temp  = ""
```

---

## READ '.'

### CODE

```cpp
if(ch != '.')
```

FALSE.

temp is empty.

Nothing inserted.

---

## READ 'g'

### CODE

```cpp
temp += ch;
```

Now:

```txt
temp = "g"
```

---

## READ 'e'

Now:

```txt
temp = "ge"
```

---

## READ 'e'

Now:

```txt
temp = "gee"
```

---

## READ 'k'

Now:

```txt
temp = "geek"
```

---

## READ 's'

Now:

```txt
temp = "geeks"
```

---

## READ '.'

### CODE

```cpp
words.push_back(temp);
```

Now:

```txt
words = ["geeks"]
```

Reset:

```txt
temp = ""
```

---

## BUILD SECOND WORD

Eventually:

```txt
temp = "for"
```

STORE:

```txt
words = ["geeks", "for"]
```

---

## BUILD THIRD WORD

Eventually:

```txt
temp = "geeks"
```

---

## LOOP ENDS

VERY IMPORTANT:

Last word still inside temp.

### CODE

```cpp
if(temp != "") {
    words.push_back(temp);
}
```

Now:

```txt
words = ["geeks", "for", "geeks"]
```

---

## REVERSE VECTOR

### CODE

```cpp
reverse(words.begin(), words.end());
```

Result:

```txt
["geeks", "for", "geeks"]
```

---

## BUILD FINAL ANSWER

Eventually:

```txt
ans = "geeks.for.geeks"
```

---

# TIME COMPLEXITY

## 1. Traversing String

### CODE

```cpp
for(char ch : s)
```

Visits every character once.

### TC

```txt
O(n)
```

---

## 2. Reversing Vector

### CODE

```cpp
reverse(words.begin(), words.end());
```

Touches all words once.

### TC

```txt
O(n)
```

---

## 3. Building Final String

### CODE

```cpp
for(int i = 0; i < words.size(); i++)
```

Again traverses all characters once.

### TC

```txt
O(n)
```

---

# FINAL TIME COMPLEXITY

```txt
O(n)
```

Because:

```txt
O(n) + O(n) + O(n)
= O(n)
```

---

# SPACE COMPLEXITY

## VECTOR

### CODE

```cpp
vector<string> words;
```

Stores all words.

### SC

```txt
O(n)
```

---

## TEMP STRING

### CODE

```cpp
string temp = "";
```

Stores one word.

Still within:

```txt
O(n)
```

---

## FINAL ANSWER STRING

### CODE

```cpp
string ans = "";
```

Stores output string.

---

# FINAL SPACE COMPLEXITY

```txt
O(n)
```

---

# INTERVIEW EXPLANATION

You can explain like this:

> “I traverse the string character-by-character and build words using a temporary string.
>
> Whenever I encounter a dot, I treat it as the end of a word and store the completed word into a vector while ignoring extra dots.
>
> After extracting all valid words, I reverse the vector and rebuild the final string using single dots between words.”
````
