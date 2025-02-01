
# Prefix Sum Technique in Kotlin

The Prefix Sum technique is a useful tool to efficiently handle range sum queries. Below is an implementation of the prefix sum technique in Kotlin.

## Code Implementation

```kotlin
fun buildPrefixSum(arr: IntArray): IntArray {
    val n = arr.size
    val prefixSum = IntArray(n)
    prefixSum[0] = arr[0]

    for (i in 1 until n) {
        prefixSum[i] = prefixSum[i - 1] + arr[i]
    }
    return prefixSum
}

fun querySum(prefixSum: IntArray, l: Int, r: Int): Int {
    return if (l == 0) {
        prefixSum[r]
    } else {
        prefixSum[r] - prefixSum[l - 1]
    }
}

fun main() {
    val arr = intArrayOf(2, 4, 5, 3, 7)
    val prefixSum = buildPrefixSum(arr)

    // Query the sum of subarray from index 1 to 3
    val result = querySum(prefixSum, 1, 3)
    println(result)  // Output: 12
}
```

## Explanation:
1. `buildPrefixSum(arr: IntArray)`: This function builds the prefix sum array where each element at index `i` contains the sum of all elements from `arr[0]` to `arr[i]`.
2. `querySum(prefixSum: IntArray, l: Int, r: Int)`: This function returns the sum of elements in the range `[l..r]`. It uses the prefix sum array to compute the result in constant time.
3. **Example**: In the provided example, we query the sum of elements from index 1 to 3 in the array `arr = [2, 4, 5, 3, 7]`, and the output is `12`.

## Time Complexity:
- Preprocessing time: `O(n)` for building the prefix sum array.
- Query time: `O(1)` for each range sum query.

This technique is highly efficient for answering multiple range sum queries after an initial preprocessing step.
