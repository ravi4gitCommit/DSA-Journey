# LeetCode 4 — Median of Two Sorted Arrays



Hard

Pattern: Binary Search + Partition



## 1. Problem क्या है?

दो sorted arrays दिए गए हैं। हमें दोनों arrays को logically combine करके उनका Median find करना है।

Median:

- Odd number of elements → बीच वाला element।
- Even number of elements → बीच के दो elements का average।

Example 1:

```
nums1 = [1, 3]
nums2 = [2]

Combined = [1, 2, 3]

Output = 2.0
```

Example 2:

```
nums1 = [1, 2]
nums2 = [3, 4]

Combined = [1, 2, 3, 4]

Output = 2.5
```

## 2. Brute Force — Merge Both Arrays

दोनों sorted arrays को merge करके median निकालेंगे।

### Java Code

```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        int m = nums1.length;
        int n = nums2.length;

        int[] merged = new int[m + n];

        int i = 0;
        int j = 0;
        int k = 0;

        // Dono arrays merge karo
        while (i < m && j < n) {

            if (nums1[i] <= nums2[j]) {
                merged[k++] = nums1[i++];
            } else {
                merged[k++] = nums2[j++];
            }
        }

        // Remaining elements
        while (i < m) {
            merged[k++] = nums1[i++];
        }

        while (j < n) {
            merged[k++] = nums2[j++];
        }

        int total = m + n;

        // Odd length
        if (total % 2 == 1) {
            return merged[total / 2];
        }

        // Even length
        return (merged[total / 2 - 1] + 
                (double) merged[total / 2]) / 2.0;
    }
}
```

- Time: O(m + n)
- Space: O(m + n)

## 3. Better Approach — Without Merging

पूरा merged array बनाने की जरूरत नहीं है।

दो pointers से सिर्फ median तक elements traverse करेंगे।

- Odd length → बीच वाला element।
- Even length → बीच के दो elements।

### Java Code

```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        int m = nums1.length;
        int n = nums2.length;

        int total = m + n;

        int i = 0;
        int j = 0;

        int prev = 0;
        int curr = 0;

        // Sirf median tak traverse karo
        for (int count = 0; count <= total / 2; count++) {

            prev = curr;

            if (i < m && (j >= n || nums1[i] <= nums2[j])) {
                curr = nums1[i++];
            } else {
                curr = nums2[j++];
            }
        }

        // Odd length
        if (total % 2 == 1) {
            return curr;
        }

        // Even length
        return (prev + (double) curr) / 2.0;
    }
}
```

- Time: O(m + n)
- Space: O(1)

## 4. Optimal Approach — Binary Search + Partition

Most Important

Main Idea: दोनों arrays को ऐसे partition करना है कि:

1. Left side में कुल elements = Right side में कुल elements (या odd case में left में एक extra)।
2. Left side का maximum element, Right side के minimum element से छोटा या बराबर हो।

हमें छोटी array पर Binary Search लगानी है।

### Partition समझो

```
nums1 = [1, 3]
nums2 = [2, 4, 5]

Left Part:  [1, 2, 3]
Right Part: [4, 5]
```

- `maxLeft = 3`
- `minRight = 4`

क्योंकि `3 <= 4`, partition correct है।

Median = `4` (कुल elements odd हैं)।

### Partition Formula

मान लो:

- `m` = nums1 length
- `n` = nums2 length
- `totalLeft = (m + n + 1) / 2`
- `cut1` = nums1 में left partition के elements
- `cut2` = nums2 में left partition के elements

```
cut2 = totalLeft - cut1
```

### Partition Conditions

```
nums1Left <= nums2Right

nums2Left <= nums1Right
```

अगर ये दोनों conditions true हैं, partition correct है।

### Java Code (LeetCode)

```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {

        // Binary Search hamesha chhoti array par
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1);
        }

        int m = nums1.length;
        int n = nums2.length;

        int low = 0;
        int high = m;

        int totalLeft = (m + n + 1) / 2;

        while (low <= high) {

            int cut1 = low + (high - low) / 2;
            int cut2 = totalLeft - cut1;

            // Left aur Right ke boundary elements
            int left1 = (cut1 == 0)
                    ? Integer.MIN_VALUE : nums1[cut1 - 1];

            int right1 = (cut1 == m)
                    ? Integer.MAX_VALUE : nums1[cut1];

            int left2 = (cut2 == 0)
                    ? Integer.MIN_VALUE : nums2[cut2 - 1];

            int right2 = (cut2 == n)
                    ? Integer.MAX_VALUE : nums2[cut2];

            // Correct partition
            if (left1 <= right2 && left2 <= right1) {

                // Odd total elements
                if ((m + n) % 2 == 1) {
                    return Math.max(left1, left2);
                }

                // Even total elements
                int maxLeft = Math.max(left1, left2);
                int minRight = Math.min(right1, right2);

                return (maxLeft + (double) minRight) / 2.0;
            }

            // nums1 ka left part bada hai
            else if (left1 > right2) {
                high = cut1 - 1;
            }

            // nums1 ka left part chhota hai
            else {
                low = cut1 + 1;
            }
        }

        return 0.0;
    }
}
```

- Time: O(log(min(m, n)))
- Space: O(1)

## 5. Dry Run

```
nums1 = [1, 3]
nums2 = [2, 4, 5]

m = 2
n = 3

totalLeft = (2 + 3 + 1) / 2 = 3
```

Step 1:

```
cut1 = 1
cut2 = 2

nums1:
Left  = [1]
Right = [3]

nums2:
Left  = [2, 4]
Right = [5]
```

Boundary values:

```
left1  = 1
right1 = 3

left2  = 4
right2 = 5
```

Check:

```
left1 <= right2 → 1 <= 5 ✓

left2 <= right1 → 4 <= 3 ✗
```

Partition correct नहीं है। `left2 > right1`, इसलिए `cut1` बढ़ाना होगा।

Step 2:

```
cut1 = 2
cut2 = 1

nums1:
Left  = [1, 3]
Right = []

nums2:
Left  = [2]
Right = [4, 5]
```

Boundary values:

```
left1  = 3
right1 = +Infinity

left2  = 2
right2 = 4
```

Check:

```
3 <= 4 ✓
2 <= Infinity ✓
```

Partition correct!

Total elements = 5 (Odd)

```
Median = max(left1, left2)
       = max(3, 2)
       = 3
```

Output: 3.0

## 6. Complexity Comparison

| Approach                | Time             | Space  |
| ----------------------- | ---------------- | ------ |
| Brute Force (Merge)     | O(m+n)           | O(m+n) |
| Better (Two Pointers)   | O(m+n)           | O(1)   |
| Optimal (Binary Search) | O(log(min(m,n))) | O(1)   |

## 7. याद रखने की लाइन

Median of Two Sorted Arrays = Binary Search on Smaller Array + Correct Partition.

तीन बातें याद रखो:

1. हमेशा छोटी array पर Binary Search लगाओ।
2. `cut2 = totalLeft - cut1`
3. Correct partition:

```
left1 <= right2 && left2 <= right1
```

Median formula:

```
// Odd
Math.max(left1, left2);

// Even
(Math.max(left1, left2) + 
 Math.min(right1, right2)) / 2.0;
```

## Binary Search Pattern Revision

| LC   | Problem                     | Pattern                 |
| ---- | --------------------------- | ----------------------- |
| 704  | Binary Search               | Normal                  |
| 35   | Search Insert Position      | Lower Bound             |
| 34   | First & Last Position       | Two Binary Searches     |
| 33   | Search Rotated Array        | Modified                |
| 153  | Find Minimum                | Modified                |
| 162  | Find Peak                   | Slope-based             |
| 74   | Search 2D Matrix            | 2D Binary Search        |
| 875  | Koko Eating Bananas         | Binary Search on Answer |
| 1011 | Capacity to Ship            | Binary Search on Answer |
| 410  | Split Array Largest Sum     | Binary Search on Answer |
| 4    | Median of Two Sorted Arrays | Partition Binary Search |

Important: LC 4 का optimal approach बाकी Binary Search problems से अलग है। यहाँ target search नहीं करना है और न ही answer space search करना है — यहाँ सही partition find करना है।
