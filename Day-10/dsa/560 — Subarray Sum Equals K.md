# LeetCode 560 — Subarray Sum Equals K

Pattern: Prefix Sum + HashMap

Question: Given an integer array `nums` and an integer `k`, find the total number of continuous subarrays whose sum equals `k`.

Example:

- Input: `nums = [1,1,1]`, `k = 2`
- Output: `2`
- Explanation: `[1,1]` दो बार आता है।

Important: Subarray continuous होना चाहिए। इस problem में negative numbers भी हो सकते हैं, इसलिए सामान्य sliding window काम नहीं करता।

## 1. Brute Force — Nested Loops

Logic:

1. हर index से subarray start करो।
2. आगे के elements जोड़ते जाओ।
3. जब sum `k` हो, count बढ़ा दो।

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

मान लो current prefix sum `sum` है।

हमें ऐसा previous prefix sum चाहिए:

\\[ previousSum = sum-k \\]

क्योंकि:

\\[ sum-previousSum=k \\]

इसलिए HashMap में previous prefix sums की frequencies store करेंगे।

Steps:

1. `map.put(0, 1)` — शुरुआत में empty prefix का sum `0` एक बार मानो।
2. Current prefix sum calculate करो।
3. `sum - k` HashMap में है तो उसका count answer में add करो।
4. Current prefix sum की frequency update करो।

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();

        // Empty prefix sum ki frequency 1
        map.put(0, 1);

        int sum = 0;
        int count = 0;

        for (int num : nums) {

            // Current prefix sum
            sum += num;

            // Required previous prefix sum check karo
            count += map.getOrDefault(sum - k, 0);

            // Current prefix sum ki frequency update karo
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

Initial:

- `map = {0=1}`
- `sum = 0`
- `count = 0`

| num | sum | sum - k | Count | Map update             |
| --- | --- | ------- | ----- | ---------------------- |
| 1   | 1   | -1      | 0     | `{0=1, 1=1}`           |
| 1   | 2   | 0       | 1     | `{0=1, 1=1, 2=1}`      |
| 1   | 3   | 1       | 2     | `{0=1, 1=1, 2=1, 3=1}` |

Output: 2

## Comparison

| Approach             | Time               | Space      |
| -------------------- | ------------------ | ---------- |
| Brute Force          | \\(O(n^2)\\)       | \\(O(1)\\) |
| Prefix Sum + HashMap | \\(O(n)\\) average | \\(O(n)\\) |

## याद रखने की लाइन

Subarray Sum Equals K:

1. Prefix sum calculate करो।
2. `sum - k` को HashMap में search करो।
3. उसकी frequency को answer में add करो।
4. Current sum की frequency update करो।

Golden Formula:

\\[ \boxed{count += map.get(sum-k)} \\]

ध्यान रखो: यह formula subarray sum problems का बहुत important pattern है।
