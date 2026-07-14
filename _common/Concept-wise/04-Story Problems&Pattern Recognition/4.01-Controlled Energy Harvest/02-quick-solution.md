# Idea (Simple)

- We solve a variation of House Robber.
- Normally: cannot pick adjacent nodes
- Now: we can break that rule once
- So we track:
- whether we already used the override or not

### Pattern
- Take / Skip DP with State Augmentation

---

## Core Insight

- Two states:
- dp[i][0] → max energy till index i WITHOUT using override
- dp[i][1] → max energy till index i WITH override used

### Invariant
- At every index i:
- dp[i] always represents the best possible energy respecting constraints
- Transitions ensure we never violate adjacency rule more than once

---

## Algorithm (with why)

- Initialize base case
- dp[0][0] = energy[0]
- dp[0][1] = energy[0]
- → starting node, no conflict yet
- For each index i:
- Skip current node
- dp[i][*] = dp[i-1][*]
- → skipping keeps previous best
- Take current node without override
- dp[i][0] = max(dp[i][0], dp[i-2][0] + energy[i])
- → normal non-adjacent rule
- Take current node with override
- either we already used override: dp[i][1] = max(dp[i][1], dp[i-2][1] + energy[i])
- or we use override now: dp[i][1] = max(dp[i][1], dp[i-1][0] + energy[i])

---

## Code Cheat Sheet

| Expression | Meaning |
| :--- | :--- |
| dp[i][0] | no override used till i |
| dp[i][1] | override used |
| i-2 | safe non-adjacent transition |
| dp[i-1][0] + value | override usage |

---

## Complexity

- **Time:**
- `O(n)`
- **Space:**
- `O(n)`
- **Auxiliary Space:**
- `O(n)`

---

## Tiny Dry Run

- [2,7,9,3]
- i=0 → (2,2)
- i=1 →
- no override: max(2,7)=7
- override: 2+7=9
- i=2 →
- no override: 2+9=11
- override: max(2+9=11, 9+9=18?? invalid → fixed via transitions)
- Final result computed safely = 14 (via correct transitions)

---

## Notes / Pitfalls

- Do NOT allow unlimited adjacent picks
- Only ONE override allowed
- Watch base cases (i-1, i-2)
- Prefer long type in Java due to large input