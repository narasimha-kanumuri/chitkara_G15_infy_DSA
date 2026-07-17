## quick-solution.md

### Idea (Simple)
We need the minimum toll to cross a staircase.
The courier may move by `1` step or `2` steps.
One landed toll step can be made free once.
So the DP must remember whether the free-step pass is still unused or already used.

### Pattern Recognition Clues
- We are minimizing cost, not counting paths.
- The answer for a step depends on earlier reachable steps.
- The coupon/free-step decision changes future states.
- This suggests bottom-up DP with a coupon-used state.

### State Discovery
The first idea is `dp[i]`: minimum cost to reach step `i`.
But `dp[i]` is not enough.
It does not tell whether the free-step pass has already been used.
That missing information affects later choices.
So we split the state:
- `withoutPass[i]`: minimum cost to reach position `i` with the pass still unused
- `withPass[i]`: minimum cost to reach position `i` after the pass has been used

### Pattern
Bottom-Up DP with One-Time Coupon State

### Core Insight
- From any position, we may come from `i - 1` or `i - 2`.
- If the pass is unused, landing on a toll step normally adds `cost[i]`.
- If we use the pass on the current toll step, we add `0` for that step and move into the used-pass state.

### Invariant
For every position `i`:
- `withoutPass[i]` is the cheapest cost to reach `i` without using the pass.
- `withPass[i]` is the cheapest cost to reach `i` after using the pass.
Both values are always the best known costs after considering all valid previous moves.

### Algorithm (with one-line why at important steps)
1. Add a virtual finish position `n + 1` with cost `0`.
   - Why: crossing beyond the last step becomes a normal DP target.
2. Set `withoutPass[0] = 0` and `withPass[0] = INF`.
   - Why: at the ground, no toll is paid and the pass has not been used.
3. For each position `i` from `1` to `n + 1`, take the cheaper previous value from `i - 1` and `i - 2`.
   - Why: those are the only allowed jump lengths.
4. Update `withoutPass[i]` by paying `cost[i]`.
   - Why: the pass remains unused only if we do not use it here.
5. Update `withPass[i]` using two choices: the pass was used earlier, or it is used on this toll step now.
   - Why: these are the only ways to enter or remain in the used-pass state.
6. Return `min(withoutPass[n + 1], withPass[n + 1])`.
   - Why: using the pass is optional, so either final state may be best.

### Mental Cheat Sheet
| Item | Meaning |
|---|---|
| Pattern | Min-cost bottom-up DP with one-time coupon state |
| Simple failed state | `dp[i]` forgets whether the pass was used |
| State | position + pass status |
| Transition source | best of `i - 1` and `i - 2` |
| Virtual finish | `n + 1` with toll `0` |
| Answer | `min(withoutPass[n + 1], withPass[n + 1])` |

### Complexity
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`

### Tiny Dry Run
For `cost = [4, 1, 6, 3]`:
- Step `1`: pay `4`, or use pass and pay `0`
- Step `2`: cheapest without pass is `1`; cheapest with pass is `0`
- Continue using best of previous one-step and two-step positions
- Final virtual finish has cost `0`
- Minimum total toll is `1`

### Notes / Pitfalls
- Do not use one DP array; coupon status matters.
- Do not use the coupon on the virtual finish position.
- The pass is optional, so compare both final states.
- Use `long`, because total toll can exceed `int` range.
