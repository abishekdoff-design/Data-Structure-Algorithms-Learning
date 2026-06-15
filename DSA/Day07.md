# 15: 3Sum

## Problem Statement

Given an integer array `nums`, return all unique triplets `[nums[i], nums[j], nums[k]]` where:

```
nums[i] + nums[j] + nums[k] = 0
```

**Example:**

```python
nums = [-1, 0, 1, 2, -1, -4]
# Output: [[-1, -1, 2], [-1, 0, 1]]
```

---

## Core Idea: Reduce 3Sum to Two Sum

Fix one element `a`. The remaining task becomes finding `b + c = -a` — a Two Sum on the rest of the array.

---

## Why Sort First?

Sorting `[-1, 0, 1, 2, -1, -4]` → `[-4, -1, -1, 0, 1, 2]` does two things:

1. **Pointer movement becomes predictable.** Moving `left` right increases the sum. Moving `right` left decreases it.
2. **Duplicates go adjacent.** Skipping repeated values is a simple `==` check instead of a set lookup.

---

## Algorithm

```
Sort the array.

For each index i (fixed element = nums[i]):
    Skip if nums[i] == nums[i-1]  → duplicate fixed value

    Set left = i + 1, right = last index

    While left < right:
        sum = nums[i] + nums[left] + nums[right]

        If sum > 0  → right--
        If sum < 0  → left++
        If sum == 0:
            Record [nums[i], nums[left], nums[right]]
            left++, right--
            Skip while nums[left] == nums[left-1]  → duplicate pointer value
```

---

## Implementation

```python
class Solution:
    def threeSum(self, nums: list[int]) -> list[list[int]]:
        res = []
        nums.sort()

        for i, a in enumerate(nums):
            if i > 0 and a == nums[i - 1]:
                continue

            l, r = i + 1, len(nums) - 1

            while l < r:
                total = a + nums[l] + nums[r]

                if total > 0:
                    r -= 1
                elif total < 0:
                    l += 1
                else:
                    res.append([a, nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while l < r and nums[l] == nums[l - 1]:
                        l += 1

        return res
```

---

## Complexity

| | Complexity | Reason |
|---|---|---|
| Time | O(n²) | Sorting is O(n log n); the nested two-pointer loop is O(n²) |
| Space | O(1) | Only pointers and variables; output array excluded |

---

## Mistakes I Made

**1. Skipped sorting.** Without sorted input, pointer movement can't predict sum direction and duplicate skipping breaks.

**2. Indentation errors.** Python treats indentation as syntax. `elif` must align with `if`. Every nested block must be consistently indented.

**3. Forgot `res = []`.** `res.append(...)` throws a `NameError` if the list isn't initialized before the loop.

---

## Takeaways

- 3Sum is Two Sum with one element fixed.
- Sorting unlocks both pointer movement and duplicate skipping.
- Duplicate handling is where most bugs hide — both at the fixed index and at the pointers after a match.

---

## Pattern: Sort + Two Pointer + Duplicate Skip

Applies to: 2Sum variants, 3Sum, 4Sum, closest-sum problems, pair-search problems.
