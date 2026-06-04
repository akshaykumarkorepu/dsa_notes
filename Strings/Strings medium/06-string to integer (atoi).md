

# PROBLEM:
Implement Atoi

# PATTERN:
String Parsing + Simulation

# WHY THIS PATTERN:
We are manually simulating how a string gets converted into an integer while handling:
- spaces
- sign
- invalid characters
- overflow

This is a classic parsing problem because we process the string character by character and build the answer manually.

---

# CORE IDEA:

We process the string in 4 phases:

1. Skip leading spaces
2. Detect sign (`+` or `-`)
3. Read digits continuously
4. Build number safely while checking overflow

The MOST IMPORTANT logic is:

```cpp
ans = ans * 10 + digit;
```

This appends the new digit to the current number.

Example:

```txt
12 -> 123

12 * 10 + 3
```

---

# IMPORTANT OBSERVATION

atoi only reads the INITIAL VALID INTEGER PORTION.

The moment a non-digit character appears:
STOP parsing completely.

Example:

```txt
"123abc45"
```

Answer:
```txt
123
```

NOT:
```txt
12345
```

---

# WHY I GOT STUCK / MIGHT FORGET:

- Forgetting to stop parsing after first invalid character
- Forgetting overflow handling BEFORE multiplication
- Confusing:
```cpp
return ans * 10 + digit;
```
with:
```cpp
ans = ans * 10 + digit;
```
- Forgetting why we use:
```cpp
(INT_MAX - digit)/10
```

---

# BRUTE FORCE

Not really necessary here because optimal parsing solution is already straightforward.

But interviewer may expect basic progression.

A naive idea would be:
- remove spaces
- store digits separately
- then convert

But this becomes messy for:
- overflow
- signs
- invalid characters

So direct simulation is cleaner and optimal.

---

# OPTIMAL APPROACH

---

# STEP 1 — Skip Leading Spaces

Use pointer `i`.

```cpp
while(i < n && s[i] == ' ') {
    i++;
}
```

Example:

```txt
"    -42"
```

Pointer moves to:
```txt
'-'
```

---

# STEP 2 — Detect Sign

Default:
```cpp
sign = 1
```

If:
```txt
'-'
```
then:
```cpp
sign = -1
```

Code:

```cpp
if(i < n && (s[i] == '+' || s[i] == '-')) {

    if(s[i] == '-') {
        sign = -1;
    }

    i++;
}
```

IMPORTANT:
We increment `i` because sign is NOT part of actual number.

---

# STEP 3 — Read Digits

Loop while current character is digit.

```cpp
while(i < n && isdigit(s[i]))
```

---

# STEP 4 — Convert Character to Digit

```cpp
digit = s[i] - '0';
```

Example:

```txt
'7' - '0'
= 7
```

ASCII conversion:
```txt
'7' = 55
'0' = 48
55 - 48 = 7
```

---

# STEP 5 — Overflow Check

MOST IMPORTANT PART OF THE PROBLEM.

Before:

```cpp
ans = ans * 10 + digit;
```

we check:

```cpp
if(ans > (INT_MAX - digit)/10)
```

---

# WHY THIS FORMULA WORKS

We want:

```txt
ans * 10 + digit <= INT_MAX
```

Rearranging:

```txt
ans <= (INT_MAX - digit)/10
```

If current `ans` exceeds this:
overflow WILL happen.

So we stop BEFORE multiplication.

---

# WHY WE CHECK BEFORE MULTIPLICATION

WRONG:

```cpp
if(ans * 10 + digit > INT_MAX)
```

because:
```txt
ans * 10
```

itself may overflow BEFORE comparison happens.

So we avoid multiplication completely.

---

# STEP 6 — Build Number

```cpp
ans = ans * 10 + digit;
```

Example:

```txt
ans = 56
digit = 7

56 * 10 + 7
= 567
```

---

# STEP 7 — Return Final Answer

```cpp
return ans * sign;
```

If:
```txt
sign = -1
```

then:
```txt
123 -> -123
```

---

# COMPLETE CODE

```cpp
class Solution {
public:

    int myAtoi(string &s) {

        int n = s.size();
        int i = 0;

        // STEP 1: Skip leading spaces
        while(i < n && s[i] == ' ') {
            i++;
        }

        // STEP 2: Check sign
        int sign = 1;

        if(i < n && (s[i] == '+' || s[i] == '-')) {

            if(s[i] == '-') {
                sign = -1;
            }

            i++;
        }

        // STEP 3: Build number
        long long ans = 0;

        while(i < n && isdigit(s[i])) {

            int digit = s[i] - '0';

            // STEP 4: Overflow check
            if(ans > (INT_MAX - digit)/10) {

                if(sign == 1)
                    return INT_MAX;
                else
                    return INT_MIN;
            }

            ans = ans * 10 + digit;

            i++;
        }

        // STEP 5: Return final answer
        return ans * sign;
    }
};
```

---

# IMPORTANT CODE SNIPPETS

---

# Skip Spaces

```cpp
while(i < n && s[i] == ' ')
```

---

# Detect Sign

```cpp
if(s[i] == '+' || s[i] == '-')
```

---

# Convert Character to Integer

```cpp
digit = s[i] - '0';
```

---

# Build Number

```cpp
ans = ans * 10 + digit;
```

---

# Overflow Check

```cpp
if(ans > (INT_MAX - digit)/10)
```

---

# CLEAR DRY RUN 1

# Input:

```txt
"   -56abc"
```

---

# STEP 1 — Skip Spaces

Pointer moves from:

```txt
' ' -> ' ' -> ' '
```

Now reaches:
```txt
'-'
```

---

# STEP 2 — Detect Sign

```txt
sign = -1
```

Move pointer to:
```txt
'5'
```

---

# STEP 3 — Read Digits

---

# Read '5'

```txt
digit = 5

ans = 0*10 + 5
    = 5
```

---

# Read '6'

```txt
digit = 6

ans = 5*10 + 6
    = 56
```

---

# Next Character

```txt
'a'
```

Not a digit.

Loop stops immediately.

IMPORTANT:
We NEVER continue to:
```txt
'b'
or
'c'
```

because atoi stops at first invalid character.

---

# Final Return

```txt
56 * (-1)
= -56
```

Answer:
```txt
-56
```

---

# CLEAR DRY RUN 2 — OVERFLOW CASE

# Input:

```txt
"999999999999"
```

---

# Eventually:

```txt
ans = 999999999
digit = 9
```

Overflow check:

```txt
999999999 > (2147483647 - 9)/10
```

becomes:

```txt
999999999 > 214748363
```

TRUE.

This means:

```txt
999999999*10 + 9
```

would exceed:
```txt
2147483647
```

So overflow will happen.

Return:

```txt
INT_MAX
```

which is:

```txt
2147483647
```

---

# NEGATIVE OVERFLOW

Input:

```txt
"-999999999999"
```

Overflow occurs again.

But now:

```txt
sign = -1
```

So return:

```txt
INT_MIN
```

which is:

```txt
-2147483648
```

---

# INTERVIEW EXPLANATION FLOW

If interviewer asks this question, explain in THIS order:

---

# 1. Problem Understanding

Say:

> “We need to simulate atoi manually while handling spaces, signs, invalid characters, and overflow.”

---

# 2. Explain Parsing Steps

Say:

> “I process the string from left to right using a pointer.”

Then explain:

1. Skip spaces
2. Detect sign
3. Read digits
4. Handle overflow

---

# 3. Explain Number Building

MOST IMPORTANT INTERVIEW PART.

Say:

> “To append a digit, I multiply current number by 10 and add new digit.”

Example:

```txt
12 -> 123

12*10 + 3
```

---

# 4. Explain Overflow

Say:

> “Before appending a digit, I check whether the next operation would exceed INT_MAX.”

Then explain:

```cpp
if(ans > (INT_MAX - digit)/10)
```

IMPORTANT:
Mention derivation:

```txt
ans * 10 + digit <= INT_MAX
```

This creates VERY strong interviewer impression.

---

# 5. Explain Stopping Condition

Say:

> “The moment a non-digit character appears, parsing stops completely.”

Example:

```txt
"123abc45"
```

returns:
```txt
123
```

NOT:
```txt
12345
```

---

# TIME COMPLEXITY

```txt
O(n)
```

Why?

We traverse the string only once.

Each character is processed at most once.

---

# SPACE COMPLEXITY

```txt
O(1)
```

Why?

Only variables are used:
- i
- sign
- ans
- digit

No extra data structures used.

---

# FINAL INTUITION

This problem is basically:

> “Manually create an integer from a string safely.”

Core operations:
- parse characters
- build number digit by digit
- stop at invalid character
- prevent overflow BEFORE it happens
````
