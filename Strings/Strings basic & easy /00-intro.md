
# STRINGS DSA — BASIC + INTRO NOTES

---

# 1. What is a String?

A string is a sequence of characters.

## Examples

```text
"hello"
"abc"
"12345"
```

## In C++

```cpp
string s = "hello";
```

## Internally

```text
h e l l o
0 1 2 3 4
```

Strings are basically arrays of characters.

---

# 2. Important Properties of Strings

# Size / Length

```cpp
s.length()
s.size()
```

## Example

```cpp
string s = "hello";

cout << s.length(); // 5
```

## Time Complexity

```text
O(1)
```

---

# Access Characters

```cpp
s[i]
```

## Example

```cpp
cout << s[0]; // h
```

## Time Complexity

```text
O(1)
```

---

# 3. Traversing a String

# Using Loop

```cpp
for(int i=0; i<s.length(); i++){
    cout << s[i];
}
```

---

# Using Range-Based Loop

```cpp
for(char ch : s){
    cout << ch;
}
```

---

# 4. Input in Strings

# Normal Input

```cpp
cin >> s;
```

## Problem

Stops at spaces.

## Example

Input:

```text
hello world
```

Only stores:

```text
hello
```

---

# Full Line Input

```cpp
getline(cin, s);
```

Stores complete sentence.

---

# 5. Important String Operations

# Concatenation

```cpp
string a = "hello";
string b = "world";

string c = a + b;
```

## Output

```text
helloworld
```

---

# Push Back

Adds character at end.

```cpp
s.push_back('a');
```

---

# Pop Back

Removes last character.

```cpp
s.pop_back();
```

---

# Substring

```cpp
s.substr(start, length)
```

## Example

```cpp
string s = "abcdef";

cout << s.substr(2,3);
```

## Output

```text
cde
```

---

# Reverse String

Using STL:

```cpp
reverse(s.begin(), s.end());
```

---

# Sort String

```cpp
sort(s.begin(), s.end());
```

Useful in:

- Anagram problems
- Frequency-based questions

---

# 6. ASCII Values (VERY IMPORTANT)

Characters internally store numbers.

## Examples

```text
'a' -> 97
'b' -> 98
'A' -> 65
'0' -> 48
```

---

# Convert char to int

```cpp
char ch = '7';

int num = ch - '0';
```

---

# Convert lowercase to uppercase

```cpp
char upper = ch - 32;
```

Better way:

```cpp
toupper(ch)
tolower(ch)
```

---

# 7. Character Checking Functions

Very important in parsing questions.

```cpp
isdigit(ch)
isalpha(ch)
islower(ch)
isupper(ch)
```

## Example

```cpp
if(isdigit(ch)){
    // number
}
```

---

# 8. Most Important String Interview Patterns

These patterns repeat everywhere.

---

# Pattern 1 — Frequency Counting

Used in:

- Anagrams
- Duplicate characters
- Hashing problems

## Example

```cpp
vector<int> freq(26,0);

for(char ch : s){
    freq[ch - 'a']++;
}
```

---

# Pattern 2 — Two Pointers

Most important string pattern.

Used in:

- Palindrome
- Reverse vowels
- Valid palindrome
- Compression

## Example

```cpp
int left = 0;
int right = s.length()-1;

while(left < right){

    if(s[left] != s[right]){
        return false;
    }

    left++;
    right--;
}
```

---

# Pattern 3 — Sliding Window

Used in:

- Longest substring
- Distinct characters
- Minimum window

## Core Idea

```text
Expand -> Shrink -> Maintain condition
```

---

# Pattern 4 — Hashing

Use:

```cpp
unordered_map<char,int>
```

Used in:

- Frequency
- First unique character
- Character replacement

---

# Pattern 5 — Build Answer String

Very common.

```cpp
string ans = "";

for(char ch : s){

    if(condition){
        ans += ch;
    }
}
```

---

# 9. Most Common String Questions

These are beginner MUST-DO questions.

---

# Easy

- Reverse String
- Palindrome Check
- Valid Palindrome
- Remove Spaces
- Toggle Case
- Count Vowels
- Largest Word
- Frequency of Characters
- Remove Duplicates

---

# Medium

- Anagrams
- Longest Common Prefix
- String Compression
- Longest Substring Without Repeating Characters
- Roman to Integer
- String to Integer (atoi)
- Count and Say

---

# Advanced

- KMP Algorithm
- Rabin Karp
- Z Algorithm
- Trie + Strings
- Rolling Hash

You usually learn these later.

---

# 10. Time Complexities You MUST Remember

| Operation | Time Complexity |
|---|---|
| Access Character | O(1) |
| Traverse String | O(N) |
| Reverse | O(N) |
| Sort | O(N log N) |
| Concatenation | O(N) |
| Substring | O(K) |

---

# 11. Important Interview Tips

# Always Ask Yourself

---

# 1. Is this frequency based?

Use:

```cpp
freq array
unordered_map
```

---

# 2. Is this palindrome-like?

Use:

```text
Two pointers
```

---

# 3. Is this longest/minimum substring?

Use:

```text
Sliding Window
```

---

# 4. Is order important?

If NOT:

```cpp
sort()
```

can simplify the problem.

---

# 12. Most Important STL Functions

```cpp
reverse()
sort()
substr()
push_back()
pop_back()
toupper()
tolower()
getline()
```

Master these first.

---

# 13. Biggest Mistakes Beginners Make

# Mistake 1

Using:

```cpp
s[i] = s[i] + 1;
```

without checking bounds.

---

# Mistake 2

Forgetting:

```text
'a' != 'A'
```

Case sensitivity matters.

---

# Mistake 3

Using:

```cpp
cin >> s
```

when sentence input exists.

---

# Mistake 4

Not understanding ASCII conversion.

---

# Mistake 5

Overusing nested loops.

Usually there is:

- Hashing
- Sliding window
- Two pointer optimization

---

# 14. GOLDEN RULE FOR STRINGS

Whenever you see a string question, think:

```text
1. Frequency?
2. Two pointers?
3. Sliding window?
4. Hash map?
5. Sorting?
```

Most interview questions come from these only.

---

# 15. Recommended Learning Order

# Phase 1 — Basics

- Traversal
- Reverse
- Palindrome
- Frequency count
- ASCII

---

# Phase 2 — Intermediate

- Anagrams
- Hashing
- Two pointers
- String building

---

# Phase 3 — Important Interview Patterns

- Sliding Window
- Longest substring
- Compression
- Parsing

---

# Phase 4 — Advanced

- KMP
- Rabin Karp
- Trie
- Rolling Hash

---

# 16. One-Line Summary

```text
Strings = Arrays + Hashing + Two Pointers + Sliding Window
```

Master these and most string questions become pattern recognition instead of brute force.
````
