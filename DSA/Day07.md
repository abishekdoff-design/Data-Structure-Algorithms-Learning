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




# 11: Container With Most Water

## Problem Statement

Given an integer array `height`, where `height[i]` represents the height of a vertical line, find two lines that together with the x-axis form a container that stores the maximum amount of water.

The container cannot be tilted.

**Example:**

```python
height = [1,8,6,2,5,4,8,3,7]
# Output: 49
```

---

## Core Idea: Two Pointer + Greedy Elimination

Brute force checks every pair: `O(n^2)`.
Two pointers start at the widest possible container — index 0 and index n-1 — then shrink inward, discarding one side per step instead of checking all pairs.

---

## Why Move the Smaller Height Pointer

Area is bounded by the shorter of the two lines, since water spills over the lower wall

Proof that discarding the shorter pointer loses nothing:

Suppose `height[left] < height[right]`. Any container that still uses `left` paired with some other index `k` (where `k < right`) has width `≤ right - left`, and height `≤ height[left]` (since `left` is still the constraint, or worse). So no pairing of `left` with anything inside the current window can beat what's already been computed. `left` is safe to discard.

This is what makes the elimination greedy and exhaustive — every discarded pointer position is provably suboptimal, not just heuristically unlikely.

---

## Algorithm
left = 0

right = len(height) - 1

maximum_area = 0
while left < right:

width = right - left

container_height = min(height[left], height[right])

area = width * container_height

maximum_area = max(maximum_area, area)

---

## Implementation

```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        left = 0
        right = len(height) - 1
        maximum_area = 0

        while left < right:
            width = right - left
            container_height = min(height[left], height[right])
            area = width * container_height
            maximum_area = max(maximum_area, area)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1

        return maximum_area
```

Verified against: `[1,8,6,2,5,4,8,3,7] -> 49`, `[4,3,2,1,4] -> 16`, `[1,2,1] -> 2`, `[2,3,4,5,18,17,6] -> 17`, `[1,1] -> 1`.

---

## Complexity

| | Complexity | Reason |
|---|---|---|
| Time | O(n) | `left` and `right` each move at most n times total before crossing |
| Space | O(1) | Only scalar variables, no auxiliary structures |

---

## Mistakes I Made

**1. Assumed the taller line mattered more.**

The shorter line caps the water level regardless of how tall the other side is. Moving the taller pointer can never increase the effective height — it can only shrink width.

**2. Moved pointers without justifying the discard.**

It's not enough to say "move the shorter one" — the reason it's safe is that the shorter pointer, paired with anything still inside the window, is bounded by a width strictly less than the current one and a height no greater than the current minimum. So it can't produce a better result than what's already been recorded.

**3. Defaulted to brute force first.**

`O(n^2)` checks every pair explicitly. The two-pointer approach replaces explicit checking with a per-step elimination proof, cutting this to `O(n)`.

---

## Takeaways

- Two-pointer technique applies when a value is bounded by two endpoints moving toward each other.
- A greedy step is only valid if you can prove the discarded option is dominated, not just assume it.
- Identify the limiting factor in the formula before deciding what to optimize.
- Going from `O(n^2)` to `O(n)` here came from eliminating provably suboptimal pairs, not from a heuristic shortcut.

---

## Pattern: Two Pointer + Greedy Elimination

Applies to:

- Container / boundary-area problems
- Pair-comparison problems on sorted or static arrays
- Maximum/minimum range problems bounded by two endpoints
