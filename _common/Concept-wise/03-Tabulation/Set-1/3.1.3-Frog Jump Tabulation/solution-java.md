## solution-java.md

### Language
Java

### Code Concepts Used
| Java Built-in / Concept | Meaning / Role | Very Simple Definition | Why Used in This Program | How to Use It |
|---|---|---|---|---|
| `long` | Stores energy values | A large integer type | Height differences, penalties, and total energy may be large | Declare variables and arrays as `long` |
| `long[]` | Stores heights and DP values | An array of large integer values | We need one value per stone for each DP state | Use `new long[n + 1]` |
| `Arrays.fill(...)` | Marks DP cells with `INF` | Puts the same value in every array cell | Uncomputed states should start as unreachable | Use `Arrays.fill(array, INF)` |
| `Math.abs(...)` | Computes jump cost | Returns the positive difference | Jump energy is based on height difference | Use `Math.abs(a - b)` |
| `Math.min(...)` | Chooses the cheaper candidate | Returns the smaller of two values | DP transitions must keep minimum energy | Use `Math.min(x, y)` |
| `BufferedInputStream` | Reads input quickly | Fast byte-based input reader | Useful for large `n` | Use it inside a small scanner class |

### Plain-English Implementation Walkthrough
1. We need to know if the previous jump was a 2-stone jump.  
   So we keep two DP arrays instead of one.

2. `lastNotTwo[i]` means the frog reached stone `i` and the previous jump is not length `2`.  
   This includes the start state at stone `1`.

3. `lastTwo[i]` means the frog reached stone `i` using a 2-stone jump.  
   This state is important because the next 2-stone jump may need penalty.

4. The frog starts at stone `1` with energy `0`.  
   So `lastNotTwo[1] = 0`.

5. All other states start as unreachable.  
   So we fill both arrays with `INF` first.

6. To reach stone `i` by a 1-stone jump, the frog must come from stone `i - 1`.  
   The previous jump can be anything, because a 1-stone jump never creates fatigue.

7. To reach stone `i` by a 2-stone jump, the frog must come from stone `i - 2`.  
   This is why the code checks `if (i >= 3)`.

8. If the previous state is `lastNotTwo[i - 2]`, the current 2-stone jump has no fatigue penalty.  
   The previous jump was not a 2-stone jump.

9. If the previous state is `lastTwo[i - 2]`, the current 2-stone jump adds penalty.  
   This means two long jumps happened consecutively.

10. At the last stone, either ending state is valid.  
    So the method returns the smaller value from `lastNotTwo[n]` and `lastTwo[n]`.

### Complete Solution Code
```java
import java.io.BufferedInputStream;
import java.io.IOException;
import java.util.Arrays;

public class Main {
    private static final long INF = Long.MAX_VALUE / 4;

    private static long minimumEnergy(int n, long[] height, long penalty) {
        if (n == 1) {
            return 0;
        }

        long[] lastNotTwo = new long[n + 1];
        long[] lastTwo = new long[n + 1];

        Arrays.fill(lastNotTwo, INF);
        Arrays.fill(lastTwo, INF);

        lastNotTwo[1] = 0;

        for (int i = 2; i <= n; i++) {
            long oneStepCost = Math.abs(height[i] - height[i - 1]);
            long bestFromPreviousStone = Math.min(lastNotTwo[i - 1], lastTwo[i - 1]);
            lastNotTwo[i] = bestFromPreviousStone + oneStepCost;

            if (i >= 3) {
                long twoStepCost = Math.abs(height[i] - height[i - 2]);
                long fromNotTwo = lastNotTwo[i - 2] + twoStepCost;
                long fromTwo = lastTwo[i - 2] + twoStepCost + penalty;
                lastTwo[i] = Math.min(fromNotTwo, fromTwo);
            }
        }

        return Math.min(lastNotTwo[n], lastTwo[n]);
    }

    public static void main(String[] args) throws Exception {
        FastScanner fs = new FastScanner();

        int n = fs.nextInt();
        long[] height = new long[n + 1];

        for (int i = 1; i <= n; i++) {
            height[i] = fs.nextLong();
        }

        long penalty = fs.nextLong();
        System.out.println(minimumEnergy(n, height, penalty));
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
- `minimumEnergy(int n, long[] height, long penalty)`
  - Computes the minimum energy needed to reach stone `n`.
- `lastNotTwo[i]`
  - Best energy to reach stone `i` when the previous jump is not length `2`.
- `lastTwo[i]`
  - Best energy to reach stone `i` when the previous jump is length `2`.
- `lastNotTwo[1] = 0`
  - The frog starts at stone `1` with no cost.
  - Treating the start as not-a-2-jump prevents the first 2-stone jump from getting penalty.
- 1-stone transition
  - Uses the cheaper of both states from stone `i - 1`.
  - The new ending state becomes `lastNotTwo[i]`.
- 2-stone transition
  - Uses stone `i - 2`.
  - Coming from `lastNotTwo[i - 2]` has no penalty.
  - Coming from `lastTwo[i - 2]` adds penalty.
- Final return
  - The frog may reach the final stone with either a 1-stone or 2-stone jump.

### Test Cases
#### Test 1
Input:
`1`
`7`
`100`

Output:
`0`

#### Test 2
Input:
`5`
`10 30 40 20 50`
`15`

Output:
`40`

#### Test 3
Input:
`4`
`5 100 5 100`
`50`

Output:
`95`

#### Test 4
Input:
`5`
`1 100 2 100 3`
`10`

Output:
`12`

#### Test 5
Input:
`6`
`8 8 8 8 8 8`
`25`

Output:
`0`

### Language-Specific Notes
- Use `long` instead of `int` for safe arithmetic.
- `INF = Long.MAX_VALUE / 4` avoids overflow when adding costs.
- `Arrays.fill` is needed because most DP states start as unreachable.
- The code is iterative, so it avoids recursion depth issues.
- The solution uses 1-based indexing to match stone numbers in the problem.

### Complexity Summary
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`
