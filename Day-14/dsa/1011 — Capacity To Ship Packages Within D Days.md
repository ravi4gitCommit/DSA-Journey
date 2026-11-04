# LeetCode 1011 — Capacity To Ship Packages Within D Days



Medium

Pattern: Binary Search on Answer



## 1. Problem क्या है?

एक array `weights` दिया है, जिसमें packages के weights हैं। हमें सभी packages को `days` दिनों के अंदर ship करना है।

हमें ship की minimum capacity find करनी है।

Rules:

1. Packages को दिए गए order में ही ship करना है।
2. एक दिन में consecutive packages ship होंगे।
3. किसी दिन का total weight ship capacity से ज्यादा नहीं हो सकता।

Example:

```
weights = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
days = 5

Output = 15
```

Capacity `15` होने पर:

```
Day 1: 1 + 2 + 3 + 4 + 5 = 15
Day 2: 6 + 7 = 13
Day 3: 8 = 8
Day 4: 9 = 9
Day 5: 10 = 10
```

Total = 5 days.

## 2. Brute Force — Linear Search on Answer

हर possible capacity check करेंगे।

Search Range:

- Minimum capacity = Maximum package weight.
- Maximum capacity = सभी packages का total weight.

हर capacity के लिए calculate करेंगे कि कितने days लगेंगे।

### Java Code

```
class Solution {
    public int shipWithinDays(int[] weights, int days) {

        int low = 0;
        int high = 0;

        // Minimum aur maximum capacity
        for (int weight : weights) {
            low = Math.max(low, weight);
            high += weight;
        }

        // Har possible capacity check karo
        for (int capacity = low; capacity <= high; capacity++) {

            int requiredDays = 1;
            int currentWeight = 0;

            for (int weight : weights) {

                if (currentWeight + weight > capacity) {
                    requiredDays++;
                    currentWeight = 0;
                }

                currentWeight += weight;
            }

            if (requiredDays <= days) {
                return capacity;
            }
        }

        return -1;
    }
}
```

- Time: O(n × sum(weights))
- Space: O(1)

## 3. Better / Optimal — Binary Search on Answer

Main Logic: Capacity बढ़ाने पर required days कम या बराबर होते हैं।

हमें minimum valid capacity चाहिए।

### Search Range

```
low = Maximum package weight
high = Total sum of weights
```

### Feasibility Check

हर capacity पर check करेंगे कि packages कितने दिनों में ship हो सकते हैं।

- अगर `requiredDays <= days` → Capacity valid है, और कम capacity try करो।
- अगर `requiredDays > days` → Capacity बढ़ानी पड़ेगी।

### Java Code (LeetCode)

```
class Solution {
    public int shipWithinDays(int[] weights, int days) {

        int low = 0;
        int high = 0;

        // Search range find karo
        for (int weight : weights) {

            low = Math.max(low, weight);
            high += weight;
        }

        int answer = high;

        while (low <= high) {

            int mid = low + (high - low) / 2;

            int requiredDays = 1;
            int currentWeight = 0;

            // Mid capacity par required days check karo
            for (int weight : weights) {

                if (currentWeight + weight > mid) {
                    requiredDays++;
                    currentWeight = 0;
                }

                currentWeight += weight;
            }

            if (requiredDays <= days) {

                // Valid capacity, aur minimum try karo
                answer = mid;
                high = mid - 1;
            }

            else {

                // Capacity kam hai, increase karo
                low = mid + 1;
            }
        }

        return answer;
    }
}
```

- Time: O(n × log(sum(weights)))
- Space: O(1)

## 4. Dry Run

```
weights = [1,2,3,4,5,6,7,8,9,10]
days = 5

low = 10
high = 55
```

| Step | low | high | mid (capacity) | Days Required |
| ---- | --- | ---- | -------------- | ------------- |
| 1    | 10  | 55   | 32             | 2             |
| 2    | 10  | 31   | 20             | 4             |
| 3    | 10  | 19   | 14             | 6             |
| 4    | 15  | 19   | 17             | 4             |
| 5    | 15  | 16   | 15             | 5             |
| 6    | 15  | 14   | —              | Stop          |

Output: 15

## 5. Complexity Comparison

| Approach      | Time            | Space |
| ------------- | --------------- | ----- |
| Brute Force   | O(n × sum)      | O(1)  |
| Binary Search | O(n × log(sum)) | O(1)  |

## 6. याद रखने की लाइन

Ship Packages = Minimum Capacity + Required Days Check.

```
if (requiredDays <= days) {
    answer = mid;
    high = mid - 1;
} else {
    low = mid + 1;
}
```

Search Range याद रखो:

```
low = maximum package weight;
high = total sum of weights;
```

## 7. LC 875 vs LC 1011 — Important Difference

| Concept | LC 875 — Koko        | LC 1011 — Ship Packages |
| ------- | -------------------- | ----------------------- |
| Find    | Minimum eating speed | Minimum ship capacity   |
| Low     | 1                    | Maximum weight          |
| High    | Maximum pile         | Total weight            |
| Check   | Required hours       | Required days           |

\| Valid | Hours
