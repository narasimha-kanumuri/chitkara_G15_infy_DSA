# Ledger Windows with Pulse Check

## Problem Statement

A financial system records the daily net transaction values of a company in an integer array.
A ledger window is any contiguous sequence of days.
Your task is to determine how many ledger windows satisfy both of the following conditions simultaneously:

- The sum of all values inside the window is exactly K.
- The length of the window is divisible by 3.

Return the total number of such ledger windows.

## Input Format

- The first line contains an integer N, the number of transaction records.
- The second line contains N space-separated integers representing the transaction values.
- The third line contains an integer K.

## Output Format

Print a single integer representing the number of valid ledger windows.

## Constraints

- $1 \le N \le 2 \times 10^5$
- $-10^9 \le A_i \le 10^9$
- $-10^{14} \le K \le 10^{14}$

## Sample Input 1

6
2 1 -1 3 0 1
3

## Sample Output 1

3

## Explanation

The valid ledger windows are:

- [2, 1]
- [3]
- [2, 1, -1, 3, 0, 1]
Each window has a total sum of 3, and its length is divisible by 3.

## Sample Input 2

5
1 2 3 4 5
6

## Sample Output 2

1

## Sample Input 3

7
0 0 0 0 0 0 0
0

## Sample Output 3

9

## Explanation

Every contiguous window whose length is a multiple of 3 has a sum of 0.
Count all such windows.

## Notes

- A ledger window must always be contiguous.
- The array may contain positive numbers, negative numbers, and zeros.
- Different windows are considered distinct if their starting or ending positions differ.

## Edge Cases to Consider

- Single-element arrays.
- Arrays containing only zeros.
- Arrays containing only negative numbers.
- No valid ledger window exists.
- Every possible ledger window is valid.
- Very large input sizes requiring efficient processing.
