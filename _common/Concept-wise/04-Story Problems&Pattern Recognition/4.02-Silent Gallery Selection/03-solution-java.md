## Language
Java

## Complete Solution Code
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    private static long maxImpact(long[] impact) {
        int n = impact.length;
        if (n == 0) {
            return 0L;
        }

        long[] dp = new long[n];
        dp[0] = Math.max(0L, impact[0]);

        if (n == 1) {
            return dp[0];
        }

        dp[1] = Math.max(dp[0], Math.max(0L, impact[1]));

        for (int i = 2; i < n; i++) {
            long skip = dp[i - 1];
            long take = dp[i - 2] + impact[i];
            dp[i] = Math.max(skip, Math.max(take, 0L));
        }

        return dp[n - 1];
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine().trim());
        long[] impact = new long[n];

        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < n; i++) {
            impact[i] = Long.parseLong(st.nextToken());
        }

        System.out.println(maxImpact(impact));
    }
}
```

## Code Deconstruction
- `maxImpact(long[] impact)`
  - Computes the maximum valid non-adjacent total.
- `dp[i]`
  - Stores the best answer for prefix `0...i`.
- `dp[0] = max(0, impact[0])`
  - Handles the first room and preserves the option to skip all rooms.
- `dp[1] = max(dp[0], impact[1], 0)`
  - For two adjacent rooms, we can open at most one of them.
- `skip = dp[i - 1]`
  - Current room is ignored.
- `take = dp[i - 2] + impact[i]`
  - Current room is opened, so previous room must be skipped.
- Final return `dp[n - 1]`
  - Gives the best valid answer for the full corridor.

## Built-ins / Syntax Reference Table
| Syntax | Meaning |
|---|---|
| `Math.max(a, b)` | returns larger of two values |
| `BufferedReader` | fast input reading |
| `StringTokenizer` | splits array input efficiently |
| `long` | safe 64-bit integer storage |

## Test Cases
### Test 1 — Standard case
Input:
```text
5
4 2 7 3 9
```
Output:
```text
20
```

### Test 2 — Contains negatives
Input:
```text
4
5 -2 1 6
```
Output:
```text
11
```

### Test 3 — All negative
Input:
```text
3
-8 -3 -10
```
Output:
```text
0
```

### Test 4 — Single room
Input:
```text
1
12
```
Output:
```text
12
```

### Test 5 — Single negative room
Input:
```text
1
-5
```
Output:
```text
0
```

## Language-Specific Notes
- `long` is better than `int` because the sum of many large impacts may overflow `int`.
- `StringTokenizer` keeps input parsing compact and fast for competitive-style input.
- This implementation uses `O(n)` DP for clarity; it can be reduced to `O(1)` extra space later.

## Complexity Summary
- Time: `O(n)`
- Space: `O(n)`
- Auxiliary Space: `O(n)`
