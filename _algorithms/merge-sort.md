---
title: "Merge Sort"
difficulty: Medium
tags: [array, divide-and-conquer, recursion, sorting, stable]
---

# Merge Sort

{% if page.tags and page.tags.size > 0 %}
<p>
{% for tag in page.tags %}
  <span style="display:inline-block;padding:0.2rem 0.5rem;margin:0.1rem 0.25rem 0.1rem 0;border-radius:999px;font-size:0.8rem;background:#e2e8f0;color:#1e293b;">#{{ tag }}</span>
{% endfor %}
</p>
{% endif %}

## What it does
Merge Sort is a divide-and-conquer sorting algorithm. It recursively splits the array into smaller halves until each part has one element, then merges those sorted parts back together in order. Because each merge step is linear and there are `log n` split levels, it runs in `O(n log n)` time consistently. It is stable and predictable, which makes it a strong general-purpose choice when extra memory is acceptable.

## When to use it
- You need guaranteed `O(n log n)` time
- You want stable sorting behavior
- You need predictable performance across inputs
- You are sorting large datasets where `O(n^2)` sorts are too slow

## When NOT to use it
- You must minimize extra memory usage (`O(n)` auxiliary space is needed)
- You need an in-place sorting algorithm
- Very small arrays where simpler sorts can be faster in practice

## Complexity
- Time: Best `O(n log n)`, Average `O(n log n)`, Worst `O(n log n)`
- Space: `O(n)`
- Notes: Stable but not in-place in this array implementation; recursion depth is `O(log n)`.

## Java implementation
```java
import java.util.Arrays;

/**
 * CLRS-style merge sort for integer arrays.
 */
public final class MergeSortCLRS {

    private MergeSortCLRS() {
        // Utility class
    }

    /**
     * Sorts the input array in ascending order.
     *
     * @param A array to sort
     */
    public static void mergeSort(int[] A) {
        if (A == null || A.length < 2) {
            return;
        }
        mergeSort(A, 0, A.length - 1); // p = 0, r = n - 1
    }

    // CLRS-style: MERGE-SORT(A, p, r)
    private static void mergeSort(int[] A, int p, int r) {
        if (p < r) {
            int q = p + (r - p) / 2; // floor((p + r) / 2) safely
            mergeSort(A, p, q);
            mergeSort(A, q + 1, r);
            merge(A, p, q, r);
        }
    }

    // CLRS-style: MERGE(A, p, q, r)
    // Merges A[p..q] and A[q+1..r], assuming each is sorted.
    private static void merge(int[] A, int p, int q, int r) {
        int n1 = q - p + 1;
        int n2 = r - q;

        int[] L = new int[n1];
        int[] R = new int[n2];

        for (int i = 0; i < n1; i++) {
            L[i] = A[p + i];
        }
        for (int j = 0; j < n2; j++) {
            R[j] = A[q + 1 + j];
        }

        int i = 0;
        int j = 0;
        int k = p;

        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) { // stable: ties come from left
                A[k++] = L[i++];
            } else {
                A[k++] = R[j++];
            }
        }

        while (i < n1) {
            A[k++] = L[i++];
        }
        while (j < n2) {
            A[k++] = R[j++];
        }
    }
}
```

## Pitfalls and edge cases
- Forgetting the base case (`p < r`) causes infinite recursion.
- Off-by-one errors in subarray bounds (`p..q` and `q+1..r`) are common.
- Merge copy loops must use correct source indices.
- It uses extra arrays (`L`, `R`) each merge, so memory cost is higher than in-place sorts.
- `null` and size `< 2` inputs should return safely.

## Worked example
1. Start with `[8, 3, 5, 2, 9, 1, 7, 4, 6]`.
2. Split recursively into halves until single elements.
3. Merge `[8]` and `[3]` -> `[3, 8]`, merge `[5]` and `[2]` -> `[2, 5]`, then merge to `[2, 3, 5, 8]`.
4. Merge right side similarly to `[1, 4, 6, 7, 9]`.
5. Final merge of `[2, 3, 5, 8]` and `[1, 4, 6, 7, 9]` -> `[1, 2, 3, 4, 5, 6, 7, 8, 9]`.

## Demo (Java 17)
```java
import java.util.Arrays;

public class MergeSortCLRSDemo {
    public static void main(String[] args) {
        // Test 1: normal case
        int[] a1 = {8, 3, 5, 2, 9, 1, 7, 4, 6};
        MergeSortCLRS.mergeSort(a1);
        assertArrayEquals(new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9}, a1, "normal case");

        // Test 2: edge case (already sorted)
        int[] a2 = {1, 2, 3, 4, 5};
        MergeSortCLRS.mergeSort(a2);
        assertArrayEquals(new int[]{1, 2, 3, 4, 5}, a2, "already sorted");

        // Test 3: edge case (duplicates + negatives)
        int[] a3 = {4, -1, 4, 2, -1, 0};
        MergeSortCLRS.mergeSort(a3);
        assertArrayEquals(new int[]{-1, -1, 0, 2, 4, 4}, a3, "duplicates and negatives");

        // Test 4: edge case (empty)
        int[] a4 = {};
        MergeSortCLRS.mergeSort(a4);
        assertArrayEquals(new int[]{}, a4, "empty");

        System.out.println("All demo tests passed.");
    }

    private static void assertArrayEquals(int[] expected, int[] actual, String name) {
        if (!Arrays.equals(expected, actual)) {
            throw new AssertionError(
                "Failed " + name
                    + " | expected=" + Arrays.toString(expected)
                    + ", actual=" + Arrays.toString(actual)
            );
        }
        System.out.println("Passed: " + name);
    }
}
```
