# 📘 DSA Day 02 — Best Time to Buy and Sell Stock

## 🔹 Problem

LeetCode 121 — Best Time to Buy and Sell Stock

Given an array where:

```python
prices[i]
```

represents stock price on day `i`.

Goal:

* Buy once
* Sell once in future
* Return maximum possible profit

---

# 🧠 Core Logic Learned

This problem is based on:

* array traversal
* running minimum tracking
* optimization thinking

Instead of checking every buy/sell pair using nested loops, we optimize using a single traversal.

---

# ⚡ Important Formula

Profit Formula:

profit = selling_price - buying_price

Example:

* buy at `1`
* sell at `6`

Profit:

```python
6 - 1 = 5
```

---

# 🔥 Key DSA Pattern

## Running Minimum Tracking

Continuously track:

* minimum buying price seen so far
* maximum profit seen so far

---

# 🛠 Fundamentals Used

## ✅ Variables

Used to track changing state.

```python
min_price
max_profit
current_profit
```

---

## ✅ Arrays / Lists

Input stored in list.

Example:

```python
[7,1,5,3,6,4]
```

---

## ✅ Loop Traversal

Used:

```python
for loop
```

Reason:

* traverse prices one by one

---

## ✅ Conditions

Used:

```python
if
```

Reason:

* update minimum price
* update maximum profit

---

# 🚀 Optimization Thinking

## ❌ Brute Force

Using nested loops:

Time Complexity:
O(n²)

Very slow for large inputs.

---

## ✅ Optimal Approach

Using single traversal:

Time Complexity:
O(n)

Efficient solution.

---

# 💾 Space Complexity

Only few variables used.

Space Complexity:
O(1)

---

# 🐞 Debugging Learned

## Errors Faced

### 1. Function Scope Error

Problem:

```python
NameError: maxProfit not defined
```

Reason:

* function was inside class
* needed proper method structure

---

### 2. Variable Name Mismatch

Mistake:

```python
current_price
```

Correct:

```python
current_profit
```

Learned:

* variable naming consistency is important

---

### 3. Typo Bug

Mistake:

```python
max_proft
```

Correct:

```python
max_profit
```

Learned:

* small typos cause runtime issues

---

# ✅ Final Learning Outcome

Today I learned:

* state tracking
* optimization mindset
* time complexity basics
* debugging runtime errors
* array traversal patterns
* how to think step-by-step in DSA

---

# 📈 Progress

✔ Solved LeetCode 121
✔ Understood optimal solution
✔ Learned O(n) traversal optimization
✔ Practiced debugging and logic building
✔ Improved Python syntax understanding
