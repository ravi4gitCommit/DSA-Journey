# LeetCode 242 — Valid Anagram

Pattern: Frequency Counting / Character Count

Question: Given two strings `s` and `t`, check whether `t` is an anagram of `s`.

Anagram का मतलब: दोनों strings में same characters same frequency में हों, लेकिन order अलग हो सकता है।

Example: `s = "anagram"`, `t = "nagaram"` → `true`

## 1. Brute Force — हर character को search करना

Logic: `s` के हर character को `t` में ढूँढो। मिलने पर उसे used mark कर दो।

```
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        boolean[] used = new boolean[t.length()];

        for (int i = 0; i < s.length(); i++) {
            boolean found = false;

            for (int j = 0; j < t.length(); j++) {
                if (!used[j] && s.charAt(i) == t.charAt(j)) {
                    used[j] = true;
                    found = true;
                    break;
                }
            }

            if (!found) {
                return false;
            }
        }

        return true;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(n)\\)

## 2. Better — Sorting

Logic: दोनों strings को character array में convert करके sort करो। अगर दोनों sorted arrays बराबर हैं, तो anagram हैं।

```
import java.util.Arrays;

class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        char[] a = s.toCharArray();
        char[] b = t.toCharArray();

        Arrays.sort(a);
        Arrays.sort(b);

        return Arrays.equals(a, b);
    }
}
```

- Time: \\(O(n \log n)\\)
- Space: \\(O(n)\\)

## 3. Optimal — Frequency Counting

Logic: हर character की frequency count करो। `s` के लिए `+1` और `t` के लिए `-1`। अगर सभी counts `0` हैं, तो दोनों anagrams हैं।

```
class Solution {
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        int[] freq = new int[26];

        for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
            freq[t.charAt(i) - 'a']--;
        }

        for (int count : freq) {
            if (count != 0) {
                return false;
            }
        }

        return true;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(1)\\), क्योंकि array का size हमेशा 26 है।

Note: यह optimal code lowercase English letters (`a`–`z`) की constraint के लिए है।

## Dry Run

Input: `s = "anagram"`, `t = "nagaram"`

| Character | `s` count | `t` count |
| --------- | --------- | --------- |
| a         | 3         | 3         |
| n         | 1         | 1         |
| g         | 1         | 1         |
| r         | 1         | 1         |
| m         | 1         | 1         |

सभी characters की frequencies बराबर हैं, इसलिए output `true` होगा।

## तीनों approaches का comparison

| Approach           | Time             | Space      |
| ------------------ | ---------------- | ---------- |
| Brute Force        | \\(O(n^2)\\)     | \\(O(n)\\) |
| Sorting            | \\(O(n\log n)\\) | \\(O(n)\\) |
| Frequency Counting | \\(O(n)\\)       | \\(O(1)\\) |

याद रखने की लाइन: Valid Anagram → Same characters + Same frequency → Frequency Count.
