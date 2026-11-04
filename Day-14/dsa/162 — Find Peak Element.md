# LeetCode 162 — Find Peak Element



Medium

Pattern: Binary Search



## 1. Problem क्या है?

एक array दिया है। हमें किसी एक Peak Element का index return करना है।

Peak Element वह होता है जो अपने दोनों neighbours से बड़ा हो।

\\[ nums[i] > nums[i-1] \quad \text{and} \quad nums[i] > nums[i+1] \\]

Example:

```
nums = [1, 2, 3, 1]

Output = 2

nums[2] = 3
```

क्योंकि `3 > 2` और `3 > 1`।

ध्यान दो:

- Array के बाहर elements को `-∞` माना जाता है।
- एक से ज्यादा peak हो सकते हैं।
- किसी भी एक peak का index return करना है।

## 2. Brute Force — Linear Search

हर element को उसके neighbours से compare करेंगे।

```
class Solution {
    public int findPeakElement(int[] nums) {

        int n = nums.length;

        for (int i = 0; i < n; i++) {

            // Left neighbour check
            boolean left = (i == 0 || nums[i] > nums[i - 1]);

            // Right neighbour check
            boolean right = (i == n - 1 || nums[i] > nums[i + 1]);

            if (left && right) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: O(n)
- Space: O(1)

## 3. Better Approach — Find Maximum Element

पूरे array में maximum element find करेंगे।

अगर array में सभी adjacent elements अलग-अलग हैं, तो global maximum हमेशा peak होगा।

```
class Solution {
    public int findPeakElement(int[] nums) {

        int maxIndex = 0;

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] > nums[maxIndex]) {
                maxIndex = i;
            }
        }

        return maxIndex;
    }
}
```

- Time: O(n)
- Space: O(1)

Brute और Better दोनों O(n) हैं। Better approach में neighbour checks की जगह केवल maximum element find करते हैं। Asymptotically दोनों की complexity समान है।

## 4. Optimal Approach — Binary Search

Main Logic: Array में किसी भी peak तक पहुँचने के लिए slope का उपयोग करेंगे।

- अगर `nums[mid] < nums[mid + 1]` है, तो हम upward slope पर हैं। Right side में कोई peak जरूर मिलेगी।
- अगर `nums[mid] > nums[mid + 1]` है, तो हम downward slope पर हैं। Left side में या `mid` पर peak मिलेगी।

### Java Code (LeetCode)

```
class Solution {
    public int findPeakElement(int[] nums) {

        int low = 0;
        int high = nums.length - 1;

        while (low < high) {

            int mid = low + (high - low) / 2;

            // Upward slope: Right side search
            if (nums[mid] < nums[mid + 1]) {
                low = mid + 1;
            }

            // Downward slope: Left side search
            else {
                high = mid;
            }
        }

        return low;
    }
}
```

- Time: O(log n)
- Space: O(1)

## 5. Dry Run

```
nums = [1, 2, 3, 1]

low = 0, high = 3

Step 1:
mid = 1
nums[1] = 2
nums[2] = 3

2 < 3
Right side search

low = 2

Step 2:
low = 2, high = 3
mid = 2

nums[2] = 3
nums[3] = 1

3 > 1
Left side search

high = 2

Now low = high = 2

Output = 2
```

## 6. Complexity Comparison

| Approach                 | Time     | Space |
| ------------------------ | -------- | ----- |
| Brute Force              | O(n)     | O(1)  |
| Better (Maximum Element) | O(n)     | O(1)  |
| Optimal (Binary Search)  | O(log n) | O(1)  |

## 7. याद रखने की लाइन

Find Peak Element = Compare `nums[mid]` with `nums[mid + 1]`.

```
if (nums[mid] < nums[mid + 1]) {
    low = mid + 1;
} else {
    high = mid;
}
```

- Increasing slope → Right जाओ।
- Decreasing slope → Left जाओ।
- `low == high` → Peak index मिल गया।

Important: यहाँ `while (low < high)` इस्तेमाल होता है क्योंकि हम एक possible peak को search space में बनाए रखते हैं।

## Binary Search Pattern Revision

| LeetCode | Problem                              | Pattern                   |
| -------- | ------------------------------------ | ------------------------- |
| 704      | Binary Search                        | Normal                    |
| 35       | Search Insert Position               | Lower Bound               |
| 34       | First & Last Position                | Two Binary Searches       |
| 33       | Search in Rotated Sorted Array       | Modified                  |
| 153      | Find Minimum in Rotated Sorted Array | Modified                  |
| 162      | Find Peak Element                    | Slope-based Binary Search |
