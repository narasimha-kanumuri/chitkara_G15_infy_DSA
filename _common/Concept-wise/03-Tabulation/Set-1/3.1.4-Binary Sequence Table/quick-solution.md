## quick-solution.md

### Idea (Simple)
We need to count binary strings of length `n`.
The string must not contain consecutive `1`s.
Also, the final bit must be `0`.
So we count strings by their last bit, because the next valid bit depends on the previous bit.

### Pattern Recognition Clues
- We are asked to count all valid sequences.
- The same smaller-length answers help build larger-length answers.
- Placing `1` depends on what the previous bit was.
- This suggests bottom-up DP with a last-bit state.

### State Discovery
The first idea is `dp[i]`: number of valid binary strings of length `i`.
But `dp[i]` is not enough.
It tells how many valid strings exist, but it does not tell whether they end with `0` or `1`.
That missing last-bit information decides whether we can append `1` next.
So we split the state:
- `endWithZero[i]`: valid strings of length `i` ending with `0`
- `endWithOne[i]`: valid strings of length `i` ending with `1`

### Pattern
Bottom-Up DP with Last Bit State

### Core Insight
- A `0` can be placed after both `0` and `1`.
- A `1` can be placed only after `0`.
- Since the final bit must be `0`, the answer is `endWithZero[n]`.

### Invariant
For every length `i`:
- `endWithZero[i]` stores all valid strings of length `i` ending in `0`.
- `endWithOne[i]` stores all valid strings of length `i` ending in `1`.
No counted string contains consecutive `1`s.

### Algorithm (with one-line why at important steps)
1. Create two arrays: `endWithZero` and `endWithOne`.
   - Why: the next valid bit depends on the previous bit.
2. Set `endWithZero[1] = 1` and `endWithOne[1] = 1`.
   - Why: both one-bit strings `0` and `1` are valid.
3. For each length `i` from `2` to `n`, compute `endWithZero[i]` from both previous states.
   - Why: `0` can be appended after any valid previous ending.
4. Compute `endWithOne[i]` only from `endWithZero[i - 1]`.
   - Why: `1` cannot be appended after another `1`.
5. Return `endWithZero[n]`.
   - Why: the problem requires the sequence to end with `0`.

### Mental Cheat Sheet
| Item | Meaning |
|---|---|
| Pattern | Counting DP with last-bit state |
| Simple failed state | `dp[i]` forgets whether the string ends with `0` or `1` |
| Needed memory | last bit of the current string |
| `0` transition | `endWithZero[i] = endWithZero[i-1] + endWithOne[i-1]` |
| `1` transition | `endWithOne[i] = endWithZero[i-1]` |
| Answer | `endWithZero[n]` |

### Complexity
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`

### Tiny Dry Run
For `n = 4`:
- Length `1`: zero-ending = `1`, one-ending = `1`
- Length `2`: zero-ending = `2`, one-ending = `1`
- Length `3`: zero-ending = `3`, one-ending = `2`
- Length `4`: zero-ending = `5`, one-ending = `3`
- Answer = `5`

### Notes / Pitfalls
- Do not return the total of both states; the string must end with `0`.
- Do not allow `endWithOne[i]` to come from `endWithOne[i - 1]`.
- Use `BigInteger` when exact large counts are required.
- Define base cases before writing transitions.
