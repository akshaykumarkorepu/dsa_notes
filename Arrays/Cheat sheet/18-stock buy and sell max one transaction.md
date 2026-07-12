
# Stock Buy and Sell – Max One Transaction Allowed

**PATTERN:** Running Minimum (Prefix Minimum) + Greedy  
→ **Trigger:** *"when I see one buy + one sell, buy before sell, maximum profit/difference, and only one transaction is allowed."*

---

## BRUTE FORCE

→ **Idea:** Try every day as the buying day, then try selling on every future day and keep the maximum profit.

**Crucial Snippet**

```cpp
for (int buy = 0; buy < n; buy++)
    for (int sell = buy + 1; sell < n; sell++)
        maxProfit = max(maxProfit, prices[sell] - prices[buy]);
```

→ **TC / SC:** **O(n²)** / **O(1)**

---

## OPTIMAL

→ **First instinct:** *"I immediately maintain the minimum price seen so far and treat every day as a possible selling day."*

→ **Core idea:** Maintain two variables:
- `minPrice` = minimum stock price seen so far (best buying price).
- `maxProfit` = maximum profit found so far.

For every price:
1. Calculate profit if sold today using `profit = currentPrice - minPrice`.
2. Update `maxProfit`.
3. Update `minPrice` for future days.

**Crucial Snippets**

```cpp
int minPrice = prices[0];
int maxProfit = 0;
```

```cpp
int profit = prices[i] - minPrice;
maxProfit = max(maxProfit, profit);
minPrice = min(minPrice, prices[i]);
```

→ **TC / SC:** **O(n)** / **O(1)**

---

## WATCH OUT FOR

→ Don't search for the **maximum future price**. The key observation is that for every selling day, you only need the **minimum price seen before it**.

---

## INTERVIEW FLOW (what I say out loud, in order)

1. Brute force checks every buy-sell pair → **O(n²)**.
2. Observation: for every selling day, only the minimum previous price matters.
3. Maintain `minPrice` while traversing once.
4. For each day, compute `profit = currentPrice - minPrice`, update `maxProfit`, then update `minPrice`.
5. Single traversal gives **O(n)** time and **O(1)** space.
```**
````
