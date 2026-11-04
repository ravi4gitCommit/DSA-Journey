# LeetCode 74 — Search a 2D Matrix



Medium

Pattern: Binary Search



## 1. Problem क्या है?

एक `m × n` matrix दिया है। हमें check करना है कि `target` matrix में मौजूद है या नहीं।

इस matrix की दो conditions हैं:

1. हर row ascending order में sorted है।
2. हर row का पहला element, पिछली row के last element से बड़ा है।

Example:

```
matrix = [
  [1,  3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 60]
]

target = 3

Output = true
```

## 2. Brute Force — Linear Search

हर row और हर column में target search करेंगे।

```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {

        for (int i = 0; i < matrix.length; i++) {

            for (int j = 0; j < matrix[i].length; j++) {

                if (matrix[i][j] == target) {
                    return true;
                }
            }
        }

        return false;
    }
}
```

- Time: O(m × n)
- Space: O(1)

## 3. Better Approach — Row Selection + Binary Search

पहले उस row को find करेंगे जिसमें target हो सकता है।

फिर उस row में Binary Search लगाएंगे।

Logic:

- अगर target, किसी row के first और last element के बीच है, तो उसी row में search करो।
- उस row में target नहीं मिले तो `false` return करो।

### Java Code

```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {

        int rows = matrix.length;
        int cols = matrix[0].length;

        // Correct row find karo
        for (int i = 0; i < rows; i++) {

            if (target >= matrix[i][0] &&
                target <= matrix[i][cols - 1]) {

                int low = 0;
                int high = cols - 1;

                // Selected row mein Binary Search
                while (low <= high) {

                    int mid = low + (high - low) / 2;

                    if (matrix[i][mid] == target) {
                        return true;
                    }

                    else if (matrix[i][mid] < target) {
                        low = mid + 1;
                    }

                    else {
                        high = mid - 1;
                    }
                }

                return false;
            }
        }

        return false;
    }
}
```

- Time: O(m + log n)
- Space: O(1)

## 4. Optimal Approach — Single Binary Search

Main Idea: पूरे 2D Matrix को एक sorted 1D array की तरह treat कर सकते हैं।

अगर matrix में `m` rows और `n` columns हैं, तो total elements:

\\[ N = m \times n \\]

1D index `mid` को 2D index में convert करेंगे:

\\[ row = mid / n \\]

\\[ col = mid \\% n \\]

### Java Code (LeetCode)

```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {

        int rows = matrix.length;
        int cols = matrix[0].length;

        int low = 0;
        int high = rows * cols - 1;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            // 1D index ko 2D index mein convert karo
            int row = mid / cols;
            int col = mid % cols;

            if (matrix[row][col] == target) {
                return true;
            }

            else if (matrix[row][col] < target) {
                low = mid + 1;
            }

            else {
                high = mid - 1;
            }
        }

        return false;
    }
}
```

- Time: O(log(m × n))
- Space: O(1)

## 5. Dry Run

```
matrix = [
  [1,  3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 60]
]

target = 3
```

Total elements = 3 × 4 = 12

```
low = 0
high = 11

Step 1:
mid = 5

row = 5 / 4 = 1
col = 5 % 4 = 1

matrix[1][1] = 11

11 > 3
high = 4

Step 2:
low = 0, high = 4
mid = 2

row = 2 / 4 = 0
col = 2 % 4 = 2

matrix[0][2] = 5

5 > 3
high = 1

Step 3:
low = 0, high = 1
mid = 0

row = 0 / 4 = 0
col = 0 % 4 = 0

matrix[0][0] = 1

1 < 3
low = 1

Step 4:
low = 1, high = 1
mid = 1

matrix[0][1] = 3

Target found!
```

Output: true

## 6. Complexity Comparison

| Approach    | Time          | Space |
| ----------- | ------------- | ----- |
| Brute Force | O(m × n)      | O(1)  |
| Better      | O(m + log n)  | O(1)  |
| Optimal     | O(log(m × n)) | O(1)  |

## 7. याद रखने की लाइन

2D Matrix Binary Search = 1D Index को 2D Index में Convert करो।

```
int row = mid / cols;
int col = mid % cols;
```

- `mid / cols` → Row
- `mid % cols` → Column

Important: यह approach LeetCode 74 की दोनों sorted-matrix conditions पर depend करती है। इसे हर sorted 2D matrix पर apply नहीं कर सकते।

## Binary Search Pattern Revision

| LeetCode | Problem                 | Pattern               |
| -------- | ----------------------- | --------------------- |
| 704      | Binary Search           | Normal                |
| 35       | Search Insert Position  | Lower Bound           |
| 34       | First & Last Position   | Two Binary Searches   |
| 33       | Search in Rotated Array | Modified              |
| 153      | Find Minimum            | Modified              |
| 162      | Find Peak Element       | Slope-based           |
| 74       | Search a 2D Matrix      | 2D → 1D Binary Search |
