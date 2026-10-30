# LeetCode 205 — Isomorphic Strings

Pattern: HashMap / Character Mapping

Question: Given two strings `s` and `t`, check whether they are isomorphic.

दो strings isomorphic तब होती हैं जब एक string के characters को consistently दूसरे string के characters से replace किया जा सके।

Example:

- Input: `s = "egg"`, `t = "add"`
- Output: `true`
- Mapping: `e → a`, `g → d`

## 1. Brute Force — Mapping Check

Logic: हर character का mapping check करो। एक character हमेशा उसी दूसरे character से map होना चाहिए और दो अलग characters एक ही character से map नहीं होने चाहिए।

```
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        for (int i = 0; i < s.length(); i++) {
            for (int j = 0; j < i; j++) {

                // Same source character, different target
                if (s.charAt(i) == s.charAt(j) &&
                    t.charAt(i) != t.charAt(j)) {
                    return false;
                }

                // Different source characters, same target
                if (s.charAt(i) != s.charAt(j) &&
                    t.charAt(i) == t.charAt(j)) {
                    return false;
                }
            }
        }

        return true;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(1)\\)

## 2. Better — Two HashMaps

Logic:

- First Map: `s` के character का mapping `t` के character से।
- Second Map: `t` के character का mapping `s` के character से।
- Mapping inconsistent होने पर `false` return करो।

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        Map<Character, Character> map1 = new HashMap<>();
        Map<Character, Character> map2 = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {
            char a = s.charAt(i);
            char b = t.charAt(i);

            if (map1.containsKey(a) && map1.get(a) != b) {
                return false;
            }

            if (map2.containsKey(b) && map2.get(b) != a) {
                return false;
            }

            map1.put(a, b);
            map2.put(b, a);
        }

        return true;
    }
}
```

- Time: \\(O(n)\\) average
- Space: \\(O(u)\\)

## 3. Optimal — Two Frequency/Mapping Arrays

Logic: दो arrays बनाओ। हर character का last-seen index store करो। अगर दोनों characters के last-seen indexes अलग हैं, तो mapping inconsistent है।

```
class Solution {
    public boolean isIsomorphic(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }

        int[] mapS = new int[65536];
        int[] mapT = new int[65536];

        for (int i = 0; i < s.length(); i++) {
            char a = s.charAt(i);
            char b = t.charAt(i);

            if (mapS[a] != mapT[b]) {
                return false;
            }

            // Store last seen index + 1
            mapS[a] = i + 1;
            mapT[b] = i + 1;
        }

        return true;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(1)\\), fixed-size arrays.

## Dry Run

Input: `s = "egg"`, `t = "add"`

| Index | s | t | Mapping      |
| ----- | - | - | ------------ |
| 0     | e | a | e → a        |
| 1     | g | d | g → d        |
| 2     | g | d | Same mapping |

Output: `true`

Counter Example: `s = "foo"`, `t = "bar"` → `false`, क्योंकि `o` को एक ही character से map होना चाहिए, लेकिन यहाँ `o → a` और `o → r` हो रहा है।

## Comparison

| Approach     | Time         | Space      |
| ------------ | ------------ | ---------- |
| Brute Force  | \\(O(n^2)\\) | \\(O(1)\\) |
| Two HashMaps | \\(O(n)\\)   | \\(O(u)\\) |
| Two Arrays   | \\(O(n)\\)   | \\(O(1)\\) |

याद रखने की लाइन: Isomorphic Strings → One-to-one mapping → Two-way character mapping check करो।
