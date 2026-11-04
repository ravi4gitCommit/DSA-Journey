# LeetCode 875 — Koko Eating Bananas



Medium

Pattern: Binary Search on Answer



## 1. Problem क्या है?

Koko के पास bananas के कई piles हैं। उसे `h` hours के अंदर सभी bananas खाने हैं।

हमें minimum eating speed `k` find करनी है।

- हर hour Koko maximum `k` bananas खा सकती है।
- एक hour में केवल एक pile से bananas खा सकती है।
- अगर pile में `k` से कम bananas बचे हैं, तो वह पूरा pile खाकर बाकी समय wait करेगी।

Example:

```
piles = [3, 6, 7, 11]
h = 8

Output = 4
```

Speed `4` पर:

| Pile  | Required Hours |
| ----- | -------------- |
| 3     | 1              |
| 6     | 2              |
| 7     | 2              |
| 11    | 3              |
| Total | 8              |

इसलिए minimum speed = `4`।

## 2. Brute Force — Linear Search on Answer

हर possible speed `k` को check करेंगे।

- Minimum speed = 1
- Maximum speed = maximum pile
- हर speed के लिए required hours calculate करेंगे।
- पहली valid speed answer होगी।

### Java Code

```
class Solution {
    public int minEatingSpeed(int[] piles, int h) {

        int max = 0;

        // Maximum pile find karo
        for (int pile : piles) {
            max = Math.max(max, pile);
        }

        // Har possible speed check karo
        for (int k = 1; k <= max; k++) {

            long hours = 0;

            for (int pile : piles) {

                // Required hours calculate karo
                hours += (pile + (long) k - 1) / k;
            }

            if (hours <= h) {
                return k;
            }
        }

        return -1;
    }
}
```

- Time: O(n × maxPile)
- Space: O(1)

## 3. Better Approach — Binary Search on Answer

Main Idea: Eating speed बढ़ाने पर required hours कम होते हैं।

उदाहरण:

```
Speed = 2 → 15 hours
Speed = 3 → 10 hours
Speed = 4 → 8 hours
Speed = 5 → 6 hours
```

हमें minimum valid speed चाहिए।

इसलिए Binary Search लगाएंगे।

### Search Range

```
low = 1
high = maximum pile
```

### Required Hours Formula

\\[ hours = \left\lceil \frac{pile}{k} \right\rceil \\]

Java में बिना floating-point calculation के:

```
hours += (pile + k - 1) / k;
```

### Java Code (LeetCode)

```
class Solution {
    public int minEatingSpeed(int[] piles, int h) {

        int low = 1;
        int high = 0;

        // Maximum pile find karo
        for (int pile : piles) {
            high = Math.max(high, pile);
        }

        int answer = high;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            long hours = 0;

            // Mid speed par total hours calculate karo
            for (int pile : piles) {
                hours += (pile + (long) mid - 1) / mid;
            }

            if (hours <= h) {

                // Speed valid hai, aur minimum try karo
                answer = mid;
                high = mid - 1;
            }

            else {

                // Speed kam hai, increase karo
                low = mid + 1;
            }
        }

        return answer;
    }
}
```

- Time: O(n × log(maxPile))
- Space: O(1)

## 4. Dry Run

```
piles = [3, 6, 7, 11]
h = 8

low = 1
high = 11
```

| Step | low | high | mid (speed) | Hours | Decision     |
| ---- | --- | ---- | ----------- | ----- | ------------ |
| 1    | 1   | 11   | 6           | 6     | Left search  |
| 2    | 1   | 5    | 3           | 10    | Right search |
| 3    | 4   | 5    | 4           | 8     | Left search  |
| 4    | 4   | 3    | —           | —     | Stop         |

Answer = 4

## 5. Complexity Comparison

| Approach                | Time                | Space |
| ----------------------- | ------------------- | ----- |
| Brute Force             | O(n × maxPile)      | O(1)  |
| Binary Search on Answer | O(n × log(maxPile)) | O(1)  |

## 6. याद रखने की लाइन

Binary Search on Answer = Minimum/Maximum Answer + Feasibility Check.

```
if (hours <= h) {
    answer = mid;
    high = mid - 1;
} else {
    low = mid + 1;
}
```

- Valid answer → Left search (minimum speed चाहिए)।
- Invalid answer → Right search (speed बढ़ानी है)।

## 7. Binary Search Pattern Revision

| LeetCode | Problem                 | Pattern                 |
| -------- | ----------------------- | ----------------------- |
| 704      | Binary Search           | Normal                  |
| 35       | Search Insert Position  | Lower Bound             |
| 34       | First & Last Position   | Two Binary Searches     |
| 33       | Search in Rotated Array | Modified                |
| 153      | Find Minimum            | Modified                |
| 162      | Find Peak Element       | Slope-based             |
| 74       | Search a 2D Matrix      | 2D Binary Search        |
| 875      | Koko Eating Bananas     | Binary Search on Answer |

याद रखो: जब question में minimum possible speed, maximum possible capacity या minimum time निकालना हो और किसी answer को valid/invalid check कर सकते हो, तो Binary Search on Answer पहचानो।
