# LeetCode 1 — Two Sum

Pattern: HashMap · Seen-Before Lookup

Question: Integer array `nums` aur integer `target` diya hai. Do numbers ke indices return karo jinka sum `target` ke equal ho. Ek hi element ko do baar use nahi kar sakte.

```
Input:  nums = [2,7,11,15], target = 9
Output: [0,1]
```

Kyunki `nums[0] + nums[1] = 2 + 7 = 9`.

## 1. Brute Force Approach

Har pair check karo ki dono numbers ka sum target ke equal hai ya nahi.

```
class Solution {
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {

                if (nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }

        return new int[]{};
    }
}
```

- Time: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\)

## 2. Better Approach — HashMap

Har number ke liye required number calculate karo:

\\[ \text{complement}=\text{target}-\text{nums}[i] \\]

HashMap mein check karo ki complement pehle se mila hai ya nahi. Agar mil gaya, toh dono indices return karo.

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];

            // Kya required number pehle mil chuka hai?
            if (seen.containsKey(complement)) {
                return new int[]{seen.get(complement), i};
            }

            // Current number ka index store karo
            seen.put(nums[i], i);
        }

        return new int[]{};
    }
}
```

- Time: \\(O(n)\\) average
- Extra Space: \\(O(n)\\)

## Dry Run

`nums = [2,7,11,15]`, `target = 9`

| Index | Number | Complement | Action                      |
| ----- | ------ | ---------- | --------------------------- |
| 0     | 2      | 7          | 7 nahi mila; `2 → 0` store  |
| 1     | 7      | 2          | 2 mil gaya; indices `[0,1]` |

Output: `[0,1]`

## Approach Comparison

| Approach    | Time               | Extra Space |
| ----------- | ------------------ | ----------- |
| Brute Force | \\(O(n^2)\\)       | \\(O(1)\\)  |
| HashMap     | \\(O(n)\\) average | \\(O(n)\\)  |

## Yaad rakhne ki line

Two Sum = Complement calculate karo → HashMap mein check karo → phir current number store karo.

Order important hai: pehle check, phir insert. Isse ek hi element ko do baar use karne se bach sakte hain.
