

# Day 3: LeetCode 217 - Contains Duplicate

## Problem Statement

Given an integer array `nums`, return `true` if any value appears at least twice in the array, and return `false` if every element is distinct.

---

## Core Concepts

* **Hash-Based Data Structures:** Utilizing a Python `set()` for $O(1)$ average-time complexity lookups and insertions.
* **Single-Pass Traversal:** Finding an optimal solution by iterating through the array exactly once instead of relying on a brute-force $O(n^2)$ nested loop or a $O(n \log n)$ sorting approach.
* **Early Termination:** Optimizing runtime by immediately returning `True` the moment a duplicate is detected.

---

## Lessons Learned & Debugging

During implementation, several key syntactical and logical errors were identified and corrected:

* **Scope & Variable Naming:** Fixed an issue where `self` was incorrectly overwritten (`self = set()`). Defined a dedicated auxiliary variable instead (`seen = set()`).
* **Control Flow & Indentation:** Corrected a premature `return False` statement that was placed inside the loop. The `False` return must occur *after* the loop completes without finding duplicates.
* **State Management:** Fixed the placement of `seen.add(num)` to ensure it executes at the end of each iteration, preventing false positives while accurately tracking historical elements.

---

## Final Solution

```python
class Solution:
    def containsDuplicate(self, nums: list[int]) -> bool:
        seen = set()

        for num in nums:
            if num in seen:
                return True
            seen.add(num)

        return False

```

---

## 📊 Complexity Analysis

* **Time Complexity:** $O(n)$
The algorithm iterates through the array of length $n$ at most once. Hash set lookups and insertions operate in $O(1)$ average time.
* **Space Complexity:** $O(n)$
In the worst-case scenario (where all elements are unique), the `seen` set will store all $n$ elements.

---

## Key Takeaways

1. Prefer hash sets over lists for frequent membership testing (`if num in seen`).
2. Always verify Python indentation to ensure loop conditions and return statements execute within the correct scope.
3. Prioritize single-pass algorithms over sorting-based approaches when auxiliary space is permissible.
