# Maximum Subarray (LeetCode 53)

## Problem Statement
Given an integer array `nums`, find the contiguous subarray with the largest sum and return its sum.

## Approach: Kadane's Algorithm

Kadane's Algorithm efficiently finds the maximum subarray sum in a single traversal of the array.

### Key Idea
For each element:
- Either start a new subarray from the current element.
- Or extend the previous subarray.

The maximum value encountered during this process is the answer.

## Algorithm
1. Initialize `current_sum` and `max_sum` with the first element.
2. Traverse the array from the second element.
3. Update:
   ```python
   current_sum = max(num, current_sum + num)

Here's the cleaned-up version, GitHub README-ready, minimal slop:

---

# 268: Missing Number

**Problem:** Given an array of `n` distinct integers in range `[0, n]`, find the missing one.

```python
nums = [3, 0, 1]  # → 2
```

---

## The Insight

An array of length `n` should hold values from `0` to `n` — that's `n + 1` possible values, one guaranteed missing. Sum them both and subtract.

```python
expected = n * (n + 1) // 2
actual   = sum(nums)
return expected - actual
```

---

## Why Not Sort?

Sorting works. It's just O(n log n) with extra steps. The math does the same job in one pass.

---

## Solution

```python
class Solution:
    def missingNumber(self, nums: list[int]) -> int:
        n = len(nums)
        return n * (n + 1) // 2 - sum(nums)
```

**Time:** O(n) · **Space:** O(1)

---

## The Trap

`n = 4` → range is `[0, 4]` → **5 values**, not 4. The off-by-one trips people up. Array length gives you `n`; the range gives you `n + 1` slots.

---

**Takeaway:** When the problem involves a known sequence with one element missing, reach for expected-vs-actual arithmetic before touching sort or sets.

---

**Two facts I verified and want to flag:**

1. Your complexity analysis is correct — `sum(nums)` is a single O(n) traversal in Python, and no auxiliary structure is allocated.
2. The formula `n * (n + 1) // 2` assumes integers, which holds here since the problem guarantees distinct integers. No edge case issue.

One small thing: your original notes say "the array is traversed once while computing the actual sum" — technically `len(nums)` is O(1) in Python (stored, not counted), so the only traversal is `sum()`. Minor, but worth knowing for interview precision.
