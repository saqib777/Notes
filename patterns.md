
# DSA Patterns & Techniques

## 1. Two Pointers

Use when working with sorted arrays or when you need to find a pair.
```python
def two_pointers(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current = arr[left] + arr[right]
        if current == target:
            return [left, right]
        elif current < target:
            left += 1
        else:
            right -= 1
    return []
```

**When to use:** sorted array, pair sum, palindrome check, container with most water.

---

## 2. Sliding Window

Use when asked about subarrays or substrings of a fixed or variable size.
```python
def sliding_window_fixed(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)
    return max_sum
```

**When to use:** max/min subarray of size k, longest substring without repeats.

---

## 3. Hash Map (Frequency Count)

Use when you need to track occurrences or find complements.
```python
def frequency_count(arr):
    count = {}
    for item in arr:
        count[item] = count.get(item, 0) + 1
    return count
```

**When to use:** anagram check, two sum, group anagrams, top k frequent elements.

---

## 4. Binary Search

Use on sorted arrays when you need O(log n) search.
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

**When to use:** search in sorted array, find boundary, rotated array search.

---

## 5. Fast and Slow Pointers

Use for cycle detection in linked lists.
```python
def has_cycle(head):
    slow, fast = head, head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

**When to use:** detect cycle, find middle of linked list, find duplicate number.

---

## 6. Depth First Search (DFS)

Use for tree/graph traversal going deep before wide.
```python
def dfs(node, visited=set()):
    if node is None or node in visited:
        return
    visited.add(node)
    for neighbor in node.neighbors:
        dfs(neighbor, visited)
```

**When to use:** path finding, tree traversal, connected components, cycle detection.

---

## Pattern Recognition Guide

| If the problem says... | Think... |
|------------------------|----------|
| Sorted array, find pair | Two Pointers |
| Subarray / substring of size k | Sliding Window |
| Count occurrences / find complement | Hash Map |
| Sorted array, O(log n) required | Binary Search |
| Linked list cycle | Fast & Slow Pointers |
| Tree / graph traversal | DFS / BFS |
| Optimization (min/max) with subproblems | Dynamic Programming |
