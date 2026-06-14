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

# 153: Find Minimum in Rotated Sorted Array

## Problem Statement

Given an ascending sorted array rotated between `1` and `n` times, find the minimum element in `O(log n)`.

```python
nums = [4,5,6,7,0,1,2]
# Output: 0
```

The original order was `[0,1,2,4,5,6,7]`. Rotation shifts the smallest value away from index 0.

---

## Core Observation

A rotated sorted array always splits into two sorted halves:

```text
[4, 5, 6, 7 | 0, 1, 2]
 left sorted   right sorted
```

The minimum sits at the rotation boundary. Binary search locates that boundary by checking which half is sorted at every step.

---

## Algorithm

Initialize search bounds and a running minimum:

```python
left, right = 0, len(nums) - 1
result = nums[0]
```

Loop until the bounds cross:

```python
while left <= right:
```

**Step 1 — Early exit if the range is already sorted:**

```python
if nums[left] < nums[right]:
    result = min(result, nums[left])
    break
```

No rotation in this range. The leftmost value is the smallest.

**Step 2 — Compute mid and update result:**

```python
mid = (left + right) // 2
result = min(result, nums[mid])
```

**Step 3 — Determine which half contains the rotation point:**

```python
if nums[mid] >= nums[left]:
    # Left half [left..mid] is sorted → minimum is in the right half
    left = mid + 1
else:
    # Right half [mid..right] has the dip → minimum is in the left half
    right = mid - 1
```

---

## Full Implementation

```python
class Solution:
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
```

---

## Complexity

| | |
|---|---|
| Time | `O(log n)` — search space halves each iteration |
| Space | `O(1)` — only `left`, `right`, `mid`, `result` |

---

## What Tripped Me Up

**1. `min(nums)` is wrong for this problem**
It gives the right answer but runs in `O(n)`. The constraint requires `O(log n)`, so binary search is mandatory.

**2. Getting the direction wrong after finding the sorted half**

When `nums[mid] >= nums[left]`, the left half is sorted — meaning the minimum is *not* there. Move `left = mid + 1` to search the right half.

When `nums[mid] < nums[left]`, the right half is sorted — the rotation point (and minimum) is somewhere in `[left..mid]`. Move `right = mid - 1`.

Getting this backwards sends the search the wrong direction.

**3. Binary search doesn't have to find a value**

Here it locates a *condition* — the index where the array wraps. Any time a problem asks for a boundary or inflection point in sorted/partially-sorted data, binary search is a candidate.

---

## Key Takeaways

- A rotated sorted array always has at least one sorted half — use that to eliminate half the search space.
- If the current range is already sorted, `nums[left]` is the minimum candidate. Stop early.
- Track `result` across iterations rather than returning `nums[mid]` directly, since the true minimum might sit at a boundary you've already passed.

**Pattern:** Binary Search on Rotated Sorted Arrays — applies to minimum/maximum in modified sorted structures, rotation count problems, and search-in-rotated-array variants (LC 33, 154).


# 33: Search in Rotated Sorted Array

## Problem

Given a rotated sorted array of distinct integers and a target value, return its index or `-1`.

```python
nums = [4,5,6,7,0,1,2], target = 0
# Output: 4
```

**Constraint:** O(log n) time.

---

## Core Insight

A rotated sorted array always has one sorted half at every binary search step.

```text
[4, 5, 6, 7, 0, 1, 2]
 └── sorted ──┘ └─ sorted ─┘
```

Identify which half is sorted, check if the target falls in it, then discard the other half.

---

## Algorithm

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid

            # Left half is sorted
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # Right half is sorted
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```

---

## Sorted Half Identification

**Left half sorted** — `nums[left] <= nums[mid]`:

```text
[4, 5, 6, 7, 0, 1, 2]
 ^        ^
left     mid     ← left portion is normal
```

Target in range `[nums[left], nums[mid])`? Move `right = mid - 1`. Otherwise, `left = mid + 1`.

**Right half sorted** — everything else:

Target in range `(nums[mid], nums[right]]`? Move `left = mid + 1`. Otherwise, `right = mid - 1`.

---

## Complexity

| | Complexity | Reason |
|---|---|---|
| Time | O(log n) | Half the search space discarded each step |
| Space | O(1) | Three integer variables: `left`, `right`, `mid` |

---

## Bugs I Hit

**Wrong pointer direction:**
```python
# Wrong
right = mid + 1  # expands search instead of shrinking it

# Correct
right = mid - 1
```

**Off-by-one in range check:**

The conditions use strict `<` on one side to avoid double-counting `mid`:
```python
nums[left] <= target < nums[mid]   # left sorted
nums[mid] < target <= nums[right]  # right sorted
```

Getting this wrong sends the pointer into the unsorted half.

---

## Takeaways

- Rotated arrays retain enough order to run binary search — you just check which half is sorted first.
- The boundary conditions (`<=` vs `<`) matter. One wrong comparison breaks the partition logic.
- Same sorted-half trick applies to LC 153 (find minimum) and rotation-point problems.
