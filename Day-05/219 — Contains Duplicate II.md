# LeetCode 219 — Contains Duplicate II

Pattern: Sliding Window + HashSet / HashMap

Problem: Integer array `nums` aur integer `k` diya hai. Check karo ki koi do different indices `i` aur `j` hain jinke liye:

- `nums[i] == nums[j]`
- `|i - j| <= k`

Example:

```
Input:  nums = [1, 2, 3, 1], k = 3
Output: true
```

Kyunki `nums[0] == nums[3] == 1` aur indices ka difference `3` hai.

## Approach 1: Brute Force

Har element ke baad ke elements check karo. Agar same value mile aur distance `k` se kam ya barabar ho, toh `true` return karo.

```
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        for (int i = 0; i < nums.length; i++) {
            // Sirf agle k elements tak check karo
            for (int j = i + 1;
                 j < nums.length && j - i <= k;
                 j++) {

                if (nums[i] == nums[j]) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

- Time: \\(O(n \cdot \min(n,k))\\), worst case \\(O(n^2)\\)
- Extra Space: \\(O(1)\\)

## Approach 2: Better — HashMap

Idea: Har element ka last-seen index store karo. Agar same element dobara mile, toh indices ka difference check karo.

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Map<Integer, Integer> lastSeen = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {
            // Check karo ki element pehle mila tha ya nahi
            if (lastSeen.containsKey(nums[i])) {
                int previousIndex = lastSeen.get(nums[i]);

                // Kya dono indices ka distance k ke andar hai?
                if (i - previousIndex <= k) {
                    return true;
                }
            }

            // Current element ka latest index store karo
            lastSeen.put(nums[i], i);
        }

        return false;
    }
}
```

- Time: \\(O(n)\\) average
- Extra Space: \\(O(n)\\)

## Approach 3: Optimal — Sliding Window + HashSet

Recommended

Idea: HashSet mein sirf pichhle `k` elements rakho. Agar current element pehle se set mein hai, toh nearby duplicate mil gaya.

```
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> window = new HashSet<>();

        for (int i = 0; i < nums.length; i++) {

            // Window se woh element remove karo
            // jo current index se k se zyada door hai
            if (i > k) {
                window.remove(nums[i - k - 1]);
            }

            // Element pehle se window mein hai toh duplicate mila
            if (window.contains(nums[i])) {
                return true;
            }

            // Current element ko window mein add karo
            window.add(nums[i]);
        }

        return false;
    }
}
```

### Dry Run

Input: `nums = [1, 2, 3, 1]`, `k = 3`

| Index | Current element | HashSet     | Result                       |
| ----- | --------------- | ----------- | ---------------------------- |
| 0     | 1               | `{1}`       | Continue                     |
| 1     | 2               | `{1, 2}`    | Continue                     |
| 2     | 3               | `{1, 2, 3}` | Continue                     |
| 3     | 1               | `{1, 2, 3}` | `1` already present → `true` |

- Time: \\(O(n)\\) average
- Extra Space: \\(O(\min(n,k+1))\\)

## Final Comparison

| Approach                 | Time                       | Extra Space          |
| ------------------------ | -------------------------- | -------------------- |
| Brute Force              | \\(O(n \cdot \min(n,k))\\) | \\(O(1)\\)           |
| HashMap                  | \\(O(n)\\) average         | \\(O(n)\\)           |
| Sliding Window + HashSet | \\(O(n)\\) average         | \\(O(\min(n,k+1))\\) |

Yaad rakhne ki line: `LC 219` mein duplicate value ke saath index distance bhi check karna hai.

- Fixed-size window ka pattern: pichhle `k` elements maintain karo.
- `k = 0` hone par answer hamesha `false` hoga, kyunki different indices ka distance zero nahi ho sakta. Upar wala code is case ko bhi correctly handle karta hai.
