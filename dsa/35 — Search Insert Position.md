# LeetCode 35 — Search Insert Position



Easy

Pattern: Binary Search



## 1. Problem क्या है?

एक sorted array और `target` दिया है।

- Target मिल जाए तो उसका index return करो।
- Target नहीं मिले तो वह किस index पर insert होगा, वह return करो ताकि array sorted रहे।

Example:

```
nums = [1, 3, 5, 6]
target = 5

Output = 2
```

```
nums = [1, 3, 5, 6]
target = 2

Output = 1
```

## 2. Brute Force — Linear Search

हर element को check करेंगे। पहला ऐसा index जहाँ `nums[i] >= target` हो, वही answer है।

```
class Solution {
    public int searchInsert(int[] nums, int target) {

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] >= target) {
                return i;
            }
        }

        return nums.length;
    }
}
```

- Time: O(n)
- Space: O(1)

## 3. Optimal — Binary Search

Logic: हमें पहला ऐसा index चाहिए जहाँ `nums[index] >= target` हो।

1. `low = 0`, `high = n - 1`
2. Middle index निकालो।
3. अगर `nums[mid] >= target`, तो answer `mid` हो सकता है। Left side में search करो।
4. अगर `nums[mid] < target`, तो right side में search करो।
5. अंत में `low` ही सही insertion position होगा।

Java Code (LeetCode):

```
class Solution {
    public int searchInsert(int[] nums, int target) {

        int low = 0;
        int high = nums.length - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (nums[mid] >= target) {
                high = mid - 1;
            } 
            else {
                low = mid + 1;
            }
        }

        return low;
    }
}
```

- Time: O(log n)
- Space: O(1)

## 4. Dry Run

```
nums = [1, 3, 5, 6]
target = 2

Step 1:
low = 0, high = 3
mid = 1
nums[1] = 3

3 >= 2
high = mid - 1 = 0

Step 2:
low = 0, high = 0
mid = 0
nums[0] = 1

1 < 2
low = mid + 1 = 1

Loop समाप्त (low > high)

Output = 1
```

## 5. Complexity Comparison

| Approach      | Time     | Space |
| ------------- | -------- | ----- |
| Linear Search | O(n)     | O(1)  |
| Binary Search | O(log n) | O(1)  |

## याद रखने की लाइन

Search Insert Position = First index where `nums[i] >= target`.

- Target मिल जाए → उसका index।
- Target न मिले → insertion index।
- सभी elements छोटे हों → `nums.length`।

Important: LC 704 और LC 35 दोनों Binary Search हैं, लेकिन LC 35 में हमें `low` return करना है, `-1` नहीं।
