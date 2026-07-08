# Day 1 — Question 2

# Emergency Power Segment

## Difficulty

🟡 Medium

## Problem Statement

A backup power station records the amount of energy generated every minute.
You are given an array power[] where power[i] represents the energy generated during the iᵗʰ minute.
Your task is to find the length of the shortest contiguous time segment whose:

- total generated energy is at least T, and
- the segment contains at least two even-valued readings.
If no such segment exists, return -1.

## Input Format

The input consists of:

- An integer N — number of readings.
- An array power of length N.
- An integer T.

## Output Format

Print a single integer representing the length of the shortest valid contiguous segment.
If no valid segment exists, print -1.

## Constraints

- 1 ≤ N ≤ 2 × 10^5
- 0 ≤ power[i] ≤ 10^6
- 1 ≤ T ≤ 10^12

## Sample Input 1

6
2 1 3 4 2 5
8

## Sample Output 1

3

## Explanation

One valid shortest segment has length 3.
Its total energy is at least 8, and it contains at least two even numbers.

## Sample Input 2

5
1 3 5 7 9
10

## Sample Output 2

-1

## Explanation

No contiguous segment contains two even readings.

## Sample Input 3

7
2 2 1 1 6 1 3
6

## Sample Output 3

2

## Sample Input 4

4
4 1 2 1
20

## Sample Output 4

-1

## Notes

- The chosen segment must be contiguous.
- A segment satisfies the requirement only if both conditions are true:
- Total energy ≥ T
- Number of even readings ≥ 2
- Return only the minimum possible segment length.

## Edge Cases to Consider

- Array contains no even numbers.
- Exactly two even numbers exist.
- Single-element arrays.
- Target cannot be reached.
- Multiple shortest valid segments.
- All values are zero except one.
- Very large values of T.
- Entire array is the only valid answer.

## Expected Function Signature

- **C++**

```cpp
int shortestEmergencySegment(vector<int>& power, long long T);
```
