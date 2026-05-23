# NOTE

# PROBLEM:
Geek Training / Ninja Training

---

# PATTERN:
DP on Days + Previous State

---

# WHY THIS PATTERN:

At every day:

- we have multiple choices
- current choice depends on previous choice
- we need maximum points

This is a classic:

```text
DP(index, previous_state)
```

pattern.

---

# DP SHORTCUT NOTES

## SIGNALS THAT THIS IS DP

### 1. Multiple choices every index/day

At each day:

- Running
- Fighting
- Learning

---

### 2. Restriction based on previous choice

```text
Cannot do same activity on consecutive days
```

This means:

```text
Current decision depends on previous decision
```

BIG DP SIGNAL.

---

### 3. Maximize something

```text
Maximum total merit points
```

Another DP signal.

---

# CORE IDEA

We define:

```cpp
solve(day, last)
```

Meaning:

```text
Maximum points till "day"
when previous activity was "last"
```

---

# STATE DEFINITION

| Variable | Meaning |
|---|---|
| day | current day |
| last | activity done on previous day |

---

# LAST VALUES

| last | Meaning |
|---|---|
| 0 | Running |
| 1 | Fighting |
| 2 | Learning |
| 3 | No previous activity |

---

# HOW TO THINK IN INTERVIEW

Say:

```text
Since the current day's choice depends on the previous day's activity,
I will use DP with state:

(day, lastActivity)

where lastActivity tells me what was performed previously.
```

Then say:

```text
For every day, I try all 3 activities except the previous one
and take the maximum.
```

That is the entire question.

---

# RECURSION

# RECURSION IDEA

At every day:

- try all 3 tasks
- skip the previous task
- take maximum

---

# RECURSION TRANSITION

```cpp
points =
mat[day][task] +
solve(day-1, task)
```

Why?

Because:

```text
If I do "task" today,
then yesterday's restriction becomes "task"
```

---

# BASE CASE

```cpp
day == 0
```

Choose maximum task except `last`.

---

# RECURSION CODE

```cpp
class Solution {
  public:
  
    int solve(int day,
              int last,
              vector<vector<int>> &mat) {
        
        // BASE CASE
        if(day == 0) {
            
            int maxi = 0;
            
            for(int task = 0; task < 3; task++) {
                
                if(task != last) {
                    maxi = max(maxi, mat[0][task]);
                }
            }
            
            return maxi;
        }
        
        int maxi = 0;
        
        // Try all 3 tasks
        for(int task = 0; task < 3; task++) {
            
            if(task != last) {
                
                int points =
                    mat[day][task] +
                    solve(day - 1,
                          task,
                          mat);
                
                maxi = max(maxi, points);
            }
        }
        
        return maxi;
    }
    
    
    int maximumPoints(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        return solve(n - 1, 3, mat);
    }
};
```

---

# IMPORTANT RECURSION SNIPPET

```cpp
for(int task = 0; task < 3; task++) {
    
    if(task != last) {
        
        int points =
            mat[day][task] +
            solve(day - 1, task, mat);
        
        maxi = max(maxi, points);
    }
}
```

---

# RECURSION DRY RUN

Input:

```cpp
[
 [1,2,5],
 [3,1,1],
 [3,3,3]
]
```

Start:

```cpp
solve(2,3)
```

Meaning:

```text
Day = 2
No restriction
```

---

# OPTION 1 → Running

```cpp
3 + solve(1,0)
```

Now Running cannot be chosen on Day 1.

---

# solve(1,0)

Allowed:

- Fighting
- Learning

---

## Fighting

```cpp
1 + solve(0,1)
```

---

## solve(0,1)

Cannot choose Fighting.

Choices:

```text
Running = 1
Learning = 5
```

Maximum = 5

So:

```cpp
1 + 5 = 6
```

---

## Learning

```cpp
1 + solve(0,2)
```

Cannot choose Learning.

Choices:

```text
Running = 1
Fighting = 2
```

Maximum = 2

So:

```cpp
1 + 2 = 3
```

---

Maximum:

```cpp
solve(1,0)=6
```

So:

```cpp
3 + 6 = 9
```

Similarly:

```text
Option 2 = 11
Option 3 = 11
```

Final answer:

```text
11
```

---

# RECURSION TREE

```text
solve(2,3)
├── Running  -> solve(1,0)
│   ├── Fighting -> solve(0,1)
│   └── Learning -> solve(0,2)
│
├── Fighting -> solve(1,1)
│   ├── Running  -> solve(0,0)
│   └── Learning -> solve(0,2)
│
└── Learning -> solve(1,2)
    ├── Running  -> solve(0,0)
    └── Fighting -> solve(0,1)
```

---

# WHY RECURSION IS BAD

Observe repeated states:

```cpp
solve(0,0)
solve(0,1)
solve(0,2)
```

Repeated again and again.

This is:

```text
Overlapping Subproblem
```

Hence:

```text
Use Memoization
```

---

# RECURSION TC AND SC

```text
TC = O(3^N)

SC = O(N)
```

---

# MEMOIZATION

# MEMOIZATION IDEA

Store already computed states.

---

# DP STATE

```cpp
dp[day][last]
```

Meaning:

```text
Answer of solve(day,last)
```

---

# MEMOIZATION FLOW

Before calculating:

```cpp
if(dp[day][last] != -1)
```

Then:

```text
Return stored answer
```

Else:

```text
Compute and store
```

---

# MEMOIZATION CODE

```cpp
class Solution {
  public:
  
    int solve(int day,
              int last,
              vector<vector<int>> &mat,
              vector<vector<int>> &dp) {
        
        // Already solved
        if(dp[day][last] != -1) {
            return dp[day][last];
        }
        
        // BASE CASE
        if(day == 0) {
            
            int maxi = 0;
            
            for(int task = 0; task < 3; task++) {
                
                if(task != last) {
                    maxi = max(maxi, mat[0][task]);
                }
            }
            
            return dp[day][last] = maxi;
        }
        
        int maxi = 0;
        
        for(int task = 0; task < 3; task++) {
            
            if(task != last) {
                
                int points =
                    mat[day][task] +
                    solve(day - 1,
                          task,
                          mat,
                          dp);
                
                maxi = max(maxi, points);
            }
        }
        
        return dp[day][last] = maxi;
    }
    
    
    int maximumPoints(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        vector<vector<int>> dp(n,
                               vector<int>(4, -1));
        
        return solve(n - 1, 3, mat, dp);
    }
};
```

---

# IMPORTANT MEMOIZATION SNIPPET

```cpp
if(dp[day][last] != -1) {
    return dp[day][last];
}
```

---

# MEMOIZATION DRY RUN

Initially:

```cpp
dp =
[
 [-1,-1,-1,-1],
 [-1,-1,-1,-1],
 [-1,-1,-1,-1]
]
```

---

Suppose:

```cpp
solve(0,1)
```

gets computed.

Answer:

```cpp
5
```

Store:

```cpp
dp[0][1] = 5
```

---

Next time:

```cpp
solve(0,1)
```

comes again.

Now:

```cpp
dp[0][1] != -1
```

Directly return:

```cpp
5
```

No recursion again.

---

# MEMOIZATION TC AND SC

```text
TC = O(N * 4 * 3)

SC = O(N * 4) + O(N recursion stack)
```

Simplified:

```text
TC = O(N)

SC = O(N)
```

---

# TABULATION

# TABULATION IDEA

Convert:

```text
Top Down DP
```

into:

```text
Bottom Up DP
```

---

# TABULATION THINKING

Memoization:

```cpp
solve(day,last)
```

Tabulation:

```cpp
dp[day][last]
```

Current state depends on:

```cpp
dp[day-1][task]
```

So build:

```text
Day 0 → Day 1 → Day 2
```

---

# BASE CASE BUILDING

```cpp
dp[0][0] = max(mat[0][1], mat[0][2]);

dp[0][1] = max(mat[0][0], mat[0][2]);

dp[0][2] = max(mat[0][0], mat[0][1]);

dp[0][3] = max({
    mat[0][0],
    mat[0][1],
    mat[0][2]
});
```

---

# TABULATION CODE

```cpp
class Solution {
  public:
    
    int maximumPoints(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        vector<vector<int>> dp(n, vector<int>(4, 0));
        
        // BASE CASE
        
        dp[0][0] = max(mat[0][1], mat[0][2]);
        
        dp[0][1] = max(mat[0][0], mat[0][2]);
        
        dp[0][2] = max(mat[0][0], mat[0][1]);
        
        dp[0][3] = max({
            mat[0][0],
            mat[0][1],
            mat[0][2]
        });
        
        
        // TABULATION
        
        for(int day = 1; day < n; day++) {
            
            for(int last = 0; last < 4; last++) {
                
                int maxi = 0;
                
                for(int task = 0; task < 3; task++) {
                    
                    if(task != last) {
                        
                        int points =
                            mat[day][task] +
                            dp[day - 1][task];
                        
                        maxi = max(maxi, points);
                    }
                }
                
                dp[day][last] = maxi;
            }
        }
        
        return dp[n - 1][3];
    }
};
```

---

# IMPORTANT TABULATION SNIPPET

```cpp
int points =
    mat[day][task] +
    dp[day - 1][task];
```

---

# TABULATION DRY RUN

Day 0:

```cpp
dp[0] = [5,5,2,5]
```

---

Day 1:

```cpp
dp[1][0] = 6
dp[1][1] = 8
dp[1][2] = 8
dp[1][3] = 8
```

---

Day 2:

```cpp
dp[2][3] = 11
```

Final answer:

```text
11
```

---

# TABULATION TC AND SC

```text
TC = O(N * 4 * 3)

SC = O(N * 4)
```

Simplified:

```text
TC = O(N)

SC = O(N)
```

---

# SPACE OPTIMIZATION

# SPACE OPTIMIZATION IDEA

Observe:

```cpp
dp[day][last]
```

only depends on:

```cpp
dp[day-1][task]
```

We only need previous row.

---

# SPACE OPTIMIZATION TRICK

Replace:

```cpp
dp[n][4]
```

with:

```cpp
prev[4]
curr[4]
```

---

# SPACE OPTIMIZED CODE

```cpp
class Solution {
  public:
    
    int maximumPoints(vector<vector<int>>& mat) {
        
        int n = mat.size();
        
        vector<int> prev(4, 0);
        
        // BASE CASE
        
        prev[0] = max(mat[0][1], mat[0][2]);
        
        prev[1] = max(mat[0][0], mat[0][2]);
        
        prev[2] = max(mat[0][0], mat[0][1]);
        
        prev[3] = max({
            mat[0][0],
            mat[0][1],
            mat[0][2]
        });
        
        
        // SPACE OPTIMIZATION
        
        for(int day = 1; day < n; day++) {
            
            vector<int> curr(4, 0);
            
            for(int last = 0; last < 4; last++) {
                
                int maxi = 0;
                
                for(int task = 0; task < 3; task++) {
                    
                    if(task != last) {
                        
                        int points =
                            mat[day][task] +
                            prev[task];
                        
                        maxi = max(maxi, points);
                    }
                }
                
                curr[last] = maxi;
            }
            
            prev = curr;
        }
        
        return prev[3];
    }
};
```

---

# IMPORTANT SPACE OPTIMIZATION SNIPPET

```cpp
mat[day][task] + prev[task]
```

instead of:

```cpp
mat[day][task] + dp[day-1][task]
```

---

# SPACE OPTIMIZATION DRY RUN

Initially:

```cpp
prev = [5,5,2,5]
```

After Day 1:

```cpp
prev = [6,8,8,8]
```

After Day 2:

```cpp
prev[3] = 11
```

Answer:

```text
11
```

---

# SPACE OPTIMIZATION TC AND SC

```text
TC = O(N * 4 * 3)

SC = O(4)
```

Simplified:

```text
TC = O(N)

SC = O(1)
```

---

# INTERVIEW FLOW

If this question comes in interview:

---

# STEP 1

Say:

```text
This is a DP problem because:

1. We have multiple choices every day
2. Current choice depends on previous choice
3. We need maximum points
```

---

# STEP 2

Define state:

```text
(day, last)
```

---

# STEP 3

Explain transition:

```text
Try all tasks except previous task
and take maximum.
```

---

# STEP 4

Write recursion.

---

# STEP 5

Mention overlapping subproblems.

---

# STEP 6

Convert to memoization.

---

# STEP 7

Convert to tabulation.

---

# STEP 8

Optimize space.

---

# WHY PEOPLE GET STUCK

## 1. Forgetting what `last` means

Remember:

```text
last = previous activity
```

NOT current activity.

---

## 2. Confusing dp transition

Remember:

```cpp
mat[day][task] + dp[day-1][task]
```

because:

```text
If I do task today,
then yesterday restriction becomes task.
```

---

## 3. Confusing dp dimensions

Need:

```cpp
4 columns
```

because:

```text
0,1,2,3
```

---

# MEMORY TRICK

Think:

```text
Today's task becomes tomorrow's restriction
```

That single sentence solves the entire problem.
