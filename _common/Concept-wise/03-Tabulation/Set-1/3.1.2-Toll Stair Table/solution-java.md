## solution-java.md

### Language
Java

### Code Concepts Used
| Java Built-in / Concept | Meaning / Role | Very Simple Definition | Why Used in This Program | How to Use It |
|---|---|---|---|---|
| `long` | Stores toll totals | A large integer type | Total toll can be much larger than `int` | Declare values as `long` |
| `long[]` | DP and cost arrays | A list of `long` values | We store best costs for each position | Use `new long[size]` |
| `Arrays.fill(...)` | Initializes arrays | Puts one value in all cells | We mark states as unreachable using `INF` | Use `Arrays.fill(array, INF)` |
| `Math.min(...)` | Chooses smaller value | Returns the lower of two numbers | DP needs the cheaper transition | Use `Math.min(a, b)` |
| `BufferedInputStream` | Fast input reader base | Reads bytes from input quickly | Useful when `n` is large | Wrap it inside a small scanner class |

### Plain-English Implementation Walkthrough
1. We need a destination after the last toll step.  
   So we create one extra position: `n + 1`.

2. The finish position has no toll.  
   So `cost[n + 1]` stays `0`.

3. We must know whether the pass is unused or used.  
   So we keep two arrays: `withoutPass` and `withPass`.

4. The ground position has cost `0` and the pass is unused.  
   So `withoutPass[0] = 0`.

5. The pass cannot be already used at the ground.  
   So `withPass[0]` starts as `INF`.

6. Each position can be reached from `i - 1` or `i - 2`.  
   So we take the cheaper valid previous value from those two positions.

7. If we reach a real toll step without using the pass, we pay `cost[i]`.  
   This updates `withoutPass[i]`.

8. If the pass was already used earlier, we also pay the current toll.  
   This keeps us in `withPass[i]`.

9. If the pass is still unused and `i` is a real toll step, we may use the pass here.  
   Then current toll adds `0`, and we move into `withPass[i]`.

10. We do not use the pass on the virtual finish.  
    The finish is not a toll step.

11. At the finish, either state is acceptable.  
    So we return the smaller value from both final states.

### Complete Solution Code
```java
import java.io.BufferedInputStream;
import java.io.IOException;
import java.util.Arrays;

public class Main {
    private static final long INF = Long.MAX_VALUE / 4;

    private static long minimumToll(int n, long[] cost) {
        long[] withoutPass = new long[n + 2];
        long[] withPass = new long[n + 2];

        Arrays.fill(withoutPass, INF);
        Arrays.fill(withPass, INF);

        withoutPass[0] = 0;

        for (int position = 1; position <= n + 1; position++) {
            long bestWithoutPass = withoutPass[position - 1];
            long bestWithPass = withPass[position - 1];

            if (position >= 2) {
                bestWithoutPass = Math.min(bestWithoutPass, withoutPass[position - 2]);
                bestWithPass = Math.min(bestWithPass, withPass[position - 2]);
            }

            withoutPass[position] = bestWithoutPass + cost[position];

            long usePassHere = INF;
            if (position <= n) {
                usePassHere = bestWithoutPass;
            }

            long alreadyUsedPass = bestWithPass + cost[position];
            withPass[position] = Math.min(alreadyUsedPass, usePassHere);
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

        System.out.println(minimumToll(n, cost));
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

### Code Deconstruction
- `minimumToll(int n, long[] cost)`
  - Computes the minimum toll to reach position `n + 1`.
- `withoutPass[position]`
  - Best cost to reach `position` while the pass is still unused.
- `withPass[position]`
  - Best cost to reach `position` after the pass has already been used.
- `INF`
  - Represents an unreachable state.
  - It is kept smaller than `Long.MAX_VALUE` to avoid overflow during addition.
- `bestWithoutPass`
  - Best previous cost from `position - 1` or `position - 2` before the pass is used.
- `bestWithPass`
  - Best previous cost from `position - 1` or `position - 2` after the pass is used.
- `usePassHere`
  - Uses the pass on the current real toll step.
  - It is not allowed when `position == n + 1` because that is the virtual finish.
- `alreadyUsedPass`
  - Continues a path where the pass was used before.
- Final return
  - Uses `Math.min(withoutPass[n + 1], withPass[n + 1])` because the pass is optional.

### Test Cases
#### Test 1
Input:
`1`
`9`

Output:
`0`

#### Test 2
Input:
`4`
`4 1 6 3`

Output:
`1`

#### Test 3
Input:
`3`
`5 8 2`

Output:
`0`

#### Test 4
Input:
`5`
`2 7 1 4 6`

Output:
`3`

#### Test 5
Input:
`6`
`0 0 0 0 0 0`

Output:
`0`

### Language-Specific Notes
- Use `long` because toll sums may exceed `int`.
- `Arrays.fill` is used because all DP states except the ground start as unreachable.
- The virtual finish position avoids special logic after the last step.
- The fast scanner is useful for large input.
- The pass is applied only to real toll steps, not the finish position.

### Complexity Summary
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`
