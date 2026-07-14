## quick-solution.md

### Idea (Simple)

We want the cheapest way to reach beyond the last toll step.

The twist is that one landed step can become free exactly once.

So the state must remember both:

- where we are, and

- whether the pass has already been used.

### Pattern

Bottom-Up DP with two states per position

### Core Insight

- For every step, track the cheapest cost in two cases:

- pass not used yet

- pass already used

- When landing on a step, either pay its toll or use the pass there.

- The answer is built from smaller previous steps using `1`-step or `2`-step transitions.

### Invariant

At every position `i`:

- `withoutPass[i]` = minimum toll to reach `i` without using the pass yet

- `withPass[i]` = minimum toll to reach `i` after the pass has been used

These values always represent the best possible costs for their respective states.

### Algorithm (with one-line why at important steps)

1. Create a cost array for steps `1..n`, and add one extra virtual step `n + 1` with toll `0`.

- Why: reaching beyond the last step becomes a normal DP destination.

1. Let `withoutPass[0] = 0` and `withPass[0] = INF`.

- Why: we start on the ground with zero cost and the pass unused.

1. For each position `i` from `1` to `n + 1`, find the best previous cost from `i - 1` or `i - 2`.

- Why: only `1`-step and `2`-step moves are allowed.

1. Compute `withoutPass[i] = bestPreviousWithoutPass + cost[i]`.

- Why: if the pass is still unused, we must pay this landed toll.

1. Compute `withPass[i]` as the minimum of:

- coming from an already-used-pass state and paying `cost[i]`

- coming from a not-yet-used-pass state and using the pass on this step

- Why: these are the only two valid ways to arrive after the pass has been used.

1. Return `min(withoutPass[n + 1], withPass[n + 1])`.

- Why: the pass is optional, so the final answer may come from either state.

### Code Cheat Sheet

| Code / Built-in | Meaning / Role |

|---|---|

| `withoutPass[i]` | cheapest cost to reach `i` with pass still unused |

| `withPass[i]` | cheapest cost to reach `i` after pass has been used |

| `INF` | very large value for impossible / not-yet-valid states |

| `bestPrevious` | cheaper of the two allowed previous positions |

### Complexity

- Time: `O(n)`

- Space: `O(n)`

- Auxiliary Space: `O(n)`

### Tiny Dry Run

For `n = 4`, `cost = [4, 1, 6, 3]`:

- reach step `1`: pay `4`, or use pass there → `0`

- reach step `2`: pay `1`, or use pass there → `0`

- keep extending with best `1`-step / `2`-step transitions

- final answer becomes `1`

### Notes / Pitfalls

- Do not use one single DP array; the pass status matters.

- The destination beyond the staircase has toll `0`.

- The pass is optional, not mandatory.

- Use `long`, not `int`, because total toll can become large.

