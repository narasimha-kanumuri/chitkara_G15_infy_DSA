# Day 1 — Question 1
# Badge Recovery Report

## Difficulty
Easy

## Topics
- Arrays
- Traversal
- Counting
- Frequency Analysis

---

## Problem Statement

An organization assigns every employee a unique badge ID from **1** to **n**.

Due to a synchronization error:

- Exactly one badge ID appears **twice**.
- Exactly one badge ID is **missing**.

You are given an array `badges` of length `n` containing the recorded badge IDs.

Return:

1. the duplicated badge ID
2. the missing badge ID

---

## Input Format

The first line contains an integer `n`.

The second line contains `n` space-separated integers representing the badge IDs.

---

## Output Format

Print two integers separated by a space:

duplicate missing

---

## Constraints

- 2 ≤ n ≤ 200000
- 1 ≤ badges[i] ≤ n
- Exactly one value is duplicated.
- Exactly one value is missing.

---

## Example 1

### Input

5

1 2 2 4 5

### Output

2 3

### Explanation

Badge ID **2** appears twice.

Badge ID **3** does not appear.

---

## Example 2

### Input

4

1 1 3 4

### Output

1 2

---

## Example 3

### Input

6

1 2 3 4 6 6

### Output

6 5

---

## Notes

- The duplicate always appears exactly twice.
- The missing value always belongs to the range **1...n**.
- The order of output must be:

duplicate missing

---

## Edge Cases

- Duplicate is the smallest badge ID.
- Duplicate is the largest badge ID.
- Missing value is 1.
- Missing value is n.
- Minimum valid input size.
- Maximum constraint values.

---

## Sample Visualization

- **Original IDs**

1 2 3 4 5

- **Recorded IDs**

1 2 2 4 5

- **Result**

- **Duplicate:** 2
- **Missing:** 3