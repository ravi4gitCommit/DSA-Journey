# LeetCode 49 — Group Anagrams

Pattern: HashMap · Frequency Counting / Sorting

Question: Strings ki array `strs` di gayi hai. Saare anagrams ko ek group mein collect karo.

Anagrams mein characters aur unki frequency same hoti hai, bas order different ho sakta hai.

Example:

```
Input:  strs = ["eat","tea","tan","ate","nat","bat"]

Output: [["eat","tea","ate"],["tan","nat"],["bat"]]
```

## 1. Brute Force Approach

Har string ko kisi existing group ki string ke saath compare karo. Agar dono anagrams hain, toh us group mein add karo; warna naya group banao.

```
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> groups = new ArrayList<>();
        boolean[] used = new boolean[strs.length];

        for (int i = 0; i < strs.length; i++) {
            if (used[i]) {
                continue;
            }

            List<String> group = new ArrayList<>();
            group.add(strs[i]);
            used[i] = true;

            for (int j = i + 1; j < strs.length; j++) {
                if (!used[j] && areAnagrams(strs[i], strs[j])) {
                    group.add(strs[j]);
                    used[j] = true;
                }
            }

            groups.add(group);
        }

        return groups;
    }

    private boolean areAnagrams(String a, String b) {
        if (a.length() != b.length()) {
            return false;
        }

        char[] first = a.toCharArray();
        char[] second = b.toCharArray();

        Arrays.sort(first);
        Arrays.sort(second);

        return Arrays.equals(first, second);
    }
}
```

- Time: \\(O(n^2 \cdot k\log k)\\)
- Extra Space: \\(O(nk)\\) for the groups and bookkeeping, excluding output details.

Here, `n` = number of strings, `k` = maximum string length.

## 2. Better Approach — Sorting + HashMap

Har string ke characters sort karo. Anagrams ki sorted strings same hongi, isliye sorted string ko HashMap ki key bana sakte hain.

```
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();

        for (String str : strs) {
            char[] chars = str.toCharArray();
            Arrays.sort(chars);

            String key = new String(chars);

            map.computeIfAbsent(key, k -> new ArrayList<>()).add(str);
        }

        return new ArrayList<>(map.values());
    }
}
```

- Time: \\(O(n \cdot k\log k)\\)
- Extra Space: \\(O(nk)\\), including stored keys and groups.

## 3. Optimal Approach — Character Frequency + HashMap ⭐

LeetCode 49 mein strings lowercase English letters se bani hoti hain. Har string mein `a` se `z` tak characters ki frequency count karo. Same frequency wale strings anagrams hain.

```
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();

        for (String str : strs) {
            int[] freq = new int[26];

            // Har character ki frequency count karo
            for (int i = 0; i < str.length(); i++) {
                freq[str.charAt(i) - 'a']++;
            }

            // Frequency array ko unique key mein badlo
            StringBuilder key = new StringBuilder();

            for (int count : freq) {
                key.append('#').append(count);
            }

            map.computeIfAbsent(key.toString(), k -> new ArrayList<>())
               .add(str);
        }

        return new ArrayList<>(map.values());
    }
}
```

- Time: \\(O(n \cdot k)\\)
- Extra Space: \\(O(nk)\\), including keys and grouped strings.

`#` separator counts ko unambiguously represent karta hai, jaise `[1, 11]` aur `[11, 1]` alag keys banayenge.

## Dry Run

Input: `["eat","tea","tan","ate","nat","bat"]`

| String | Sorted / Frequency Key | Group   |
| ------ | ---------------------- | ------- |
| `eat`  | `aet`                  | Group 1 |
| `tea`  | `aet`                  | Group 1 |
| `tan`  | `ant`                  | Group 2 |
| `ate`  | `aet`                  | Group 1 |
| `nat`  | `ant`                  | Group 2 |
| `bat`  | `abt`                  | Group 3 |

Output groups:

```
[["eat","tea","ate"],["tan","nat"],["bat"]]
```

Groups ka order alag ho sakta hai; LeetCode is order ko fix nahi karta.

## Approach Comparison

| Approach            | Time                 | Main Idea             |
| ------------------- | -------------------- | --------------------- |
| Brute Force         | \\(O(n^2 k\log k)\\) | Har pair compare karo |
| Sorting + HashMap   | \\(O(nk\log k)\\)    | Sorted string key     |
| Frequency + HashMap | \\(O(nk)\\)          | Character counts key  |

## Yaad rakhne ki line

Group Anagrams = Same characters/frequencies → Same key → HashMap mein group.

- Sorting approach: standard aur easy to remember.
- Frequency approach: lowercase English letters ke liye faster asymptotic solution.
