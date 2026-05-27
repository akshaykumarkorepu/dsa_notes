
# NOTE

# PROBLEM: Partition Equal Subset Sum

# PATTERN: DP on Subsequences → Subset Sum

# WHY THIS PATTERN:

At every index we have 2 choices:

```text
Take the element
Not Take the element
```

This is exactly the subsequence pattern.

We are trying to check:

```text
Can we form a target sum?
```

which is classic:

# Subset Sum DP

---

# QUESTION EXPLANATION

We are given an array.

We need to divide it into:

```text
2 subsets
```

such that:

```text
sum(subset1) == sum(subset2)
```

---

Example:

```text
arr = [1,5,11,5]
```

Possible partition:

```text
[11]
[1,5,5]
```

Both sums:

```text
11
```

So answer:

```text
true
```

---

# MAIN OBSERVATION / SHORTCUT

If total sum is:

```text
sum
```

and both subsets are equal:

```text
subset1 = subset2 = x
```

Then:

```text
x + x = sum
```

So:

```text
2x = sum
```

Therefore:

```text
x = sum/2
```

---

# CORE IDEA

Instead of finding TWO subsets,

we only try to find:

# ONE subset with sum = totalSum/2

Because remaining elements automatically form the second subset.

---

# IMPORTANT SHORTCUT NOTE

# If total sum is ODD:

Equal partition is IMPOSSIBLE.

Because:

```text
odd / 2 = decimal
```

and subset sums must be integers.

---

# DP SHORTCUT NOTES

# STATE

```text
f(index,target)
```

means:

```text
Can we form target using indices 0 → index ?
```

---

# CHOICES

At every index:

```text
Take
Not Take
```

---

# BASE CASES

```text
target == 0 → true
```

because target successfully formed.

---

```text
index == 0
```

Check:

```text
arr[0] == target
```

---

# WHY I MIGHT FORGET

```text
I forget why we only check one subset.
```

Remember:

```text
If one subset becomes sum/2,
remaining elements automatically become sum/2.
```

---

# RECURSION IDEA

We recursively try:

```text
Take current element
Not take current element
```

until:

```text
target becomes 0
```

---

# RECURSION DRY RUN

```text
arr = [1,5,11,5]
```

Total Sum:

```text
22
```

Target:

```text
11
```

Now problem becomes:

```text
Can we form subset sum = 11 ?
```

---

Initial Call:

```text
f(3,11)
```

Meaning:

```text
Can I form 11 using indices 0 → 3 ?
```

---

Current element:

```text
arr[3] = 5
```

Choices:

---

# NOT TAKE

```text
f(2,11)
```

---

# TAKE

Since:

```text
5 <= 11
```

Take it.

New target:

```text
11 - 5 = 6
```

Call:

```text
f(2,6)
```

---

Now left side:

```text
f(2,11)
```

Current element:

```text
11
```

---

# TAKE 11

New target:

```text
11 - 11 = 0
```

Call:

```text
f(1,0)
```

Base case:

```text
target == 0
```

So:

```text
true
```

Hence subset formed successfully.

---

# RECURSION TREE

```text
                     f(3,11)
                    /       \
             notTake        take
              f(2,11)       f(2,6)
              /      \
       notTake      take
        f(1,11)     f(1,0)
                        |
                     TRUE
```

---

# RECURSION CODE

```cpp
class Solution {
  public:
  
    bool f(int index, int target, vector<int>& arr){
        
        // Target formed
        if(target == 0)
            return true;
        
        // Only first element left
        if(index == 0)
            return arr[0] == target;
        
        // Not take
        bool notTake =
            f(index-1, target, arr);
        
        // Take
        bool take = false;
        
        if(arr[index] <= target){
            take =
            f(index-1,
              target-arr[index],
              arr);
        }
        
        return take || notTake;
    }
  
    bool equalPartition(vector<int>& arr) {
        
        int sum = 0;
        
        for(int x : arr)
            sum += x;
        
        // Odd sum impossible
        if(sum % 2 != 0)
            return false;
        
        int target = sum / 2;
        
        return f(arr.size()-1,
                 target,
                 arr);
    }
};
```

---

# RECURSION TC

```text
O(2^n)
```

---

# RECURSION SC

```text
O(n)
```

Recursion stack.

---

# MEMOIZATION IDEA

In recursion:

```text
Same states repeat again and again.
```

State:

```text
(index,target)
```

Store answers in DP table.

---

# MEMOIZATION DRY RUN IDEA

Suppose:

```text
f(2,6)
```

gets called multiple times.

Instead of recomputing:

Store answer in:

```text
dp[2][6]
```

Next time directly return it.

---

# MEMOIZATION CODE

```cpp
class Solution {
  public:
  
    bool solve(int index,
               int target,
               vector<int>& arr,
               vector<vector<int>>& dp){
        
        if(target == 0)
            return true;
        
        if(index == 0)
            return arr[0] == target;
        
        // Already computed
        if(dp[index][target] != -1)
            return dp[index][target];
        
        bool notTake =
            solve(index-1,
                  target,
                  arr,
                  dp);
        
        bool take = false;
        
        if(arr[index] <= target){
            
            take =
            solve(index-1,
                  target-arr[index],
                  arr,
                  dp);
        }
        
        return dp[index][target] =
               (take || notTake);
    }
  
  
    bool equalPartition(vector<int>& arr) {
        
        int n = arr.size();
        
        int sum = 0;
        
        for(int x : arr)
            sum += x;
        
        if(sum % 2 != 0)
            return false;
        
        int target = sum / 2;
        
        vector<vector<int>> dp(
            n,
            vector<int>(target+1,-1)
        );
        
        return solve(n-1,
                     target,
                     arr,
                     dp);
    }
};
```

---

# MEMOIZATION TC

```text
O(n * target)
```

---

# MEMOIZATION SC

```text
O(n * target) + O(n)
```

DP table + recursion stack.

---

# TABULATION IDEA

Convert recursion to iterative DP.

---

# DP MEANING

```text
dp[index][target]
```

means:

```text
Can we form target
using indices 0 → index ?
```

---

# BASE CASES

# TARGET 0

```text
dp[i][0] = true
```

Because target 0 can always be formed.

---

# FIRST ELEMENT

```text
dp[0][arr[0]] = true
```

---

# TABULATION DRY RUN

```text
arr = [1,5,11,5]
target = 11
```

DP table:

```text
index → rows
target → columns
```

---

Initially:

```text
dp[i][0] = true
```

because target 0 always possible.

---

Then:

```text
dp[0][1] = true
```

because first element is 1.

---

Now fill table using:

```text
take
notTake
```

Eventually:

```text
dp[3][11] = true
```

meaning:

```text
subset sum 11 possible
```

---

# TABULATION CODE

```cpp
class Solution {
  public:
  
    bool equalPartition(vector<int>& arr) {
        
        int n = arr.size();
        
        int sum = 0;
        
        for(int x : arr)
            sum += x;
        
        if(sum % 2 != 0)
            return false;
        
        int target = sum / 2;
        
        vector<vector<bool>> dp(
            n,
            vector<bool>(target+1,false)
        );
        
        // Target 0
        for(int i=0;i<n;i++){
            dp[i][0] = true;
        }
        
        // First element
        if(arr[0] <= target){
            dp[0][arr[0]] = true;
        }
        
        for(int index=1;
            index<n;
            index++){
            
            for(int t=1;
                t<=target;
                t++){
                
                bool notTake =
                dp[index-1][t];
                
                bool take = false;
                
                if(arr[index] <= t){
                    take =
                    dp[index-1]
                      [t-arr[index]];
                }
                
                dp[index][t] =
                take || notTake;
            }
        }
        
        return dp[n-1][target];
    }
};
```

---

# TABULATION TC

```text
O(n * target)
```

---

# TABULATION SC

```text
O(n * target)
```

---

# SPACE OPTIMISATION IDEA

Observe:

```text
Current row only depends on previous row.
```

So no need for full 2D DP.

Use:

```text
prev[]
curr[]
```

---

# SPACE OPTIMISATION CODE

```cpp
class Solution {
  public:
  
    bool equalPartition(vector<int>& arr) {
        
        int n = arr.size();
        
        int sum = 0;
        
        for(int x : arr)
            sum += x;
        
        if(sum % 2 != 0)
            return false;
        
        int target = sum / 2;
        
        vector<bool> prev(target+1,false);
        
        prev[0] = true;
        
        if(arr[0] <= target)
            prev[arr[0]] = true;
        
        
        for(int index=1;
            index<n;
            index++){
            
            vector<bool> curr(target+1,false);
            
            curr[0] = true;
            
            for(int t=1;
                t<=target;
                t++){
                
                bool notTake =
                prev[t];
                
                bool take = false;
                
                if(arr[index] <= t){
                    take =
                    prev[t-arr[index]];
                }
                
                curr[t] =
                take || notTake;
            }
            
            prev = curr;
        }
        
        return prev[target];
    }
};
```

---

# SPACE OPTIMISATION TC

```text
O(n * target)
```

---

# SPACE OPTIMISATION SC

```text
O(target)
```

---

# IMPORTANT INTERVIEW EXPLANATION

If interviewer asks:

# “Why only checking target = sum/2 ?”

Say:

```text
If one subset becomes sum/2,
remaining elements automatically become sum/2

because:

remainingSum = totalSum - sum/2
             = sum/2
```

---

# HOW TO IDENTIFY THIS PROBLEM IN INTERVIEW

Keywords:

```text
Partition
Equal Sum
Subset
Can divide
```

Immediately think:

# Subset Sum DP

---

# MOST IMPORTANT MEMORY TRICK

Whenever you see:

```text
Equal partition
```

Immediately think:

# "Find subset sum = totalSum/2"

That is the entire problem reduction.
````
