# LeetCode 75 — Sort Colors

Problem: Given an array containing only `0`, `1`, and `2`, sort it in-place so that all `0`s come first, then `1`s, then `2`s.

Example:

```
Input:  nums = [2, 0, 2, 1, 1, 0]
Output:       [0, 0, 1, 1, 2, 2]
```

## Approach 1: Brute Force — Sorting

Idea: Java ke built-in `Arrays.sort()` se array sort kar do.

```
import java.util.Arrays;

class Solution {
    public void sortColors(int[] nums) {
        // Array ko ascending order mein sort karo
        Arrays.sort(nums);
    }
}
```

- Time: \\(O(n\log n)\\)
- Extra Space: Implementation-dependent; sorting ke liye auxiliary space lag sakta hai.
- Note: Easy hai, lekin problem ka intended one-pass solution nahi hai.

## Approach 2: Better — Counting Sort

Idea: Pehle `0`, `1`, aur `2` ki frequency count karo. Phir array ko usi order mein overwrite karo.

```
class Solution {
    public void sortColors(int[] nums) {
        int count0 = 0;
        int count1 = 0;
        int count2 = 0;

        // Har color ki frequency count karo
        for (int num : nums) {
            if (num == 0) {
                count0++;
            } else if (num == 1) {
                count1++;
            } else {
                count2++;
            }
        }

        int index = 0;

        // Pehle saare zero likho
        while (count0-- > 0) {
            nums[index++] = 0;
        }

        // Phir saare one likho
        while (count1-- > 0) {
            nums[index++] = 1;
        }

        // Aakhir mein saare two likho
        while (count2-- > 0) {
            nums[index++] = 2;
        }
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## Approach 3: Optimal — Dutch National Flag Algorithm

Recommended

Idea: Teen pointers use karenge:

- `low`: `0` ki correct position.
- `mid`: current element.
- `high`: `2` ki correct position.

Rules:

1. `nums[mid] == 0`: `low` aur `mid` swap karo; dono increment karo.
2. `nums[mid] == 1`: `mid` increment karo.
3. `nums[mid] == 2`: `mid` aur `high` swap karo; sirf `high` decrement karo.

```
class Solution {
    public void sortColors(int[] nums) {
        int low = 0;
        int mid = 0;
        int high = nums.length - 1;

        while (mid <= high) {
            if (nums[mid] == 0) {
                // Zero ko left side bhejo
                swap(nums, low, mid);
                low++;
                mid++;
            } else if (nums[mid] == 1) {
                // One ko middle mein rehne do
                mid++;
            } else {
                // Two ko right side bhejo
                swap(nums, mid, high);
                high--;

                // Mid ko increment nahi karna,
                // kyunki swapped element abhi check karna hai
            }
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

### Dry Run

Input: `[2, 0, 2, 1, 1, 0]`

| Step | Action                   | Array                |
| ---- | ------------------------ | -------------------- |
| 1    | `2` ko right bhejo       | `[0, 0, 2, 1, 1, 2]` |
| 2    | `0` ko left side rakho   | `[0, 0, 2, 1, 1, 2]` |
| 3    | `0` ko left side rakho   | `[0, 0, 2, 1, 1, 2]` |
| 4    | `2` ko right bhejo       | `[0, 0, 1, 1, 2, 2]` |
| 5    | `1` ko middle mein rakho | `[0, 0, 1, 1, 2, 2]` |
| 6    | `1` ko middle mein rakho | `[0, 0, 1, 1, 2, 2]` |

- Time: \\(O(n)\\) — single pass.
- Extra Space: \\(O(1)\\) — in-place.
- Key point: `2` swap karne ke baad `mid` ko increment nahi karna, kyunki naya swapped element abhi check karna baaki hai.

## Final Comparison

| Approach                      | Time             | Extra Space              |
| ----------------------------- | ---------------- | ------------------------ |
| Brute Force — `Arrays.sort()` | \\(O(n\log n)\\) | Implementation-dependent |
| Better — Counting Sort        | \\(O(n)\\)       | \\(O(1)\\)               |
| Optimal — Dutch National Flag | \\(O(n)\\)       | \\(O(1)\\)               |

Yaad rakhne ki line: `0 → left`, `1 → middle`, `2 → right`.
