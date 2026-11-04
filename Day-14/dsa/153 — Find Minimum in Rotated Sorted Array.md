# LeetCode 153 — Find Minimum in Rotated Sorted Array



Medium

Pattern: Modified Binary Search



## 1. Problem क्या है?

एक sorted array को किसी pivot से rotate किया गया है। हमें उसमें से minimum element find करना है।

Example:

```
nums = [3, 4, 5, 1, 2]

Output = 1
```

एक और example:

```
nums = [4, 5, 6, 7, 0, 1, 2]

Output = 0
```

## 2. Brute Force — Linear Search

पूरे array में minimum element find करेंगे।

```
class Solution {
    public int findMin(int[] nums) {

        int min = nums[0];

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] < min) {
                min = nums[i];
            }
        }

        return min;
    }
}
```

- Time: O(n)
- Space: O(1)

## 3. Optimal — Binary Search

Main Logic: Rotated sorted array में minimum element उस हिस्से में होगा जो सही sorted order से rotate होकर शुरुआत में आया है।

हम `nums[mid]` की तुलना `nums[high]` से करेंगे।

1. अगर `nums[mid] > nums[high]` है, तो minimum Right Half में होगा।
   - `low = mid + 1`
2. अगर `nums[mid] <= nums[high]` है, तो minimum Left Half में या `mid` पर होगा।
   - `high = mid`
3. जब `low == high` होगा, वही minimum element होगा।

### Java Code (LeetCode)

```
class Solution {
    public int findMin(int[] nums) {

        int low = 0;
        int high = nums.length - 1;

        while (low < high) {

            int mid = low + (high - low) / 2;

            // Minimum right half mein hai
            if (nums[mid] > nums[high]) {
                low = mid + 1;
            }

            // Minimum left half mein ya mid par hai
            else {
                high = mid;
            }
        }

        return nums[low];
    }
}
```

- Time: O(log n)
- Space: O(1)

## 4. Dry Run

```
nums = [4, 5, 6, 7, 0, 1, 2]

low = 0, high = 6
```

| Step | low | mid | high | Comparison | Decision |
| ---- | --- | --- | ---- | ---------- | -------- |
| 1    | 0   | 3   | 6    | 7 > 2      | low = 4  |

\| 2 | 4 | 5 | 6 | 1  nums[high]) {\
low = mid + 1;\
} else {\
high = mid;\
}

```

ध्यान रखो:

- यहाँ `while (low < high)` है, `low <= high` नहीं।
- `high = mid` होगा, `mid - 1` नहीं, क्योंकि minimum खुद `mid` पर हो सकता है।
- Array में elements distinct हैं (LeetCode 153)।

## Binary Search Revision

| Problem | LeetCode | Pattern |
|---|---:|---|
| Binary Search | 704 | Normal Binary Search |
| Search Insert Position | 35 | Lower Bound |
| First & Last Position | 34 | Two Binary Searches |
| Search in Rotated Sorted Array | 33 | Modified Binary Search |
| Find Minimum in Rotated Sorted Array | 153 | Modified Binary Search |

**Next:** Find Minimum in Rotated Sorted Array II — LeetCode 154.
```
