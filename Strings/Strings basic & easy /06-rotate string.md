
# Note

## PROBLEM: Strings Rotations of Each Other

## PATTERN: String Matching / Concatenation Trick

---

# WHY THIS PATTERN

A rotated string never changes the relative order of characters.  
It only changes the starting point.

When we concatenate a string with itself:

```cpp
s1 + s1
```

all possible rotations automatically appear inside it.

Example:

```text
s1 = "abcd"

s1 + s1 = "abcdabcd"
```

Possible rotations present inside:

```text
abcd
bcda
cdab
dabc
```

So if `s2` exists inside `(s1+s1)`,
then `s2` is definitely a valid rotation of `s1`.

---

# CORE IDEA

1. Rotations must always have equal lengths
2. All rotations of `s1` exist inside `s1+s1`
3. So:
   - check lengths
   - create doubled string
   - search for `s2` inside it

---

# IMPORTANT OBSERVATION

The question already says:

```text
Both strings have equal lengths
```

BUT in interviews,
ALWAYS still check lengths manually.

Why?

Because:
- defensive coding
- avoids edge cases
- makes solution robust
- interviewers expect it

Never skip this check.

---

# DO WE NEED BRUTE FORCE?

Usually NO.

Reason:
- optimal solution is already intuitive
- interviewer expects direct observation
- brute force does not add much value

BUT understanding brute force helps remember WHY the optimal works.

So learning it once is useful.

---

# BRUTE FORCE IDEA

Generate every possible rotation manually.

After every rotation:
- compare with `s2`
- if equal → return true

---

# HOW TO GENERATE ROTATIONS

## LEFT ROTATION

Take first character:
- remove it
- append it at end

Example:

```text
abcd
```

Take:

```text
a
```

Remaining:

```text
bcd
```

Append:

```text
bcda
```

---

# BRUTE FORCE CODE

```cpp
class Solution {
  public:
  
    bool areRotations(string &s1, string &s2) {
        
        // Rotations must have same length
        if(s1.length() != s2.length())
            return false;
        
        string temp = s1;
        int n = s1.length();
        
        // Generate all rotations
        for(int i = 0; i < n; i++) {
            
            // Check current rotation
            if(temp == s2)
                return true;
            
            // Store first character
            char first = temp[0];
            
            // Remove first character
            temp.erase(0, 1);
            
            // Add character at end
            temp.push_back(first);
        }
        
        return false;
    }
};
```

---

# BRUTE FORCE DRY RUN

## INPUT

```text
s1 = "abcd"
s2 = "cdab"
```

---

## INITIAL

```text
temp = "abcd"
```

---

## ITERATION 1

Compare:

```text
abcd == cdab ?
```

NO

Rotate:

```text
bcda
```

---

## ITERATION 2

Compare:

```text
bcda == cdab ?
```

NO

Rotate:

```text
cdab
```

---

## ITERATION 3

Compare:

```text
cdab == cdab ?
```

YES

Return true.

---

# HOW THIS ROTATION WORKS

```cpp
temp.erase(0, 1);
temp.push_back(first);
```

## `erase(0,1)`

Means:

```text
Start from index 0
Remove 1 character
```

Example:

```text
abcd -> bcd
```

---

## `push_back(first)`

Adds character at end.

Example:

```text
bcd + a -> bcda
```

Together:

```text
abcd -> bcda
```

which is one LEFT rotation.

---

# BRUTE FORCE TIME COMPLEXITY

Outer loop:

```text
O(n)
```

Each comparison:

```text
O(n)
```

Each erase:

```text
O(n)
```

Overall:

```text
TC = O(n²)
```

---

# BRUTE FORCE SPACE COMPLEXITY

We create:

```cpp
string temp = s1;
```

So:

```text
SC = O(n)
```

---

# OPTIMAL APPROACH

## KEY OBSERVATION

```text
All rotations of s1 exist inside s1+s1
```

So we:
1. Check lengths
2. Create doubled string
3. Search for `s2`

---

# INTERVIEW FLOW (VERY IMPORTANT)

## STEP 1 — State Observation

Say:

> “If `s2` is a rotation of `s1`, then all rotations of `s1` will appear inside `s1+s1`.”

---

## STEP 2 — Give Example

```text
s1 = "abcd"

s1+s1 = "abcdabcd"
```

Rotations inside:

```text
abcd
bcda
cdab
dabc
```

Since `"cdab"` exists inside it,
it is a valid rotation.

---

## STEP 3 — Explain Algorithm

Say:

> “So the algorithm becomes:
>
> 1. Check if lengths are equal
> 2. Create doubled string = s1+s1
> 3. Check whether s2 exists inside doubled string”

---

## STEP 4 — Complexity

Say:

> “Concatenation takes O(n)
> Substring search takes O(n)
>
> Overall:
> TC = O(n)
> SC = O(n)”

---

# IMPORTANT CODE SNIPPETS

## LENGTH CHECK

```cpp
if(s1.length() != s2.length())
    return false;
```

---

## CREATE DOUBLED STRING

```cpp
string doubled = s1 + s1;
```

---

## SUBSTRING CHECK

```cpp
return doubled.find(s2) != string::npos;
```

---

# WHAT DOES THIS MEAN?

```cpp
doubled.find(s2)
```

tries to find `s2` inside `doubled`.

If substring exists:
- returns starting index

If substring does NOT exist:
- returns:

```cpp
string::npos
```

which means:

```text
"No Position" / "Not Found"
```

So:

```cpp
doubled.find(s2) != string::npos
```

means:

```text
"s2 exists inside doubled string"
```

---

# VERY IMPORTANT MISTAKE

## WRONG

```cpp
string doubled = s1 + s2;
```

Why wrong?

Because `s2` itself gets appended,
so searching for `s2` becomes meaningless.

---

## CORRECT

```cpp
string doubled = s1 + s1;
```

because it generates ALL rotations of `s1`.

---

# OPTIMAL CODE

```cpp
class Solution {
  public:
  
    bool areRotations(string &s1, string &s2) {
        
        // Rotations must have equal lengths
        if(s1.length() != s2.length())
            return false;
        
        // All rotations exist inside s1+s1
        string doubled = s1 + s1;
        
        // Check if s2 exists inside doubled
        return doubled.find(s2) != string::npos;
    }
};
```

---

# OPTIMAL DRY RUN

## INPUT

```text
s1 = "abcd"
s2 = "cdab"
```

---

## STEP 1 — Length Check

```text
4 == 4
```

Continue.

---

## STEP 2 — Concatenate

```text
doubled = "abcdabcd"
```

---

## STEP 3 — Search

```cpp
doubled.find("cdab")
```

Returns:

```text
2
```

because `"cdab"` starts at index 2.

---

## STEP 4

```cpp
2 != string::npos
```

TRUE

Return true.

---

# EDGE CASES

## 1. Different lengths

```text
abc
ab
```

Return false immediately.

---

## 2. Same strings

```text
aaaa
aaaa
```

Still valid rotation.

---

## 3. Single character

```text
a
a
```

Return true.

---

## 4. Repeated characters

```text
aab
aba
```

Still works correctly.

---

# TIME COMPLEXITY

Length check:

```text
O(1)
```

Concatenation:

```text
O(n)
```

Substring search:

```text
O(n)
```

Overall:

```text
TC = O(n)
```

---

# SPACE COMPLEXITY

We create:

```cpp
s1 + s1
```

So:

```text
SC = O(n)
```

---

# WHY I MIGHT FORGET THIS

I may overcomplicate rotations by:
- manually shifting characters
- simulating rotations
- using nested loops

But the REAL trick is:

```text
All rotations exist inside s1+s1
```

That one observation solves the entire problem elegantly.
````
