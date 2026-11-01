# LeetCode 1094 — Car Pooling



Medium

Pattern: Difference Array + Prefix Sum



## 1. Problem क्या है?

एक car की capacity दी गई है और trips की list है।

हर trip में:

```
[passengers, from, to]
```

- `passengers` = कितने passengers चढ़ेंगे।
- `from` = कहाँ से चढ़ेंगे।
- `to` = कहाँ उतरेंगे।

Check करना है कि car सभी trips पूरी कर सकती है या नहीं।

Example:

```
trips = [[2,1,5], [3,3,7]]
capacity = 4

Output = false
```

क्योंकि location 3 से 5 के बीच कुल 5 passengers होंगे, जबकि capacity केवल 4 है।

## 2. Brute Force — हर Trip पर Update

हर trip के लिए `from` से `to - 1` तक passenger count बढ़ाओ। फिर check करो कि capacity exceed हुई या नहीं।

```
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {

        int[] passengers = new int[1001];

        for (int[] trip : trips) {

            int count = trip[0];
            int from = trip[1];
            int to = trip[2];

            for (int i = from; i < to; i++) {
                passengers[i] += count;

                if (passengers[i] > capacity) {
                    return false;
                }
            }
        }

        return true;
    }
}
```

- Time: O(T × L), जहाँ L maximum route length है।
- Space: O(1), क्योंकि array का size fixed है।

## 3. Optimal — Difference Array

Main Idea: हर location पर एक-एक करके passenger count update करने की जरूरत नहीं है।

Difference Array में केवल दो जगह update करते हैं:

- `diff[from] += passengers` → passengers चढ़े।
- `diff[to] -= passengers` → passengers उतरे।

इसके बाद Prefix Sum लगाकर हर location पर actual passengers निकालते हैं।

Java Code (LeetCode):

```
class Solution {
    public boolean carPooling(int[][] trips, int capacity) {

        int[] diff = new int[1001];

        // Step 1: Difference Array update
        for (int[] trip : trips) {

            int passengers = trip[0];
            int from = trip[1];
            int to = trip[2];

            diff[from] += passengers;
            diff[to] -= passengers;
        }

        // Step 2: Prefix Sum se actual passengers
        int currentPassengers = 0;

        for (int i = 0; i < 1001; i++) {

            currentPassengers += diff[i];

            if (currentPassengers > capacity) {
                return false;
            }
        }

        return true;
    }
}
```

- Time: O(T + 1001) = O(T), given fixed location limit.
- Space: O(1), given fixed-size array.

## 4. Dry Run

```
trips = [[2,1,5], [3,3,7]]
capacity = 4
```

Difference Array updates:

```
Trip 1: [2,1,5]
diff[1] += 2
diff[5] -= 2

Trip 2: [3,3,7]
diff[3] += 3
diff[7] -= 3
```

Prefix Sum:

| Location | Passengers |
| -------- | ---------- |
| 1        | 2          |
| 2        | 2          |
| 3        | 5          |
| 4        | 5          |
| 5        | 3          |
| 6        | 3          |
| 7        | 0          |

Location 3 पर 5 passengers हो गए, लेकिन capacity 4 है।

Output: `false`

## 5. Complexity Comparison

| Approach         | Time        | Space |
| ---------------- | ----------- | ----- |
| Brute Force      | O(T × L)    | O(1)  |
| Difference Array | O(T + 1001) | O(1)  |

## याद रखने की लाइन

Difference Array = Range Update के लिए सिर्फ दो updates।

```
diff[left] += value;
diff[right + 1] -= value;
```

फिर Prefix Sum लगाकर actual values निकालो।

Important: LeetCode 1094 में trip का `to` exclusive है, इसलिए `diff[to] -= passengers` होगा। General range `[left, right]` inclusive हो तो `diff[right + 1] -= value` करना है।

इस pattern का मुख्य उपयोग: जब कई बार array की किसी पूरी range में value add करनी हो, तब Difference Array इस्तेमाल करो।
