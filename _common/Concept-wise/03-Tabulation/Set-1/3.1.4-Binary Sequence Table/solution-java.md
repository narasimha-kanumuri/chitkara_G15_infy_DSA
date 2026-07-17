## solution-java.md

### Language
Java

### Code Concepts Used
| Java Built-in / Concept | Meaning / Role | Very Simple Definition | Why Used in This Program | How to Use It |
|---|---|---|---|---|
| `BigInteger` | Stores exact large counts | A number type that can grow beyond `long` | The count of valid strings grows quickly | Use constants like `BigInteger.ZERO`, and add using `.add(...)` |
| `BigInteger[]` | DP arrays for large counts | An array whose cells store `BigInteger` values | We store counts for each length and ending bit | Declare as `BigInteger[] arr = new BigInteger[n + 1]` |
| `.add(...)` | Adds two `BigInteger` values | Method form of addition | Java does not allow `+` for `BigInteger` addition | Use `a.add(b)` |
| `BufferedReader` | Reads input text | Simple input reader | The input has one integer `n` | Create it with `new InputStreamReader(System.in)` |
| `Integer.parseInt(...)` | Converts text to `int` | Turns a numeric string into an integer | We need `n` as an integer for array size and loops | Use `Integer.parseInt(line.trim())` |

### Plain-English Implementation Walkthrough
1. We need exact counts, and the count can become very large.  
   So we use `BigInteger` instead of `int` or `long`.

2. We need to know the last bit of each counted string.  
   So we create two arrays: `endWithZero` and `endWithOne`.

3. Java object arrays start with `null`.  
   So every cell is filled with `BigInteger.ZERO` before we use it.

4. For length `1`, the valid strings are `0` and `1`.  
   So `endWithZero[1] = 1` and `endWithOne[1] = 1`.

5. To build strings ending with `0`, we can append `0` after any valid previous string.  
   So we add both previous states.

6. To build strings ending with `1`, we can append `1` only after a previous string ending with `0`.  
   This prevents consecutive `1`s.

7. The problem asks for strings ending with `0`.  
   So the method returns only `endWithZero[n]`.

8. The `main` method reads `n`, calls the counting method, and prints the result.  
   This keeps input/output separate from the DP logic.

### Complete Solution Code
```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.math.BigInteger;

public class Main {
    private static BigInteger countValidSequences(int n) {
        BigInteger[] endWithZero = new BigInteger[n + 1];
        BigInteger[] endWithOne = new BigInteger[n + 1];

        for (int i = 0; i <= n; i++) {
            endWithZero[i] = BigInteger.ZERO;
            endWithOne[i] = BigInteger.ZERO;
        }

        endWithZero[1] = BigInteger.ONE;
        endWithOne[1] = BigInteger.ONE;

        for (int length = 2; length <= n; length++) {
            endWithZero[length] = endWithZero[length - 1].add(endWithOne[length - 1]);
            endWithOne[length] = endWithZero[length - 1];
        }

        return endWithZero[n];
    }

    public static void main(String[] args) throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(br.readLine().trim());
        System.out.println(countValidSequences(n));
    }
}
```

### Code Deconstruction
- `countValidSequences(int n)`
  - Computes the number of valid binary strings of length `n` that end with `0`.
- `endWithZero[length]`
  - Stores valid strings of the current length whose last bit is `0`.
- `endWithOne[length]`
  - Stores valid strings of the current length whose last bit is `1`.
- Base cases
  - `endWithZero[1] = 1` represents the string `0`.
  - `endWithOne[1] = 1` represents the string `1`.
- `endWithZero[length]` update
  - Adds both previous states because `0` can follow both `0` and `1`.
- `endWithOne[length]` update
  - Uses only `endWithZero[length - 1]` because `1` cannot follow `1`.
- Final return
  - Returns `endWithZero[n]` because the final bit must be `0`.

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
`5`

#### Test 5
Input:
`5`

Output:
`8`

#### Test 6
Input:
`6`

Output:
`13`

### Language-Specific Notes
- Use `BigInteger` because exact counts may exceed primitive number limits.
- Use `.add(...)` for `BigInteger` addition; `+` does not work for this type.
- Initialize every `BigInteger[]` cell before calling methods on it.
- The solution uses 1-based length indexing, which matches the explanation.
- No recursion is used, so there is no stack-depth risk.

### Complexity Summary
- Time Complexity: `O(n)`
- Space Complexity: `O(n)`
- Auxiliary Space: `O(n)`
