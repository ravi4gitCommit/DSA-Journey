# LeetCode 219 — Contains Duplicate II

Pattern: HashMap / HashSet + Sliding Window

Question: Given an integer array `nums` and an integer `k`, return `true` if there are two different indices `i` and `j` such that:

- `nums[i] == nums[j]`
- `|i - j| <= k`

Example: `nums = [1,2,3,1]`, `k = 3` → `true`, because `nums[0] == nums[3]` and distance `3 <= k`.

## 1. Brute Force — Nested Loops

Logic: Har element ke baad ke elements check karo. Agar same number milta hai aur index distance `k` ke andar hai, return `true`.

```
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length && j - i <= k; j++) {
                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

- Time: \\(O(n \cdot \min(n,k))\\)
- Space: \\(O(1)\\)

## 2. Better — HashMap (Last Seen Index)

Logic: Har number ka last index store karo. Agar number dobara mile, distance check karo: `i - lastIndex <= k`.

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> lastSeen = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            int num = nums[i];

            if (lastSeen.containsKey(num)) {
                if (i - lastSeen.get(num) <= k) {
                    return true;
                }
            }

            lastSeen.put(num, i);
        }

        return false;
    }
}
```

- Time: \\(O(n)\\) average
- Space: \\(O(n)\\)

## 3. Optimal — Sliding Window + HashSet

Logic: Set mein pichhle maximum `k` elements rakho.

1. Current number ko set mein add karne se pehle check karo.
2. Agar already present hai, return `true`.
3. Agar window size `k` ho chuki hai, purana element remove karo.
4. Current element add karo.

```
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> window = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {

            // Duplicate current window mein hai
            if (window.contains(nums[i])) {
                return true;
            }

            // Window mein pichhle k elements hi rakho
            if (window.size() == k) {
                window.remove(nums[i - k]);
            }

            window.add(nums[i]);
        }

        return false;
    }
}
```

- Time: \\(O(n)\\) average
- Space: \\(O(\min(n,k))\\)

Important: `k = 0` hone par answer `false` hoga, kyunki do different indices ka distance `0` nahi ho sakta. Upar wala code bhi correctly `false` return karta hai.

## Dry Run

Input: `nums = [1,2,3,1]`, `k = 3`

| Index | Number | Set before check | Result                   |
| ----- | ------ | ---------------- | ------------------------ |
| 0     | 1      | `{}`             | Add 1                    |
| 1     | 2      | `{1}`            | Add 2                    |
| 2     | 3      | `{1,2}`          | Add 3                    |
| 3     | 1      | `{1,2,3}`        | Duplicate found → `true` |

## Comparison

| Approach                 | Time                | Space              |
| ------------------------ | ------------------- | ------------------ |
| Brute Force              | \\(O(n\min(n,k))\\) | \\(O(1)\\)         |
| HashMap                  | \\(O(n)\\) average  | \\(O(n)\\)         |
| Sliding Window + HashSet | \\(O(n)\\) average  | \\(O(\min(n,k))\\) |

Yaad rakhne ki line: Nearby Duplicate → same number + index distance `<= k` → last index ya sliding window use karo.
