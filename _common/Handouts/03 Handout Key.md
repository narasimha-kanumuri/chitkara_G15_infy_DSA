# DAY 3 ANSWER KEY — 1D LINEAR DP

---

## WORKSHEET 1 — EASY

- Festival Gate Routes

### 1) Pattern Identification

- Sequential positions from 0 to n
- Count the number of ways
- Each gate depends on earlier gates
- Repetition appears in brute force recursion
- 👉 Pattern: Counting 1D DP (same family as climbing-stairs-style counting). 

### 2) Correct DP Meaning

- **Label:**
- `dp[i]` = number of valid ways to reach gate i

---

### 3) Transition

- If gate i is blocked:
- `dp[i] = 0`
- Otherwise:
- `dp[i] = dp[i-1] + dp[i-2]`
- because the last move to gate i could have come from:
- gate i-1
- gate i-2

---

### 4) Base Cases

- A clean version:
- Case A: If start is blocked
- If b == 0:
- answer = 0
- Otherwise
- `dp[0] = 1`
- For i = 1:
- if gate 1 is blocked, `dp[1] = 0`
- else `dp[1] = dp[0]`
- So:
- `dp[1] = 0` if 1 == b else `dp[0]`

### 5) Build Order

- Fill dp from left to right: i = 2 to n

### 6) Answer Location

- `dp[n]`

---

### 7) Sample 1 Answer

- n = 5, b = 3
- DP Table

| i | blocked? | dp[i] |
| --- | --- | --- |
| 0 | no | 1 |
| 1 | no | 1 |
| 2 | no | 2 |
| 3 | yes | 0 |
| 4 | no | 2 |
| 5 | no | 2 |

- **Output:**
- 2
- **Valid routes:**
- 0 → 2 → 4 → 5
- 0 → 1 → 2 → 4 → 5

---

### 8) Sample 2 Answer

- n = 4, b = 2
- DP Table

| i | blocked? | dp[i] |
| --- | --- | --- |
| 0 | no | 1 |
| 1 | no | 1 |
| 2 | yes | 0 |
| 3 | no | 1 |
| 4 | no | 1 |

- **Output:**
- 1
- **Valid route:**
- 0 → 1 → 3 → 4

---

### 9) Brute Force Reflection

- A correct short answer:
- 1. From gate i, recursion tries coming from i-1 and i-2 again and again.
- 1. The same subproblems repeat, so brute force becomes exponential.
- 1. DP stores the answer for each gate once.

---

### 10) Java Reference Solution

```java
public static int countFestivalRoutes(int n, int b) {
    if (b == 0 || b == n) return 0;

    int[] dp = new int[n + 1];
    dp[0] = 1;

    if (n >= 1) {
        dp[1] = (b == 1) ? 0 : dp[0];
    }

    for (int i = 2; i <= n; i++) {
        if (i == b) {
            dp[i] = 0;
        } else {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
    }

    return dp[n];
}
```

### 11) Reflection Answers

- **What makes this counting DP?**
- We are counting total valid ways, so the transition adds possibilities.
- **What does dp[i] mean?**
- `dp[i]` is the number of valid ways to reach gate i.

---

## WORKSHEET 2 — MEDIUM

- Rooftop Battery Climb

### 1) Pattern Identification

- Linear movement
- Need the minimum cost
- Each answer depends on previous positions
- Same family as min cost climbing stairs

### 2) Recommended DP Meaning

- Best clean meaning:
- `dp[i]` = minimum battery cost to reach position i
- Where positions are:
- platforms 0 ... n-1
- roof is position n

---

### 3) Transition

- To reach position i, the last move could come from:
- i-1
- i-2
- Cost is paid when leaving a platform.
- So:
- `dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`

### 4) Base Cases

- Because you can start at platform 0 or 1:
- `dp[0] = 0`
- `dp[1] = 0`

### 5) Answer Location

- `dp[n]`

---

### 6) Sample 1 Answer

- cost = [4, 2, 7, 3]
- n = 4
- Compute
- `dp[0] = 0`
- `dp[1] = 0`
- `dp[2] = min(dp[1] + cost[1], dp[0] + cost[0])`
- `= min(0 + 2, 0 + 4)`
- `= 2`
- `dp[3] = min(dp[2] + cost[2], dp[1] + cost[1])`
- `= min(2 + 7, 0 + 2)`
- `= 2`
- `dp[4] = min(dp[3] + cost[3], dp[2] + cost[2])`
- `= min(2 + 3, 2 + 7)`
- `= 5`
- **Output:**
- 5
- **One optimal path:**
- Start at platform 1, then jump to platform 3, then roof.

---

### 7) Sample 2 Answer

- cost = [1, 100, 1, 1, 100, 1]
- n = 6
- Compute
- `dp[0] = 0`
- `dp[1] = 0`
- `dp[2] = min(0 + 100, 0 + 1) = 1`
- `dp[3] = min(1 + 1, 0 + 100) = 2`
- `dp[4] = min(2 + 1, 1 + 1)   = 2`
- `dp[5] = min(2 + 100, 2 + 1) = 3`
- `dp[6] = min(3 + 1, 2 + 100) = 4`
- So for this exact interpretation, the output is:
- 4
- **Important note:**
- If someone expected 3, that would correspond to a different sample/problem setting than the worksheet statement as written. Since the worksheet says you pay `cost[i]` on leaving platform i and the roof is position n, the consistent output is 4 for this case.
- This kind of “definition consistency” is exactly why your framework insists students must define the state and answer location before coding.

---

### 8) Common Mistake Check — Correct Explanations

- Using + instead of min → wrong because this is optimization, not counting
- Wrong base case → common because students forget the meaning of the starting positions
- Wrong dp meaning → causes wrong recurrence
- Returning wrong index → some return `dp[n-1]` instead of `dp[n]`

---

### 9) Java Reference Solution

```java
public static int minBatteryCost(int[] cost) {
    int n = cost.length;
    int[] dp = new int[n + 1];

    dp[0] = 0;
    dp[1] = 0;

    for (int i = 2; i <= n; i++) {
        dp[i] = Math.min(dp[i - 1] + cost[i - 1],
                         dp[i - 2] + cost[i - 2]);
    }

    return dp[n];
}
```

### 10) Reflection Answers

- **How is this different from Worksheet 1?**
- 1. Worksheet 1 counts paths, so recurrence uses addition.
- 1. Worksheet 2 minimizes cost, so recurrence uses min.
- **What changed?**
- The objective changed from counting to minimizing.

---

## WORKSHEET 3 — HARD

- Signal Tower Jumps

### 1) Pattern Identification

- Sequential indices
- Minimum total energy
- Last move comes from earlier towers
- Same family as frog jump 
  
### 2) DP Meaning

- `dp[i]` = minimum energy required to reach tower i

---

### 3) Jump Cost Function

- Base jump energy:
- abs(h[a] - h[b])
- If destination b is boosted:
- 2 * abs(h[a] - h[b])
- So:
- `jumpCost(a, b) = abs(h[a] - h[b]) * (boost[b] == 1 ? 2 : 1)`

### 4) Transition

- To reach tower i, last jump could come from:
- i-1
- i-2
- So:
- `dp[i] = min(`
- `dp[i-1] + jumpCost(i-1, i),`
- `dp[i-2] + jumpCost(i-2, i)`
- `)`
- for i >= 2

---

### 5) Base Cases

- `dp[0] = 0`
- `dp[1] = jumpCost(0, 1)`

### 6) Answer Location

- `dp[n-1]`

---

### 7) Sample Answer

- h     = [10, 30, 20, 25, 10]
- boost = [0,  1,  0,  0,  1]
- Step 1: Cost helper values
- jumpCost(0,1) = |10-30| *2 = 20* 2 = 40
- jumpCost(0,2) = |10-20| * 1 = 10
- jumpCost(1,2) = |30-20| * 1 = 10
- jumpCost(1,3) = |30-25| * 1 = 5
- jumpCost(2,3) = |20-25| * 1 = 5
- jumpCost(2,4) = |20-10| *2 = 10* 2 = 20
- jumpCost(3,4) = |25-10| *2 = 15* 2 = 30
- DP Table
- `dp[0] = 0`
- `dp[1] = 40`
- `dp[2] = min(dp[1] + 10, dp[0] + 10)`
- `= min(50, 10)`
- `= 10`
- `dp[3] = min(dp[2] + 5, dp[1] + 5)`
- `= min(15, 45)`
- `= 15`
- `dp[4] = min(dp[3] + 30, dp[2] + 20)`
- `= min(45, 30)`
- `= 30`
- **Output:**
- 30
- **One optimal path:**
- 0 → 2 → 4

---

### 8) Observation Answers

- A strong response:
- 1. The minimum energy to reach tower i depends only on the best energy to reach earlier towers.
- 1. Since only jumps of length 1 or 2 are allowed, each state depends on at most two previous states.
- This “observation discovery before the final formula” is explicitly part of your DSA engine.

### 9) Data Type Reflection

- A correct answer:
- long may be safer because heights can be large and repeated jump costs may add up.

---

### 10) Java Reference Solution

```java
public static long minSignalJumpEnergy(int[] h, int[] boost) {
    int n = h.length;
    long[] dp = new long[n];

    dp[0] = 0;

    dp[1] = jumpCost(h, boost, 0, 1);

    for (int i = 2; i < n; i++) {
        long oneStep = dp[i - 1] + jumpCost(h, boost, i - 1, i);
        long twoStep = dp[i - 2] + jumpCost(h, boost, i - 2, i);
        dp[i] = Math.min(oneStep, twoStep);
    }

    return dp[n - 1];
}

private static long jumpCost(int[] h, int[] boost, int a, int b) {
    long base = Math.abs((long)h[a] - h[b]);
    return boost[b] == 1 ? 2 * base : base;
}
```

---

### 11) Reflection Answers

- **Why is this still 1D DP?**
- Because the state is still indexed by one position i, and each answer is built from earlier indices.
- **What stays the same across all three worksheets?**
- State definition, base cases, forward build order, and answer location all remain central.

---

## MINI-REVIEW PAGE — ANSWER KEY

### A) Recognition Checklist

- Correct interpretation:
- A Day 3 problem is 1D DP if the answer for index i is built from a small number of earlier indices, and brute force would repeat work.

### B) Count vs Min Quick Test

- Number of routes to reach the last gate → Count
- Minimum battery cost to reach roof → Min
- Minimum total energy for jumps → Min

---

### C) Common Mistakes — Model Answers

- Students typically circle:
- wrong `dp[i]` meaning
- wrong base case
- returned wrong index
- mixed counting and minimization
- Those match the error tendencies your teaching structure tries to surface and correct through pattern-based review.

---

### D) Exit Ticket — Model Answer

- **State:**
- `dp[i]` = answer for index i
- **Transition:**
- `dp[i]` is built from earlier indices that can reach i
- **Base cases:**
- define the smallest valid positions first
- **Answer:**
- final index / final position in the chosen state definition

---
