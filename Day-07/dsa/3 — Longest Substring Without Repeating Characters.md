# LeetCode 3 — Longest Substring Without Repeating Characters

Pattern: Variable-Size Sliding Window + HashSet / HashMap

Question: String `s` diya hai. Aisa longest substring find karo jisme koi character repeat na ho. Uski length return karo.

Example:

```
Input:  s = "abcabcbb"
Output: 3
```

Explanation: `"abc"` longest substring hai jisme koi character repeat nahi hota. Iski length `3` hai.

## 1. Brute Force Approach

Har starting index se substring banao aur check karo ki koi character repeat toh nahi ho raha.

```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length();
        int maxLength = 0;

        for (int i = 0; i < n; i++) {
            boolean[] seen = new boolean[65536];

            for (int j = i; j < n; j++) {
                char ch = s.charAt(j);

                // Character pehle mil chuka hai
                if (seen[ch]) {
                    break;
                }

                seen[ch] = true;
                maxLength = Math.max(maxLength, j - i + 1);
            }
        }

        return maxLength;
    }
}
```

- Time: \\(O(n^2)\\)
- Extra Space: \\(O(1)\\) for Java's fixed-size `char` range.

## 2. Better Approach — Sliding Window + HashSet

Ek window maintain karo jisme saare characters unique hon.

- Right pointer se character add karo.
- Agar character pehle se present hai, toh left pointer se characters remove karo jab tak duplicate hat na jaaye.
- Har valid window ki length se maximum update karo.

```
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Set<Character> set = new HashSet<>();

        int left = 0;
        int maxLength = 0;

        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);

            // Duplicate hatane ke liye left side shrink karo
            while (set.contains(ch)) {
                set.remove(s.charAt(left));
                left++;
            }

            set.add(ch);

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

- Time: \\(O(n)\\) average
- Extra Space: \\(O(\min(n, C))\\), where `C` is the character set size.

## 3. Optimal Approach — Sliding Window + HashMap

HashMap mein har character ka last seen index store karenge. Duplicate milne par left pointer ko seedha aage jump kar sakte hain, isliye `while` loop ki zarurat nahi.

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character, Integer> lastSeen = new HashMap<>();

        int left = 0;
        int maxLength = 0;

        for (int right = 0; right < s.length(); right++) {
            char ch = s.charAt(right);

            // Character current window mein pehle aaya hai?
            if (lastSeen.containsKey(ch)) {
                // Left pointer ko peeche nahi le jaana
                left = Math.max(left, lastSeen.get(ch) + 1);
            }

            // Current character ka latest index save karo
            lastSeen.put(ch, right);

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

- Time: \\(O(n)\\) average
- Extra Space: \\(O(\min(n, C))\\)

## Dry Run — Optimal Approach

Input: `s = "abcabcbb"`

| `right` | Character | `left` | Current window | Max length |
| ------- | --------- | ------ | -------------- | ---------- |
| 0       | `a`       | 0      | `a`            | 1          |
| 1       | `b`       | 0      | `ab`           | 2          |
| 2       | `c`       | 0      | `abc`          | 3          |
| 3       | `a`       | 1      | `bca`          | 3          |
| 4       | `b`       | 2      | `cab`          | 3          |
| 5       | `c`       | 3      | `abc`          | 3          |
| 6       | `b`       | 5      | `cb`           | 3          |
| 7       | `b`       | 7      | `b`            | 3          |

Output: `3`

### Important: `Math.max(left, lastSeen.get(ch) + 1)` kyun?

Example: `s = "abba"`

Jab last `a` aata hai, uska previous index `0` hai. Lekin `left` already `2` par hai. Agar `left = 0 + 1` kar diya, toh window galat ho jayegi.

`Math.max()` ensure karta hai ki `left` kabhi peeche na jaaye.

## Approach Comparison

| Approach                 | Time               | Extra Space                            |
| ------------------------ | ------------------ | -------------------------------------- |
| Brute Force              | \\(O(n^2)\\)       | \\(O(1)\\) for fixed Java `char` range |
| Sliding Window + HashSet | \\(O(n)\\) average | \\(O(\min(n,C))\\)                     |
| Sliding Window + HashMap | \\(O(n)\\) average | \\(O(\min(n,C))\\)                     |

## Yaad rakhne ki line

Longest Substring Without Repeating Characters = Variable-Size Sliding Window.

- Duplicate nahi hai → window expand karo.
- Duplicate hai → window shrink karo, ya HashMap se `left` ko jump karao.
- Har valid window par `right - left + 1` se maximum length update karo.
