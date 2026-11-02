# LeetCode 34 — Find First and Last Position of Element in Sorted Array



Medium

Pattern: Binary Search



## 1. Problem क्या है?

एक sorted array और `target` दिया है। Target की पहली और आखिरी position find करनी है।

अगर target नहीं मिलता है, तो `[-1, -1]` return करो।

Example:

```
nums = [5, 7, 7, 8, 8, 10]
target = 8

Output = [3, 4]
```

## 2. Brute Force — Linear Search

पूरे array में target की पहली और आखिरी position find करेंगे।

```
class Solution {
    public int[] searchRange(int[] nums, int target) {

        int first = -1;
        int last = -1;

        for (int i = 0; i < nums.length; i++) {

            if (nums[i] == target) {

                if (first == -1) {
                    first = i;
                }

                last = i;
            }
        }

        return new int[]{first, last};
    }
}
```

- Time: O(n)
- Space: O(1)

## 3. Optimal — Two Binary Searches

Main Idea: Binary Search दो बार लगाएँगे।

1. First Binary Search → Target की पहली position।
2. Second Binary Search → Target की आखिरी position।

### Java Code (LeetCode)

```
class Solution {

    public int[] searchRange(int[] nums, int target) {

        int first = findFirst(nums, target);
        int last = findLast(nums, target);

        return new int[]{first, last};
    }

    // First occurrence
    private int findFirst(int[] nums, int target) {

        int low = 0;
        int high = nums.length - 1;
        int answer = -1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (nums[mid] == target) {

                answer = mid;
                high = mid - 1; // Left side search

            } else if (nums[mid] < target) {

                low = mid + 1;

            } else {

                high = mid - 1;
            }
        }

        return answer;
    }

    // Last occurrence
    private int findLast(int[] nums, int target) {

        int low = 0;
        int high = nums.length - 1;
        int answer = -1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            if (nums[mid] == target) {

                answer = mid;
                low = mid + 1; // Right side search

            } else if (nums[mid] < target) {

                low = mid + 1;

            } else {

                high = mid - 1;
            }
        }

        return answer;
    }
}
```

- Time: O(log n) — दो Binary Searches।
- Space: O(1)

## 4. Dry Run

```
nums = [5, 7, 7, 8, 8, 10]
target = 8
```

First Occurrence:

```
mid = 2 → nums[2] = 7
7 < 8 → Right

mid = 4 → nums[4] = 8
Found → answer = 4
Left side search

mid = 3 → nums[3] = 8
Found → answer = 3
Left side search

Output = 3
```

Last Occurrence:

```
mid = 2 → nums[2] = 7
7 < 8 → Right

mid = 4 → nums[4] = 8
Found → answer = 4
Right side search

mid = 5 → nums[5] = 10
10 > 8 → Left

Output = 4
```

Final Output

```
[3, 4]
```

## 5. Complexity Comparison

| Approach            | Time     | Space |
| ------------------- | -------- | ----- |
| Linear Search       | O(n)     | O(1)  |
| Two Binary Searches | O(log n) | O(1)  |

## याद रखने की लाइन

First Occurrence: Target मिले तो `answer = mid`, फिर `high = mid - 1`।

Last Occurrence: Target मिले तो `answer = mid`, फिर `low = mid + 1`।

| Problem | Binary Search का काम   |
| ------- | ---------------------- |
| LC 704  | Target का index        |
| LC 35   | Insertion position     |
| LC 34   | First और Last position |

Key Concept: Duplicate elements होने पर एक बार मिलते ही रुकना नहीं है। First के लिए Left जाओ और Last के लिए Right जाओ।
