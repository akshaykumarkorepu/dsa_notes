

## PROBLEM:

Given a string containing repeated characters, rearrange the characters so that **no two adjacent characters are the same**.

Return any valid rearrangement.
If no valid rearrangement exists, return an empty string `""`.

---

## PATTERN:

**Greedy + Max Heap (Priority Queue) + Previous Element Holding**

---

## WHY THIS PATTERN:

This problem asks us to repeatedly choose the **best possible character** to place next.

What is "best"?

The character having the **highest remaining frequency**.

But there is one restriction:

> We cannot place the same character again immediately.

A Max Heap is perfect because it always gives us the character with the maximum remaining frequency.

The only challenge is preventing immediate reuse.

So we temporarily keep the previously used character outside the heap until one different character has been placed.

This creates the classic Heap pattern:

> **"Always pick the most frequent available element while temporarily blocking the previously used one."**

This exact pattern appears in many interview problems.

---

## CORE IDEA:

Imagine the most frequent character is `'a'`.

Example:

```
aaaa bbb cc
```

If we simply place all `'a'` first,

```
aaaabbbcc
```

it immediately violates the condition.

Instead we always:

1. Pick the character with highest frequency.
2. Place it.
3. Reduce its frequency.
4. Don't allow it to be used immediately again.
5. Bring it back after placing another character.

So at every step we are choosing the most frequent **available** character.

---

## BRUTE FORCE:

A true brute force solution would generate **all permutations** of the string and check whether adjacent characters are different.

For a string of length `n`:

- Total permutations = `n!`
- Checking each permutation = `O(n)`

Overall:

```
Time : O(n! × n)
Space: O(n)
```

Obviously impossible even for `n = 20`.

---

### Interview Transition

"I first thought about generating permutations, but that is factorial.

Since at every step I only need the most frequent available character, this naturally suggests using a Max Heap where I always greedily pick the highest frequency character."

---

# OPTIMAL APPROACH:

## Data Structure Used

### Max Heap

Store:

```
(frequency, character)
```

Example:

```
(4,'a')
(3,'b')
(2,'c')
```

The largest frequency always comes on top.

---

## Why Max Heap?

Because every step we need

> "the character with maximum remaining frequency."

Heap gives this in

```
O(log n)
```

instead of scanning every time.

---

## Why not Min Heap?

A Min Heap gives the least frequent character first.

That is opposite of what we need.

We always want to consume the most dangerous character (highest frequency) as early as possible.

Hence,

**Max Heap.**

---

## What is stored in Heap?

```
pair<int,char>

(first = remaining frequency)

(second = character)
```

Example:

```
(5,'a')
(3,'b')
(2,'c')
```

---

## Why are elements pushed?

Whenever a character still has remaining frequency after using it.

Example:

Current:

```
(4,'a')
```

Use one `'a'`

Remaining:

```
(3,'a')
```

We cannot discard it because three copies are still unused.

So it must eventually go back into the heap.

---

## Why are elements popped?

Because the heap top is always the character with maximum remaining frequency.

That is exactly the character we should place next.

---

## Why keep the previous character outside?

Suppose heap is

```
(4,'a')
(2,'b')
```

If we pop `'a'`

Remaining:

```
(3,'a')
```

If we immediately push it back,

heap becomes

```
(3,'a')
(2,'b')
```

Top is still `'a'`

We'll produce

```
aa
```

which is invalid.

Therefore,

after using a character,

we temporarily keep it outside.

Only after placing another character do we push it back.

---

## Heap Invariant

The heap always contains

> Every character that is currently available to use.

The previously used character is intentionally excluded until the next iteration.

This guarantees:

```
Adjacent characters are never the same.
```

---

# ALGORITHM:

### Step 1

Count frequency of every character.

Example

```
aaabbc

a -> 3
b -> 2
c -> 1
```

---

### Step 2

Insert every

```
(frequency, character)
```

into Max Heap.

---

### Step 3

Maintain

```
prev = {0,'#'}
```

This stores the character used in the previous step.

Initially no previous character exists.

---

### Step 4

Repeat until heap becomes empty.

Take top.

```
current = pq.top()
pq.pop()
```

---

Append character.

```
ans += current.second;
```

---

Decrease frequency.

```
current.first--;
```

---

Now previous character becomes available again.

If it still has frequency,

```
if(prev.first>0)
    pq.push(prev);
```

---

Store current as previous.

```
prev = current;
```

---

Repeat.

---

### Step 5

Finally,

if

```
ans.size()!=s.size()
```

then not every character was placed.

Return

```
""
```

Otherwise return answer.

---

# DRY RUN:

Input

```
aaabbc
```

Frequency

```
a =3
b =2
c =1
```

Heap

```
(3,a)
(2,b)
(1,c)
```

prev

```
(0,#)
```

---

### Iteration 1

Take

```
(3,a)
```

Answer

```
a
```

Remaining

```
(2,a)
```

Previous empty.

Store

```
prev=(2,a)
```

Heap

```
(2,b)
(1,c)
```

---

### Iteration 2

Take

```
(2,b)
```

Answer

```
ab
```

Remaining

```
(1,b)
```

Push previous

```
(2,a)
```

Store

```
prev=(1,b)
```

Heap

```
(2,a)
(1,c)
```

---

### Iteration 3

Take

```
(2,a)
```

Answer

```
aba
```

Remaining

```
(1,a)
```

Push

```
(1,b)
```

Store

```
prev=(1,a)
```

Heap

```
(1,b)
(1,c)
```

---

### Iteration 4

Take

```
(1,c)
```

Answer

```
abac
```

Push

```
(1,a)
```

Heap

```
(1,b)
(1,a)
```

---

### Iteration 5

Take

```
(1,b)
```

Answer

```
abacb
```

---

### Iteration 6

Take

```
(1,a)
```

Answer

```
abacba
```

Finished.

---

# IMPORTANT CODE SNIPPETS:

### Frequency Count

```cpp
unordered_map<char,int> freq;

for(char ch : s)
    freq[ch]++;
```

---

### Max Heap

```cpp
priority_queue<pair<int,char>> pq;
```

Default priority queue is already a Max Heap.

Equivalent to

```cpp
priority_queue<
pair<int,char>,
vector<pair<int,char>>,
less<pair<int,char>>
> pq;
```

---

### Previous Character Trick

```cpp
pair<int,char> prev = {0,'#'};
```

Most important trick of the problem.

---

### Push Previous Back

```cpp
if(prev.first>0)
    pq.push(prev);
```

Never push immediately after using.

Push only after another character is placed.

---

### Impossible Case

```cpp
if(ans.size()!=s.size())
    return "";
```

Some characters could not be placed.

---

# COMMON MISTAKES:

### Mistake 1

Immediately pushing current back.

Wrong:

```cpp
current.first--;

pq.push(current);
```

Produces

```
aa
```

---

### Mistake 2

Forgetting to decrement frequency.

```cpp
current.first--;
```

Without this,

loop never ends.

---

### Mistake 3

Pushing characters whose frequency became zero.

Correct

```cpp
if(prev.first>0)
```

---

### Mistake 4

Not checking impossible cases.

Always verify

```cpp
ans.size()==s.size()
```

---

# WHY I MIGHT FORGET THIS:

Because the "previous element" trick is not obvious.

Remember this sentence:

> "The character I just used is temporarily blocked for exactly one iteration."

That single idea solves the entire problem.

---

# INTERVIEW FLOW:

**Problem intuition**

"I need to rearrange the string so adjacent characters differ. The character with the highest frequency is the hardest to place, so I should always place it whenever possible."

**Brute force**

"A brute force permutation approach is factorial and infeasible."

**Observation**

"At every step, I only need the most frequent character that isn't the one I just used."

**Choice of Data Structure**

"So I'll use a Max Heap storing `(frequency, character)`."

**Preventing duplicates**

"After using a character, I won't push it back immediately. I'll keep it outside the heap for one iteration, ensuring the next character is different."

**Completion**

"If I can't build a string of the original length, then no valid arrangement exists."

---

# TIME COMPLEXITY:

### Frequency Counting

```
O(n)
```

---

### Heap Construction

At most 26 lowercase letters.

```
O(26 log 26)
```

which is effectively constant.

---

### Main Loop

Runs exactly `n` times.

Each iteration performs:

- one pop
- at most one push

Each heap operation costs

```
O(log k)
```

where

```
k = number of distinct characters
```

Since

```
k ≤ 26
```

```
log 26 = constant
```

Overall

```
O(n)
```

For a general alphabet with `k` distinct symbols, the complexity is:

```
O(n log k)
```

---

# SPACE COMPLEXITY:

Frequency map:

```
O(k)
```

Heap:

```
O(k)
```

Answer string:

```
O(n)
```

Auxiliary space (excluding output):

```
O(k)
```

For lowercase English letters:

```
O(1)
```

---

# EDGE CASES:

### Only one character

```
a
```

Answer

```
a
```

---

### All unique

```
abcd
```

Already valid.

---

### One character dominates

```
aaaaabc
```

Impossible.

---

### Equal frequencies

```
aabb
```

Many valid answers.

---

### All same

```
aaaa
```

Return

```
""
```

---

# PATTERN RECOGNITION:

Use this Heap pattern whenever you see:

- Rearrange elements with adjacency restrictions.
- Rearrange so neighboring elements are different.
- Reorganize strings.
- Schedule tasks with cooldown.
- Always choose the highest-frequency available element.
- Temporarily block the previously used element.

Typical problems:

- Rearrange String
- Reorganize String (LeetCode 767)
- Task Scheduler
- Distant Barcodes

The recurring pattern is:

> **Greedy + Max Heap + Temporarily hold the previously used element until it becomes valid to use again.**

---

# Clean C++ Code

```cpp
class Solution {
public:
    string rearrangeString(string &s) {

        unordered_map<char, int> freq;

        // Count frequency of each character
        for (char ch : s) {
            freq[ch]++;
        }

        // Max Heap -> (frequency, character)
        priority_queue<pair<int, char>,
                       vector<pair<int, char>>,
                       less<pair<int, char>>> pq;

        for (auto &it : freq) {
            pq.push({it.second, it.first});
        }

        string ans = "";

        // Stores the previously used character
        pair<int, char> prev = {0, '#'};

        while (!pq.empty()) {

            auto current = pq.top();
            pq.pop();

            // Place the current character
            ans += current.second;

            // One occurrence has been used
            current.first--;

            // Previous character becomes available again
            if (prev.first > 0) {
                pq.push(prev);
            }

            // Hold current character for the next iteration
            prev = current;
        }

        // Not all characters could be placed
        if (ans.size() != s.size())
            return "";

        return ans;
    }
};
```

---

# Intuition Behind Every Important Line

### Count frequencies

```cpp
freq[ch]++;
```

We need to know which character is most frequent so that we can prioritize placing it.

---

### Max Heap

```cpp
priority_queue<pair<int,char>> pq;
```

Always gives the character with the highest remaining frequency.

---

### Previous Variable

```cpp
pair<int,char> prev = {0,'#'};
```

Temporarily blocks the character used in the previous step to prevent adjacent duplicates.

---

### Pop Top

```cpp
auto current = pq.top();
pq.pop();
```

Choose the most frequent available character.

---

### Append Character

```cpp
ans += current.second;
```

Place it into the answer.

---

### Decrease Frequency

```cpp
current.first--;
```

One occurrence has now been consumed.

---

### Push Previous Back

```cpp
if(prev.first > 0)
    pq.push(prev);
```

After placing a different character, the previous one becomes valid to use again.

---

### Save Current

```cpp
prev = current;
```

Current character is now blocked for the next iteration.

---

### Final Check

```cpp
if(ans.size() != s.size())
```

If some characters were never placed, then no valid arrangement exists.

---

# Easy-to-Remember Summary

- Count frequencies.
- Put `(frequency, character)` into a **Max Heap**.
- Repeatedly pick the most frequent available character.
- After using a character, **hold it outside the heap for one iteration**.
- Reinsert the previous character only after placing a different one.
- If the constructed string is shorter than the original, return `""`.

**One-line memory trick:**

> **"Pick the most frequent character, block it for one turn, then make it available again."**
````
