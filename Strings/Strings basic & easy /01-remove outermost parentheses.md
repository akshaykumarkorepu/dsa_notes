
# NOTE

# PROBLEM:

Remove Outermost Parentheses

---

# PATTERN:

String Traversal + Counter / Balance Tracking

---

# WHY THIS PATTERN:

We need to process nested parentheses.

Whenever we see:

```text
(
```

depth increases.

Whenever we see:

```text
)
```

depth decreases.

The outermost brackets are exactly the brackets responsible for:

```text
0 -> 1
and
1 -> 0
```

So this becomes a balance/depth tracking problem.

---

# CORE IDEA

We maintain:

```cpp
int balance
```

which tells current nesting depth.

---

# For '('

If:

```cpp
balance > 0
```

then it is NOT outermost.

So include it.

Then increase balance.

---

# For ')'

First decrease balance.

Then check:

```cpp
balance > 0
```

If true:

```text
This ')' is not outermost
```

So include it.

---

# MOST IMPORTANT OBSERVATION

We SKIP:

---

# Opening outer bracket

When:

```text
balance == 0
```

before increment.

## Example

```text
0 -> 1
```

---

# Closing outer bracket

When:

```text
balance becomes 0
```

after decrement.

## Example

```text
1 -> 0
```

---

# WHY THIS APPROACH WORKS

Every primitive valid substring starts with:

```text
(
```

at depth 0

and ends with:

```text
)
```

returning depth back to 0.

We simply avoid adding those two brackets.

---

# EDGE CASES

# Case 1 — Smallest Primitive

## Input

```text
()
```

## Output

```text
""
```

Because both brackets are outermost.

---

# Case 2 — Multiple Primitives

## Input

```text
()()
```

Primitive decomposition:

```text
() + ()
```

## Output

```text
""
```

---

# Case 3 — Nested Parentheses

## Input

```text
((()))
```

## Output

```text
(())
```

Only outermost removed.

---

# WHY I MIGHT FORGET THIS

I may forget:

- When exactly to increment/decrement balance
- Why we check before increment for '('
- Why we check after decrement for ')'

---

# GOLDEN MEMORY TRICK

# For '('

```text
Check first
Then increment
```

---

# For ')'

```text
Decrement first
Then check
```

---

# INTERVIEW EXPLANATION FLOW

# Step 1 — Start with observation

Say:

```text
We need to remove only the outermost brackets of every primitive valid substring.
```

---

# Step 2 — Explain balance idea

```text
I can track current nesting depth using a balance counter.

'(' increases balance
')' decreases balance
```

---

# Step 3 — Explain outermost condition

```text
The outermost opening bracket happens when balance goes from 0 to 1.

The outermost closing bracket happens when balance goes from 1 to 0.
```

---

# Step 4 — Explain inclusion logic

```text
For '(':
If balance is already greater than 0, it means this bracket is inside another bracket, so we include it.

For ')':
We first decrease balance. If balance is still greater than 0, then it is not outermost, so we include it.
```

---

# OPTIMAL CODE

```cpp
class Solution {
  public:
  
    string removeOuter(string s) {
        
        string ans = "";
        int balance = 0;
        
        for(char ch : s){
            
            // Opening bracket
            if(ch == '('){
                
                // Include only if not outermost
                if(balance > 0){
                    ans += ch;
                }
                
                balance++;
            }
            
            // Closing bracket
            else{
                
                balance--;
                
                // Include only if not outermost
                if(balance > 0){
                    ans += ch;
                }
            }
        }
        
        return ans;
    }
};
```

---

# IMPORTANT CODE SNIPPETS

# Opening Bracket Logic

```cpp
if(balance > 0){
    ans += ch;
}

balance++;
```

---

# Closing Bracket Logic

```cpp
balance--;

if(balance > 0){
    ans += ch;
}
```

---

# DETAILED DRY RUN

# Input

```text
s = "(()())(())"
```

---

# Initial State

```text
balance = 0
ans = ""
```

---

# ITERATION 1

## Character

```text
(
```

## Current balance

```text
0
```

## Condition

```cpp
if(balance > 0)
```

FALSE.

Do NOT include.

## Why?

```text
This is outermost opening bracket.
```

Increase balance:

```text
balance = 1
```

Answer:

```text
""
```

---

# ITERATION 2

## Character

```text
(
```

balance = 1

Condition TRUE.

Include it.

```text
ans = "("
```

Increase balance:

```text
balance = 2
```

---

# ITERATION 3

## Character

```text
)
```

Decrease balance first:

```text
balance = 1
```

Condition TRUE.

Include it.

```text
ans = "()"
```

---

# ITERATION 4

## Character

```text
(
```

balance = 1

Condition TRUE.

Include.

```text
ans = "()("
```

Increase balance:

```text
balance = 2
```

---

# ITERATION 5

## Character

```text
)
```

Decrease balance:

```text
balance = 1
```

Condition TRUE.

Include.

```text
ans = "()()"
```

---

# ITERATION 6

## Character

```text
)
```

Decrease balance:

```text
balance = 0
```

Condition FALSE.

Do NOT include.

## Why?

```text
This is outermost closing bracket.
```

---

# First Primitive Completed

```text
(()())
```

became:

```text
()()
```

---

# Continue Traversal

Even though balance became 0,
we continue because another primitive may exist.

Next primitive:

```text
(())
```

---

# Final Answer

```text
()()()
```

---

# TIME COMPLEXITY

# O(N)

## Why?

We traverse the string once.

Each character is processed once.

---

# SPACE COMPLEXITY

# O(N)

Because answer string stores resulting characters.

---

# COMMON MISTAKES

# Mistake 1

Checking:

```cpp
if(balance >= 0)
```

Wrong.

Correct:

```cpp
if(balance > 0)
```

---

# Mistake 2

Forgetting order for closing bracket.

Wrong:

```cpp
if(balance > 0)
balance--;
```

Correct:

```cpp
balance--;
if(balance > 0)
```

---

# Mistake 3

Stopping traversal when balance becomes 0.

Wrong because:

```text
balance == 0
```

only means:

```text
Current primitive ended
```

NOT entire string.

---

# PATTERN RECOGNITION

This question teaches:

- Parentheses depth tracking
- Counter-based parsing
- String traversal
- Primitive substring recognition

---

# QUESTIONS WITH SIMILAR THINKING

- Valid Parentheses
- Maximum Nesting Depth
- Remove Invalid Parentheses
- Generate Parentheses
- Minimum Remove to Make Valid Parentheses

---

# FINAL MEMORY LINE

```text
Skip '(' when balance == 0
Skip ')' when balance becomes 0
```
````
