# LeetCode 1004 — Max Consecutive Ones III

Pattern: Variable-Size Sliding Window

Question: Binary array `nums` aur integer `k` diya hai. Maximum consecutive `1`s find karo, agar tum maximum `k` zeros ko `1` mein flip kar sakte ho.

Example:

```
nums = [1,1,1,0,0,0,1,1,1,1,0]
k = 2

Output = 6
```

Hum do zeros flip karke `[0,0,1,1,1,1]` jaise valid window bana sakte hain, jiski length `6` hai.

## 1. Brute Force Approach

Har starting index se subarray banao. Zeros count karo; agar zeros `k` se zyada ho jaayein toh aage check karne ki zarurat nahi.

```
class Solution {
    public int longestOnes(int[] nums, int k) {
        int n = nums.length;
        int maxLength = 0;

        for (int i = 0; i < n; i++) {
            int zeros = 0;

            for (int j = i; j < n; j++) {
                if (nums[j] == 0) {
                    zeros++;
                }

                if (zeros > k) {
                    break;
                }

                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }
}
```

- Time: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\)

## 2. Better Approach — Sliding Window with `while`

Right pointer se window expand karo. Jab zeros `k` se zyada ho jaayein, left pointer move karke window valid banao.

```
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0;
        int zeros = 0;
        int maxLength = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) {
                zeros++;
            }

            // Jab zeros limit se zyada ho, window shrink karo
            while (zeros > k) {
                if (nums[left] == 0) {
                    zeros--;
                }
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## 3. Optimal Approach — Single Shrink with `if` ⭐

Is problem mein maximum window length chahiye, isliye ek aur standard solution hai: jab zeros `k` se zyada ho jaayein, left pointer ko sirf ek step move karo.

```
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0;
        int zeros = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) {
                zeros++;
            }

            // Invalid window par left ko ek step move karo
            if (zeros > k) {
                if (nums[left] == 0) {
                    zeros--;
                }
                left++;
            }
        }

        // Window length maximum valid length ke barabar hoti hai
        return nums.length - left;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

### Dry Run

`nums = [1,1,1,0,0,0,1,1,1,1,0]`, `k = 2`

| Window          | Zero count | Length  |
| --------------- | ---------- | ------- |
| `[1,1,1,0,0]`   | 2          | 5       |
| `[1,1,0,0,0]`   | 3          | Invalid |
| `[0,0,1,1,1,1]` | 2          | 6       |
| `[1,1,1,1,0]`   | 1          | 5       |

Output: `6`

## Approach Comparison

| Approach                 | Time         | Extra Space |
| ------------------------ | ------------ | ----------- |
| Brute Force              | \\(O(n^2)\\) | \\(O(1)\\)  |
| Sliding Window (`while`) | \\(O(n)\\)   | \\(O(1)\\)  |
| Single Shrink (`if`)     | \\(O(n)\\)   | \\(O(1)\\)  |

## Yaad rakhne ki line

LeetCode 1004 = Variable-Size Sliding Window + Zero Count.

- `zeros <= k` → window valid.
- `zeros > k` → left pointer move karo.
- Maximum valid window length return karo.

Important: `while` approach window ko valid rakhta hai; `if` approach maximum length ko maintain karta hai. Dono linear time mein kaam karte hain.
