# Day 03 — Tabulation (Bottom-Up DP)

## problem.md

### Title

Toll Stair Table

### Concepts Involved

Tabulation, State Tracking, Optimization DP

### Problem Statement

A courier wants to move from the ground to beyond the last step of a toll staircase.

The staircase has `n` toll steps numbered from `1` to `n`.

Each step `i` has a toll `cost[i]` that must be paid only if the courier lands on that step.

From any position, the courier may move either:

- `1` step up, or

- `2` steps up

The courier also owns one optional travel pass.

The pass may be used at most once, and if it is used on a landed step, that step’s toll becomes `0`.

The ground is before step `1`, and the destination is the position just after step `n`.

No toll is paid for the ground or for the final position beyond the last step.

Find the minimum total toll required to finish the climb.

### Input Format

- First line: integer `n`

- Second line: `n` space-separated integers `cost[1], cost[2], ..., cost[n]`

### Output Format

- Print the minimum total toll required to reach beyond the last step

### Constraints

- `1 <= n <= 200000`

- `0 <= cost[i] <= 1000000000`

### Examples

#### Example 1

Input:

`4`

`4 1 6 3`

Output:

`1`

#### Example 2

Input:

`3`

`5 8 2`

Output:

`0`

#### Example 3

Input:

`5`

`2 7 1 4 6`

Output:

`3`

### Edge-Case Thinking Prompts

- What if `n = 1`?

- What if all toll values are `0`?

- Is it always best to use the pass?

- Can the best route skip the most expensive step completely?

### Thinking Like a Programmer

- What information changes while climbing?

- Is step number alone enough to describe the best state?

- How do we represent whether the pass has already been used?

- Why is this a minimum-cost DP and not a counting DP?


