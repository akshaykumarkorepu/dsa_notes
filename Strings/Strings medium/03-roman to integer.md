

## PROBLEM: Roman to Integer

## PATTERN: String Traversal + Greedy

---

# WHY THIS PATTERN

We traverse the string from left to right.

At every character, we decide:

- ADD current value
OR
- SUBTRACT current value

using only the next character.

This is greedy because we make the best decision immediately using local information.

---

# CORE IDEA

Roman numerals follow two rules:

## 1. Normally symbols are added

Example:

```text
VIII = 5 + 1 + 1 + 1 = 8
```

---

## 2. If a smaller symbol appears before a larger symbol, it becomes subtractive

Example:

```text
IV = 5 - 1 = 4
IX = 10 - 1 = 9
```

So the entire problem reduces to:

```text
IF current value < next value
→ SUBTRACT

ELSE
→ ADD
```

---

# IMPORTANT ROMAN VALUES

| Symbol | Value |
|---|---|
| I | 1 |
| V | 5 |
| X | 10 |
| L | 50 |
| C | 100 |
| D | 500 |
| M | 1000 |

---

# ALL SUBTRACTIVE PAIRS

These are the ONLY subtractive combinations:

```text
IV = 4
IX = 9
XL = 40
XC = 90
CD = 400
CM = 900
```

Everything else is additive.

---

# BRUTE FORCE APPROACH

## WHY BRUTE FORCE IS IMPORTANT HERE

This question has a VERY natural brute force approach.

Many people initially think:

> "Let me manually handle all special Roman pairs."

Interviewers may expect this progression.

So understanding brute force helps explain how we reach the optimal solution.

---

# BRUTE FORCE IDEA

Traverse the string.

At every index:

Check if current + next forms a special subtractive pair:

- IV
- IX
- XL
- XC
- CD
- CM

If yes:

→ add special value

Else:

→ add normal Roman value

---

# BRUTE FORCE EXAMPLE

## INPUT

```text
MCMIV
```

---

## STEP 1

Current:

```text
M
```

Next:

```text
C
```

Does this form any special pair?

```text
NO
```

So:

```text
Add value of M
```

```text
sum = 1000
```

Move normally.

---

## STEP 2

Current:

```text
C
```

Next:

```text
M
```

This forms:

```text
CM
```

CM is a special pair.

```text
CM = 900
```

So:

```text
sum = 1000 + 900
sum = 1900
```

Now BOTH characters are already processed.

So skip next character.

---

## STEP 3

Current:

```text
I
```

Next:

```text
V
```

This forms:

```text
IV
```

```text
IV = 4
```

So:

```text
sum = 1900 + 4
sum = 1904
```

Done.

---

# BRUTE FORCE CODE

```cpp
class Solution {
public:

    int romanToDecimal(string s) {

        int sum = 0;

        unordered_map<char, int> mp;

        mp['I'] = 1;
        mp['V'] = 5;
        mp['X'] = 10;
        mp['L'] = 50;
        mp['C'] = 100;
        mp['D'] = 500;
        mp['M'] = 1000;

        for(int i = 0; i < s.length(); i++) {

            // IV
            if(i + 1 < s.length() &&
               s[i] == 'I' && s[i + 1] == 'V') {

                sum += 4;
                i++;
            }

            // IX
            else if(i + 1 < s.length() &&
                    s[i] == 'I' && s[i + 1] == 'X') {

                sum += 9;
                i++;
            }

            // XL
            else if(i + 1 < s.length() &&
                    s[i] == 'X' && s[i + 1] == 'L') {

                sum += 40;
                i++;
            }

            // XC
            else if(i + 1 < s.length() &&
                    s[i] == 'X' && s[i + 1] == 'C') {

                sum += 90;
                i++;
            }

            // CD
            else if(i + 1 < s.length() &&
                    s[i] == 'C' && s[i + 1] == 'D') {

                sum += 400;
                i++;
            }

            // CM
            else if(i + 1 < s.length() &&
                    s[i] == 'C' && s[i + 1] == 'M') {

                sum += 900;
                i++;
            }

            // Normal addition
            else {

                sum += mp[s[i]];
            }
        }

        return sum;
    }
};
```

---

# PROBLEMS WITH BRUTE FORCE

## 1. Too many conditions

Need to manually remember:

```text
IV
IX
XL
XC
CD
CM
```

---

## 2. Messy code

Lots of if-else blocks.

---

## 3. Harder to maintain

Easy to make mistakes.

Easy to miss one pair.

---

# MOST IMPORTANT OBSERVATION

Look carefully at ALL subtractive pairs:

| Pair | Values |
|---|---|
| IV | 1 < 5 |
| IX | 1 < 10 |
| XL | 10 < 50 |
| XC | 10 < 100 |
| CD | 100 < 500 |
| CM | 100 < 1000 |

Notice:

```text
In EVERY subtractive case:

current < next
```

ALWAYS.

---

# OPTIMIZATION IDEA

Instead of manually checking:

```text
IV
IX
XL
XC
CD
CM
```

we can use ONE universal rule:

```text
IF current < next
→ subtract current

ELSE
→ add current
```

This gives the optimal solution.

---

# OPTIMAL APPROACH

1. Store Roman values in hashmap
2. Traverse string once
3. Compare current value with next value
4. If current < next:
   subtract current
5. Else:
   add current

---

# MOST IMPORTANT IDEA TO REMEMBER FOREVER

Think:

```text
"If next is bigger,
current becomes negative."
```

Example:

```text
IX
```

I becomes:

```text
-1
```

X becomes:

```text
+10
```

Final answer:

```text
9
```

This mental model makes the question very easy.

---

# IMPORTANT BOUNDARY CHECK

Before accessing:

```cpp
s[i + 1]
```

we MUST check:

```cpp
i + 1 < s.length()
```

## WHY?

At the last character:

```cpp
s[i + 1]
```

would go out of bounds.

This prevents runtime errors.

---

# IMPORTANT CODE SNIPPETS

## 1. Roman Mapping

```cpp
unordered_map<char, int> mp;

mp['I'] = 1;
mp['V'] = 5;
mp['X'] = 10;
mp['L'] = 50;
mp['C'] = 100;
mp['D'] = 500;
mp['M'] = 1000;
```

---

## 2. Core Optimal Logic

```cpp
if(i + 1 < s.length() && mp[s[i]] < mp[s[i + 1]]) {

    sum -= mp[s[i]];
}
else {

    sum += mp[s[i]];
}
```

THIS is the entire optimized solution.

---

# OPTIMAL CODE

```cpp
class Solution {
public:

    int romanToDecimal(string &s) {

        unordered_map<char, int> mp;

        mp['I'] = 1;
        mp['V'] = 5;
        mp['X'] = 10;
        mp['L'] = 50;
        mp['C'] = 100;
        mp['D'] = 500;
        mp['M'] = 1000;

        int sum = 0;

        for(int i = 0; i < s.length(); i++) {

            // Subtractive case
            if(i + 1 < s.length() &&
               mp[s[i]] < mp[s[i + 1]]) {

                sum -= mp[s[i]];
            }

            // Normal addition
            else {

                sum += mp[s[i]];
            }
        }

        return sum;
    }
};
```

---

# DETAILED DRY RUN

## INPUT

```text
MCMIV
```

---

# INITIAL

```text
sum = 0
```

---

# i = 0

Current:

```text
M = 1000
```

Next:

```text
C = 100
```

```text
1000 < 100 ?
```

NO

ADD

```text
sum = 1000
```

---

# i = 1

Current:

```text
C = 100
```

Next:

```text
M = 1000
```

```text
100 < 1000
```

YES

SUBTRACT

```text
sum = 1000 - 100
sum = 900
```

---

# i = 2

Current:

```text
M = 1000
```

Next:

```text
I = 1
```

```text
1000 < 1 ?
```

NO

ADD

```text
sum = 900 + 1000
sum = 1900
```

---

# i = 3

Current:

```text
I = 1
```

Next:

```text
V = 5
```

```text
1 < 5
```

YES

SUBTRACT

```text
sum = 1900 - 1
sum = 1899
```

---

# i = 4

Current:

```text
V = 5
```

No next character exists.

ADD

```text
sum = 1899 + 5
sum = 1904
```

---

# FINAL ANSWER

```text
1904
```

---

# EDGE CASES

## 1. Single Character

Input:

```text
V
```

Output:

```text
5
```

---

## 2. Fully Additive

```text
VIII
```

```text
5 + 1 + 1 + 1
```

Answer:

```text
8
```

---

## 3. Subtractive Pattern

```text
IX
```

```text
-1 + 10
```

Answer:

```text
9
```

---

## 4. Last Character

Always check boundary before accessing next character.

---

# WHY I GOT STUCK / MIGHT FORGET

## 1. Forgetting subtractive logic

Initially feels like all symbols should be added.

---

## 2. Forgetting boundary check

Very common mistake:

```cpp
s[i + 1]
```

without:

```cpp
i + 1 < s.length()
```

---

## 3. Overcomplicating with many conditions

You do NOT need separate handling for:

```text
IV
IX
XL
CM
```

One comparison handles everything:

```cpp
current < next
```

---

# INTERVIEW EXPLANATION

You can explain like this:

> Initially, I thought of manually handling all subtractive Roman pairs like IV, IX, XL, etc.
>
> But then I noticed that every subtractive pair follows one common property:
>
> current value < next value
>
> So instead of hardcoding all cases, I generalized it into one condition.
>
> Then I traverse the string once and:
>
> - subtract when current < next
> - otherwise add
>
> This gives an O(n) solution.

---

# TIME COMPLEXITY

## O(n)

### WHY?

We traverse the string once.

Each character is processed once.

Hashmap lookup is O(1).

Total:

```text
O(n)
```

---

# SPACE COMPLEXITY

## O(1)

### WHY?

Hashmap stores only 7 Roman symbols.

Size never grows with input.

So space remains constant.
````
