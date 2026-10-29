# LeetCode 76 — Minimum Window Substring

Pattern: Variable-Size Sliding Window + Frequency Count

Question: Strings `s` aur `t` diye hain. `s` ka smallest substring return karo jisme `t` ke saare characters required frequency ke saath present hon. Agar possible nahi hai, toh `""` return karo.

```
Input:  s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
```

## 1. Brute Force Approach

Har starting index se substring check karo. Jab saare required characters mil jaayein, minimum length update karo.

```
class Solution {
    public String minWindow(String s, String t) {
        int n = s.length();
        int minLength = Integer.MAX_VALUE;
        int start = 0;

        for (int left = 0; left < n; left++) {
            int[] freq = new int[128];

            for (int i = 0; i < t.length(); i++) {
                freq[t.charAt(i)]++;
            }

            int missing = t.length();

            for (int right = left; right < n; right++) {
                char ch = s.charAt(right);

                if (freq[ch] > 0) {
                    missing--;
                }
                freq[ch]--;

                if (missing == 0) {
                    int length = right - left + 1;

                    if (length < minLength) {
                        minLength = length;
                        start = left;
                    }
                    break;
                }
            }
        }

        return minLength == Integer.MAX_VALUE
                ? "" : s.substring(start, start + minLength);
    }
}
```

- Time: \\(O(n^2+m)\\)
- Extra Space: \\(O(1)\\), fixed ASCII character set ke liye.

## 2. Optimal Approach — Sliding Window + Frequency Array ⭐

Is problem ka standard optimal solution sliding window hai.

- Expand: Right pointer se character add karo.
- Valid: Jab `missing == 0`, saare required characters mil gaye.
- Shrink: Left pointer se characters remove karke smallest valid window find karo.

```
class Solution {
    public String minWindow(String s, String t) {
        if (t.length() > s.length()) {
            return "";
        }

        int[] need = new int[128];

        // t ke required characters ki frequency
        for (int i = 0; i < t.length(); i++) {
            need[t.charAt(i)]++;
        }

        int left = 0;
        int missing = t.length();
        int start = 0;
        int minLength = Integer.MAX_VALUE;

        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);

            if (need[ch] > 0) {
                missing--;
            }
            need[ch]--;

            // Window valid hai; ab smallest window dhoondho
            while (missing == 0) {
                int length = right - left + 1;

                if (length < minLength) {
                    minLength = length;
                    start = left;
                }

                char leftChar = s.charAt(left);
                need[leftChar]++;

                if (need[leftChar] > 0) {
                    missing++;
                }

                left++;
            }
        }

        return minLength == Integer.MAX_VALUE
                ? "" : s.substring(start, start + minLength);
    }
}
```

- Time: \\(O(n+m)\\)
- Extra Space: \\(O(1)\\), fixed ASCII character set ke liye.

## Dry Run

`s = "ADOBECODEBANC"`, `t = "ABC"`

| Window   | Valid? | Best window |
| -------- | ------ | ----------- |
| `ADOBEC` | Yes    | `ADOBEC`    |
| `CODEBA` | No     | `ADOBEC`    |
| `BANC`   | Yes    | `BANC`      |

Output: `"BANC"`

## Approach Comparison

| Approach       | Time           | Extra Space |
| -------------- | -------------- | ----------- |
| Brute Force    | \\(O(n^2+m)\\) | \\(O(1)\\)  |
| Sliding Window | \\(O(n+m)\\)   | \\(O(1)\\)  |

## Yaad rakhne ki line

LeetCode 76 = Variable-Size Sliding Window + Frequency Count.

`missing == 0` → window valid hai, left se shrink karo.

`missing > 0` → right se expand karo.

LeetCode 209 se difference: 209 mein sum `target` meet karta hai; 76 mein character frequencies match karni hoti hain.
