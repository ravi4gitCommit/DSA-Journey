# LeetCode 239 — Sliding Window Maximum

Pattern: Fixed-Size Sliding Window + Monotonic Deque

Problem: Integer array `nums` aur window size `k` diya hai. Har contiguous window of size `k` ka maximum element return karna hai.

Example:

```
Input:  nums = [1, 3, -1, -3, 5, 3, 6, 7]
        k = 3

Output: [3, 3, 5, 5, 6, 7]
```

Har window ka maximum:

| Window        | Maximum |
| ------------- | ------- |
| `[1, 3, -1]`  | 3       |
| `[3, -1, -3]` | 3       |
| `[-1, -3, 5]` | 5       |
| `[-3, 5, 3]`  | 5       |
| `[5, 3, 6]`   | 6       |
| `[3, 6, 7]`   | 7       |

## Approach 1: Brute Force

Idea: Har window ke saare `k` elements check karke maximum find karo.

```
class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];

        for (int i = 0; i <= n - k; i++) {
            int max = nums[i];

            // Current window ke saare elements check karo
            for (int j = i; j < i + k; j++) {
                max = Math.max(max, nums[j]);
            }

            result[i] = max;
        }

        return result;
    }
}
```

- Time: \\(O(nk)\\)
- Extra Space: \\(O(1)\\), output array ko exclude karke.

## Approach 2: Better — Max Heap (PriorityQueue)

Idea: Max Heap se current window ka maximum efficiently mil sakta hai. Har element ke saath uska index bhi store karenge, taaki window se bahar ke elements remove kar sakein.

Java ka `PriorityQueue` default min-heap hota hai, isliye maximum ke liye descending comparator use karenge.

```
import java.util.PriorityQueue;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];

        // Maximum element ko top par rakhne wala heap
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
            (a, b) -> Integer.compare(b[0], a[0])
        );

        for (int i = 0; i < n; i++) {
            // Current element aur uska index add karo
            maxHeap.offer(new int[]{nums[i], i});

            // Window ke bahar ke elements remove karo
            while (maxHeap.peek()[1] <= i - k) {
                maxHeap.poll();
            }

            // First complete window ke baad maximum store karo
            if (i >= k - 1) {
                result[i - k + 1] = maxHeap.peek()[0];
            }
        }

        return result;
    }
}
```

- Time: \\(O(n\log n)\\) worst case.
- Extra Space: \\(O(n)\\) worst case.

Note: Heap mein expired elements tab remove hote hain jab woh top par aate hain. Isliye heap mein stale entries reh sakti hain, aur space \\(O(n)\\) tak ho sakta hai.

## Approach 3: Optimal — Monotonic Deque

Recommended

Idea: Deque mein indices store karenge. Indices aise maintain honge ki unke corresponding values decreasing order mein hon. Isliye deque ke front par hamesha current window ka maximum rahega.

Rules:

1. Front se expired indices remove karo.
2. Back se un indices ko remove karo jinki values current element se chhoti ya equal hain.
3. Current index add karo.
4. Jab window complete ho, front ka element answer hai.

```
import java.util.ArrayDeque;
import java.util.Deque;

class Solution {
    public int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] result = new int[n - k + 1];

        // Deque mein element values nahi, unke indices store honge
        Deque<Integer> deque = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {

            // Step 1: Window se bahar ke indices remove karo
            while (!deque.isEmpty() && deque.peekFirst() <= i - k) {
                deque.pollFirst();
            }

            // Step 2: Chhoti ya equal values ko back se remove karo
            while (!deque.isEmpty()
                    && nums[deque.peekLast()] <= nums[i]) {
                deque.pollLast();
            }

            // Step 3: Current index add karo
            deque.offerLast(i);

            // Step 4: Complete window ka maximum store karo
            if (i >= k - 1) {
                result[i - k + 1] = nums[deque.peekFirst()];
            }
        }

        return result;
    }
}
```

### Dry Run

Input: `nums = [1, 3, -1, -3, 5, 3, 6, 7]`, `k = 3`

| Window        | Deque ke relevant values | Maximum |
| ------------- | ------------------------ | ------- |
| `[1, 3, -1]`  | `[3, -1]`                | 3       |
| `[3, -1, -3]` | `[3, -1, -3]`            | 3       |
| `[-1, -3, 5]` | `[5]`                    | 5       |
| `[-3, 5, 3]`  | `[5, 3]`                 | 5       |
| `[5, 3, 6]`   | `[6]`                    | 6       |
| `[3, 6, 7]`   | `[7]`                    | 7       |

Output:

```
[3, 3, 5, 5, 6, 7]
```

- Time: \\(O(n)\\) — har index deque mein maximum ek baar add aur ek baar remove hota hai.
- Extra Space: \\(O(k)\\) — deque mein maximum `k` indices.

## Final Comparison

| Approach        | Time             | Extra Space  |
| --------------- | ---------------- | ------------ |
| Brute Force     | \\(O(nk)\\)      | \\(O(1)\\)\* |
| Max Heap        | \\(O(n\log n)\\) | \\(O(n)\\)   |
| Monotonic Deque | \\(O(n)\\)       | \\(O(k)\\)   |

\*Output array ko exclude karke.

Yaad rakhne ki line: Sliding Window Maximum mein deque ka front = maximum, aur deque ke indices ki values decreasing order mein maintain hoti hain.

Pattern recognition: Jab har fixed-size window ka maximum ya minimum chahiye, toh Monotonic Deque ek standard optimal approach hai.
