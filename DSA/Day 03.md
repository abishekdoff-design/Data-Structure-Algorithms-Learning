# DSA Day 03 — LeetCode 88: Merge Sorted Array

## Problem Overview

### LeetCode 88 — Merge Sorted Array

Given two sorted arrays:

* `nums1`
* `nums2`

Merge both arrays into `nums1` such that the final array remains sorted in non-decreasing order.

Example:

Input:

```python
nums1 = [1,2,3,0,0,0]
m = 3

nums2 = [2,5,6]
n = 3
```

Output:

```python
[1,2,2,3,5,6]
```

---

# Learning Objective

Today's goal was to understand:

* Two Pointer technique
* In-place array modification
* Pointer movement
* Array traversal optimization
* Time and Space Complexity analysis

---

# Initial Understanding

At first glance, the problem looks like:

1. Combine arrays
2. Sort again

Example:

```python
[1,2,3] + [2,5,6]
```

Result:

```python
[1,2,3,2,5,6]
```

Sort:

```python
[1,2,2,3,5,6]
```

Although correct, this approach ignores an important observation:

**Both arrays are already sorted.**

---

# Key Insight

When a problem contains:

* Sorted Array
* Merge
* Comparison

It often suggests using:

## Two Pointers

Instead of sorting again, compare elements directly and place them in the correct position.

---

# Two Pointer Strategy

Three pointers are used:

```python
p1 = m - 1
p2 = n - 1
p  = m + n - 1
```

### Purpose

| Pointer | Meaning                          |
| ------- | -------------------------------- |
| p1      | Last valid element in nums1      |
| p2      | Last element in nums2            |
| p       | Last available position in nums1 |

---

# Why Start From The End?

Given:

```python
nums1 = [1,2,3,0,0,0]
```

The empty slots already exist at the end.

If merging starts from the front:

```python
[1,2,3,0,0,0]
 ^
```

Existing values may be overwritten before being processed.

Starting from the end avoids losing important data.

---

# Dry Run

Initial State:

```python
nums1 = [1,2,3,0,0,0]
nums2 = [2,5,6]
```

Compare:

```python
3 vs 6
```

Larger value:

```python
6
```

Place at last position:

```python
[1,2,3,0,0,6]
```

Move:

```python
p2--
p--
```

---

Compare:

```python
3 vs 5
```

Larger:

```python
5
```

Result:

```python
[1,2,3,0,5,6]
```

---

Compare:

```python
3 vs 2
```

Larger:

```python
3
```

Result:

```python
[1,2,3,3,5,6]
```

Continue until:

```python
[1,2,2,3,5,6]
```

---

# DSA Pattern Learned

## Two Pointers

This problem introduced the Two Pointer technique.

Characteristics:

* Compare elements from two locations
* Move pointers strategically
* Reduce unnecessary operations
* Improve efficiency

This pattern frequently appears in:

* Sorted arrays
* String problems
* Linked Lists
* Sliding Window problems

---

# Python Fundamentals Practiced

During this problem I used:

### Variables

```python
p1
p2
p
```

Used for tracking positions.

---

### Loops

```python
while
```

Used for traversal until one array is exhausted.

---

### Conditions

```python
if
else
```

Used to compare values and determine which element should be placed next.

---

### Arrays / Lists

Used indexing operations extensively:

```python
nums1[p1]
nums2[p2]
```

---

# Complexity Analysis

### Time Complexity

O(m + n)

Reason:

* Each element is processed only once.

---

### Space Complexity

O(1)

Reason:

* No extra array created.
* Modification performed directly inside nums1.

---

# Debugging Learnings

While solving this problem, I reinforced:

* Pointer movement tracking
* Understanding index positions
* Avoiding overwrite issues
* Visual dry-running before coding

---

# Key Takeaways

✅ Learned Two Pointer technique

✅ Understood why merging from the end is safer

✅ Practiced array traversal

✅ Improved pointer visualization skills

✅ Learned in-place modification strategy

✅ Reinforced Time Complexity and Space Complexity concepts

---

# Personal Reflection

This problem helped me understand that many DSA questions are not about memorizing code but recognizing patterns.

My biggest learning today was identifying:

* Sorted Array + Merge
* ⇒ Two Pointer Pattern


