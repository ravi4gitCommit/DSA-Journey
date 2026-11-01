# LeetCode 1109 — Corporate Flight Bookings



Medium

Pattern: Difference Array + Prefix Sum



## 1. Problem क्या है?

एक `bookings` array दिया है। हर booking में:

```
[first, last, seats]
```

इसका मतलब `first` से `last` तक सभी flights में `seats` की booking हुई है।

हमें हर flight पर total booked seats निकालनी हैं।

Example:

```
bookings = [[1,2,10], [2,3,20], [2,5,25]]
n = 5

Output = [10,55,45,25,25]
```

## 2. Brute Force — Direct Range Update

हर booking के लिए `first` से `last` तक सभी flights में seats add करेंगे।

```
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {

        int[] answer = new int[n];

        for (int[] booking : bookings) {

            int first = booking[0];
            int last = booking[1];
            int seats = booking[2];

            for (int i = first; i <= last; i++) {
                answer[i - 1] += seats;
            }
        }

        return answer;
    }
}
```

- Time: O(B × n)
- Space: O(1) extra, output array छोड़कर।

## 3. Optimal — Difference Array

Main Idea: पूरी range में बार-बार update करने की जगह केवल दो positions पर update करेंगे।

हर booking के लिए:

```
diff[first - 1] += seats;
diff[last] -= seats;
```

फिर Prefix Sum लगाकर हर flight की actual seats निकालेंगे।

Java Code (LeetCode):

```
class Solution {
    public int[] corpFlightBookings(int[][] bookings, int n) {

        int[] diff = new int[n + 1];

        // Step 1: Difference Array updates
        for (int[] booking : bookings) {

            int first = booking[0];
            int last = booking[1];
            int seats = booking[2];

            diff[first - 1] += seats;
            diff[last] -= seats;
        }

        // Step 2: Prefix Sum
        int[] answer = new int[n];
        int currentSeats = 0;

        for (int i = 0; i < n; i++) {

            currentSeats += diff[i];
            answer[i] = currentSeats;
        }

        return answer;
    }
}
```

- Time: O(B + n)
- Space: O(n) — Difference Array और output array।

## 4. Dry Run

```
bookings = [[1,2,10], [2,3,20], [2,5,25]]
n = 5
```

Difference Array updates:

| Booking  | Update                       |
| -------- | ---------------------------- |
| [1,2,10] | diff[0] += 10, diff[2] -= 10 |
| [2,3,20] | diff[1] += 20, diff[3] -= 20 |
| [2,5,25] | diff[1] += 25, diff[5] -= 25 |

Difference Array:

```
Index:  0   1   2   3   4   5
Diff:  10  45 -10 -20   0 -25
```

Prefix Sum:

| Flight | Calculation | Seats |
| ------ | ----------- | ----- |
| 1      | 10          | 10    |
| 2      | 10 + 45     | 55    |
| 3      | 55 - 10     | 45    |
| 4      | 45 - 20     | 25    |
| 5      | 25 + 0      | 25    |

Output

```
[10, 55, 45, 25, 25]
```

## 5. Complexity Comparison

| Approach         | Time     | Space      |
| ---------------- | -------- | ---------- |
| Brute Force      | O(B × n) | O(1) extra |
| Difference Array | O(B + n) | O(n)       |

## याद रखने की लाइन

Difference Array — Range `[left, right]` में value add करनी हो:

```
diff[left] += value;
diff[right + 1] -= value;
```

फिर Prefix Sum लगाओ।

1094 vs 1109 का अंतर:

- LC 1094: `to` exclusive होता है → `diff[to] -= passengers`
- LC 1109: `last` inclusive होता है → `diff[last] -= seats` (0-based index में `diff[last]`)

दोनों में pattern एक ही है: Difference Array + Prefix Sum।
