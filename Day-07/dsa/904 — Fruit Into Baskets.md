# LeetCode 904 — Fruit Into Baskets

Pattern: Variable-Size Sliding Window + HashMap

Question: `fruits` array mein har number ek fruit type represent karta hai. Tumhe contiguous segment se maximum fruits collect karne hain, lekin tumhare paas sirf 2 baskets hain aur har basket mein ek hi type ka fruit aa sakta hai.

Matlab, longest subarray find karo jisme maximum 2 distinct numbers hon.

Example:

```
fruits = [1,2,1,2,3]

Output = 4
```

Explanation: `[1,2,1,2]` mein sirf 2 fruit types hain aur length `4` hai.

## 1. Brute Force Approach

Har starting index se subarray banao. Distinct fruit types count karo. Jaise hi 2 se zyada types ho jaayein, loop stop karo.

```
import java.util.HashSet;
import java.util.Set;

class Solution {
    public int totalFruit(int[] fruits) {
        int n = fruits.length;
        int maxLength = 0;

        for (int left = 0; left < n; left++) {
            Set<Integer> types = new HashSet<>();

            for (int right = left; right < n; right++) {
                types.add(fruits[right]);

                // 2 se zyada types allowed nahi hain
                if (types.size() > 2) {
                    break;
                }

                maxLength = Math.max(maxLength, right - left + 1);
            }
        }

        return maxLength;
    }
}
```

- Time: \\(O(n^2)\\) average
- Extra Space: \\(O(1)\\), kyunki valid window mein maximum 2 types store hote hain.

## 2. Better Approach — Sliding Window + `while`

Right pointer se fruit add karo. Agar window mein 2 se zyada distinct types ho jaayein, toh left pointer move karo jab tak sirf 2 types na reh jaayein.

```
import java.util.HashMap;
import java.util.Map;

class Solution {
    public int totalFruit(int[] fruits) {
        Map<Integer, Integer> freq = new HashMap<>();

        int left = 0;
        int maxLength = 0;

        for (int right = 0; right < fruits.length; right++) {
            // Current fruit ki frequency badhao
            freq.put(fruits[right],
                     freq.getOrDefault(fruits[right], 0) + 1);

            // Jab 2 se zyada types hon, window shrink karo
            while (freq.size() > 2) {
                int fruit = fruits[left];

                freq.put(fruit, freq.get(fruit) - 1);

                // Frequency zero hone par type remove karo
                if (freq.get(fruit) == 0) {
                    freq.remove(fruit);
                }

                left++;
            }

            maxLength = Math.max(maxLength, right - left + 1);
        }

        return maxLength;
    }
}
```

- Time: \\(O(n)\\) average
- Extra Space: \\(O(1)\\) for this problem, because the valid window has at most 2 distinct types; the map temporarily has at most 3.

## 3. Optimal Approach — Sliding Window

Approach 2 hi standard optimal solution hai. Har element maximum ek baar add aur ek baar remove hota hai, isliye total time linear hai. Alag `if`-based technique ki zarurat nahi hai.

## Dry Run

`fruits = [1,2,1,2,3]`

| Right fruit | Current window       | Distinct types  | Max length |
| ----------- | -------------------- | --------------- | ---------- |
| 1           | `[1]`                | 1               | 1          |
| 2           | `[1,2]`              | 2               | 2          |
| 1           | `[1,2,1]`            | 2               | 3          |
| 2           | `[1,2,1,2]`          | 2               | 4          |
| 3           | `[2,1,2,3]` → shrink | 3 before shrink | 4          |

Last step mein `3` add karne par 3 types ho jaate hain. Left se elements remove karte hain jab tak sirf 2 types bachein. Final valid window `[2,3]` hai.

Output: `4`

## Approach Comparison

| Approach                 | Time                 | Extra Space            |
| ------------------------ | -------------------- | ---------------------- |
| Brute Force              | \\(O(n^2)\\) average | \\(O(1)\\)             |
| Sliding Window + HashMap | \\(O(n)\\) average   | \\(O(1)\\) for 2 types |

## Yaad rakhne ki line

LeetCode 904 = Longest Subarray with at Most 2 Distinct Elements.

- Right pointer se expand karo.
- `freq.size() > 2` ho toh left se shrink karo.
- Har valid window par `right - left + 1` se maximum update karo.

Connection: Ye pattern LeetCode 3 jaisa hai, lekin wahan duplicate characters allowed nahi the; yahan maximum 2 distinct fruit types allowed hain.
