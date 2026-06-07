# Day 5 - Maximum Subarray (LeetCode 53)

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
