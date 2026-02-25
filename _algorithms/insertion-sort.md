---
title: "Insertion Sort"
difficulty: Easy
tags: [sorting, array, in-place]
---

# Insertion Sort

{% if page.tags and page.tags.size > 0 %}
<p>
{% for tag in page.tags %}
  <span style="display:inline-block;padding:0.2rem 0.5rem;margin:0.1rem 0.25rem 0.1rem 0;border-radius:999px;font-size:0.8rem;background:#e2e8f0;color:#1e293b;">#{{ tag }}</span>
{% endfor %}
</p>
{% endif %}

## What it does
Insertion Sort builds a sorted prefix of the array one element at a time. For each position, it takes the current value and shifts larger values in the sorted prefix to the right, then inserts the value in the correct spot. It is simple, stable, and in-place, making it great for small inputs.

## When to use it
- Small arrays where low overhead matters
- Nearly sorted data (few inversions)
- As a base case inside hybrid sorting algorithms
- When you need a stable in-place sort and simplicity

## When NOT to use it
- Large random arrays where `O(n^2)` is too slow
- Performance-critical paths with big datasets
- Cases where `O(n log n)` algorithms are a better default

## Complexity
- Time: Best `O(n)`, Average `O(n^2)`, Worst `O(n^2)`
- Space: `O(1)`
- Notes: Stable and in-place; best case happens when the input is already sorted.

## Java implementation
```java
/**
 * In-place stable insertion sort for integer arrays.
 */
public final class InsertionSort {

    private InsertionSort() {
        // Utility class
    }

    /**
     * Sorts the input array in ascending order using insertion sort.
     *
     * @param nums array to sort
     * @return the same array reference, sorted in ascending order
     * @throws IllegalArgumentException if nums is null
     */
    public static int[] insertionSort(int[] nums) {
        if (nums == null) {
            throw new IllegalArgumentException("Input array must not be null.");
        }

        int len = nums.length;
        for (int i = 1; i < len; i++) {
            int key = nums[i];
            int j = i - 1;

            while (j >= 0 && nums[j] > key) {
                nums[j + 1] = nums[j];
                j--;
            }

            nums[j + 1] = key;
        }

        return nums;
    }
}
```

## Pitfalls and edge cases
- `null` input should be handled explicitly (throw or guard).
- Empty and single-element arrays should return unchanged.
- Off-by-one mistakes around `j + 1` are common.
- Using `>=` instead of `>` breaks stability for equal values.

## Worked example
1. Start with `[5, 2, 4, 6, 1, 3]`; sorted prefix is `[5]`.
2. Insert `2`: shift `5` right -> `[2, 5, 4, 6, 1, 3]`.
3. Insert `4`: shift `5` right -> `[2, 4, 5, 6, 1, 3]`.
4. Insert `6`: no shifts -> `[2, 4, 5, 6, 1, 3]`.
5. Insert `1`: shift `6, 5, 4, 2` -> `[1, 2, 4, 5, 6, 3]`.
6. Insert `3`: shift `6, 5, 4` -> `[1, 2, 3, 4, 5, 6]`.

## Demo (Java 17)
```java
import java.util.Arrays;

public class InsertionSortDemo {

    public static void main(String[] args) {
        // Test 1: normal case
        int[] a1 = {5, 2, 4, 6, 1, 3};
        InsertionSort.insertionSort(a1);
        assertArrayEquals(new int[]{1, 2, 3, 4, 5, 6}, a1, "normal case");

        // Test 2: edge case (already sorted)
        int[] a2 = {1, 2, 3, 4};
        InsertionSort.insertionSort(a2);
        assertArrayEquals(new int[]{1, 2, 3, 4}, a2, "already sorted");

        // Test 3: edge case (empty array)
        int[] a3 = {};
        InsertionSort.insertionSort(a3);
        assertArrayEquals(new int[]{}, a3, "empty array");

        // Extra: duplicates to show stability behavior
        int[] a4 = {3, 1, 2, 1};
        InsertionSort.insertionSort(a4);
        assertArrayEquals(new int[]{1, 1, 2, 3}, a4, "duplicates");

        System.out.println("All demo tests passed.");
    }

    private static void assertArrayEquals(int[] expected, int[] actual, String testName) {
        if (!Arrays.equals(expected, actual)) {
            throw new AssertionError(
                "Failed " + testName
                    + " | expected=" + Arrays.toString(expected)
                    + ", actual=" + Arrays.toString(actual)
            );
        }
        System.out.println("Passed: " + testName);
    }
}
```
