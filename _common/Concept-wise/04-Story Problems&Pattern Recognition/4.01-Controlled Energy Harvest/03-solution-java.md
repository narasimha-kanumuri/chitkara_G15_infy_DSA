# Language

- Java

### Complete Solution Code

```java
class Solution {
    public int maxEnergy(int[] energy) {
        int n = energy.length;
        if (n == 0) return 0;
        if (n == 1) return energy[0];

        long[][] dp = new long[n][2];

        // Base case
        dp[0][0] = energy[0];
        dp[0][1] = energy[0];

        for (int i = 1; i < n; i++) {
            // Skip case
            dp[i][0] = dp[i-1][0];
            dp[i][1] = dp[i-1][1];

            // Take without override
            long takeNoOverride = energy[i];
            if (i > 1) takeNoOverride += dp[i-2][0];
            dp[i][0] = Math.max(dp[i][0], takeNoOverride);

            // Take with override
            long takeWithOverride = Long.MIN_VALUE;

            // Case 1: override was already used before
            if (i > 1) {
                takeWithOverride = dp[i-2][1] + energy[i];
            } else {
                takeWithOverride = energy[i];
            }

            // Case 2: use override NOW (adjacent allowed once)
            long useOverrideNow = dp[i-1][0] + energy[i];

            dp[i][1] = Math.max(dp[i][1],
                        Math.max(takeWithOverride, useOverrideNow));
        }

        return (int)Math.max(dp[n-1][0], dp[n-1][1]);
    }
}
```

## Code Deconstruction

- **dp[i][0]**
- best answer without using override
- **dp[i][1]**
- best answer after using override
- **Skip logic:**
- dp[i][] = dp[i-1][]; → keeps previous result
- **Non-adjacent take:**
- dp[i-2] + energy[i] → valid pick without conflict
- **Override logic:**
- dp[i-1][0] + energy[i] → allows adjacent pick once

### Built-ins / Syntax Reference Table

| Syntax | Meaning |
| :--- | :--- |
| Math.max(a,b) | max of two values |
| long[][] | avoids overflow |
| dp[i][j] | DP state indexing |

---

### Test Cases

- **Normal Case**
- Input: [2,7,9,3,1]
- Output: 14
- **Edge Case**
- Input: [5]
- Output: 5
- **Override Useful**
- Input: [10, 20]
- Output: 30
- **Large Case**
- Input: [100000, 1, 100000]
- Output: 200000

---

## Language-Specific Notes

- Use long internally due to large sums
- Convert final result to int
- Java handles arrays efficiently for DP

### Complexity Summary

- **Time:**
- `O(n)`
- **Space:**
- `O(n)`
- **Auxiliary:**
- `O(n)`
