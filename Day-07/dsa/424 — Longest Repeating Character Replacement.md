# LeetCode 424 — Longest Repeating Character Replacement

Pattern: Variable-Size Sliding Window + Frequency Count

Question: String `s` aur integer `k` diya hai. Maximum length ka substring find karo jisme maximum `k` characters ko kisi bhi uppercase English letter se replace karke saare characters same banaye ja sakein.

Example:

```
Input:  s = "AABABBA", k = 1
Output: 4
```

Explanation: `"AABA"` mein ek `B` ko `A` se replace karne par `"AAAA"` milta hai. Isliye maximum length `4` hai.

## 1. Brute Force Approach

Har starting index se substring banao, character frequencies count karo aur check karo ki use same character banane ke liye kitne replacements chahiye.

Formula:

\\[ \text{replacements}=\text{windowLength}-\text{maxFrequency} \\]

Agar replacements `k` se zyada nahi hain, toh window valid hai.

```
class Solution {
    public int characterReplacement(String s, int k) {
        int n = s.length();
        int maxLength = 0;

        for (int left = 0; left < n; left++) {
            int[] freq = new int[26];
            int maxFreq = 0;

            for (int right = left; right < n; right++) {
                int index = s.charAt(right) - 'A';
                freq[index]++;

                maxFreq = Math.max(maxFreq, freq[index]);

                int windowLength = right - left + 1;
                int replacements = windowLength - maxFreq;

                if (replacements <= k) {
                    maxLength = Math.max(maxLength, windowLength);
                }
            }
        }

        return maxLength;
    }
}
```

- Time: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\), because frequency array has 26 entries.

## 2. Better Approach — Sliding Window with `while`

Window expand karo. Agar replacements `k` se zyada ho jaayein, toh left pointer move karke window valid banao.

```
class Solution {
    public int characterReplacement(String s, int k) {
        int[] freq = new int[26];

        int left = 0;
        int maxFreq = 0;
        int maxLength = 0;

        for (int right = 0; right < s.length(); right++) {
            int index = s.charAt(right) - 'A';
            freq[index]++;

            maxFreq = Math.max(maxFreq, freq[index]);

            // Window ko valid banaye rakho
            while ((right - left + 1) - maxFreq > k) {
                freq[s.charAt(left) - 'A']--;
                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

## 3. Optimal Approach — Sliding Window with `if` ⭐

Is problem mein maximum length chahiye. Hum `maxFreq` ko har shrink ke baad dobara calculate karne ki jagah uski historical maximum value maintain kar sakte hain.

Agar window invalid ho, toh `left` ko ek step move kar do. Window ki length kabhi increase nahi hoti jab tak right pointer usse expand na kare.

```
class Solution {
    public int characterReplacement(String s, int k) {
        int[] freq = new int[26];

        int left = 0;
        int maxFreq = 0;

        for (int right = 0; right < s.length(); right++) {
            int index = s.charAt(right) - 'A';
            freq[index]++;

            // Ab tak dekhi gayi maximum character frequency
            maxFreq = Math.max(maxFreq, freq[index]);

            int windowLength = right - left + 1;

            // Agar replacements k se zyada hain, left ko move karo
            if (windowLength - maxFreq > k) {
                freq[s.charAt(left) - 'A']--;
                left++;
            }
        }

        return s.length() - left;
    }
}
```

- Time: \\(O(n)\\)
- Extra Space: \\(O(1)\\)

Important: Optimal `if` solution mein `maxFreq` historical maximum hai; woh current window ki exact maximum frequency hona zaroori nahi. Is problem mein yeh approach maximum length return karne ke liye valid hai.

## Dry Run — Optimal Approach

Input: `s = "AABABBA"`, `k = 1`

| Window  | Length | `maxFreq` | Replacements needed | Action                  |
| ------- | ------ | --------- | ------------------- | ----------------------- |
| `A`     | 1      | 1         | 0                   | Valid                   |
| `AA`    | 2      | 2         | 0                   | Valid                   |
| `AAB`   | 3      | 2         | 1                   | Valid                   |
| `AABA`  | 4      | 3         | 1                   | Valid                   |
| `AABAB` | 5      | 3         | 2                   | Shrink                  |
| `ABAB`  | 4      | 3         | 1                   | Valid length maintained |
| `BABBA` | 5      | 3         | 2                   | Shrink                  |

Output: `4`

## Approach Comparison

| Approach                 | Time         | Extra Space |
| ------------------------ | ------------ | ----------- |
| Brute Force              | \\(O(n^2)\\) | \\(O(1)\\)  |
| Sliding Window (`while`) | \\(O(n)\\)   | \\(O(1)\\)  |
| Sliding Window (`if`)    | \\(O(n)\\)   | \\(O(1)\\)  |

## Yaad rakhne ki line

LeetCode 424 = Variable-Size Sliding Window + Frequency Count.

\\[ \text{Replacements}=\text{Window Length}-\text{Max Frequency} \\]

- Agar replacements `k` se zyada hain → window shrink karo.
- Warna maximum length update karo.

Yeh formula tab kaam karta hai jab hum window ke sabhi characters ko uske most frequent character mein replace karte hain.
