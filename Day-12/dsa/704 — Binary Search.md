# LeetCode 704 — Binary Search



Easy

Pattern: Binary Search



## 1. Problem क्या है?

एक sorted array और एक `target` दिया है। Target मिल जाए तो उसका index return करो, नहीं तो `-1`।

Example:

```
nums = [-1, 0, 3, 5, 9, 12]
target = 9

Output = 4
```

## 2. Brute Force — Linear Search

हर element को एक-एक करके check करेंगे।

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

## 3. Optimal — Binary Search

Logic: Sorted array होने के कारण हर बार search space आधा कर सकते हैं।

1. `low = 0`, `high = n - 1`
2. Middle index निकालो।
3. अगर `nums[mid] == target`, तो `mid` return करो।
4. अगर target छोटा है, तो left half में जाओ: `high = mid - 1`
5. अगर target बड़ा है, तो right half में जाओ: `low = mid + 1`
6. Target नहीं मिला तो `-1` return करो।

Java Code (LeetCode):

```
class Solution {
    public int search(int[] nums, int target) {

        int low = 0;
        int high = nums.length - 1;

        while (low <= high) {

            // Middle index
            int mid = low + (high - low) / 2;

            if (nums[mid] == target) {
                return mid;
            }

            // Target left side mein hai
            else if (target < nums[mid]) {
                high = mid - 1;
            }

            // Target right side mein hai
            else {
                low = mid + 1;
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
nums = [-1, 0, 3, 5, 9, 12]
target = 9

Step 1:
low = 0, high = 5
mid = 2
nums[2] = 3

9 > 3 → Right side

Step 2:
low = 3, high = 5
mid = 4
nums[4] = 9

Target found!
Output = 4
```

## 5. Complexity Comparison

| Approach      | Time     | Space |
| ------------- | -------- | ----- |
| Brute Force   | O(n)     | O(1)  |
| Binary Search | O(log n) | O(1)  |

## याद रखने की लाइन

Binary Search = Sorted Array + Middle Check + Search Space Half

```
while (low <= high) {
    int mid = low + (high - low) / 2;

    if (nums[mid] == target)
        return mid;

    else if (target < nums[mid])
        high = mid - 1;

    else
        low = mid + 1;
}
return -1;
```

Important: Binary Search लगाने के लिए array sorted होना चाहिए।
