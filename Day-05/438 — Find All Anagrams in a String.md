# LeetCode 438 — Find All Anagrams in a String

Pattern: Fixed-Size Sliding Window + Frequency Array

Problem: Do strings `s` aur `p` diye hain. `s` ke un sabhi starting indices ko return karo jahan substring, `p` ka anagram ho.

Anagram ka matlab: dono strings mein same characters same frequency mein hon, lekin order alag ho sakta hai.

Example:

```
Input:  s = "cbaebabacd"
        p = "abc"

Output: [0, 6]
```

- Index `0`: `"cba"` → `"abc"` ka anagram.
- Index `6`: `"bac"` → `"abc"` ka anagram.

## Approach 1: Brute Force — Sort Each Window

Idea: Har size-`p.length()` window ko sort karo aur sorted `p` se compare karo.

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();

        int n = s.length();
        int k = p.length();

        if (k > n) {
            return result;
        }

        // p ko sort karo
        char[] pChars = p.toCharArray();
        Arrays.sort(pChars);

        // Har possible window check karo
        for (int i = 0; i <= n - k; i++) {
            char[] window = s.substring(i, i + k).toCharArray();

            Arrays.sort(window);

            // Sorted window aur sorted p same hain?
            if (Arrays.equals(window, pChars)) {
                result.add(i);
            }
        }

        return result;
    }
}
```

- Time: \\(O((n-k+1)\\,k\log k)\\)
- Extra Space: \\(O(k)\\)

## Approach 2: Better — Frequency Array for Every Window

Idea: Har window mein 26 lowercase English letters ki frequency count karo aur `p` ki frequency se compare karo.

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();

        int n = s.length();
        int k = p.length();

        if (k > n) {
            return result;
        }

        int[] pFreq = new int[26];

        // p ke characters ki frequency count karo
        for (char ch : p.toCharArray()) {
            pFreq[ch - 'a']++;
        }

        // Har size-k window ki frequency check karo
        for (int i = 0; i <= n - k; i++) {
            int[] windowFreq = new int[26];

            for (int j = i; j < i + k; j++) {
                windowFreq[s.charAt(j) - 'a']++;
            }

            if (Arrays.equals(pFreq, windowFreq)) {
                result.add(i);
            }
        }

        return result;
    }
}
```

- Time: \\(O(nk)\\)
- Extra Space: \\(O(1)\\), kyunki frequency arrays ka size fixed 26 hai.

## Approach 3: Optimal — Fixed-Size Sliding Window

Recommended

Idea: Har window ki frequency dobara calculate nahi karni. Window slide karte waqt:

1. Left se nikalne wale character ki frequency `-1`.
2. Right se aane wale character ki frequency `+1`.
3. Dono frequency arrays match karein, toh starting index add karo.

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> result = new ArrayList<>();

        int n = s.length();
        int k = p.length();

        if (k > n) {
            return result;
        }

        int[] pFreq = new int[26];
        int[] windowFreq = new int[26];

        // p aur first window ki frequency count karo
        for (int i = 0; i < k; i++) {
            pFreq[p.charAt(i) - 'a']++;
            windowFreq[s.charAt(i) - 'a']++;
        }

        // First window check karo
        if (Arrays.equals(pFreq, windowFreq)) {
            result.add(0);
        }

        // Window ko ek-ek position right slide karo
        for (int right = k; right < n; right++) {
            // Purane leftmost character ko remove karo
            char outgoing = s.charAt(right - k);
            windowFreq[outgoing - 'a']--;

            // Naya character window mein add karo
            char incoming = s.charAt(right);
            windowFreq[incoming - 'a']++;

            // Frequencies same hain toh anagram mila
            if (Arrays.equals(pFreq, windowFreq)) {
                result.add(right - k + 1);
            }
        }

        return result;
    }
}
```

- Time: \\(O(n)\\), because frequency comparison checks 26 entries, which is constant.
- Extra Space: \\(O(1)\\), because both frequency arrays have fixed size 26.

## Dry Run

Input: `s = "cbaebabacd"`, `p = "abc"`, window size `k = 3`.

| Window  | Anagram of `"abc"`? | Starting index |
| ------- | ------------------- | -------------- |
| `"cba"` | Yes                 | 0              |
| `"bae"` | No                  | —              |
| `"aeb"` | No                  | —              |
| `"eba"` | No                  | —              |
| `"bab"` | No                  | —              |
| `"aba"` | No                  | —              |
| `"bac"` | Yes                 | 6              |

Output: `[0, 6]`

## Final Comparison

| Approach                       | Time              | Extra Space |
| ------------------------------ | ----------------- | ----------- |
| Brute Force — Sort Each Window | \\(O(nk\log k)\\) | \\(O(k)\\)  |
| Better — Recount Frequencies   | \\(O(nk)\\)       | \\(O(1)\\)  |
| Optimal — Sliding Window       | \\(O(n)\\)        | \\(O(1)\\)  |

Yaad rakhne ki line: Fixed-size window mein outgoing character ko remove karo, incoming character ko add karo, aur frequency match hone par index store karo.

Pattern: `LC 438` mein anagram + substring + fixed length dikhe, toh Sliding Window + Frequency Array use karo.
