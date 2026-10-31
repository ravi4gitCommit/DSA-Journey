# LeetCode 303 — Range Sum Query - Immutable

Pattern: Prefix Sum

Question: Given an integer array `nums`, answer multiple queries of the form:

`sumRange(left, right)`

Return the sum of elements from index `left` to `right` (both inclusive).

Example:

Input: `nums = [-2, 0, 3, -5, 2, -1]`

- `sumRange(0, 2)` → `1`
- `sumRange(2, 5)` → `-1`
- `sumRange(0, 5)` → `-3`

## 1. Brute Force — Direct Sum

Logic: हर query में `left` से `right` तक loop चलाकर sum calculate करो।

```
class NumArray {
    private int[] nums;

    public NumArray(int[] nums) {
        this.nums = nums;
    }

    public int sumRange(int left, int right) {
        int sum = 0;

        for (int i = left; i <= right; i++) {
            sum += nums[i];
        }

        return sum;
    }
}
```

- Constructor: \\(O(1)\\)
- Each Query: \\(O(n)\\) worst case
- Extra Space: \\(O(1)\\)

## 2. Optimal — Prefix Sum

Logic:

Prefix sum array बनाओ, जिसमें:

\\[ prefix[i] = nums[0]+nums[1]+\dots+nums[i-1] \\]

इसमें `prefix` का size `n + 1` होगा।

Range sum निकालने का formula:

\\[ \boxed{sumRange(left,right)=prefix[right+1]-prefix[left]} \\]

```
class NumArray {
    private int[] prefix;

    public NumArray(int[] nums) {
        int n = nums.length;

        prefix = new int[n + 1];

        // Prefix sum calculate karo
        for (int i = 0; i < n; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }
    }

    public int sumRange(int left, int right) {
        return prefix[right + 1] - prefix[left];
    }
}
```

- Constructor: \\(O(n)\\)
- Each Query: \\(O(1)\\)
- Extra Space: \\(O(n)\\)

## Dry Run

Input:

`nums = [-2, 0, 3, -5, 2, -1]`

Step 1 — Prefix Sum Array

| Index | nums | prefix |
| ----- | ---- | ------ |
| 0     | -2   | 0      |
| 1     | 0    | -2     |
| 2     | 3    | -2     |
| 3     | -5   | 1      |
| 4     | 2    | -4     |
| 5     | -1   | -2     |

`prefix = [0, -2, -2, 1, -4, -2, -3]`

Step 2 — Query: `sumRange(2, 5)`

\\[ =prefix[6]-prefix[2] \\]

\\[ =-3-(-2)=\boxed{-1} \\]

Output: -1

## Comparison

| Approach    | Preprocessing | Each Query | Extra Space |
| ----------- | ------------- | ---------- | ----------- |
| Brute Force | \\(O(1)\\)    | \\(O(n)\\) | \\(O(1)\\)  |
| Prefix Sum  | \\(O(n)\\)    | \\(O(1)\\) | \\(O(n)\\)  |

## याद रखने की लाइन

Range Sum Query → Prefix Sum → Right Prefix − Left Prefix

\\[ \boxed{prefix[right+1]-prefix[left]} \\]

Important: जब एक ही array पर multiple range-sum queries पूछी जाएँ, तब Prefix Sum बहुत useful होता है।
