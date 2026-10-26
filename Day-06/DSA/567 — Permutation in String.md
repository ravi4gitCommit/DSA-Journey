# LeetCode 567 — Permutation in String

Pattern: Fixed-Size Sliding Window + Frequency Array

Question: Do strings `s1` aur `s2` diye hain. Check karo ki `s2` ke andar `s1` ka koi permutation present hai ya nahi.

Permutation ka matlab: characters same hon, lekin order different ho sakta hai.

Example:

- `s1 = "ab"`, `s2 = "eidbaooo"` → `true` (`"ba"` is a permutation of `"ab"`)
- `s1 = "ab"`, `s2 = "eidboaoo"` → `false`

## 1. Brute Force Approach

Har size-`s1.length()` window ka permutation check karne ke liye uske characters sort karo aur `s1` ko sort karke compare karo.

```
import java.util.Arrays;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int k = s1.length();

        if (k > s2.length()) {
            return false;
        }

        char[] target = s1.toCharArray();
        Arrays.sort(target);

        // Har possible window check karo
        for (int i = 0; i <= s2.length() - k; i++) {
            char[] window = s2.substring(i, i + k).toCharArray();
            Arrays.sort(window);

            if (Arrays.equals(target, window)) {
                return true;
            }
        }

        return false;
    }
}
```

- Time: \\(O((n-k+1)k\log k)\\)
- Space: \\(O(k)\\)

Yahan \\(n = s2.length()\\) aur \\(k = s1.length()\\).

## 2. Better Approach — Frequency Count for Every Window

Characters ko sort karne ki jagah, unki frequency count karo. Har window ke liye frequency dobara calculate karni hogi.

```
import java.util.Arrays;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int k = s1.length();

        if (k > s2.length()) {
            return false;
        }

        int[] target = new int[26];

        // s1 ke characters ki frequency
        for (int i = 0; i < k; i++) {
            target[s1.charAt(i) - 'a']++;
        }

        // Har window ki frequency dobara count karo
        for (int i = 0; i <= s2.length() - k; i++) {
            int[] window = new int[26];

            for (int j = i; j < i + k; j++) {
                window[s2.charAt(j) - 'a']++;
            }

            if (Arrays.equals(target, window)) {
                return true;
            }
        }

        return false;
    }
}
```

- Time: \\(O((n-k+1)k)\\)
- Space: \\(O(1)\\), because each frequency array has only 26 entries.

## 3. Optimal Approach — Sliding Window ⭐

Har window ki frequency dobara count nahi karenge. Window ko ek position right slide karenge:

1. Naya character add karo.
2. Window se bahar ja raha character remove karo.
3. Dono frequency arrays compare karo.

```
import java.util.Arrays;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int k = s1.length();

        if (k > s2.length()) {
            return false;
        }

        int[] target = new int[26];
        int[] window = new int[26];

        // First window aur s1 ki frequency banao
        for (int i = 0; i < k; i++) {
            target[s1.charAt(i) - 'a']++;
            window[s2.charAt(i) - 'a']++;
        }

        if (Arrays.equals(target, window)) {
            return true;
        }

        // Fixed-size window ko slide karo
        for (int right = k; right < s2.length(); right++) {
            // Naya character add karo
            window[s2.charAt(right) - 'a']++;

            // Leftmost, window se bahar ja raha character remove karo
            window[s2.charAt(right - k) - 'a']--;

            if (Arrays.equals(target, window)) {
                return true;
            }
        }

        return false;
    }
}
```

- Time: \\(O(n)\\), because 26 entries ki comparison constant time hai.
- Space: \\(O(1)\\), because array sizes fixed hain.

## Dry Run — Optimal Approach

`s1 = "ab"`, `s2 = "eidbaooo"`

Target frequency: `a = 1, b = 1`

| Window | Characters | Match? |
| ------ | ---------- | ------ |
| `"ei"` | e, i       | No     |
| `"id"` | i, d       | No     |
| `"db"` | d, b       | No     |
| `"ba"` | b, a       | Yes    |

Window `"ba"` mein `a` aur `b` ki frequency exactly `"ab"` jaisi hai.

Output: `true`

## Approach Comparison

| Approach                   | Time              | Extra Space |
| -------------------------- | ----------------- | ----------- |
| Brute Force (sorting)      | \\(O(nk\log k)\\) | \\(O(k)\\)  |
| Better (recount frequency) | \\(O(nk)\\)       | \\(O(1)\\)  |
| Optimal (sliding window)   | \\(O(n)\\)        | \\(O(1)\\)  |

## Yaad rakhne ki line

Permutation in String = Fixed-Size Sliding Window + Character Frequency Matching.

LeetCode 567 mein lowercase English letters assume kiye gaye hain, isliye size `26` ka frequency array use hota hai.
