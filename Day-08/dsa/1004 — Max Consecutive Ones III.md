# LeetCode 1004 — Max Consecutive Ones III

Pattern: Variable-Size Sliding Window

Question: Binary array `nums` aur integer `k` diya hai. Maximum consecutive `1`s ki length find karo, agar maximum `k` zeros ko `1` mein flip kar sakte ho.

```
Input:  nums = [1,1,1,0,0,0,1,1,1,1,0]
k = 2

Output: 6
```

## 1. Brute Force Approach

Har starting index se subarray banao. Agar zeros ki count `k` se zyada ho jaaye, toh stop karo.

```
class Solution {
    public int longestOnes(int[] nums, int k) {
        int maxLength = 0;

        for (int left = 0; left < nums.length; left++) {
            int zeros = 0;

            for (int right = left; right < nums.length; right++) {
                if (nums[right] == 0) {
                    zeros++;
                }

                if (zeros > k) {
                    break;
                }

                maxLength = Math.max(maxLength, right - left + 1);
            }
        }

        return maxLength;
    }
}
```

- Time: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\)

## 2. Better Approach — Sliding Window with `while`

Right pointer se window expand karo. Jab zeros `k` se zyada hon, left se elements remove karke window valid banao.

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

            // Window mein k se zyada zeros hain
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

## 3. Optimal Approach — Single Shrink with `if`

Maximum length maintain karne ke liye invalid window par left pointer ko sirf ek step move kar sakte hain.

```
class Solution {
    public int longestOnes(int[] nums, int k) {
        int left = 0;
        int zeros = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) {
                zeros++;
            }

            // Invalid hone par window ko ek step slide karo
            if (zeros > k) {
                if (nums[left] == 0) {
                    zeros--;
                }
                left++;
            }
        }

        return nums.length - left;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## Dry Run

`nums = [1,1,1,0,0,0,1,1,1,1,0]`, `k = 2`

| Window          | Zeros | Length  |
| --------------- | ----- | ------- |
| `[1,1,1,0,0]`   | 2     | 5       |
| `[1,1,0,0,0]`   | 3     | Invalid |
| `[0,0,1,1,1,1]` | 2     | 6       |

Output: `6`

## Approach Comparison

| Approach                 | Time         | Extra Space |
| ------------------------ | ------------ | ----------- |
| Brute Force              | \\(O(n^2)\\) | \\(O(1)\\)  |
| Sliding Window (`while`) | \\(O(n)\\)   | \\(O(1)\\)  |
| Single Shrink (`if`)     | \\(O(n)\\)   | \\(O(1)\\)  |

## Yaad rakhne ki line

LeetCode 1004 = Variable-Size Sliding Window + Zero Count.

- `zeros <= k` → valid window.
- `zeros > k` → left pointer move karo.
- Maximum window length return karo.

Difference: `while` window ko valid banata hai; `if` maximum length maintain karne ke liye ek step slide karta hai. Dono solutions \\(O(n)\\) time mein kaam karte hain.
