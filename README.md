# 🔷 Balanced Substring Analysis using Prefix Sums & Hashing

This project provides efficient algorithms to analyze balanced substrings in a binary array, where a substring is considered balanced if it contains an equal number of 0s and 1s. The solution is optimized for large-scale data using prefix sum techniques and hashing, enabling fast preprocessing and constant-time queries.

The core idea is to transform the binary array such that 1 is mapped to +1 and 0 is mapped to -1. With this transformation, the problem of checking whether a substring is balanced reduces to verifying whether the sum of the transformed values over that substring is zero. Using prefix sums, we compute a cumulative array where each index stores the sum up to that position. This allows any substring sum to be computed in constant time. A substring from index i to j is balanced if the difference between prefix[j] and prefix[i-1] is zero.

For finding the longest balanced substring, the algorithm uses a hash map to store the first occurrence of each prefix sum value. If the same prefix sum appears again, it implies that the subarray between those indices has a sum of zero, hence is balanced. By tracking the maximum distance between such repeated prefix sums, we obtain the longest balanced substring in linear time.

The time complexity of preprocessing is O(N), each query can be answered in O(1), and the longest balanced substring can be computed in O(N). The space complexity is also O(N) due to the prefix array and hash map.

Below is the implementation:

```python
class BalancedSubstring:
    def __init__(self, x):
        self.x = x
        self.n = len(x)
        self.prefix = [0] * self.n
        
        val = 0
        for i in range(self.n):
            val += 1 if x[i] == 1 else -1
            self.prefix[i] = val

    def is_balanced(self, i, j):
        if i == 0:
            return self.prefix[j] == 0
        return (self.prefix[j] - self.prefix[i - 1]) == 0

    def longest_balanced(self):
        prefix = 0
        first_occurrence = {}
        max_length = 0

        for i in range(self.n):
            prefix += 1 if self.x[i] == 1 else -1

            if prefix == 0:
                max_length = i + 1

            if prefix in first_occurrence:
                max_length = max(max_length, i - first_occurrence[prefix])
            else:
                first_occurrence[prefix] = i

        return max_length
