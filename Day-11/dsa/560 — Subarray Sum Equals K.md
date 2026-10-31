# LeetCode 560 — Subarray Sum Equals K

Pattern: Prefix Sum + HashMap

Find the total number of continuous subarrays whose sum equals `k`.

Example:

- Input: `nums = [1,1,1]`, `k = 2`
- Output: `2`

दो valid subarrays हैं: `[1,1]` और `[1,1]`।

## 1. Brute Force — Nested Loops

हर index से subarray बनाओ और उसका sum calculate करो।

```
class Solution {
    public int subarraySum(int[] nums, int k) {
        int count = 0;

        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            for (int j = i; j < nums.length; j++) {
                sum += nums[j];

                if (sum == k) {
                    count++;
                }
            }
        }

        return count;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(1)\\)

## 2. Optimal — Prefix Sum + HashMap

Logic:

अगर current prefix sum `sum` है, तो हमें previous prefix sum `sum - k` चाहिए।

\\[ sum-previousSum=k \\]

```
import java.util.*;

class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();

        map.put(0, 1);

        int sum = 0;
        int count = 0;

        for (int num : nums) {
            sum += num;

            count += map.getOrDefault(sum - k, 0);

            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }

        return count;
    }
}
```

- Time: \\(O(n)\\) average
- Space: \\(O(n)\\)

## Dry Run

Input: `nums = [1,1,1]`, `k = 2`

| num | Prefix Sum | sum - k | Count |
| --- | ---------- | ------- | ----- |
| 1   | 1          | -1      | 0     |
| 1   | 2          | 0       | 1     |
| 1   | 3          | 1       | 2     |

Output: `2`

## याद रखने की लाइन

`Prefix Sum + HashMap`

- Initialize: `map.put(0, 1)`
- Search: `sum - k`
- Update: Current prefix sum frequency.

Note: इस problem में negative numbers भी हो सकते हैं, इसलिए normal sliding window use नहीं करना है।
