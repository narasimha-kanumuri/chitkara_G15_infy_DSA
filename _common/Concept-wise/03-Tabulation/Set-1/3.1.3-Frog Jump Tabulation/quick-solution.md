## quick-solution.md

### Idea (Simple)
We need the minimum energy for a frog to reach the last stone.
The frog may jump to the next stone or skip one stone.
A 2-stone jump has a normal height-difference cost.
But if two 2-stone jumps happen consecutively, we add a fatigue penalty.
So the DP must remember whether the previous jump was a 2-stone jump.

### Pattern Recognition Clues
- We are minimizing total energy, not counting paths.
- Each stone can be reached from smaller stone indices.
- The cost of a 2-stone jump depends on the previous jump.
- This suggests bottom-up DP with a last-jump state.

### State Discovery
The first idea is `dp[i]`: minimum energy to reach stone `i`.
But `dp[i]` is not enough.
It gives the cheapest energy at `i`, but it forgets whether the last jump was length `2`.
That missing information matters because another length-`2` jump may need penalty.
So we split the state:
- `lastNotTwo[i]`: minimum energy to reach stone `i` where the previous jump is not length `2`
- `lastTwo[i]`: minimum energy to reach stone `i` where the previous jump is length `2`

### Pattern
Bottom-Up DP with Last Jump State

### Core Insight
- A 1-stone jump can come after any previous jump.
- A 2-stone jump can come after any previous jump.
- But if a 2-stone jump comes after another 2-stone jump, add penalty `p`.

### Invariant
For every stone `i`:
- `lastNotTwo[i]` stores the best valid cost to reach `i` when the last jump is not `2`.
- `lastTwo[i]` stores the best valid cost to reach `i` when the last jump is `2`.
These two states preserve exactly the information needed to decide if fatigue applies next.

### Algorithm (with one-line why at important steps)
1. Create two arrays: `lastNotTwo` and `lastTwo`.
   - Why: fatigue depends on whether the previous jump was length `2`.
2. Set `lastNotTwo[1] = 0`.
   - Why: the frog starts at stone `1` with no energy spent and no previous long jump.
3. Fill all other states with `INF`.
   - Why: those states are not reachable before we compute them.
4. For each stone `i`, compute a 1-stone jump from `i - 1`.
   - Why: a 1-stone jump can follow either previous state.
5. For each stone `i >= 3`, compute a 2-stone jump from `i - 2`.
   - Why: this is the only source for a 2-stone jump.
6. Add penalty only when the previous state was also `lastTwo[i - 2]`.
   - Why: only consecutive 2-stone jumps cause fatigue.
7. Return `min(lastNotTwo[n], lastTwo[n])`.
   - Why: the final jump may be either short or long.

### Mental Cheat Sheet
| Item | Meaning |
|---|---|
| Pattern | Minimum-cost DP with last-jump state |
| Simple failed state | `dp[i]` forgets the previous jump length |
| Needed memory | whether the last jump was length `2` |
| 1-stone transition | best of both states from `i - 1` plus height difference |
| 2-stone transition | from `lastNotTwo[i - 2]` without penalty, from `lastTwo[i - 2]` with penalty |
| Answer | `min(lastNotTwo[n], lastTwo[n])` |

### Complexity
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`

### Tiny Dry Run
For `height = [1, 100, 2, 100, 3]` and `p = 10`:
- Jump `1 -> 3`: cost `|1 - 2| = 1`
- Jump `3 -> 5`: cost `|2 - 3| = 1`
- These are two consecutive 2-stone jumps, so add penalty `10`
- Total = `1 + 1 + 10 = 12`

### Notes / Pitfalls
- Do not use only one `dp[i]` value; it loses fatigue information.
- Do not add penalty to the first 2-stone jump.
- Add penalty only when a 2-stone jump follows another 2-stone jump.
- Use `long`, because heights, penalty, and total energy can be large.
