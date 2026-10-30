# LeetCode 387 — First Unique Character in a String

Pattern: Frequency Counting + String Traversal

Question: Given a string `s`, find the index of the first non-repeating character. If no unique character exists, return `-1`.

Example:

- Input: `s = "leetcode"`
- Output: `0`
- Explanation: `l` केवल एक बार आता है और उसका index `0` है।

## 1. Brute Force — Nested Loops

Logic:

1. हर character को बाकी सभी characters से compare करो।
2. अगर वह कहीं और नहीं मिलता, तो उसका index return करो।
3. अगर कोई unique character नहीं मिला, तो `-1` return करो।

```
class Solution {
    public int firstUniqChar(String s) {

        for (int i = 0; i < s.length(); i++) {
            boolean unique = true;

            for (int j = 0; j < s.length(); j++) {
                if (i != j && s.charAt(i) == s.charAt(j)) {
                    unique = false;
                    break;
                }
            }

            if (unique) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: \\(O(n^2)\\)
- Space: \\(O(1)\\)

## 2. Better — HashMap

Logic:

1. HashMap में हर character की frequency count करो।
2. String को दोबारा left to right traverse करो।
3. जिसकी frequency `1` हो, उसका index return करो।

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int firstUniqChar(String s) {
        Map<Character, Integer> freq = new HashMap<>();

        // Step 1: Frequency count
        for (char ch : s.toCharArray()) {
            freq.put(ch, freq.getOrDefault(ch, 0) + 1);
        }

        // Step 2: First unique character find karo
        for (int i = 0; i < s.length(); i++) {
            if (freq.get(s.charAt(i)) == 1) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: \\(O(n)\\) average
- Space: \\(O(u)\\), जहाँ `u` unique characters की संख्या है।

## 3. Optimal — Frequency Array

Logic:

1. `int[26]` array बनाओ।
2. हर lowercase character की frequency count करो।
3. दोबारा string traverse करो।
4. जिसकी frequency `1` हो, उसका index return करो।

```
class Solution {
    public int firstUniqChar(String s) {
        int[] freq = new int[26];

        // Step 1: Frequency count
        for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
        }

        // Step 2: First unique character find karo
        for (int i = 0; i < s.length(); i++) {
            if (freq[s.charAt(i) - 'a'] == 1) {
                return i;
            }
        }

        return -1;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(1)\\), क्योंकि array का size हमेशा 26 है।

Note: Frequency array वाला solution lowercase English letters (`a`–`z`) की constraint के लिए है।

## Dry Run

Input: `s = "leetcode"`

| Character | Frequency |
| --------- | --------- |
| l         | 1         |
| e         | 3         |
| t         | 1         |
| c         | 1         |
| o         | 1         |
| d         | 1         |

String को left से check करेंगे:

- `l` → frequency `1`
- इसलिए index `0` return होगा।

Output: 0

## Comparison

| Approach        | Time         | Space      |
| --------------- | ------------ | ---------- |
| Brute Force     | \\(O(n^2)\\) | \\(O(1)\\) |
| HashMap         | \\(O(n)\\)   | \\(O(u)\\) |
| Frequency Array | \\(O(n)\\)   | \\(O(1)\\) |

याद रखने की लाइन: First Unique Character → Frequency count करो → दोबारा left to right traverse करो → पहली frequency `1` का index return करो।
