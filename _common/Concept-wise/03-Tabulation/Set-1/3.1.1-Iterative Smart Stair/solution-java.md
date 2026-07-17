## solution-java.md

### Language
Java

### Code Concepts Used
| Java Built-in / Concept | Meaning / Role | Very Simple Definition | Why Used in This Program | How to Use It |
|---|---|---|---|---|
| `BigInteger` | Stores very large exact numbers | A number type that can grow beyond `long` | The number of valid ways can become very large | Create constants like `BigInteger.ZERO`, add using `.add(...)` |
| `BigInteger[]` | DP arrays for large counts | An array where every cell stores a `BigInteger` | We need one count per step and per ending jump type | Declare as `BigInteger[] arr = new BigInteger[n + 1]` |
| `.add(...)` | Adds two `BigInteger` values | Method form of addition | Java does not allow `+` for `BigInteger` math | Use `a.add(b)` |
| `BufferedReader` | Reads input | Fast text input reader | Input has one integer, and this is simple and reliable | Wrap `System.in` with `InputStreamReader` |
| `Integer.parseInt(...)` | Converts text to integer | Turns a numeric string into `int` | The input `n` is read as text first | Use `Integer.parseInt(text.trim())` |

### Plain-English Implementation Walkthrough
1. We need exact counts, and counts can grow large.  
   So we use `BigInteger` instead of `int` or `long`.

2. We need to remember how the last jump reached each step.  
   So we create two arrays: `endWithOne` and `endWithTwo`.

3. Java object arrays start with `null` values.  
   So we fill every cell with `BigInteger.ZERO` before using `.add(...)`.

4. Step `1` has one valid way ending with a `1`-step jump.  
   So we set `endWithOne[1] = BigInteger.ONE`.

5. Step `2` can be reached directly by one `2`-step jump.  
   So when `n >= 2`, we set `endWithTwo[2] = BigInteger.ONE`.

6. For a final `1`-step jump, the previous path may end with `1` or `2`.  
   So `endWithOne[i]` adds both states from step `i - 1`.

7. For a final `2`-step jump, the previous path must not end with `2`.  
   So `endWithTwo[i]` only uses `endWithOne[i - 2]`.

8. The final valid path may end with either jump size.  
   So the method returns `endWithOne[n].add(endWithTwo[n])`.

### Complete Solution Code
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.math.BigInteger;

public class Main {
    private static BigInteger countWays(int n) {
        BigInteger[] endWithOne = new BigInteger[n + 1];
        BigInteger[] endWithTwo = new BigInteger[n + 1];

        for (int i = 0; i <= n; i++) {
            endWithOne[i] = BigInteger.ZERO;
            endWithTwo[i] = BigInteger.ZERO;
        }

        endWithOne[1] = BigInteger.ONE;

        if (n >= 2) {
            endWithTwo[2] = BigInteger.ONE;
        }

        for (int step = 2; step <= n; step++) {
            endWithOne[step] = endWithOne[step - 1].add(endWithTwo[step - 1]);

            if (step >= 3) {
                endWithTwo[step] = endWithOne[step - 2];
            }
        }

        return endWithOne[n].add(endWithTwo[n]);
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine().trim());
        System.out.println(countWays(n));
    }
}
```

### Code Deconstruction
- `countWays(int n)`
  - Computes the number of valid jump sequences for step `n`.
- `endWithOne[step]`
  - Stores valid ways that reach `step` with a final `1`-step jump.
- `endWithTwo[step]`
  - Stores valid ways that reach `step` with a final `2`-step jump.
- Base cases
  - `endWithOne[1] = 1` represents the sequence `[1]`.
  - `endWithTwo[2] = 1` represents the sequence `[2]`.
- Main transition
  - `endWithOne[step]` comes from both previous ending states.
  - `endWithTwo[step]` comes only from `endWithOne[step - 2]`.
- Return statement
  - Adds both ending states because either final jump is valid.

### Test Cases
#### Test 1
Input:
`1`

Output:
`1`

#### Test 2
Input:
`2`

Output:
`2`

#### Test 3
Input:
`3`

Output:
`3`

#### Test 4
Input:
`4`

Output:
`4`

#### Test 5
Input:
`5`

Output:
`6`

#### Test 6
Input:
`6`

Output:
`9`

### Language-Specific Notes
- `BigInteger` values must be added with `.add(...)`, not with `+`.
- Every `BigInteger[]` cell must be initialized before use.
- The solution is iterative, so it does not risk recursion depth issues.
- The code uses `1`-based step meaning to match the problem statement.
- The constraints allow an `O(n)` table comfortably.

### Complexity Summary
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`
