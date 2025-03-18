# Sliding Window Mind Map

## 1. **Introduction**
   - A powerful technique for solving problems involving arrays, strings, or lists.
   - Focuses on a subset of elements, processing the data in a continuous segment.

---

## 2. **Types of Sliding Window**
### A. **Fixed-Size Sliding Window**
   - The window size remains constant.
   - Problems involve processing exactly `k` elements at a time.

   #### **Key Concepts**
   - Maintain a **start** and **end** pointer.
   - Slide the window by incrementing the pointers.
   - Add the new element entering the window and remove the element leaving the window.

   #### **Steps**
   1. Initialize pointers: `start = 0`, `end = 0`.
   2. Expand the window until the size is `k`.
   3. Process the current window (e.g., compute sum, max).
   4. Slide the window by incrementing `start` and `end`.

   #### **Examples**
   - Maximum sum of subarray of size `k`.
   - First negative integer in every window of size `k`.

   #### **General Format**
   ```
   start = end = 0
   while end < len(arr):
       # calculations
       if end-start+1 < k:
           end += 1
       elif end-start+1 == k:
           1. answer <- calculation, build answer using calculation
           2. remove start from calculation
           3. slide the window to maintain k-window size
    return answer
   ```

---

### B. **Variable-Size Sliding Window**
   - The window size changes dynamically based on conditions.
   - Problems involve finding the optimal window size or sum based on constraints.
   - **Calculation/Answer/Condition is given, find the window size.**

   #### **Key Concepts**
   - Maintain a **start** pointer and expand the **end** pointer dynamically.
   - Adjust the window size to meet the constraints.

   #### **Steps**
   1. Initialize pointers: `start = 0`, `end = 0`.
   2. Expand the window by moving `end` until the condition is satisfied.
   3. Shrink the window by incrementing `start` if the condition is violated.
   4. Keep track of the result during expansion or shrinking.

   #### **Examples**
   - Smallest subarray with a sum greater than `S`.
   - Longest substring with at most `K` distinct characters.

   #### **General Format**
   ```
   start = end = 0
   while end < len(array):
       # calculations
       if condition is not met:
           end += 1
       elif condition is met:
           1. answer <- calculations, build possible answer from calculations
           2. remove from calculation, shrink window if required
           3. slide the window if req
   return answer
   ```

---

## 3. **Common Patterns**
   - **Using Deques:** For problems involving maximum or minimum in a sliding window.
   - **Using HashMaps:** For tracking characters or counts in a window.
   - **Two-Pointer Technique:** A simpler way to manage the window boundaries.

---

## 4. **Examples from Aditya Verma's Playlist**
### Fixed-Size Window
1. **Maximum Sum of Subarray of Size K**
   - Input: `[2, 3, 5, 2, 8, 1, 5], k = 3`
   - Output: `15`

2. **First Negative Integer in Every Window of Size K**
   - Input: `[12, -1, -7, 8, -15, 30, 16, 28], k = 3`
   - Output: `[-1, -1, -7, -15, -15, 0]`

### Variable-Size Window
1. **Smallest Subarray with Sum >= S**
   - Input: `[2, 3, 1, 2, 4, 3], S = 7`
   - Output: `2`

2. **Longest Substring with K Unique Characters**
   - Input: `"aabacbebebe", k = 3`
   - Output: `"cbebebe"`

---

## 5. **Advantages of Sliding Window**
   - Optimized to O(n) compared to brute force O(n^2).
   - Minimal auxiliary space.

---

## 6. **Challenges**
   - Identifying when to expand or shrink the window.
   - Managing auxiliary data structures (e.g., HashMaps, Deques).

---

## 7. **Key Takeaways**
   - Fixed-size windows are simpler; variable-size windows require dynamic adjustments.
   - Understand constraints clearly to determine when to shrink or expand.
   - Practice common problems to solidify the concepts.

---

## 8. **References**
   - Aditya Verma's Sliding Window Playlist on [YouTube](https://www.youtube.com/playlist?list=PL_z_8CaSLPWeT1ffjiImo0sYTcnLzo-wY)