## solution-java.md

### Language

Java

### Complete Solution Code

```java

import java.io.BufferedInputStream;

import java.io.IOException;

import java.util.Arrays;

public class Main {

private static final long INF = Long.MAX_VALUE / 4;

private static long minToll(int n, long[] cost) {

long[] withoutPass = new long[n + 2];

long[] withPass = new long[n + 2];

Arrays.fill(withoutPass, INF);

Arrays.fill(withPass, INF);

withoutPass[0] = 0;

for (int i = 1; i <= n + 1; i++) {

long prevWithoutPass = withoutPass[i - 1];

if (i >= 2) {

prevWithoutPass = Math.min(prevWithoutPass, withoutPass[i - 2]);

}

long prevWithPass = withPass[i - 1];

if (i >= 2) {

prevWithPass = Math.min(prevWithPass, withPass[i - 2]);

}

withoutPass[i] = prevWithoutPass + cost[i];

withPass[i] = Math.min(prevWithPass + cost[i], prevWithoutPass);

}

return Math.min(withoutPass[n + 1], withPass[n + 1]);

}

public static void main(String[] args) throws Exception {

FastScanner fs = new FastScanner();

int n = fs.nextInt();

long[] cost = new long[n + 2];

for (int i = 1; i <= n; i++) {

cost[i] = fs.nextLong();

}

cost[n + 1] = 0; // virtual finish position

System.out.println(minToll(n, cost));

}

private static class FastScanner {

private final BufferedInputStream in = new BufferedInputStream(System.in);

private final byte[] buffer = new byte[1 << 16];

private int ptr = 0;

private int len = 0;

private int read() throws IOException {

if (ptr >= len) {

len = in.read(buffer);

ptr = 0;

if (len <= 0) {

return -1;

}

}

return buffer[ptr++];

}

int nextInt() throws IOException {

return (int) nextLong();

}

long nextLong() throws IOException {

int c;

do {

c = read();

} while (c <= ' ');

long sign = 1;

if (c == '-') {

sign = -1;

c = read();

}

long value = 0;

while (c > ' ') {

value = value * 10 + (c - '0');

c = read();

}

return value * sign;

}

}

}
```


## Code Deconstruction

- `minToll(int n, long[] cost)` is the core DP function.
  - It returns the minimum payable toll to reach beyond the last step.
- `withoutPass[i]`
  - Best cost to reach position `i` while the pass is still unused.
- `withPass[i]`
  - Best cost to reach position `i` after the pass has already been used.
- `cost[n + 1] = 0`
  - Creates a virtual destination so that finishing can be handled like a normal DP step.
- `prevWithoutPass`
  - Best previous value from `i - 1` or `i - 2` when the pass is still unused.
- `prevWithPass`
  - Best previous value from `i - 1` or `i - 2` when the pass has already been used.
- `withoutPass[i] = prevWithoutPass + cost[i]`
  - If the pass is still unused after reaching `i`, we must pay the toll at `i`.
- `withPass[i] = Math.min(prevWithPass + cost[i], prevWithoutPass)`
  - Two ways to end with the pass already used:
    - it was used earlier, so we pay this toll now
    - we use the pass on this current step, so this step costs `0`
- Final answer:
  - `Math.min(withoutPass[n + 1], withPass[n + 1])`
  - The pass is optional, so either final state may be cheaper.

### Built-ins / Syntax Reference Table

| Java syntax / built-in | Meaning / Role |
| --- | --- |

`Arrays.fill(arr, INF)`initializes DP arrays with a large value`Math.min(a, b)`keeps the cheaper of two candidate costs`BufferedInputStream`fast input reading for large constraints`long`prevents overflow for large total toll values

### Test Cases

#### Test 1

Input: `1` `9`

Output: `0`

#### Test 2

Input: `4` `4 1 6 3`

Output: `1`

#### Test 3

Input: `3` `5 8 2`

Output: `0`

#### Test 4

Input: `5` `2 7 1 4 6`

Output: `3`

#### Test 5

Input: `6` `0 0 0 0 0 0`

Output: `0`

### Language-Specific Notes

- `long` is safer than `int` because costs can sum to very large values.
- A fast scanner is used to handle large input sizes comfortably.
- The solution is iterative and avoids recursion depth issues.
- The code uses a virtual finish position to simplify the final answer logic.

### Complexity Summary

- Time: `O(n)`
- Space: `O(n)`
- Auxiliary Space: `O(n)`
