# LeetCode 283 — Move Zeroes

Problem: Array ke saare `0` ko end mein move karna hai, bina non-zero elements ka relative order change kiye. Array ko in-place modify karna hai.

Example:

```
Input:  nums = [0, 1, 0, 3, 12]
Output:       [1, 3, 12, 0, 0]
```

## Approach 1: Brute Force — Extra Array

Idea: Pehle saare non-zero elements temporary array mein store karo, phir zeros ko end mein add karo.

```
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;
        int[] temp = new int[n];
        int index = 0;

        // Step 1: Saare non-zero elements copy karo
        for (int num : nums) {
            if (num != 0) {
                temp[index++] = num;
            }
        }

        // Step 2: Bache hue elements zero hi rahenge
        // Step 3: Temporary array ko original array mein copy karo
        for (int i = 0; i < n; i++) {
            nums[i] = temp[i];
        }
    }
}
```

- Time Complexity: \\(O(n)\\)
- Extra Space: \\(O(n)\\)

## Approach 2: Naive In-Place Shifting

Idea: Jab bhi zero mile, uske baad ke elements ko ek position left shift karo aur last position par zero rakho.

```
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length;

        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {

                // Zero ke baad ke elements ko left shift karo
                for (int j = i; j < n - 1; j++) {
                    nums[j] = nums[j + 1];
                }

                // Last position par zero rakho
                nums[n - 1] = 0;
            }
        }
    }
}
```

Dry run:

```
[0, 1, 0, 3, 12]
[1, 0, 3, 12, 0]
[1, 3, 12, 0, 0]
```

- Time Complexity: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\)

Yeh extra array use nahi karta, lekin baar-baar shifting karne ke kaaran slow hai.

## Approach 3: Optimal — Two Pointers / Write Index

Idea: `index` batayega ki next non-zero element kahan store karna hai. Pehle saare non-zero elements aage likho, phir remaining positions ko zero se fill kar do.

Recommended

```
class Solution {
    public void moveZeroes(int[] nums) {
        int index = 0;

        // Step 1: Non-zero elements ko aage copy karo
        for (int num : nums) {
            if (num != 0) {
                nums[index] = num;
                index++;
            }
        }

        // Step 2: Remaining positions ko zero se fill karo
        while (index < nums.length) {
            nums[index] = 0;
            index++;
        }
    }
}
```

### Dry run

Input: `[0, 1, 0, 3, 12]`

| Step | Action                          | Array               |
| ---- | ------------------------------- | ------------------- |
| 1    | `1` ko aage likho               | `[1, 1, 0, 3, 12]`  |
| 2    | `3` ko aage likho               | `[1, 3, 0, 3, 12]`  |
| 3    | `12` ko aage likho              | `[1, 3, 12, 3, 12]` |
| 4    | Remaining positions ko `0` karo | `[1, 3, 12, 0, 0]`  |

- Time Complexity: \\(O(n)\\)
- Extra Space: \\(O(1)\\)
- Relative order: Maintained
- In-place: Yes

## Final Comparison

| Approach                  | Time         | Extra Space |
| ------------------------- | ------------ | ----------- |
| Brute Force — Extra Array | \\(O(n)\\)   | \\(O(n)\\)  |
| Naive Shifting            | \\(O(n^2)\\) | \\(O(1)\\)  |
| Optimal — Write Index     | \\(O(n)\\)   | \\(O(1)\\)  |

Yaad rakhne ki line: Non-zero elements ko aage copy karo, aur baaki positions ko zero se fill kar do.

LeetCode par Approach 3 submit karo.
