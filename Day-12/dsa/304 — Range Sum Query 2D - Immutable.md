# LeetCode 304 — Range Sum Query 2D - Immutable

Pattern: 2D Prefix Sum (Matrix Prefix Sum)

Question: Given a 2D matrix, answer multiple queries:

`sumRegion(row1, col1, row2, col2)`

Return the sum of all elements inside the rectangle from `(row1, col1)` to `(row2, col2)`, both inclusive.

Example:

Input matrix:

```
[
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]
```

Query: `sumRegion(2, 1, 4, 3)`

Output: `8`

## 1. Brute Force — Direct Matrix Traversal

Logic:

1. Query में दिए गए `row1` से `row2` तक loop चलाओ।
2. हर row में `col1` से `col2` तक elements जोड़ो।
3. Total sum return करो।

```
class NumMatrix {
    private int[][] matrix;

    public NumMatrix(int[][] matrix) {
        this.matrix = matrix;
    }

    public int sumRegion(int row1, int col1,
                         int row2, int col2) {

        int sum = 0;

        for (int i = row1; i <= row2; i++) {
            for (int j = col1; j <= col2; j++) {
                sum += matrix[i][j];
            }
        }

        return sum;
    }
}
```

- Constructor: \\(O(1)\\)
- Each Query: \\(O(m \times n)\\) worst case
- Extra Space: \\(O(1)\\)

## 2. Optimal — 2D Prefix Sum

Logic: एक 2D prefix sum array बनाओ। इससे किसी भी rectangular region का sum केवल 4 prefix values की मदद से निकाल सकते हैं।

Step 1 — Prefix Sum Formula

\\[ prefix[i][j] = matrix[0][0]\text{ से }matrix[i-1][j-1]\text{ तक का sum} \\]

इसलिए prefix array का size `(m+1) × (n+1)` होगा।

\\[ \boxed{ P[i][j]=matrix[i-1][j-1]+P[i-1][j]+P[i][j-1]-P[i-1][j-1] } \\]

Step 2 — Range Sum Formula

\\[ \boxed{ \begin{aligned} sumRegion ={}& P[row2+1][col2+1]\\\ &-P[row1][col2+1]\\\ &-P[row2+1][col1]\\\ &+P[row1][col1] \end{aligned} } \\]

ध्यान रखो: Last में `P[row1][col1]` को add करते हैं, क्योंकि यह area दो बार subtract हो गया था।

### Complete Java Code

```
class NumMatrix {
    private int[][] prefix;

    public NumMatrix(int[][] matrix) {
        int m = matrix.length;
        int n = matrix[0].length;

        prefix = new int[m + 1][n + 1];

        // 2D Prefix Sum calculate karo
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {

                prefix[i][j] =
                    matrix[i - 1][j - 1]
                    + prefix[i - 1][j]
                    + prefix[i][j - 1]
                    - prefix[i - 1][j - 1];
            }
        }
    }

    public int sumRegion(int row1, int col1,
                         int row2, int col2) {

        return prefix[row2 + 1][col2 + 1]
             - prefix[row1][col2 + 1]
             - prefix[row2 + 1][col1]
             + prefix[row1][col1];
    }
}
```

- Constructor: \\(O(m \times n)\\)
- Each Query: \\(O(1)\\)
- Extra Space: \\(O(m \times n)\\)

## Dry Run

Query: `sumRegion(2, 1, 4, 3)`

[Range Sum Query 2D - Immutable - LeetCode](https://images.openai.com/static-rsc-4/pAH6fdp2331PYBcmtl_bWLSzDAr_i3xDCpCgQiV3Wm9u1J-X5m7ntMZPsgVUWAs-cwR8xY6Wmxq8hZxChYAHIPUn1-UZQ9g4vka4Z1wE46CDpqDioQVTHB661K3UTrl7T8qp8vxHFJuHYDN5-UODalT6DSoSt2I1bhvm9dRVC3Y?purpose=inline)

[leetcode.com](https://leetcode.com/problems/range-sum-query-2d-immutable/discuss/572648/C%2B%2BJavaPython-Prefix-sum-with-Picture-explain-Clean-and-Concise?utm_source=chatgpt.com)

Required rectangle:

```
[
  [2, 0, 1],
  [1, 0, 1],
  [0, 3, 0]
]
```

Sum:

\\[ 2+0+1+1+0+1+0+3+0=\boxed{8} \\]

Using the prefix formula:

\\[ P[5][4]-P[2][4]-P[5][1]+P[2][1] \\]

\\[ =8 \\]

Output: 8

## Comparison

| Approach      | Preprocessing | Each Query  | Extra Space |
| ------------- | ------------- | ----------- | ----------- |
| Brute Force   | \\(O(1)\\)    | \\(O(mn)\\) | \\(O(1)\\)  |
| 2D Prefix Sum | \\(O(mn)\\)   | \\(O(1)\\)  | \\(O(mn)\\) |

## याद रखने की लाइन

2D Prefix Sum → Bottom Right − Top − Left + Overlap

\\[ \boxed{A-B-C+D} \\]

- `A` = Bottom-right prefix
- `B` = Top area
- `C` = Left area
- `D` = Overlapping area (add back)

Important: LC 303 में 1D Prefix Sum था, जबकि LC 304 में 2D Prefix Sum है। दोनों का उद्देश्य multiple range queries को \\(O(1)\\) में answer करना है।
