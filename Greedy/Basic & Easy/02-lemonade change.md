
# Bus Ticket Change (Greedy)

---

## PROBLEM:

You are given a queue of passengers where each bus ticket costs **5 coins**.

Each passenger pays using either:

- `5`
- `10`
- `20`

You must serve passengers **in the given order** while always giving the correct change.

Return **true** if every passenger can be served; otherwise return **false**.

---

## PATTERN:

**Simulation Greedy (Greedy Decision Making)**

This is **not** a sorting problem.

We simply process customers one by one and make the best local decision for giving change.

---

## WHY THIS PATTERN:

Notice that:

- Customers **cannot be reordered**.
- Every customer depends on the change collected from previous customers.
- Every decision affects future customers.
- We need to preserve our available change in the best possible way.

Since we only need to make the best decision at each customer without reconsidering previous decisions, this is a **Greedy Simulation** problem.

---

## CORE IDEA:

Maintain only the count of:

- `five` → Number of ₹5 notes available.
- `ten` → Number of ₹10 notes available.

We never need to store ₹20 notes because they are **never used to give change**.

For every customer:

- Update our available notes.
- Immediately decide whether giving change is possible.

---

## GREEDY OBSERVATION:

The only difficult customer is the one paying with **₹20**.

They need **₹15** as change.

There are two possible ways to give change:

**Option 1**

```
10 + 5
```

**Option 2**

```
5 + 5 + 5
```

The Greedy observation is:

> **Always prefer giving one ₹10 and one ₹5 whenever possible.**

---

## WHY GREEDY WORKS:

A ₹5 note is much more valuable than a ₹10 note.

Why?

- Every customer paying ₹10 requires one ₹5 as change.
- Every customer paying ₹20 also requires at least one ₹5.

A ₹10 note alone cannot help a customer paying ₹10.

Therefore, ₹5 notes are the scarce and most valuable resource.

Whenever possible, we should use:

```
10 + 5
```

instead of

```
5 + 5 + 5
```

This preserves more ₹5 notes for future customers.

---

## GREEDY CHOICE:

For every customer paying ₹20:

If possible:

```
Give:
10 + 5
```

Otherwise:

```
Give:
5 + 5 + 5
```

If neither is possible:

```
Return false
```

---

## WHY THIS CHOICE IS SAFE:

Suppose we currently have:

```
five = 3
ten = 1
```

A customer pays ₹20.

### Choice 1

```
Give:
10 + 5

Remaining

five = 2
ten = 0
```

### Choice 2

```
Give:
5 + 5 + 5

Remaining

five = 0
ten = 1
```

Which state is better?

Clearly the first one.

Reason:

Future customers paying ₹10 require a ₹5 note.

If we spend all three ₹5 notes now, we may fail to serve future customers.

Keeping extra ₹5 notes always provides more flexibility.

Hence this Greedy choice never hurts future decisions.

This satisfies the **Greedy Choice Property**.

---

## SORTING:

**Sorting is NOT required.**

Reason:

The queue order is fixed.

Changing the order changes the problem itself.

Passengers must be served exactly in the given order.

---

## INVARIANT:

After processing every customer:

- Every processed customer has received correct change.
- `five` stores the exact number of ₹5 notes currently available.
- `ten` stores the exact number of ₹10 notes currently available.
- The remaining notes are sufficient to serve all processed customers.

If at any point change cannot be given, the answer is immediately **false**.

---

## BRUTE FORCE:

A brute-force approach is **not necessary**.

### Why?

There are no meaningful decisions to explore except for customers paying ₹20, where there are only two possible ways of giving change.

Since the Greedy strategy can be formally proven to be optimal, exploring every possible combination is unnecessary.

Interviewers generally expect the direct Greedy solution.

---

## OPTIMAL APPROACH:

Maintain two counters:

```
five = number of ₹5 notes
ten = number of ₹10 notes
```

Process every customer.

### Case 1: Customer pays ₹5

No change is required.

```
five++
```

---

### Case 2: Customer pays ₹10

Need one ₹5 note as change.

If no ₹5 note is available:

```
return false
```

Otherwise:

```
five--
ten++
```

---

### Case 3: Customer pays ₹20

Need ₹15 change.

First try:

```
10 + 5
```

If possible:

```
ten--
five--
```

Otherwise, if three ₹5 notes are available:

```
five -= 3
```

Otherwise:

```
return false
```

If all customers are processed successfully:

```
return true
```

---

## ALGORITHM:

1. Initialize:

```
five = 0
ten = 0
```

2. Traverse every customer.

3. If payment is ₹5:

```
five++
```

4. If payment is ₹10:

- Check if `five > 0`.
- If not, return false.
- Otherwise:

```
five--
ten++
```

5. If payment is ₹20:

First check:

```
ten >= 1 && five >= 1
```

If true:

```
ten--
five--
```

Otherwise, check:

```
five >= 3
```

If true:

```
five -= 3
```

Else:

```
return false
```

6. If traversal completes successfully:

```
return true
```

---

## DRY RUN:

### Example 1

```
arr = [5,5,5,10,20]
```

Initially:

```
five = 0
ten = 0
```

### Customer 1

Pays ₹5

```
five = 1
ten = 0
```

---

### Customer 2

Pays ₹5

```
five = 2
ten = 0
```

---

### Customer 3

Pays ₹5

```
five = 3
ten = 0
```

---

### Customer 4

Pays ₹10

Need one ₹5.

```
five = 2
ten = 1
```

---

### Customer 5

Pays ₹20

Need ₹15.

Prefer:

```
10 + 5
```

```
five = 1
ten = 0
```

Everyone is served successfully.

```
Answer = true
```

---

### Example 2

```
arr = [5,5,10,10,20]
```

Initially:

```
five = 0
ten = 0
```

After first two customers:

```
five = 2
ten = 0
```

Third customer:

Pays ₹10

```
five = 1
ten = 1
```

Fourth customer:

Pays ₹10

```
five = 0
ten = 2
```

Fifth customer:

Pays ₹20

Need ₹15.

Cannot give:

```
10 + 5
```

because no ₹5 exists.

Cannot give:

```
5 + 5 + 5
```

because no ₹5 exists.

```
Answer = false
```

---

## COMMON MISTAKES:

### Mistake 1

Giving:

```
5 + 5 + 5
```

before checking for:

```
10 + 5
```

This wastes valuable ₹5 notes.

Always prefer:

```
10 + 5
```

---

### Mistake 2

Keeping count of ₹20 notes.

Not required.

₹20 notes are never used as change.

---

### Mistake 3

Sorting the array.

The queue order is fixed.

Sorting changes the problem.

---

### Mistake 4

Continuing after change becomes impossible.

Immediately return:

```
false
```

---

## INTERVIEW FLOW:

> "Since customers must be served in the given order, sorting is not allowed."

> "I only maintain the number of ₹5 and ₹10 notes because ₹20 notes are never used to give change."

> "For a ₹5 payment, I simply collect the note."

> "For a ₹10 payment, I must give one ₹5 as change."

> "For a ₹20 payment, I first try giving ₹10 + ₹5 because it preserves more ₹5 notes."

> "If that isn't possible, I try giving three ₹5 notes."

> "If neither option works, I immediately return false."

> "If every customer is served successfully, I return true."

---

## TIME COMPLEXITY:

### **O(n)**

### Reasoning:

- We traverse the array exactly once.
- Each customer is processed only once.
- Every iteration performs only a constant number of operations (increment, decrement, or comparisons).

Therefore,

```
Time Complexity = O(n)
```

where **n = number of customers**.

---

## SPACE COMPLEXITY:

### **O(1)**

### Reasoning:

We only maintain two integer variables:

- `five`
- `ten`

Their size does not depend on the number of customers.

No additional data structures are used.

Therefore,

```
Space Complexity = O(1)
```

---

## EDGE CASES:

### 1. First customer pays ₹10

```
[10]
```

No ₹5 note is available.

```
Answer = false
```

---

### 2. First customer pays ₹20

```
[20]
```

Cannot give ₹15 change.

```
Answer = false
```

---

### 3. Every customer pays ₹5

```
[5,5,5,5]
```

No change is ever required.

```
Answer = true
```

---

### 4. Only one ₹5 available for a ₹20 customer

```
[5,20]
```

Need ₹15.

Impossible.

```
Answer = false
```

---

### 5. Both options are available

```
five = 3
ten = 1
```

Always prefer:

```
10 + 5
```

instead of:

```
5 + 5 + 5
```

---

### 6. Single customer

```
[5]  → true
[10] → false
[20] → false
```

---

## PATTERN RECOGNITION:

Think of this Greedy pattern whenever:

- The processing order is fixed.
- Every decision affects future resources.
- You need to make the best local decision immediately.
- The state can be represented using a few variables.
- The goal is to preserve the most valuable resource for future operations.

Typical clues:

- "Serve in order."
- "Maintain available resources."
- "Give change immediately."
- "Allocate resources as you process."

This is a classic **Simulation Greedy** problem where the local choice of using:

```
10 + 5
```

instead of:

```
5 + 5 + 5
```

preserves the scarce resource (₹5 notes), leading to the globally optimal solution.

---

# C++ Code

```cpp
class Solution {
public:
    bool canServeAll(vector<int>& arr) {

        int five = 0;
        int ten = 0;

        for (int bill : arr) {

            // Customer pays with ₹5
            if (bill == 5) {
                five++;
            }

            // Customer pays with ₹10
            else if (bill == 10) {

                // Need one ₹5 as change
                if (five == 0)
                    return false;

                five--;
                ten++;
            }

            // Customer pays with ₹20
            else {

                // Prefer giving ₹10 + ₹5
                if (ten >= 1 && five >= 1) {
                    ten--;
                    five--;
                }

                // Otherwise give three ₹5 notes
                else if (five >= 3) {
                    five -= 3;
                }

                // Cannot give change
                else {
                    return false;
                }
            }
        }

        return true;
    }
};
```

---

# Intuition Behind Every Important Line

### `int five = 0, ten = 0;`

We only track the notes that can be used to give future change.

₹20 notes are never used as change, so they do not need to be stored.

---

### `for (int bill : arr)`

Process customers exactly in queue order because the order cannot be changed.

---

### `if (bill == 5)`

No change is required.

Simply collect one ₹5 note.

---

### `if (five == 0) return false;`

A customer paying ₹10 needs one ₹5 as change.

If no ₹5 is available, serving everyone becomes impossible.

---

### `five--; ten++;`

Give away one ₹5 note and collect one ₹10 note.

---

### `if (ten >= 1 && five >= 1)`

For a customer paying ₹20, first try giving:

```
10 + 5
```

This preserves more ₹5 notes for future customers.

---

### `else if (five >= 3)`

If no ₹10 note is available, the only remaining way to give ₹15 is:

```
5 + 5 + 5
```

---

### `return false;`

Neither way of making ₹15 exists.

Therefore, serving all customers is impossible.

---

# Tricky Condition Explained

## Why do we check `10 + 5` before `5 + 5 + 5`?

Because ₹5 notes are the most valuable resource.

They are required for:

- Every ₹10 customer.
- Every ₹20 customer.

Using one ₹10 note whenever possible preserves more ₹5 notes, giving us greater flexibility for future customers.

This is exactly why the Greedy strategy is optimal.

---

# Easy-to-Remember Interview Summary

- Queue order is fixed → **No sorting**.
- Track only the number of ₹5 and ₹10 notes.
- ₹5 payment → collect a ₹5.
- ₹10 payment → give one ₹5.
- ₹20 payment → **prefer `10 + 5`; otherwise use `5 + 5 + 5`.**
- If change cannot be given at any point, return **false**.
- This Greedy choice is optimal because it preserves the scarce resource—₹5 notes—for future customers.
````
