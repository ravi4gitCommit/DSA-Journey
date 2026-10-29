# LeetCode 347 — Top K Frequent Elements

Pattern: Frequency Counting + HashMap + Heap / Bucket Sort

Question: Given an integer array `nums` and an integer `k`, return the `k` most frequent elements.

Example:

- Input: `nums = [1,1,1,2,2,3]`, `k = 2`
- Output: `[1,2]`

क्योंकि `1` की frequency `3` है, `2` की frequency `2` है और `3` की frequency `1` है।

## 1. Brute Force — Frequency Count + Sorting

Logic:

1. HashMap से हर number की frequency निकालो।
2. Unique numbers को frequency के आधार पर sort करो।
3. सबसे ज्यादा frequency वाले पहले `k` numbers return करो।

```
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();

        // Har number ki frequency count karo
        for (int num : nums) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        // Unique numbers ko frequency ke according sort karo
        List<Integer> list = new ArrayList<>(freq.keySet());

        list.sort((a, b) -> freq.get(b) - freq.get(a));

        // Pehle k most frequent numbers return karo
        int[] ans = new int[k];

        for (int i = 0; i < k; i++) {
            ans[i] = list.get(i);
        }

        return ans;
    }
}
```

- Time: \\(O(n + u\log u)\\)
- Space: \\(O(u)\\)

यहाँ \\(n\\) = array length और \\(u\\) = unique numbers की संख्या।

## 2. Better — Min Heap

Logic: Frequency Map बनाओ और फिर size `k` की Min Heap रखो। Heap में frequency सबसे कम वाला element ऊपर रहता है। Heap का size `k` से बड़ा होने पर सबसे कम frequency वाला element निकाल दो।

इस तरह अंत में heap में `k` most frequent elements बचेंगे।

```
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();

        // Frequency count
        for (int num : nums) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        // Min Heap: sabse kam frequency wala element top par
        PriorityQueue<Integer> minHeap = new PriorityQueue<>(
            (a, b) -> freq.get(a) - freq.get(b)
        );

        for (int num : freq.keySet()) {
            minHeap.offer(num);

            // Heap mein sirf k elements rakho
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }

        int[] ans = new int[k];

        for (int i = k - 1; i >= 0; i--) {
            ans[i] = minHeap.poll();
        }

        return ans;
    }
}
```

- Time: \\(O(n + u\log k)\\)
- Space: \\(O(u+k)\\), HashMap सहित कुल auxiliary space \\(O(u+k)\\) है।

## 3. Optimal — Bucket Sort

Logic: Kisi bhi number ki maximum frequency `n` ho sakti hai. Isliye frequency ko index ki tarah use kar sakte hain.

- `freq` Map mein number ki frequency store karo.
- `buckets[f]` mein un numbers ko rakho jinki frequency `f` hai.
- Highest frequency se neeche aao aur `k` numbers collect kar lo.

```
import java.util.*;

class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        Map<Integer, Integer> freq = new HashMap<>();

        // Step 1: Har number ki frequency count karo
        for (int num : nums) {
            freq.put(num, freq.getOrDefault(num, 0) + 1);
        }

        // Step 2: Frequency ke according buckets banao
        List<Integer>[] buckets = new List[nums.length + 1];

        for (int num : freq.keySet()) {
            int count = freq.get(num);

            if (buckets[count] == null) {
                buckets[count] = new ArrayList<>();
            }

            buckets[count].add(num);
        }

        // Step 3: Highest frequency se numbers collect karo
        int[] ans = new int[k];
        int index = 0;

        for (int f = buckets.length - 1; f >= 1; f--) {
            if (buckets[f] != null) {
                for (int num : buckets[f]) {
                    ans[index++] = num;

                    if (index == k) {
                        return ans;
                    }
                }
            }
        }

        return ans;
    }
}
```

- Time: \\(O(n)\\)
- Space: \\(O(n)\\)

## Dry Run

Input: `nums = [1,1,1,2,2,3]`, `k = 2`

Step 1 — Frequency Map

| Number | Frequency |
| ------ | --------- |
| 1      | 3         |
| 2      | 2         |
| 3      | 1         |

Step 2 — Buckets

Freq 3

Number: `1`

Freq 2

Number: `2`

Freq 1

Number: `3`

Step 3: Highest frequency से चलेंगे: `1` फिर `2`। हमें दो numbers चाहिए, इसलिए output `[1, 2]`।

## तीनों approaches का comparison

| Approach              | Time               | Extra Space  |
| --------------------- | ------------------ | ------------ |
| Brute Force + Sorting | \\(O(n+u\log u)\\) | \\(O(u)\\)   |
| Min Heap              | \\(O(n+u\log k)\\) | \\(O(u+k)\\) |
| Bucket Sort           | \\(O(n)\\)         | \\(O(n)\\)   |

यहाँ \\(n\\) = array length और \\(u\\) = unique elements की संख्या।

याद रखने की लाइन:

- Sorting: Frequency count → Sort → First `k`
- Heap: Frequency count → Min Heap of size `k`
- Bucket: Frequency count → Frequency buckets → Traverse backwards

Interview में: Bucket Sort वाला approach linear time \\(O(n)\\) में solution देता है। Output का order कोई भी हो सकता है; equal-frequency elements के बीच किसी भी valid selection को स्वीकार किया जाता है।
