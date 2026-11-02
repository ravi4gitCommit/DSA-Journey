# LeetCode 33 — Search in Rotated Sorted Array



Medium

Pattern: Modified Binary Search



## 1. Problem क्या है?

एक sorted array को किसी pivot से rotate किया गया है। तुम्हें `target` find करना है।

- Target मिल जाए → उसका index return करो।
- नहीं मिले → `-1` return करो।

Example:

```
nums = [4, 5, 6, 7, 0, 1, 2]
target = 0

Output = 4
```

## 2. Brute Force — Linear Search

हर element को check करेंगे।

```
class Solution {
    public int search(int[] nums, int target) {

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] == target) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: O(n)
- Space: O(1)

## 3. Optimal — Modified Binary Search

Main Logic: हर बार array के दो हिस्सों में से कम-से-कम एक हिस्सा sorted होगा।

1. `low`, `high` और `mid` निकालो।
2. अगर `nums[mid] == target`, तो `mid` return करो।
3. Check करो कि Left Half sorted है या Right Half।
4. Sorted half में target मौजूद है या नहीं, उसके आधार पर search space छोटा करो।

### Java Code (LeetCode)

```
class Solution {
    public int search(int[] nums, int target) {

        int low = 0;
        int high = nums.length - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            // Target found
            if (nums[mid] == target) {
                return mid;
            }

            // Check Left Half is sorted
            if (nums[low] <= nums[mid]) {

                // Target Left Half mein hai
                if (target >= nums[low] && target < nums[mid]) {
                    high = mid - 1;
                }

                // Target Right Half mein hai
                else {
                    low = mid + 1;
                }
            }

            // Otherwise Right Half is sorted
            else {

                // Target Right Half mein hai
                if (target > nums[mid] && target <= nums[high]) {
                    low = mid + 1;
                }

                // Target Left Half mein hai
                else {
                    high = mid - 1;
                }
            }
        }

        return -1;
    }
}
```

- Time: O(log n)
- Space: O(1)

## 4. Dry Run

```
nums = [4, 5, 6, 7, 0, 1, 2]
target = 0
```

| Step | low | mid | high | Decision                      |
| ---- | --- | --- | ---- | ----------------------------- |
| 1    | 0   | 3   | 6    | Left sorted, target right में |
| 2    | 4   | 5   | 6    | Right sorted, target left में |
| 3    | 4   | 4   | 4    | Target found                  |

```
nums[4] = 0

Output = 4
```

## 5. Complexity Comparison

| Approach               | Time     | Space |
| ---------------------- | -------- | ----- |
| Brute Force            | O(n)     | O(1)  |
| Modified Binary Search | O(log n) | O(1)  |

## याद रखने की लाइन

Rotated Sorted Array = Identify Sorted Half + Check Target Range + Eliminate Half.

```
if (nums[low] <= nums[mid]) {
    // Left half sorted
} else {
    // Right half sorted
}
```

Important: LeetCode 33 में सभी elements distinct हैं। इसलिए `nums[low] <= nums[mid]` से sorted half identify कर सकते हैं।

### Binary Search Pattern Revision

| LeetCode | Concept                        |
| -------- | ------------------------------ |
| 704      | Normal Binary Search           |
| 35       | Search Insert Position         |
| 34       | First & Last Occurrence        |
| 33       | Search in Rotated Sorted Array |
