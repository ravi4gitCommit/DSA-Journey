# LC 1482 — Minimum Number of Days to Make Bouquets

Pattern: Binary Search on Answer + Greedy Feasibility

## 1. Problem क्या है?

हमें `bloomDay[]` दिया है।

- एक bouquet बनाने के लिए `k` adjacent flowers चाहिए।
- कुल `m` bouquets बनाने हैं।
- एक flower केवल एक bouquet में use हो सकता है।
- Find the minimum day जिस दिन सभी bouquets बन सकें।

Example:

```
bloomDay = [1, 10, 3, 10, 2]
m = 3
k = 1
```

Output: `3`

Day 3 तक flowers bloom हो चुके होंगे: `[1, 3, 2]` यानी 3 bouquets बन सकते हैं।

## 2. Brute Force — Try Every Day

हर possible day को check करेंगे: `1` से `max(bloomDay)` तक।

- उस दिन कितने adjacent bloomed flowers मिलते हैं, count करो।
- हर `k` consecutive flowers पर एक bouquet बनाओ।
- अगर bouquets `m` हो जाएँ, तो वही minimum day है।

```
class Solution {
    public int minDays(int[] bloomDay, int m, int k) {
        if ((long) m * k > bloomDay.length) return -1;

        int maxDay = 0;

        for (int day : bloomDay) {
            maxDay = Math.max(maxDay, day);
        }

        for (int day = 1; day <= maxDay; day++) {
            int bouquets = 0;
            int flowers = 0;

            for (int bloom : bloomDay) {
                if (bloom <= day) {
                    flowers++;

                    if (flowers == k) {
                        bouquets++;
                        flowers = 0;
                    }
                } else {
                    flowers = 0;
                }
            }

            if (bouquets >= m) return day;
        }

        return -1;
    }
}
```

Time: \\(O(n \times maxDay)\\)\
Space: \\(O(1)\\)

## 3. Optimal — Binary Search on Answer

Observation:

अगर किसी day पर bouquets बन सकते हैं, तो उससे बड़े day पर भी बन सकते हैं।

इसलिए Binary Search लगा सकते हैं।

- `low = minimum bloomDay`
- `high = maximum bloomDay`
- `mid = possible day`

isPossible(mid):

1. `bloomDay[i] <= mid` है तो flower bloomed है।
2. Consecutive bloomed flowers count करो।
3. `k` flowers पूरे होने पर एक bouquet बनाओ और count reset करो।
4. अगर flower bloomed नहीं है, तो consecutive count reset करो।
5. अगर bouquets `m` हो जाएँ, तो possible है।

### Java Code

```
class Solution {
    public int minDays(int[] bloomDay, int m, int k) {

        int n = bloomDay.length;

        if ((long) m * k > n) return -1;

        int low = Integer.MAX_VALUE;
        int high = Integer.MIN_VALUE;

        for (int day : bloomDay) {
            low = Math.min(low, day);
            high = Math.max(high, day);
        }

        int ans = -1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (isPossible(bloomDay, m, k, mid)) {
                ans = mid;
                high = mid - 1; // Minimum day खोजो
            } else {
                low = mid + 1;
            }
        }

        return ans;
    }

    private boolean isPossible(int[] bloomDay, int m, int k, int day) {

        int bouquets = 0;
        int flowers = 0;

        for (int bloom : bloomDay) {

            if (bloom <= day) {
                flowers++;

                if (flowers == k) {
                    bouquets++;
                    flowers = 0;
                }
            } else {
                flowers = 0;
            }

            if (bouquets >= m) return true;
        }

        return false;
    }
}
```

Time Complexity: \\(O(n \times \log(maxDay-minDay+1))\\)

Space Complexity: \\(O(1)\\)

## 4. Dry Run

```
bloomDay = [1, 10, 3, 10, 2]
m = 3
k = 1
```

| Day (`mid`) | Available flowers | Bouquets | Result       |
| ----------- | ----------------- | -------- | ------------ |
| 5           | 1, 3, 2           | 3        | Possible     |
| 2           | 1, 2              | 2        | Not possible |
| 3           | 1, 3, 2           | 3        | Possible     |

Answer = `3`

## 5. Brute vs Optimal

| Approach      | Time                             | Space |
| ------------- | -------------------------------- | ----- |
| Brute Force   | \\(O(n \times maxDay)\\)         | O(1)  |
| Binary Search | \\(O(n \log(maxDay-minDay+1))\\) | O(1)  |

## Exam / Interview में याद रखने की लाइन

Minimum Number of Days to Make Bouquets = Binary Search on Answer + Greedy.

- Search space: `min(bloomDay)` to `max(bloomDay)`.
- Feasibility: Count consecutive bloomed flowers.
- `k` flowers → 1 bouquet.
- Possible → `high = mid - 1`.
- Not possible → `low = mid + 1`.
- Impossible condition: `(long) m * k > n`.
