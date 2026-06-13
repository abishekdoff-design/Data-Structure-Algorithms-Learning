# 152: Maximum Product Subarray

**Difficulty:** Medium
**Topic:** Dynamic Programming, Arrays
**Status:** ✅ Solved

---

## Problem

Given an integer array `nums`, find the contiguous subarray that produces the maximum product and return that product.

```python
Input:  nums = [2, 3, -2, 4]
Output: 6
# Subarray [2, 3] → 2 × 3 = 6
```

---

## Why This Is Different From Maximum Sum

In sum problems, a negative number always reduces the result — so you discard it. In product problems, two negatives multiply into a positive, so a minimum product at one index can flip into the maximum at the next.

```text
current_min = -6
next element = -4
-6 × -4 = 24  ← new maximum
```

This means you can't track only the running maximum. You need both.

---

## Approach: DP with Two States

At every index, track:

- `current_max` — largest product ending here
- `current_min` — smallest product ending here

For each element, three candidates exist:

1. Start fresh: `current`
2. Extend the max: `prev_max × current`
3. Extend the min: `prev_min × current`

Take `max()` of the three for the new maximum, `min()` for the new minimum.

---

## Implementation

```python
class Solution:
    def maxProduct(self, nums):
        current_max = nums[0]
        current_min = nums[0]
        answer = nums[0]

        for current in nums[1:]:
            prev_max = current_max
            prev_min = current_min

            current_max = max(current, prev_max * current, prev_min * current)
            current_min = min(current, prev_max * current, prev_min * current)

            answer = max(answer, current_max)

        return answer
```

---

## Complexity

| | |
|---|---|
| Time | O(n) — single pass |
| Space | O(1) — three variables only |

---

## Mistakes Made

**1. Tracking only `current_max`**
Missed that a negative minimum can become the new maximum after sign flip.

**2. Updating `current_max` before saving it**
Used the already-updated value to calculate `current_min`. Fix: store `prev_max` and `prev_min` before any update.

**3. Returning a list instead of an integer**
Returned `[answer, current_max]` — wrong type. Correct: `return answer`.

---

## Key Takeaway

Product DP requires two states because sign can flip direction. Always snapshot state before updating variables in-place.

**Pattern:** DP with dual extremes — useful for negative numbers, sign changes, and continuous product problems.
