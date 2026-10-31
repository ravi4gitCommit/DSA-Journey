# LeetCode 523 — Continuous Subarray Sum

Pattern: Prefix Sum + HashMap (Remainder)

Question: Given an integer array `nums` and an integer `k`, check whether there is a continuous subarray of length at least `2` whose sum is a multiple of `k`.

Mathematically:

\\[ subarraySum \bmod k = 0 \\]

Example:

- Input: `nums = [23,2,4,6,7]`, `k = 6`
- Output: `true`
- Explanation: Subarray `[2,4]` ka sum `6` hai, jo `6` se divisible hai.

## 1. Brute Force — Nested Loops

Logic:

1. Har index se subarray start karo.
2. Running sum calculate karo.
3. Subarray ki length at least `2` ho aur sum `k` se divisible ho, toh `true` return karo.

```
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {

        for (int i = 0; i < nums.length; i++) {
            int sum = 0;

            for (int j = i; j < nums.length; j++) {
                sum += nums[j];

                if (j - i + 1 >= 2 && sum % k == 0) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(1)\\)

Note: LeetCode constraints में `k` positive है, इसलिए यहाँ `k = 0` की समस्या नहीं है।

## 2. Optimal — Prefix Sum + HashMap

Logic:

अगर दो prefix sums का remainder same है, तो उनके बीच के subarray का sum `k` से divisible होगा।

\\[ prefix[j]\\%k = prefix[i]\\%k \\]

इसलिए:

\\[ (prefix[j]-prefix[i])\\%k=0 \\]

Steps:

1. HashMap में remainder और उसका earliest index store करो।
2. Initially `map.put(0, -1)` रखो।
3. Running sum calculate करो।
4. `sum % k` निकालो।
5. Remainder पहले से मौजूद है और index distance कम से कम `2` है, तो `true` return करो।
6. नहीं तो remainder का earliest index store करो।

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();

        // Remainder 0 ka starting index -1
        map.put(0, -1);

        int sum = 0;

        for (int i = 0; i < nums.length; i++) {

            // Running prefix sum
            sum += nums[i];

            // Current remainder
            int remainder = sum % k;

            // Same remainder pehle mila hai?
            if (map.containsKey(remainder)) {

                // Subarray length at least 2 honi chahiye
                if (i - map.get(remainder) >= 2) {
                    return true;
                }

            } else {
                // Earliest index hi store rakho
                map.put(remainder, i);
            }
        }

        return false;
    }
}
```

- Time: \\(O(n)\\) average
- Space: \\(O(\min(n,k))\\) for positive `k`.

## Dry Run

Input: `nums = [23,2,4,6,7]`, `k = 6`

Initially: `map = {0=-1}`

| Index | Number | Prefix Sum | Remainder | Result                       |
| ----- | ------ | ---------- | --------- | ---------------------------- |
| 0     | 23     | 23         | 5         | Store 5 → 0                  |
| 1     | 2      | 25         | 1         | Store 1 → 1                  |
| 2     | 4      | 29         | 5         | Same remainder, distance = 2 |

Index `0` और `2` के बीच subarray `[2,4]` है।

\\[ 2+4=6,\quad 6\\%6=0 \\]

Output: true

## Comparison

| Approach             | Time               | Space              |
| -------------------- | ------------------ | ------------------ |
| Brute Force          | \\(O(n^2)\\)       | \\(O(1)\\)         |
| Prefix Sum + HashMap | \\(O(n)\\) average | \\(O(\min(n,k))\\) |

## याद रखने की लाइन

Continuous Subarray Sum → Same Remainder + Index Distance ≥ 2

- `map.put(0, -1)`
- `remainder = sum % k`
- Same remainder मिले → `i - previousIndex >= 2`
- First occurrence ही store रखो।

LC 560 और LC 523 में अंतर:

| LC 560                   | LC 523                        |
| ------------------------ | ----------------------------- |
| Subarray sum exactly `k` | Subarray sum divisible by `k` |
| `sum - k` search         | Same remainder search         |
| Frequency store          | Earliest index store          |
