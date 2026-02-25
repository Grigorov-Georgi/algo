---
title: Two Sum
difficulty: Easy
tags: [array, hash-map]
---

## Problem

Given an array of integers and a target, return the indices of two numbers that add up to the target.

## Key idea

Use a hash map to store previously seen values and their indices.

## Complexity

- Time: O(n)
- Space: O(n)

## Python example

```python
def two_sum(nums, target):
    seen = {}
    for i, value in enumerate(nums):
        need = target - value
        if need in seen:
            return [seen[need], i]
        seen[value] = i
    return []
```
