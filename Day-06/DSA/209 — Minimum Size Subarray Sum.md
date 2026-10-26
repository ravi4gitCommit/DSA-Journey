# LeetCode 209 — Minimum Size Subarray Sum

Pattern: Variable-Size Sliding Window

Question: Ek positive integer array `nums` aur target `target` diya hai. Contiguous subarray ki minimum length return karo jiska sum `target` se greater than or equal to ho. Agar aisa subarray nahi hai, toh `0` return karo.

Example:

```
nums = [2,3,1,2,4,3]
target = 7

Output: 2
```

Explanation: Subarray `[4,3]` ka sum `7` hai aur length `2` hai.

## 1. Brute Force Approach

Har starting index se subarray ka sum calculate karo. Jaise hi sum target tak pahunchta hai, minimum length update karo.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int minLength = Integer.MAX_VALUE;

        // Har possible starting index
        for (int i = 0; i < n; i++) {
            int sum = 0;

            // Subarray ko right side mein extend karo
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

Positive numbers hone ki wajah se prefix sum array strictly increasing hota hai. Har starting index ke liye binary search se pehla prefix sum dhoondh sakte hain jo target sum complete kare.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int[] prefix = new int[n + 1];

        // Prefix sum banao
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }

        int minLength = Integer.MAX_VALUE;

        for (int i = 0; i < n; i++) {
            int required = prefix[i] + target;

            // prefix[i + 1 ... n] mein required dhoondho
            int left = i + 1;
            int right = n;

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

## 3. Optimal Approach — Variable-Size Sliding Window ⭐

Yahan window ka size fixed nahi hai.

- Expand: Right pointer se element add karo.
- Shrink: Jab `sum >= target` ho, left se elements remove karke window chhoti karo.
- Har valid window par minimum length update karo.

```
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int left = 0;
        int sum = 0;
        int minLength = Integer.MAX_VALUE;

        for (int right = 0; right < nums.length; right++) {
            // Window mein naya element add karo
            sum += nums[right];

            // Jab tak sum target meet kare, window chhoti karo
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

- Time: \\(O(n)\\) — dono pointers maximum `n` times move karte hain.
- Extra Space: \\(O(1)\\)

## Dry Run — Optimal Approach

`nums = [2,3,1,2,4,3]`, `target = 7`

| Window      | Sum | Length | Action             |
| ----------- | --- | ------ | ------------------ |
| `[2,3,1,2]` | 8   | 4      | Valid; shrink      |
| `[3,1,2]`   | 6   | 3      | Expand             |
| `[3,1,2,4]` | 10  | 4      | Valid; shrink      |
| `[1,2,4]`   | 7   | 3      | Valid; shrink      |
| `[2,4]`     | 6   | 2      | Expand             |
| `[2,4,3]`   | 9   | 3      | Valid; shrink      |
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

`sum >= target` hone par window ko shrink karo; sum target se chhota hone par right pointer se expand karo.

Important: Ye optimal sliding-window logic positive integers ke liye kaam karta hai. Negative numbers hone par ye approach generally valid nahi hoti.
