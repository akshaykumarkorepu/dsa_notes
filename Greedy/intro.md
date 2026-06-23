
# Greedy Algorithms - Complete Interview Notes

---

# 1. What is a Greedy Algorithm?

A **Greedy Algorithm** is an algorithmic paradigm where we make the **best possible choice at the current step** without considering future consequences.

- Every decision is **final**.
- We **never backtrack** or reconsider previous choices.
- The goal is that these **locally optimal choices** together produce the **globally optimal solution**.

> **Definition:**  
> A Greedy Algorithm builds the solution incrementally by repeatedly choosing the locally optimal option.

---

## Key Characteristics

- Makes the best decision at the current step.
- Decisions are irreversible.
- Does not explore all possibilities.
- Usually much faster than Dynamic Programming.
- Does **not** work for every optimization problem.

---

# 2. Greedy vs Dynamic Programming

| Greedy | Dynamic Programming |
|---------|---------------------|
| Makes one best choice at each step | Considers multiple choices |
| Never revisits previous decisions | Explores different possibilities |
| No backtracking | Stores subproblem answers |
| Usually O(n) or O(n log n) | Often O(n²), O(n³), etc. |
| Needs proof of correctness | Guarantees optimal answer when applicable |
| May fail | Gives optimal solution for valid DP problems |

### Interview Rule

Ask yourself:

> **"Can I make one decision now and never regret it later?"**

If yes, think **Greedy**.

---

# 3. When Does Greedy Work?

Greedy works only if the problem satisfies **both** of these properties.

---

## 1. Greedy Choice Property

A **locally optimal choice** should lead to the **globally optimal solution**.

In simple words,

> The best decision right now should never hurt the final answer.

### Example

### Activity Selection

Choose the meeting that **finishes earliest**.

Why?

Because it leaves the maximum time for future meetings.

---

## 2. Optimal Substructure

After making the greedy choice,

the remaining problem should still be solved optimally using the same approach.

Example:

After selecting one activity,

the remaining activities again form an Activity Selection problem.

---

> **If both properties exist, Greedy is usually applicable.**

---

# 4. Biggest Misconception

Many beginners think:

> "Choosing the largest or smallest element always makes a Greedy solution."

❌ Wrong.

Greedy works only when the greedy choice is **provably safe**.

---

### Example where Greedy Fails

Coins = {1,3,4}

Target = 6

Greedy:

4 + 1 + 1 = 3 coins

Optimal:

3 + 3 = 2 coins

Greedy fails.

---

# 5. General Approach to Solve Greedy Problems

Most Greedy problems follow this workflow.

---

## Step 1

Understand what needs to be optimized.

Examples:

- Maximum profit
- Minimum cost
- Maximum meetings
- Minimum intervals removed
- Maximum tasks completed

---

## Step 2

Find the Greedy Choice

Ask:

> **What is the safest decision I can make right now?**

Examples:

- Earliest finish time
- Highest profit
- Smallest cost
- Maximum ratio
- Minimum weight
- Largest value

---

## Step 3

Sort (if necessary)

Nearly **70% of Greedy problems begin with sorting.**

Common sorting parameters:

- Finish time
- Start time
- Deadline
- Profit
- Ratio
- Cost
- Frequency
- Weight

---

## Step 4

Traverse once

While traversing,

- Accept good choices
- Reject bad choices
- Never backtrack

---

## Step 5

Return answer

---

# 6. How to Identify a Greedy Problem

Whenever you read a problem,

ask these questions.

---

### Question 1

Can I make one decision now and never revisit it?

If YES,

Greedy is a candidate.

---

### Question 2

Does making the best current choice naturally help future choices?

Examples:

- Earliest finish leaves maximum remaining time.
- Smallest cookie preserves larger cookies.
- Highest value/weight gives maximum profit.

---

### Question 3

Will changing this decision later ever improve the answer?

If NO,

Greedy may work.

---

### Question 4

Can the solution be built one step at a time?

If YES,

Think Greedy.

---

### Question 5

Does sorting simplify the problem significantly?

If YES,

It is a strong Greedy hint.

---

# 7. Common Keywords That Indicate Greedy

Optimization words:

- Maximum
- Minimum
- Largest
- Smallest
- Highest
- Lowest
- Earliest
- Latest
- Fewest

Problem words:

- Schedule
- Assign
- Arrange
- Merge
- Select
- Connect
- Split
- Deadline
- Profit
- Cost
- Resource Allocation

> **These are hints, not guarantees.**

---

# 8. The Most Important Part - Finding the Greedy Choice

Every Greedy problem revolves around one question:

> **"What is the safest decision I can make right now that will never reduce the optimal answer?"**

Examples:

| Problem | Greedy Choice |
|----------|---------------|
| Activity Selection | Earliest finishing activity |
| Assign Cookies | Smallest cookie satisfying smallest child |
| Fractional Knapsack | Highest value/weight ratio |
| Job Sequencing | Highest profit first |
| Huffman Coding | Merge two smallest frequencies |
| Jump Game | Farthest reachable index |
| Minimum Platforms | Earliest arrival/departure event |

---

# 9. How to Derive the Greedy Choice

Instead of asking

> Which choice is the best now?

Ask

> **Which choice preserves the maximum opportunities for future decisions?**

---

### Example 1

Activity Selection

Wrong ideas:

- Earliest start
- Shortest duration

Correct:

- Earliest finish

Reason:

Leaves maximum remaining time.

---

### Example 2

Assign Cookies

Wrong:

Give largest cookie first.

Correct:

Give the smallest sufficient cookie to the least greedy child.

Reason:

Save larger cookies for greedier children.

---

> **A good Greedy choice preserves future flexibility.**

---

# 10. Why Sorting is Used So Often

Sorting creates order.

Without order,

safe Greedy decisions become difficult.

Examples:

| Problem | Sort By |
|----------|----------|
| Activity Selection | Finish Time |
| Merge Intervals | Start Time |
| Assign Cookies | Greed & Cookie Size |
| Fractional Knapsack | Value / Weight |
| Job Sequencing | Profit or Deadline |
| Minimum Platforms | Arrival & Departure |

Sorting usually exposes the property that makes the greedy choice obvious.

---

# 11. Greedy Correctness (Very Important)

Interviewers may ask:

> **"Why does your greedy choice work?"**

Your answer should always explain **why this local choice never hurts the global optimum**.

---

## Exchange Argument (Most Common)

Assume another optimal solution makes a different first choice.

Show that replacing it with your greedy choice **does not make the answer worse**.

Therefore,

your greedy choice is also part of an optimal solution.

---

### Example

Activity Selection

Choosing the earliest finishing activity always leaves **at least as much remaining time** as any other activity.

Hence,

it can never reduce the number of activities selected.

---

### Example

Fractional Knapsack

Choosing the highest value/weight item gives maximum value for every unit of capacity.

Replacing it with any lower ratio decreases the total value.

---

> You usually don't need a formal mathematical proof in interviews—an intuitive explanation or exchange argument is enough.

---

# 12. Common Greedy Patterns

---

## Pattern 1 - Sort + Select

Idea:

Sort.

Select the best candidate.

Skip conflicting ones.

Examples:

- Activity Selection
- N Meetings in One Room
- Attend Maximum Events
- Non-overlapping Intervals
- Minimum Arrows

---

## Pattern 2 - Sort + Merge

Idea:

Sort intervals.

Merge overlapping intervals.

Examples:

- Merge Intervals
- Insert Interval
- Meeting Rooms

---

## Pattern 3 - Sort + Assign

Idea:

Assign resources optimally.

Examples:

- Assign Cookies
- Boats to Save People
- Task Assignment

---

## Pattern 4 - Heap Based Greedy

Idea:

Always keep the best candidate available.

Examples:

- IPO
- Furthest Building
- Meeting Rooms II
- Maximum Performance
- Kth Largest Element

---

## Pattern 5 - Ratio Based Greedy

Idea:

Sort according to efficiency.

Examples:

- Fractional Knapsack

Sort by

```
value / weight
```

---

## Pattern 6 - Scheduling Greedy

Idea:

Sort by

- Deadline
- Finish Time
- Profit

Examples:

- Activity Selection
- Job Sequencing
- Course Schedule III

---

## Pattern 7 - Interval Greedy

Idea:

Process intervals in sorted order.

Examples:

- Merge Intervals
- Erase Overlap Intervals
- Minimum Arrows
- Meeting Rooms

---

## Pattern 8 - Reachability Greedy

Idea:

Always maximize reachable region.

Examples:

- Jump Game
- Jump Game II
- Gas Station

---

## Pattern 9 - String Greedy

Examples:

- Partition Labels
- Remove Duplicate Letters
- Reorganize String

---

## Pattern 10 - Mathematical Greedy

Observation-based.

Examples:

- Candy
- Patching Array
- Integer Replacement

---

# 13. Standard Interview Flow

Whenever you solve a Greedy problem, follow this sequence.

### Step 1

Understand the objective.

What needs to be optimized?

---

### Step 2

Identify the Greedy choice.

What decision is safest?

---

### Step 3

Check if sorting is needed.

If yes,

decide the sorting criteria.

---

### Step 4

Explain why the greedy choice is correct.

This is often more important than the implementation.

---

### Step 5

Traverse once,

making only greedy decisions.

---

### Step 6

Analyze Time Complexity and Space Complexity.

---

# 14. Common Mistakes

❌ Choosing largest/smallest blindly

❌ Sorting by the wrong parameter

❌ Forgetting to sort

❌ Using Greedy when DP is required

❌ Not proving why the greedy choice works

❌ Ignoring edge cases

❌ Assuming every optimization problem is Greedy

---

# 15. Time Complexity Pattern

Most Greedy problems look like this:

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Overall

```
O(n log n)
```

Without sorting

```
O(n)
```

With Heap

```
O(n log n)
```

---

# 16. Most Important Greedy Problems for Interviews

### Interval Problems

- Activity Selection
- N Meetings in One Room
- Merge Intervals
- Insert Interval
- Non-overlapping Intervals
- Minimum Number of Arrows

---

### Assignment Problems

- Assign Cookies
- Boats to Save People

---

### Scheduling Problems

- Job Sequencing with Deadlines
- Course Schedule III
- Minimum Platforms

---

### Ratio Problems

- Fractional Knapsack

---

### Heap Based Greedy

- IPO
- Furthest Building
- Meeting Rooms II

---

### Reachability Problems

- Jump Game
- Jump Game II
- Gas Station

---

### Other Important Problems

- Huffman Coding
- Candy
- Lemonade Change
- Partition Labels
- Reorganize String
- Remove Duplicate Letters

---

# 17. Quick Recognition Checklist

When reading a problem, ask yourself:

- Is it asking for **maximum** or **minimum**?
- Can I make one **permanent decision** at a time?
- Does sorting reveal an obvious order?
- Is there a choice that **preserves future opportunities**?
- Can I justify why that choice is always safe?
- After making one choice, does the remaining problem look similar?

If the answer to most of these is **YES**, the problem is very likely a **Greedy Algorithm**.

---

# 18. Final Greedy Mindset

Don't ask:

> **"What is the optimal solution?"**

Instead ask:

> **"Is there a decision I can make right now that is guaranteed to never hurt the optimal solution?"**

If such a decision exists **and you can justify why it is safe**, you have found the **Greedy Choice**, which is the heart of every Greedy Algorithm.

---

# Greedy Problem Solving Template

```
1. Read the problem carefully.

2. Identify what needs to be optimized.

3. Find the safest local (Greedy) choice.

4. Decide whether sorting is required.
   - If yes, determine the correct sorting criteria.

5. Prove why the Greedy choice is correct.

6. Traverse once while making Greedy decisions.

7. Return the answer.

8. Analyze Time Complexity and Space Complexity.
```
````
