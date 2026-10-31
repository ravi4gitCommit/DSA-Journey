# LeetCode 238 — Product of Array Except Self

Pattern: Prefix Product + Suffix Product

Question: Given an array `nums`, return an array where each element is the product of all elements except itself.

Important:

- Division operator `/` use नहीं करना है।
- Solution \\(O(n)\\) time में होना चाहिए।

Example:

- Input: `nums = [1,2,3,4]`
- Output: `[24,12,8,6]`

Explanation:

- Index 0 → `2 × 3 × 4 = 24`
- Index 1 → `1 × 3 × 4 = 12`
- Index 2 → `1 × 2 × 4 = 8`
- Index 3 → `1 × 2 × 3 = 6`

## 1. Brute Force — Nested Loops

Logic: हर index के लिए बाकी सभी elements का product calculate करो। Current index को skip कर दो।

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];

        for (int i = 0; i < n; i++) {
            int product = 1;

            for (int j = 0; j < n; j++) {
                if (i != j) {
                    product *= nums[j];
                }
            }

            ans[i] = product;
        }

        return ans;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(1)\\) extra, output array को छोड़कर।

## 2. Better — Prefix Array + Suffix Array

Logic:

दो arrays बनाओ:

- `prefix[i]` = current index के left के सभी elements का product।
- `suffix[i]` = current index के right के सभी elements का product।

Final answer:

\\[ ans[i] = prefix[i] \times suffix[i] \\]

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;

        int[] prefix = new int[n];
        int[] suffix = new int[n];
        int[] ans = new int[n];

        // Prefix product
        prefix[0] = 1;

        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] * nums[i - 1];
        }

        // Suffix product
        suffix[n - 1] = 1;

        for (int i = n - 2; i >= 0; i--) {
            suffix[i] = suffix[i + 1] * nums[i + 1];
        }

        // Final answer
        for (int i = 0; i < n; i++) {
            ans[i] = prefix[i] * suffix[i];
        }

        return ans;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(n)\\) extra, output array को छोड़कर।

## 3. Optimal — Prefix + Suffix in Output Array

Logic:

अलग-अलग prefix और suffix arrays बनाने की जरूरत नहीं है।

1. पहले left product को `ans` में store करो।
2. फिर right से चलते हुए suffix product multiply करो।

इससे extra space \\(O(1)\\) हो जाता है (output array को छोड़कर)।

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];

        // Step 1: Left side ka product
        int prefix = 1;

        for (int i = 0; i < n; i++) {
            ans[i] = prefix;
            prefix *= nums[i];
        }

        // Step 2: Right side ka product
        int suffix = 1;

        for (int i = n - 1; i >= 0; i--) {
            ans[i] *= suffix;
            suffix *= nums[i];
        }

        return ans;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\), output array को छोड़कर।

## Dry Run

Input: `nums = [1,2,3,4]`

Step 1 — Prefix Product

| Index | Value | Prefix stored in ans |
| ----- | ----- | -------------------- |
| 0     | 1     | 1                    |
| 1     | 2     | 1                    |
| 2     | 3     | 2                    |
| 3     | 4     | 6                    |

`ans = [1,1,2,6]`

Step 2 — Suffix Product

| Index | Suffix | Final ans |
| ----- | ------ | --------- |
| 3     | 1      | 6         |
| 2     | 4      | 8         |
| 1     | 12     | 12        |
| 0     | 24     | 24        |

Output: [24, 12, 8, 6]

## Comparison

| Approach                  | Time         | Extra Space |
| ------------------------- | ------------ | ----------- |
| Brute Force               | \\(O(n^2)\\) | \\(O(1)\\)  |
| Prefix + Suffix Arrays    | \\(O(n)\\)   | \\(O(n)\\)  |
| Prefix + Suffix in Output | \\(O(n)\\)   | \\(O(1)\\)  |

## याद रखने की लाइन

Product Except Self = Left Product × Right Product

- Left to Right → Prefix product
- Right to Left → Suffix product
- दोनों को multiply करके answer निकालो।

Important: इसमें division नहीं करना है। Zero होने पर भी यह approach सही काम करता है।
