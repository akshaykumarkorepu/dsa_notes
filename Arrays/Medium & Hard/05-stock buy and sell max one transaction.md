
## PROBLEM:

You are given an array `prices[]` where `prices[i]` represents the stock price on the `iᵗʰ` day.

You are allowed to perform **at most one transaction**:

- Buy once
- Sell once
- Buy must happen before selling.

Return the **maximum profit** possible.

If no profit is possible, return `0`.

---

# PATTERN:

**Running Minimum (Prefix Minimum) + Greedy**

---

# WHY THIS PATTERN:

The question asks for the **maximum profit** using **only one buy and one sell**.

For every selling day, the only buying price that matters is the **minimum price seen before that day**.

Instead of repeatedly searching previous prices (like the brute force does), we keep track of the minimum price while traversing the array once.

---

# CORE IDEA:

Instead of asking:

> **"If I buy today, when should I sell?"**

Reverse the thinking.

Ask:

> **"If I sell today, what is the cheapest price I have seen before today?"**

For every day:

```
Profit = Current Price - Minimum Price Seen So Far
```

Keep the maximum profit and continue updating the minimum price.

---

# BRUTE FORCE:

## Intuition

Treat every day as the buying day.

For each buying day, try selling on every future day and calculate the profit.

Store the maximum profit among all possible buy-sell pairs.

---

## Code

```cpp
class Solution {
public:
    int maximumProfit(vector<int>& prices) {

        int n = prices.size();
        int maxProfit = 0;

        for (int buy = 0; buy < n; buy++) {

            for (int sell = buy + 1; sell < n; sell++) {

                int profit = prices[sell] - prices[buy];

                maxProfit = max(maxProfit, profit);
            }
        }

        return maxProfit;
    }
};
```

---

## Dry Run

```
prices = [7,10,1,3,6,9]

Buy = 7

Sell with
10 → 3
1  → -6
3  → -4
6  → -1
9  → 2

Best = 3

-----------------

Buy = 10

Sell with
1
3
6
9

No better

-----------------

Buy = 1

Sell with
3 → 2
6 → 5
9 → 8

Best = 8
```

Answer = **8**

---

## Time Complexity

**O(n²)**

Reason:

For every buying day, we check every future selling day.

---

## Space Complexity

**O(1)**

Reason:

Only a few variables are used.

---

# OPTIMAL APPROACH:

## Observation

In the brute force solution, for every selling day we repeatedly search for the best buying day.

Example:

```
Sell = 9

Previous Prices

7 10 1 3 6
```

Which one actually matters?

Only:

```
1
```

The minimum.

So instead of searching repeatedly, maintain the minimum price seen so far while traversing.

---

# ALGORITHM:

1. Store the first price as the minimum price.
2. Initialize maximum profit as `0`.
3. Traverse the array.
4. Treat the current price as today's selling price.
5. Compute

```
profit = currentPrice - minPrice
```

6. Update the maximum profit.
7. Update the minimum price if today's price is smaller.
8. Continue until the array ends.
9. Return the maximum profit.

---

# DRY RUN:

```
prices = [7,10,1,3,6,9,2]
```

Initially

```
minPrice = 7
maxProfit = 0
```

---

### Day 0

Current Price

```
7
```

Compute Profit

```
profit = 7 - 7 = 0
```

Meaning:

If we sell today, we also buy today.

No profit.

Update Answer

```
maxProfit = max(0,0)

= 0
```

Update Minimum Price

```
minPrice = min(7,7)

= 7
```

Current State

```
minPrice = 7
maxProfit = 0
```

---

### Day 1

Current Price

```
10
```

Compute Profit

```
10 - 7 = 3
```

Meaning:

Buy at **7**

Sell at **10**

Profit = **3**

Update Answer

```
maxProfit = max(0,3)

= 3
```

Update Minimum

```
min(7,10)

= 7
```

Buying at 7 is still cheaper.

Current State

```
minPrice = 7
maxProfit = 3
```

---

### Day 2

Current Price

```
1
```

Compute Profit

```
1 - 7 = -6
```

Selling today gives a loss.

Update Answer

```
max(3,-6)

= 3
```

Update Minimum

```
min(7,1)

= 1
```

We found a cheaper buying price.

From now on, every future selling day will use **1** as the buying price.

Current State

```
minPrice = 1
maxProfit = 3
```

---

### Day 3

Current Price

```
3
```

Profit

```
3 - 1 = 2
```

Update Answer

```
max(3,2)

= 3
```

Update Minimum

```
min(1,3)

= 1
```

Current State

```
minPrice = 1
maxProfit = 3
```

---

### Day 4

Current Price

```
6
```

Profit

```
6 - 1 = 5
```

Meaning:

Buy at **1**

Sell at **6**

Update Answer

```
max(3,5)

= 5
```

Update Minimum

```
min(1,6)

= 1
```

Current State

```
minPrice = 1
maxProfit = 5
```

---

### Day 5

Current Price

```
9
```

Profit

```
9 - 1 = 8
```

Meaning:

Buy at **1**

Sell at **9**

Update Answer

```
max(5,8)

= 8
```

Update Minimum

```
min(1,9)

= 1
```

Current State

```
minPrice = 1
maxProfit = 8
```

---

### Day 6

Current Price

```
2
```

Profit

```
2 - 1 = 1
```

Update Answer

```
max(8,1)

= 8
```

Update Minimum

```
min(1,2)

= 1
```

Final Answer

```
8
```

---

# IMPORTANT CODE SNIPPETS:

### Store minimum price

```cpp
minPrice = min(minPrice, prices[i]);
```

### Profit if sold today

```cpp
int profit = prices[i] - minPrice;
```

### Update answer

```cpp
maxProfit = max(maxProfit, profit);
```

---

# COMMON MISTAKES:

### Mistake 1

Thinking Dynamic Programming is required.

Only one transaction is allowed.

A greedy approach is sufficient.

---

### Mistake 2

Trying to find the maximum future price.

Instead, think:

> "For today's selling price, what is the cheapest buying price before today?"

---

### Mistake 3

Returning a negative answer.

Example:

```
7 6 5 4
```

Correct answer:

```
0
```

---

### Mistake 4

Forgetting that buying must happen before selling.

Traversing left to right naturally guarantees this.

---

# WHY I MIGHT FORGET THIS:

It's easy to think:

> "I need the maximum selling price."

Actually,

every day is treated as a selling day.

The only question is:

> **"What is the cheapest price I've seen before today?"**

That gives the best possible profit for today.

---

# INTERVIEW FLOW:

### Step 1

Start with brute force.

> "The brute force solution considers every day as the buying day and checks every future selling day. This takes O(n²)."

---

### Step 2

Observation.

> "While analyzing the brute force solution, I noticed that for every selling day, I'm repeatedly searching for the minimum previous price."

---

### Step 3

Optimization.

> "Instead of searching every time, I maintain the minimum price seen so far while traversing the array."

---

### Step 4

Algorithm.

> "For every day, I treat today's price as the selling price, calculate the profit using the minimum price seen so far, update the maximum profit, and then update the minimum price if today's price is smaller."

---

### Step 5

Finish.

> "Since each element is processed once and every iteration performs constant work, the solution runs in O(n) time and O(1) extra space."

---

# TIME COMPLEXITY:

### Brute Force

**O(n²)**

Reason:

```
(n-1)+(n-2)+...+1

= n(n-1)/2

= O(n²)
```

For every buying day, we check every possible future selling day.

---

### Optimal

**O(n)**

Reason:

The array is traversed exactly once.

Each iteration performs:

- One subtraction
- One `max`
- One `min`

Each operation is O(1).

Therefore:

```
n × O(1)

= O(n)
```

---

# SPACE COMPLEXITY:

### Brute Force

**O(1)**

Only variables are used.

---

### Optimal

**O(1)**

Only these variables are maintained:

- `minPrice`
- `maxProfit`
- `profit`

No extra arrays or data structures are used.

---

# EDGE CASES:

### Single Element

```
[5]

Answer = 0
```

---

### Strictly Decreasing

```
7 6 5 4

Answer = 0
```

---

### Strictly Increasing

```
1 3 6 9

Buy first

Sell last
```

---

### Equal Prices

```
4 4 4 4

Answer = 0
```

---

### Minimum Price Appears Late

```
8 9 10 2 7

Buy = 2

Sell = 7

Profit = 5
```

---

# PATTERN RECOGNITION:

Use this pattern whenever you see:

- One buy and one sell
- Maximum difference
- Buy before sell
- Best previous value
- Maximum gain from one transaction
- Prefix Minimum
- Running Minimum

Immediately think:

> **Running Minimum + Maximum Difference**

---

# Clean C++ Code

```cpp
class Solution {
public:
    int maximumProfit(vector<int>& prices) {

        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 0; i < prices.size(); i++) {

            int profit = prices[i] - minPrice;

            maxProfit = max(maxProfit, profit);

            minPrice = min(minPrice, prices[i]);
        }

        return maxProfit;
    }
};
```

---

# Intuition Behind Every Important Line

```cpp
int minPrice = prices[0];
```

Initially, the first day's price is the only buying option available.

---

```cpp
int maxProfit = 0;
```

If no profitable transaction exists, return `0`.

---

```cpp
for(int i = 0; i < prices.size(); i++)
```

Traverse every day once, treating each day as a possible selling day.

---

```cpp
int profit = prices[i] - minPrice;
```

Assume we sell today.

The best buying price is the minimum price seen so far.

---

```cpp
maxProfit = max(maxProfit, profit);
```

Keep the best profit found so far.

---

```cpp
minPrice = min(minPrice, prices[i]);
```

If today's price is smaller, it becomes the new best buying price for future days.

---

```cpp
return maxProfit;
```

Return the maximum profit after checking every selling day.

---

# Easy-to-Remember Summary

- **Brute Force:** Try every buy-sell pair → **O(n²)**
- **Observation:** For every selling day, only the **minimum previous price** matters.
- **Pattern:** Running Minimum + Greedy.
- **Formula:**

```
profit = currentPrice - minPrice
```

- **Order:**

```
Compute Profit

↓

Update Answer

↓

Update Minimum Price
```

- **Complexity:**

```
Time  : O(n)

Space : O(1)
```

## Memory Trick

> **Carry the cheapest buying price as you move through the array. Every day, ask: "If I sell today, what's my profit?"**
