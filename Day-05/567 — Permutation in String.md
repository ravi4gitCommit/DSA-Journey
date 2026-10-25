# LeetCode 567 — Permutation in String

Pattern: Fixed-Size Sliding Window + Frequency Array

Problem: Do strings `s1` aur `s2` diye hain. Check karo ki `s2` mein `s1` ki koi permutation present hai ya nahi.

Permutation ka matlab: characters same frequency mein hon, bas unka order alag ho sakta hai.

Example:

```
Input:  s1 = "ab"
        s2 = "eidbaooo"

Output: true
```

Kyunki `s2` mein substring `"ba"` present hai, jo `"ab"` ki permutation hai.

## Approach 1: Brute Force — Generate Permutations

Idea: `s1` ki saari possible permutations generate karo aur check karo ki koi permutation `s2` mein present hai ya nahi.

```
import java.util.HashSet;
import java.util.Set;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        Set<String> permutations = new HashSet<>();

        // s1 ki saari unique permutations generate karo
        generate(s1.toCharArray(), 0, permutations);

        // Kya koi permutation s2 mein present hai?
        for (String permutation : permutations) {
            if (s2.contains(permutation)) {
                return true;
            }
        }

        return false;
    }

    private void generate(char[] chars, int index, Set<String> result) {
        // Saare characters arrange ho gaye
        if (index == chars.length) {
            result.add(new String(chars));
            return;
        }

        for (int i = index; i < chars.length; i++) {
            swap(chars, index, i);

            generate(chars, index + 1, result);

            // Backtracking: original arrangement restore karo
            swap(chars, index, i);
        }
    }

    private void swap(char[] chars, int i, int j) {
        char temp = chars[i];
        chars[i] = chars[j];
        chars[j] = temp;
    }
}
```

- Time: Worst case factorial growth, approximately \\(O(m! \cdot n)\\), where \\(m=s1.length()\\), \\(n=s2.length()\\).
- Extra Space: \\(O(m \cdot m!)\\) for storing permutations, ignoring recursion overhead.

Note: Yeh approach correct hai, lekin large input ke liye bahut slow hai.

## Approach 2: Better — Frequency Array for Every Window

Idea: `s1` ki frequency count karo. Phir `s2` ki har size-`s1.length()` window ki frequency calculate karke compare karo.

```
import java.util.Arrays;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();

        if (m > n) {
            return false;
        }

        int[] s1Freq = new int[26];

        // s1 ke characters ki frequency count karo
        for (int i = 0; i < m; i++) {
            s1Freq[s1.charAt(i) - 'a']++;
        }

        // s2 ki har size-m window check karo
        for (int i = 0; i <= n - m; i++) {
            int[] windowFreq = new int[26];

            for (int j = i; j < i + m; j++) {
                windowFreq[s2.charAt(j) - 'a']++;
            }

            if (Arrays.equals(s1Freq, windowFreq)) {
                return true;
            }
        }

        return false;
    }
}
```

- Time: \\(O(nm)\\)
- Extra Space: \\(O(1)\\), because frequency arrays ka size fixed 26 hai.

## Approach 3: Optimal — Fixed-Size Sliding Window

Recommended

Idea: Har window ki frequency dobara calculate nahi karni. Window ko ek position slide karte waqt:

1. Outgoing character ki frequency `-1` karo.
2. Incoming character ki frequency `+1` karo.
3. Dono frequency arrays match karein, toh permutation mil gayi.

```
import java.util.Arrays;

class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();

        // Agar s1 bada hai toh permutation fit nahi hogi
        if (m > n) {
            return false;
        }

        int[] s1Freq = new int[26];
        int[] windowFreq = new int[26];

        // s1 aur s2 ki first window ki frequency count karo
        for (int i = 0; i < m; i++) {
            s1Freq[s1.charAt(i) - 'a']++;
            windowFreq[s2.charAt(i) - 'a']++;
        }

        // First window check karo
        if (Arrays.equals(s1Freq, windowFreq)) {
            return true;
        }

        // Window ko ek-ek position right slide karo
        for (int right = m; right < n; right++) {

            // Purane leftmost character ko remove karo
            char outgoing = s2.charAt(right - m);
            windowFreq[outgoing - 'a']--;

            // Naya character add karo
            char incoming = s2.charAt(right);
            windowFreq[incoming - 'a']++;

            // Frequencies same hain toh permutation mil gayi
            if (Arrays.equals(s1Freq, windowFreq)) {
                return true;
            }
        }

        return false;
    }
}
```

- Time: \\(O(n)\\), because har comparison mein fixed 26 entries check hoti hain.
- Extra Space: \\(O(1)\\).

### Dry Run

Input: `s1 = "ab"`, `s2 = "eidbaooo"`

Window size \\(=2\\).

| Window | Frequency matches `"ab"`? |
| ------ | ------------------------- |
| `"ei"` | No                        |
| `"id"` | No                        |
| `"db"` | No                        |
| `"ba"` | Yes → `true`              |

## Final Comparison

| Approach                            | Time             | Extra Space         |
| ----------------------------------- | ---------------- | ------------------- |
| Brute Force — Generate Permutations | Factorial growth | \\(O(m \cdot m!)\\) |
| Better — Recount Each Window        | \\(O(nm)\\)      | \\(O(1)\\)          |
| Optimal — Sliding Window            | \\(O(n)\\)       | \\(O(1)\\)          |

Yaad rakhne ki line: `LC 567` mein substring ki length `s1.length()` fix rakho, outgoing character remove karo, incoming character add karo, aur frequencies match hone par `true` return karo.

LC 438 vs LC 567:

- LC 438: Saare anagram starting indices return karne hain.
- LC 567: Sirf check karna hai ki koi permutation present hai ya nahi.

Note: Yeh solutions lowercase English letters (`a`–`z`) assume karte hain, as per the problem constraints.
