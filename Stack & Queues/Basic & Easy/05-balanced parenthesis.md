

## PROBLEM:

Given a string containing only:

```text
(
)
{
}
[
]
```

Determine whether the expression is balanced.

A balanced expression satisfies:

1. Every opening bracket has a corresponding closing bracket.
2. Brackets close in the correct order.
3. No closing bracket appears before its matching opening bracket.

### Examples

```text
[{()}]      → true
[()()]{}    → true
([]         → false
([{]})      → false
```

---

## PATTERN:

**Stack Pattern (Matching / Validation Stack)**

This is one of the most common Stack problems.

### Keywords that hint toward this pattern:

- Matching brackets
- Balanced parentheses
- Nested structures
- Valid expressions
- Most recent opening element must be processed first

---

## WHY THIS PATTERN:

Brackets follow **LIFO (Last In First Out)** behavior.

Example:

```text
[{()}]
```

Opening order:

```text
[
{
(
```

Closing order:

```text
)
}
]
```

Notice:

```text
Last Opened → First Closed
```

That is exactly the behavior of a Stack.

---

## CORE IDEA:

Store all **unmatched opening brackets** inside the stack.

### Opening Bracket

```text
(
{
[
```

Push it.

Why?

Because it is waiting for a future matching bracket.

---

### Closing Bracket

```text
)
}
]
```

Check:

```text
Does it match the latest unmatched opening bracket?
```

If yes:

```text
Pop the opening bracket
```

because it has now been matched.

If no:

```text
Return false
```

---

### What is stored in the stack?

```text
Unmatched opening brackets
```

---

### Why elements are pushed?

```text
Opening brackets are waiting for a future match.
```

---

### Why elements are popped?

```text
A matching closing bracket has been found.
```

---

### Invariant Maintained

```text
Stack always contains unmatched opening brackets only.
```

---

### Why Stack is necessary?

We always need access to:

```text
Most recently opened bracket
```

Stack provides this in:

```text
O(1)
```

using:

```cpp
st.top()
```

---

## BRUTE FORCE:

Not necessary.

A brute force solution would repeatedly search backward for matching brackets.

Example:

```text
([{()}])
```

For every closing bracket, we may scan left to find its matching opening bracket.

This can easily become:

```text
O(N²)
```

and is unnecessarily complicated.

Since the Stack solution is direct and optimal, interviewers usually expect it immediately.

---

## OPTIMAL APPROACH:

Use a Stack.

### Rules

1. If opening bracket:
   - Push.

2. If closing bracket:
   - Stack must not be empty.
   - Top must contain matching opening bracket.
   - Otherwise return false.

3. Pop matched opening bracket.

4. After traversal:
   - Stack must be empty.

---

## ALGORITHM:

1. Create an empty stack.
2. Traverse the string.
3. If current character is:
   - `(` or `{` or `[`
   - Push into stack.
4. Otherwise:
   - If stack is empty → return false.
   - Check matching bracket.
   - If mismatch → return false.
   - Pop matched opening bracket.
5. After loop:
   - Return `st.empty()`.

---

## DRY RUN:

### Input

```text
[{()}]
```

### Initial

```text
Stack = Empty
```

---

### Character '['

Push.

```text
[
```

---

### Character '{'

Push.

```text
[
{
```

---

### Character '('

Push.

```text
[
{
(
```

---

### Character ')'

Top:

```text
(
```

Match.

Pop.

```text
[
{
```

---

### Character '}'

Top:

```text
{
```

Match.

Pop.

```text
[
```

---

### Character ']'

Top:

```text
[
```

Match.

Pop.

```text
Empty
```

---

### End of String

```cpp
return st.empty();
```

returns:

```text
true
```

---

### Invalid Dry Run

Input:

```text
([{]})
```

Stack:

```text
(
[
{
```

Current:

```text
]
```

Top:

```text
{
```

Expected:

```text
[
```

Mismatch.

Return:

```text
false
```

immediately.

---

## IMPORTANT CODE SNIPPETS:

### Opening Bracket Check

```cpp
if(ch == '(' || ch == '{' || ch == '[')
{
    st.push(ch);
}
```

---

### Empty Stack Check

```cpp
if(st.empty())
    return false;
```

Why?

Because a closing bracket appeared before an opening bracket.

Example:

```text
)(
```

---

### Matching Check

```cpp
if(ch == ')' && st.top() != '(')
    return false;

if(ch == '}' && st.top() != '{')
    return false;

if(ch == ']' && st.top() != '[')
    return false;
```

---

### Pop Matched Bracket

```cpp
st.pop();
```

---

### Final Check

```cpp
return st.empty();
```

Equivalent to:

```cpp
if(st.empty())
    return true;
else
    return false;
```

---

## COMMON MISTAKES:

### Mistake 1

Writing:

```cpp
if(ch == 'C')
```

instead of:

```cpp
if(ch == '(')
```

Very common typo.

---

### Mistake 2

Accessing:

```cpp
st.top()
```

before checking:

```cpp
st.empty()
```

Can cause runtime errors.

Wrong:

```cpp
if(st.top() != '(')
```

Correct:

```cpp
if(st.empty())
    return false;
```

before using `top()`.

---

### Mistake 3

Forgetting:

```cpp
st.pop();
```

after a successful match.

Then matched brackets remain inside the stack.

---

### Mistake 4

Returning true immediately after loop.

Wrong:

```cpp
return true;
```

Input:

```text
(((
```

would incorrectly return true.

Need:

```cpp
return st.empty();
```

---

## WHY I MIGHT FORGET THIS:

Most people focus on:

```text
Matching brackets
```

instead of:

```text
Unmatched opening brackets
```

The moment you realize:

```text
Stack = Unmatched Opening Brackets
```

the entire solution becomes obvious.

---

## INTERVIEW FLOW:

"Since brackets close in reverse order of opening, this is a Stack problem."

"Whenever I see an opening bracket, I push it because it is waiting for a future match."

"When I encounter a closing bracket, I verify whether it matches the latest unmatched opening bracket stored at the top of the stack."

"If it matches, I pop it because the pair is now complete."

"If the stack is empty or the brackets do not match, the expression is invalid."

"At the end, the stack must be empty because all opening brackets should have been matched."

---

## TIME COMPLEXITY:

### O(N)

### Reasoning

Each character is processed exactly once.

For every bracket:

- One push at most
- One pop at most

Operations:

```cpp
push()
pop()
top()
```

are all:

```text
O(1)
```

Therefore:

```text
O(N)
```

---

## SPACE COMPLEXITY:

### O(N)

### Reasoning

Worst case:

```text
((((((((
```

All brackets are stored in the stack.

Stack size:

```text
N
```

Therefore:

```text
O(N)
```

---

## EDGE CASES:

### Case 1

```text
)(
```

Output:

```text
false
```

Closing bracket appears first.

---

### Case 2

```text
(((
```

Output:

```text
false
```

Unmatched opening brackets remain.

---

### Case 3

```text
)))
```

Output:

```text
false
```

No opening brackets available.

---

### Case 4

```text
()
```

Output:

```text
true
```

Perfect match.

---

### Case 5

```text
([{}])
```

Output:

```text
true
```

Nested matching.

---

### Case 6

```text
([)]
```

Output:

```text
false
```

Correct brackets but wrong order.

---

## PATTERN RECOGNITION:

Think Stack immediately when you see:

### Keywords

- Balanced Parentheses
- Valid Parentheses
- Matching Symbols
- Nested Structures
- Expression Validation
- XML/HTML Tag Matching
- Undo Operations

---

### Ask Yourself

```text
Do I need the most recently inserted item first?
```

If yes:

```text
Stack
```

---

## CLEAN C++ CODE

```cpp
class Solution {
public:
    bool isBalanced(string& s) {

        stack<char> st;

        for(char ch : s) {

            if(ch == '(' || ch == '{' || ch == '[') {
                st.push(ch);
            }
            else {

                if(st.empty())
                    return false;

                if(ch == ')' && st.top() != '(')
                    return false;

                if(ch == '}' && st.top() != '{')
                    return false;

                if(ch == ']' && st.top() != '[')
                    return false;

                st.pop();
            }
        }

        return st.empty();
    }
};
```

---

## INTUITION BEHIND EVERY IMPORTANT LINE

### Create Stack

```cpp
stack<char> st;
```

Store unmatched opening brackets.

---

### Detect Opening Brackets

```cpp
if(ch == '(' || ch == '{' || ch == '[')
```

Opening brackets are waiting for future matches.

---

### Push

```cpp
st.push(ch);
```

Remember this opening bracket.

---

### Empty Check

```cpp
if(st.empty())
    return false;
```

Closing bracket arrived with nothing to match.

---

### Latest Unmatched Opening Bracket

```cpp
st.top()
```

Get the most recent unmatched opening bracket.

---

### Remove Matched Opening Bracket

```cpp
st.pop();
```

Bracket pair is complete.

---

### Final Validation

```cpp
return st.empty();
```

If stack is empty, every opening bracket found a matching closing bracket.

---

## EASY-TO-REMEMBER SUMMARY

```text
Stack = Unmatched Opening Brackets

Opening Bracket  -> Push
Matching Closing -> Pop
Mismatch         -> False
Empty Stack      -> False

End of String:
    Stack Empty     -> True
    Stack Not Empty -> False
```

### One-Line Memory Trick

> Push openings, verify closings, pop matches, and the stack must be empty at the end.
````
