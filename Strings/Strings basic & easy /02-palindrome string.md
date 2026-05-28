# NOTE

# PROBLEM: Palindrome String

## PATTERN:
Two Pointers

---

# WHY THIS PATTERN:

We need to compare characters from both ends of the string.

A palindrome reads the same:
- left → right
- right → left

Instead of reversing the string or using extra space, we can directly compare:
- first character with last
- second with second last
- and so on

This is exactly what the **Two Pointer Pattern** does.

---

# CORE IDEA:

- Keep one pointer at the start (`i`)
- Keep another pointer at the end (`j`)
- Compare both characters

If:
- `s[i] != s[j]`
    → not a palindrome immediately

Else:
- move inward
    - `i++`
    - `j--`

If all characters match till pointers cross:
→ palindrome

---

# EDGE CASES:

- Single character → always palindrome
- Even length string
- Odd length string
- All same characters
- First mismatch immediately

Examples:

```cpp
"a"       -> true
"abba"    -> true
"abcba"   -> true
"abca"    -> false
```

---

# WHY I GOT STUCK / MIGHT FORGET:

- I may unnecessarily think about reversing the string
- I may use extra space
- I may forget that once mismatch occurs → directly return false
- I may overcomplicate a very simple two-pointer question

REMEMBER:

> “Palindrome problems usually scream TWO POINTERS.”

---

# BRUTE FORCE (Only Mention If Interviewer Asks)

## Idea:
Reverse the string and compare.

```cpp
string t = s;
reverse(t.begin(), t.end());

if(t == s) return true;
```

## Problem:
- Extra space used
- Not optimal

So we move to the optimal two-pointer approach.

---

# OPTIMAL APPROACH — TWO POINTERS

---

# STEP-BY-STEP INTUITION

Suppose:

```cpp
s = "abba"
```

We compare:

```cpp
a == a
b == b
```

Everything matches.

So it is a palindrome.

Now:

```cpp
s = "abca"
```

Compare:

```cpp
a == a
b != c
```

Mismatch found.

So not a palindrome.

---

# DRY RUN 1

## Input:

```cpp
s = "abba"
```

Initial:

```cpp
i = 0
j = 3
```

---

## Iteration 1

```cpp
s[i] = 'a'
s[j] = 'a'
```

Match.

Move pointers:

```cpp
i++
j--
```

Now:

```cpp
i = 1
j = 2
```

---

## Iteration 2

```cpp
s[i] = 'b'
s[j] = 'b'
```

Match.

Move:

```cpp
i = 2
j = 1
```

Now:

```cpp
i > j
```

Loop ends.

Return:

```cpp
true
```

---

# DRY RUN 2

## Input:

```cpp
s = "abc"
```

Initial:

```cpp
i = 0
j = 2
```

---

## Iteration 1

```cpp
s[i] = 'a'
s[j] = 'c'
```

Mismatch found.

Immediately:

```cpp
return false
```

---

# INTERVIEW EXPLANATION FLOW

If interviewer asks:

## “Explain your approach”

You say:

> “Since a palindrome reads the same from both directions, I used the two-pointer approach. One pointer starts from the beginning and another from the end. I compare both characters at every step. If they are different, the string cannot be a palindrome, so I return false immediately. Otherwise, I move both pointers inward until they cross.”

---

# IMPORTANT OBSERVATION

We only need to check till:

```cpp
i < j
```

Why?

Because after pointers cross:
- all required comparisons are already done

---

# IMPORTANT CODE SNIPPETS

## Pointer Initialization

```cpp
int i = 0;
int j = s.size() - 1;
```

---

## Main Logic

```cpp
while(i < j)
{
    if(s[i] != s[j])
        return false;

    i++;
    j--;
}
```

---

## Final Return

```cpp
return true;
```

---

# FULL OPTIMAL CODE (C++)

```cpp
class Solution {
  public:
  
    bool isPalindrome(string& s) {
        
        int i = 0;
        int j = s.size() - 1;
        
        while(i < j)
        {
            if(s[i] != s[j])
            {
                return false;
            }
            
            i++;
            j--;
        }
        
        return true;
    }
};
```

---

# TIME COMPLEXITY (TC)

## TC:

```cpp
O(n)
```

Why?

- In worst case, we compare all characters once
- Each pointer moves inward only once

Example:

```cpp
"racecar"
```

We check roughly `n/2` comparisons.

Still considered:

```cpp
O(n)
```

---

# SPACE COMPLEXITY (SC)

## SC:

```cpp
O(1)
```

Why?

We only use:
- two integer pointers

No extra array/string/hashmap used.

---

# HOW TO IDENTIFY THIS PATTERN IN FUTURE

Whenever you see:
- palindrome
- reverse symmetry
- compare from both ends

Think:

# “TWO POINTERS”

---

# MEMORY TRICK

Imagine:

```cpp
left ↔ right
```

Both pointers walk toward the center checking equality.

That is literally how palindrome checking works.

---

# FINAL TAKEAWAY

## Most Important Thing To Remember:

You DO NOT need:
- reversing
- extra string
- stack
- hashmap

Just:

```cpp
compare both ends
move inward
```

That is the entire problem.
