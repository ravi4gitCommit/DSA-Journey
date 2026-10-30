# LeetCode 1480 — Running Sum of 1D Array

Pattern: Prefix Sum

Question: Given an array `nums`, return an array where each element is the sum of all previous elements including itself.

Formula:

\\[ prefix[i] = prefix[i-1] + nums[i] \\]

Example:

- Input: `nums = [1,2,3,4]`
- Output: `[1,3,6,10]`

## 1. Brute Force — Nested Loop

Logic: Har index ke liye starting index `0` se current index tak sum calculate karo.

```
class Solution {
    public int[] runningSum(int[] nums) {
        int[] ans = new int[nums.length];

        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            for (int j = 0; j <= i; j++) {
                sum += nums[j];
            }

            ans[i] = sum;
        }

        return ans;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(n)\\) for output array.

## 2. Better — Prefix Sum Using Separate Array

Logic: Previous prefix sum में current element add करो और result को नए array में store करो।

```
class Solution {
    public int[] runningSum(int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n];

        prefix[0] = nums[0];

        for (int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }

        return prefix;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(n)\\) for output array.

## 3. Optimal — In-place Prefix Sum

Logic: उसी input array में previous sum add करते जाओ। अलग array बनाने की जरूरत नहीं है।

```
class Solution {
    public int[] runningSum(int[] nums) {

        for (int i = 1; i < nums.length; i++) {
            nums[i] = nums[i - 1] + nums[i];
        }

        return nums;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## Dry Run

Input: `nums = [1,2,3,4]`

| Index | Calculation | Running Sum |
| ----- | ----------- | ----------- |
| 0     | 1           | 1           |
| 1     | 1 + 2       | 3           |
| 2     | 3 + 3       | 6           |
| 3     | 6 + 4       | 10          |

Output: [1, 3, 6, 10]

## Comparison

| Approach              | Time         | Extra Space |
| --------------------- | ------------ | ----------- |
| Brute Force           | \\(O(n^2)\\) | \\(O(n)\\)  |
| Separate Prefix Array | \\(O(n)\\)   | \\(O(n)\\)  |
| In-place Prefix Sum   | \\(O(n)\\)   | \\(O(1)\\)  |

याद रखने की लाइन:

Prefix Sum → `prefix[i] = prefix[i-1] + nums[i]`

Important: Running Sum prefix sum का सबसे basic question है। यही concept आगे Range Sum Query और Subarray Sum problems में काम आता है।
