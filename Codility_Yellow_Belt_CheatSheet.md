# Codility Yellow Belt Cheat Sheet - Python

**Focus**: Lessons 1–9 + most important algorithms for **Respectable** difficulty tasks.

## 1. Python Data Structures & Built-ins (Fast & Clean)

### Lists
```python
nums = [1, 2, 3]
nums.append(4)                    # O(1)
nums.pop()                        # O(1)
nums.pop(0)                       # O(N) → avoid, use deque
nums.insert(0, 10)                # O(N)
nums.sort()                       # O(N log N) in-place
sorted(nums)                      # returns new list
nums[::-1]                        # reverse copy
enumerate(nums)                   # (index, value)
```

### Dictionary & Counter
```python
from collections import Counter, defaultdict

freq = Counter(arr)
freq.most_common(1)               # most frequent element

d = defaultdict(int)
d[x] += 1                         # no KeyError
d = defaultdict(list)
d[key].append(value)
```

### Deque (Best for Stack & Queue)
```python
from collections import deque
dq = deque()

dq.append(1)          # right
dq.appendleft(1)      # left
dq.pop()              # right O(1)
dq.popleft()          # left O(1)
```

### Heap (Priority Queue)
```python
import heapq
heap = []
heapq.heappush(heap, (priority, value))
heapq.heappop(heap)               # smallest first
```

### Sets
```python
seen = set()
if x not in seen:
    seen.add(x)
```

### Useful One-liners
```python
max_val = max(arr) if arr else 0
min_val = min(arr) if arr else 0

# Prefix sums
prefix = [0]
for x in A:
    prefix.append(prefix[-1] + x)
```

---

## 2. Core Algorithms for Yellow Belt

| Algorithm                | When to Use                                      | Time Complexity | Key Idea |
|--------------------------|--------------------------------------------------|-----------------|----------|
| **Kadane’s Algorithm**   | Max contiguous subarray sum (Lesson 9)          | O(N)            | Dynamic programming |
| **Prefix Sums**          | Range sum queries, subarray problems (Lesson 5) | O(N)            | Precompute cumulative sum |
| **Two Pointers**         | Sorted arrays, pair sums, triangles             | O(N)            | Left + Right pointers |
| **Sliding Window**       | Contiguous subarray with condition              | O(N)            | Two pointers same direction |
| **Stack**                | Brackets, Fish, Next Greater Element (Lesson 7) | O(N)            | LIFO |
| **Boyer-Moore Voting**   | Find majority / dominator (Lesson 8)            | O(N)            | Candidate + counter |
| **Binary Search**        | Sorted data or optimization problems            | O(log N)        | Divide and conquer |
| **Caterpillar Method**   | Multiple two-pointer movements                   | O(N)            | Advanced two pointers |

---

### Kadane’s Algorithm (Maximum Slice)
```python
def solution(A):
    if not A:
        return 0
    max_ending_here = max_so_far = A[0]
    for x in A[1:]:
        max_ending_here = max(x, max_ending_here + x)
        max_so_far = max(max_so_far, max_ending_here)
    return max_so_far
```

---

### Prefix Sums
```python
def prefix_sums(A):
    prefix = [0] * (len(A) + 1)
    for i in range(1, len(A) + 1):
        prefix[i] = prefix[i-1] + A[i-1]
    return prefix

# Range sum from i to j (inclusive)
# prefix[j+1] - prefix[i]
```

---

### Two Pointers (Opposite Direction)
```python
def two_pointers(A, target):
    A = sorted(A)                    # if not already sorted
    left, right = 0, len(A) - 1
    while left < right:
        total = A[left] + A[right]
        if total == target:
            return True  # or record pair
        elif total < target:
            left += 1
        else:
            right -= 1
    return False
```

---

### Sliding Window (Variable Size)
```python
def max_sum_subarray(A, k):   # example: max sum of size k
    if len(A) < k:
        return 0
    current = sum(A[:k])
    max_sum = current
    for i in range(k, len(A)):
        current += A[i] - A[i-k]
        max_sum = max(max_sum, current)
    return max_sum
```

---

### Stack Template
```python
def solution(A):
    stack = []
    for item in A:
        while stack and condition(stack[-1], item):
            stack.pop()
        stack.append(item)
    return stack
```

**Common uses**: Brackets matching, Fish eating, Monotonic stack.

---

### Boyer-Moore Voting Algorithm (Dominator)
```python
def solution(A):
    if not A:
        return -1
    candidate = None
    count = 0
    for num in A:                     # Phase 1: Find candidate
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1
    
    # Phase 2: Verify
    if sum(1 for x in A if x == candidate) > len(A) // 2:
        return candidate
    return -1
```

---

### Binary Search Template
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

---

## Pro Tips for Yellow Belt

- **Always** consider time complexity — aim for **O(N)** or **O(N log N)**
- Use `Counter`, `defaultdict`, `deque`, `bisect`, and `heapq` aggressively
- Handle edge cases: `[]`, single element, all negative, all same values
- Sorting + Two Pointers solves many problems
- Practice **100% correctness + 100% performance**

---

**Good luck with your Codility Yellow Belt!** 🚀
