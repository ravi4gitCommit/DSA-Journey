# LeetCode 724 — Find Pivot Index

Pattern: Prefix Sum

Question: Find the pivot index where the sum of all elements on the left equals the sum of all elements on the right.

- Pivot index खुद left या right sum में include नहीं होगा।
- अगर multiple pivot indices हैं, तो सबसे left वाला return करो।
- अगर कोई pivot नहीं है, तो `-1` return करो।

Example:

- Input: `nums = [1,7,3,6,5,6]`
- Output: `3`

क्योंकि index `3` पर value `6` है:

- Left Sum = `1 + 7 + 3 = 11`
- Right Sum = `5 + 6 = 11`

## 1. Brute Force — Nested Loops

Logic: हर index के लिए अलग-अलग left sum और right sum calculate करो।

```
class Solution {
    public int pivotIndex(int[] nums) {

        for (int i = 0; i < nums.length; i++) {
            int leftSum = 0;
            int rightSum = 0;

            // Left side sum
            for (int j = 0; j < i; j++) {
                leftSum += nums[j];
            }

            // Right side sum
            for (int j = i + 1; j < nums.length; j++) {
                rightSum += nums[j];
            }

            if (leftSum == rightSum) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(1)\\)

## 2. Better — Prefix Sum Array

Logic:

1. एक prefix sum array बनाओ।
2. Total sum निकालो।
3. हर index पर:
   - Left Sum = `prefix[i - 1]`
   - Right Sum = `totalSum - prefix[i]`
4. दोनों बराबर हों तो index return करो।

```
class Solution {
    public int pivotIndex(int[] nums) {
        int n = nums.length;

        int[] prefix = new int[n];
        prefix[0] = nums[0];

        // Prefix sum calculate karo
        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }

        int totalSum = prefix[n - 1];

        for (int i = 0; i < n; i++) {
            int leftSum = (i == 0) ? 0 : prefix[i - 1];
            int rightSum = totalSum - prefix[i];

            if (leftSum == rightSum) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(n)\\)

## 3. Optimal — Single Pass + Total Sum

Logic:

1. पहले पूरे array का `totalSum` निकालो।
2. `leftSum = 0` रखो।
3. हर index पर right sum निकालो:

\\[ rightSum = totalSum-leftSum-nums[i] \\]

4. अगर `leftSum == rightSum`, तो index return करो।
5. नहीं तो current element को `leftSum` में add करो।

```
class Solution {
    public int pivotIndex(int[] nums) {

        int totalSum = 0;

        // Total sum calculate karo
        for (int num : nums) {
            totalSum += num;
        }

        int leftSum = 0;

        for (int i = 0; i < nums.length; i++) {

            int rightSum = totalSum - leftSum - nums[i];

            if (leftSum == rightSum) {
                return i;
            }

            leftSum += nums[i];
        }

        return -1;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## Dry Run

Input: `nums = [1,7,3,6,5,6]`

Total Sum = `28`

| Index | Value | Left Sum | Right Sum |
| ----- | ----- | -------- | --------- |
| 0     | 1     | 0        | 27        |
| 1     | 7     | 1        | 20        |
| 2     | 3     | 8        | 17        |
| 3     | 6     | 11       | 11        |

Index `3` पर दोनों sums equal हैं।

Output: 3

## Comparison

| Approach         | Time         | Extra Space |
| ---------------- | ------------ | ----------- |
| Brute Force      | \\(O(n^2)\\) | \\(O(1)\\)  |
| Prefix Sum Array | \\(O(n)\\)   | \\(O(n)\\)  |
| Single Pass      | \\(O(n)\\)   | \\(O(1)\\)  |

याद रखने की लाइन:

Pivot Index → `Right Sum = Total Sum - Left Sum - Current Element`

Important: Current element को left या right sum में include नहीं करना है।
