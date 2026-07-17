## quick-solution.md

### Idea (Simple)
We need to count valid ways to reach step `n`.
At each move, we can jump `1` step or `2` steps.
The restriction is important: two `2`-step jumps cannot come one after another.
So the solution must remember the last jump used.

### Pattern Recognition Clues
- We are asked to count all valid ways, not find one path.
- The same smaller step counts are reused many times.
- The next valid choice depends on the previous jump.
- This suggests dynamic programming with a last-choice state.

### State Discovery
The first idea is `dp[i]`: number of ways to reach step `i`.
But `dp[i]` is not enough.
It tells how many ways reach `i`, but it does not tell which jump was used last.
The next `2`-step jump depends on whether the previous jump was also `2`.
So we split the state:
- `endWithOne[i]`: ways to reach step `i` ending with a `1`-step jump
- `endWithTwo[i]`: ways to reach step `i` ending with a `2`-step jump

### Pattern
Dynamic Programming with Last Choice State

### Core Insight
- A final `1`-step jump can follow any valid previous path.
- A final `2`-step jump can follow only a path that did not end with `2`.
- The answer is the sum of both valid ending states at step `n`.

### Invariant
For every step `i`:
- `endWithOne[i]` stores all valid ways to reach `i` with last jump `1`.
- `endWithTwo[i]` stores all valid ways to reach `i` with last jump `2`.
No path with two consecutive `2`-step jumps is counted.

### Algorithm (with one-line why at important steps)
1. Create two arrays: `endWithOne` and `endWithTwo`.
   - Why: the previous jump affects the next valid jump.
2. Set `endWithOne[1] = 1`.
   - Why: step `1` can only be reached by one `1`-step jump.
3. If `n >= 2`, set `endWithTwo[2] = 1`.
   - Why: step `2` can be reached directly by one `2`-step jump.
4. For each step `i` from `2` to `n`, compute `endWithOne[i]`.
   - Why: a final `1` can come after any valid path to `i - 1`.
5. For each step `i >= 3`, compute `endWithTwo[i]` from `endWithOne[i - 2]`.
   - Why: a final `2` cannot come after another final `2`.
6. Return `endWithOne[n] + endWithTwo[n]`.
   - Why: every valid path ends with either a `1`-step or `2`-step jump.

### Mental Cheat Sheet
| Item | Meaning |
|---|---|
| Pattern | DP with last choice state |
| Simple failed state | `dp[i]` forgets the last jump |
| Needed memory | whether the previous jump was `1` or `2` |
| Transition for `1` | `endWithOne[i] = endWithOne[i-1] + endWithTwo[i-1]` |
| Transition for `2` | `endWithTwo[i] = endWithOne[i-2]` |
| Answer | `endWithOne[n] + endWithTwo[n]` |

### Complexity
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`

### Tiny Dry Run
For `n = 4`:
- Step `1`: `endWithOne = 1`, `endWithTwo = 0`
- Step `2`: `endWithOne = 1`, `endWithTwo = 1`
- Step `3`: `endWithOne = 2`, `endWithTwo = 1`
- Step `4`: `endWithOne = 3`, `endWithTwo = 1`
- Answer = `3 + 1 = 4`

### Notes / Pitfalls
- Do not use only `dp[i]`; it loses the last-jump information.
- Do not compute `endWithTwo[i]` from `endWithTwo[i - 2]`.
- The first direct `2`-step jump is valid.
- Use `BigInteger` when exact large counts are required.
