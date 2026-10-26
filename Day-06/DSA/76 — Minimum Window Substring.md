# LeetCode 76 — Minimum Window Substring

Pattern: Variable-Size Sliding Window + Frequency Count

Question: Do strings `s` aur `t` diye hain. `s` ka smallest substring find karo jisme `t` ke saare characters required frequency ke saath present hon. Agar possible nahi hai, toh `""` return karo.

Example:

```
s = "ADOBECODEBANC"
t = "ABC"

Output = "BANC"
```

`"BANC"` smallest substring hai jisme `A`, `B`, aur `C` teeno present hain.

## 1. Brute Force Approach

Har possible substring check karo. Jo substring `t` ke saare characters required frequency ke saath contain kare, usmein se minimum length return karo.

```
class Solution {
    public String minWindow(String s, String t) {
        int n = s.length();
        int minLength = Integer.MAX_VALUE;
        int start = 0;

        // Har possible starting index
        for (int i = 0; i < n; i++) {
            int[] freq = new int[128];
            int formed = 0;

            // t ke required characters count karo
            for (int j = 0; j < t.length(); j++) {
                freq[t.charAt(j)]++;
            }

            // Substring ko expand karo
            for (int j = i; j < n; j++) {
                char ch = s.charAt(j);

                if (freq[ch] > 0) {
                    formed++;
                }
                freq[ch]--;

                if (formed == t.length()) {
                    int length = j - i + 1;

                    if (length < minLength) {
                        minLength = length;
                        start = i;
                    }
                    break;
                }
            }
        }

        return minLength == Integer.MAX_VALUE
                ? ""
                : s.substring(start, start + minLength);
    }
}
```

- Time: \\(O(n^2+m)\\) — fixed-size ASCII frequency array ke saath.
- Space: \\(O(1)\\) auxiliary space, assuming fixed ASCII character set.

## 2. Better Approach — Sliding Window with Frequency Count

Har substring ko zero se check karne ke bajay, ek window maintain karo. Right pointer se character add karo aur jab window valid ho, left pointer se shrink karo.

```
class Solution {
    public String minWindow(String s, String t) {
        if (t.length() > s.length()) {
            return "";
        }

        int[] need = new int[128];

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

            // Jab saare required characters mil jaayein
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
                ? ""
                : s.substring(start, start + minLength);
    }
}
```

- Time: \\(O(n+m)\\)
- Space: \\(O(1)\\), fixed ASCII character set ke liye.

## 3. Optimal Approach — Frequency-Based Sliding Window ⭐

Upar diya gaya Approach 2 hi standard optimal approach hai. Is problem ke liye usse asymptotically faster standard approach ki zarurat nahi hai.

Important variables:

- `need[ch]`: required frequency aur current window ke beech ka difference.
- `missing`: `t` ke kitne characters abhi match nahi hue.
- `left`, `right`: current window ke boundaries.
- `minLength`, `start`: ab tak mila smallest valid substring.

### Dry Run

`s = "ADOBECODEBANC"`, `t = "ABC"`

| Window   | Valid? | Best window |
| -------- | ------ | ----------- |
| `ADOBEC` | Yes    | `ADOBEC`    |
| `DOBEC`  | No     | `ADOBEC`    |
| `CODEBA` | No     | `ADOBEC`    |
| `BANC`   | Yes    | `BANC`      |

Output: `"BANC"`

## Approach Comparison

| Approach                   | Time           | Extra Space |
| -------------------------- | -------------- | ----------- |
| Brute Force                | \\(O(n^2+m)\\) | \\(O(1)\\)  |
| Sliding Window + Frequency | \\(O(n+m)\\)   | \\(O(1)\\)  |
| Optimal standard solution  | \\(O(n+m)\\)   | \\(O(1)\\)  |

## Yaad rakhne ki line

LeetCode 76 = Variable-Size Sliding Window.

Window valid ho jaaye toh `left++` karke shrink karo; invalid ho jaaye toh `right++` se expand karo.

Difference from LeetCode 209: 209 mein sum `target` meet karta hai; 76 mein substring ke characters ki frequency `t` ki required frequency meet karti hai.
