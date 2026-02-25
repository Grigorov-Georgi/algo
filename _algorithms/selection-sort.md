---
title: "Selection Sort"
difficulty: Easy
tags: [sorting, array, in-place]
---

# Selection Sort

{% if page.tags and page.tags.size > 0 %}
<p>
{% for tag in page.tags %}
  <span style="display:inline-block;padding:0.2rem 0.5rem;margin:0.1rem 0.25rem 0.1rem 0;border-radius:999px;font-size:0.8rem;background:#e2e8f0;color:#1e293b;">#{{ tag }}</span>
{% endfor %}
</p>
{% endif %}

## What it does
Selection Sort repeatedly selects the minimum element from the unsorted part of the array and places it at the current position. After each pass, the sorted prefix grows by one element. It is simple and in-place, but it is not stable in its common swap-based form. Its runtime is predictable but slow for larger inputs.

## When to use it
- Small arrays where simplicity is preferred
- Learning sorting fundamentals
- Environments where minimizing extra memory is important
- Cases where predictable comparison count is useful

## When NOT to use it
- Large datasets (too slow in practice)
- When you need stable ordering for equal values
- Cases where adaptive behavior for nearly sorted data matters

## Complexity
- Time: Best `O(n^2)`, Average `O(n^2)`, Worst `O(n^2)`
- Space: `O(1)`
- Notes: In-place and usually not stable. Compared to insertion sort, it has the same worst-case `O(n^2)` but a worse best-case: selection sort stays `O(n^2)` while insertion sort can be `O(n)` on nearly/already sorted input.

## Java implementation
```java
/**
 * Utility class providing Selection Sort implementation.
 *
 * Characteristics:
 * - In-place
 * - Not stable (by default)
 * - Time: O(n^2) in all cases
 * - Space: O(1)
 */
public final class SelectionSort {

    private SelectionSort() {
        // prevent instantiation
    }

    /**
     * Sorts the given array in ascending order using selection sort.
     *
     * @param arr the array to sort (must not be null)
     */
    public static void sort(int[] arr) {
        if (arr == null || arr.length < 2) {
            return;
        }

        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int minIndex = i;

            // find index of minimum element in remaining unsorted portion
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[minIndex]) {
                    minIndex = j;
                }
            }

            // swap if needed
            if (minIndex != i) {
                swap(arr, i, minIndex);
            }
        }
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

## Pitfalls and edge cases
- Algorithm is not stable in default swap-based form.
- `null`, empty, or one-element arrays should return unchanged.
- Off-by-one bounds (`i < n - 1`, `j = i + 1`) are common mistakes.
- It does unnecessary comparisons even when input is already sorted.

## Worked example
1. Start with `[64, 25, 12, 22, 11]`; minimum is `11`, swap with first -> `[11, 25, 12, 22, 64]`.
2. Remaining `[25, 12, 22, 64]`; minimum is `12`, swap -> `[11, 12, 25, 22, 64]`.
3. Remaining `[25, 22, 64]`; minimum is `22`, swap -> `[11, 12, 22, 25, 64]`.
4. Remaining `[25, 64]` already ordered; done -> `[11, 12, 22, 25, 64]`.

## Demo (Java 17)
```java
import java.util.Arrays;

public class SelectionSortDemo {
    public static void main(String[] args) {
        // Test 1: normal case
        int[] a1 = {64, 25, 12, 22, 11};
        SelectionSort.sort(a1);
        assertArrayEquals(new int[]{11, 12, 22, 25, 64}, a1, "normal case");

        // Test 2: edge case (already sorted)
        int[] a2 = {1, 2, 3, 4};
        SelectionSort.sort(a2);
        assertArrayEquals(new int[]{1, 2, 3, 4}, a2, "already sorted");

        // Test 3: edge case (duplicates and negatives)
        int[] a3 = {3, -1, 3, 0, -1};
        SelectionSort.sort(a3);
        assertArrayEquals(new int[]{-1, -1, 0, 3, 3}, a3, "duplicates and negatives");

        // Test 4: edge case (empty)
        int[] a4 = {};
        SelectionSort.sort(a4);
        assertArrayEquals(new int[]{}, a4, "empty");

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
