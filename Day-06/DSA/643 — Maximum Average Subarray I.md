# LeetCode 643 — Maximum Average Subarray I

Pattern: Fixed-Size Sliding Window

Question: Given an integer array `nums` and an integer `k`, find the contiguous subarray of length exactly `k` that has the maximum average. Return the maximum average.

Example: `nums = [2,1,5,1,3,2], k = 3`

- `[2,1,5]` → sum = 8, average = 2.67
- `[1,5,1]` → sum = 7, average = 2.33
- `[5,1,3]` → sum = 9, average = 3.00
- `[1,3,2]` → sum = 6, average = 2.00

Output: `3.0`

## 1. Brute Force Approach

Har possible window ka sum calculate karo, phir average nikal kar maximum average store karo.

```
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        double maxAverage = Double.NEGATIVE_INFINITY;

        // Har possible starting index se window check karo
        for (int i = 0; i <= nums.length - k; i++) {
            int sum = 0;

            // Current window ke k elements ka sum
            for (int j = i; j < i + k; j++) {
                sum += nums[j];
            }

            double average = (double) sum / k;
            maxAverage = Math.max(maxAverage, average);
        }

        return maxAverage;
    }
}
```

- Time: \\(O(nk)\\)
- Space: \\(O(1)\\)

## 2. Better Approach — Prefix Sum

Prefix sum array se har window ka sum \\(O(1)\\) mein calculate kar sakte hain.

Formula:

\\[ \text{windowSum}=prefix[i+k]-prefix[i] \\]

```
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int n = nums.length;
        int[] prefix = new int[n + 1];

        // Prefix sum array banao
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        double maxAverage = Double.NEGATIVE_INFINITY;

        // Har size-k window ka sum aur average
        for (int i = 0; i <= n - k; i++) {
            int sum = prefix[i + k] - prefix[i];
            double average = (double) sum / k;

            maxAverage = Math.max(maxAverage, average);
        }

        return maxAverage;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(n)\\)

## 3. Optimal Approach — Sliding Window ⭐

Ek baar pehli `k` elements ka sum nikalo. Phir har next window mein:

- Purana leftmost element subtract karo.
- Naya rightmost element add karo.
- Maximum sum update karo.

Average ke liye maximum sum ko `k` se divide karna enough hai, kyunki har window ka size same hai.

```
class Solution {
    public double findMaxAverage(int[] nums, int k) {
        int sum = 0;

        // First window ka sum
        for (int i = 0; i < k; i++) {
            sum += nums[i];
        }

        int maxSum = sum;

        // Window ko ek-ek position right slide karo
        for (int i = k; i < nums.length; i++) {
            sum = sum - nums[i - k] + nums[i];

            maxSum = Math.max(maxSum, sum);
        }

        return (double) maxSum / k;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(1)\\)

## Dry Run — Optimal Approach

`nums = [2,1,5,1,3,2], k = 3`

| Window    | Calculation | Sum | Average |
| --------- | ----------- | --- | ------- |
| `[2,1,5]` | `2+1+5`     | 8   | 2.67    |
| `[1,5,1]` | `8-2+1`     | 7   | 2.33    |
| `[5,1,3]` | `7-1+3`     | 9   | 3.00    |
| `[1,3,2]` | `9-5+2`     | 6   | 2.00    |

Maximum sum = `9`

Maximum average = `9 / 3 = 3.0`

## Approach Comparison

| Approach       | Time        | Extra Space |
| -------------- | ----------- | ----------- |
| Brute Force    | \\(O(nk)\\) | \\(O(1)\\)  |
| Prefix Sum     | \\(O(n)\\)  | \\(O(n)\\)  |
| Sliding Window | \\(O(n)\\)  | \\(O(1)\\)  |

Yaad rakhne ki line: Fixed-size subarray mein har baar poora sum dobara calculate mat karo — old element subtract + new element add karo. यही Sliding Window hai.
