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

Day 6 – LeetCode 153: Find Minimum in Rotated Sorted Array
Problem
A sorted ascending array gets rotated between 1 and n times. Find the minimum element in O(log n).
pythonnums = [4,5,6,7,0,1,2]
# Output: 0
The original order was [0,1,2,4,5,6,7]. Rotation moved the minimum away from index 0.

Key Observation
A rotated sorted array always contains two sorted halves with a break between them:
[4, 5, 6, 7 | 0, 1, 2]
 left half      right half
The minimum sits at that break. Binary search finds it by checking which half is sorted at each step, then discarding the sorted half (it can't contain the minimum).

Algorithm
Initialize search boundaries and a running minimum:
pythonleft, right = 0, len(nums) - 1
result = nums[0]
At each step, compute mid and handle three cases.
Case 1 — current range is already sorted:
pythonif nums[left] < nums[right]:
    result = min(result, nums[left])
    break
No rotation in this window. The minimum is nums[left].
Case 2 — left half is sorted, minimum is in the right half:
pythonif nums[mid] >= nums[left]:
    left = mid + 1
Example: [4,5,6,7,0,1,2] with mid = 3 (nums[mid] = 7). Left half [4,5,6,7] is sorted — all values exceed the rotation point. Discard it.
Case 3 — mid is less than nums[left], so the rotation sits between left and mid:
pythonelse:
    right = mid - 1
Example: [6,7,0,1,2,3,4] with mid = 3 (nums[mid] = 1). nums[mid] < nums[left] means the minimum is somewhere in [left..mid]. Discard the right half.

Implementation
pythonclass Solution:
    def findMin(self, nums):
        result = nums[0]
        left, right = 0, len(nums) - 1

        while left <= right:
            if nums[left] < nums[right]:
                result = min(result, nums[left])
                break

            mid = (left + right) // 2
            result = min(result, nums[mid])

            if nums[mid] >= nums[left]:
                left = mid + 1
            else:
                right = mid - 1

        return result

Complexity
ComplexityReasonTimeO(log n)Search space halves each iterationSpaceO(1)Four scalar variables, no extra structures

Mistakes Made
Used min(nums) first. Correct answer, wrong complexity — O(n). The problem requires O(log n), so linear scan fails the constraint.
Confused which half to discard. The instinct was to chase the smaller numbers visually. The actual rule: identify the sorted half and discard it, because the minimum can only live at a discontinuity.

Takeaways

nums[mid] >= nums[left] means the left half is clean — discard it.
nums[mid] < nums[left] means the break is between left and mid — discard the right half.
nums[left] < nums[right] means the whole window is sorted — take nums[left] and stop.
Binary search works on conditions, not only exact values. This problem searches for the rotation point, not a specific number.
