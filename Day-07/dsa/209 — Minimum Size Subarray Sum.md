# LeetCode 209 — Minimum Size Subarray Sum

Pattern: Variable-Size Sliding Window

Question: Ek positive integer array `nums` aur integer `target` diya hai. Minimum length ki contiguous subarray find karo jiska sum `target` se greater than ya equal ho. Agar koi valid subarray nahi hai, toh `0` return karo.

Example:

```
Input:  nums = [2,3,1,2,4,3], target = 7
Output: 2
```

Explanation: `[4,3]` ka sum `7` hai aur length `2` hai.

## 1. Brute Force Approach

Har starting index se subarray ka sum calculate karo. Jaise hi sum target tak pahunch jaaye, minimum length update karo.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int minLength = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            int sum = 0;

            for (int j = i; j < n; j++) {
                sum += nums[j];

                if (sum >= target) {
                    minLength = Math.min(minLength, j - i + 1);
                    break;
                }
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```

- Time: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\)

## 2. Better Approach — Prefix Sum + Binary Search

Prefix sum array se har starting index ke liye binary search karke minimum valid ending index find kar sakte hain.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n + 1];

        // Prefix sum calculate karo
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        int minLength = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            int required = prefix[i] + target;
            int left = i + 1;
            int right = n;

            // Pehla prefix sum find karo jo required ke barabar ya bada ho
            while (left <= right) {
                int mid = left + (right - left) / 2;

                if (prefix[mid] >= required) {
                    minLength = Math.min(minLength, mid - i);
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```

- Time: \\(O(n\log n)\\)
- Extra Space: \\(O(n)\\)

## 3. Optimal Approach — Sliding Window ⭐

Is problem mein window ka size fixed nahi hai.

- Expand: Right pointer se element add karo.
- Shrink: Jab `sum >= target` ho, left se elements remove karo.
- Har valid window par minimum length update karo.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int minLength = Integer.MAX_VALUE;

        for (int right = 0; right < nums.length; right++) {
            // Naya element window mein add karo
            sum += nums[right];

            // Jab tak window valid hai, use chhota karo
            while (sum >= target) {
                minLength = Math.min(minLength, right - left + 1);

                // Leftmost element remove karo
                sum -= nums[left];
                left++;
            }
        }

        return minLength == Integer.MAX_VALUE ? 0 : minLength;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## Dry Run — Optimal Approach

`nums = [2,3,1,2,4,3]`, `target = 7`

| Window      | Sum | Length | Action             |
| ----------- | --- | ------ | ------------------ |
| `[2,3,1,2]` | 8   | 4      | Valid, shrink      |
| `[3,1,2,4]` | 10  | 4      | Valid, shrink      |
| `[1,2,4]`   | 7   | 3      | Valid, shrink      |
| `[2,4,3]`   | 9   | 3      | Valid, shrink      |
| `[4,3]`     | 7   | 2      | Minimum length = 2 |

Output: `2`

## Approach Comparison

| Approach                   | Time             | Extra Space |
| -------------------------- | ---------------- | ----------- |
| Brute Force                | \\(O(n^2)\\)     | \\(O(1)\\)  |
| Prefix Sum + Binary Search | \\(O(n\log n)\\) | \\(O(n)\\)  |
| Sliding Window             | \\(O(n)\\)       | \\(O(1)\\)  |

## Yaad rakhne ki line

LeetCode 209 = Variable-Size Sliding Window.

`sum >= target` → minimum length update karo aur window shrink karo.

`sum < target` → right pointer se window expand karo.

Important: Ye sliding-window approach positive integers ke liye hai. Negative numbers hone par ye logic generally valid nahi hota.
