DSA Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. What is an Array & memory storage?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A linear data structure that stores elements of the same type in contiguous memory locations
Because memory is contiguous, each element can be accessed directly using an index via address calculation: base_address + (index * size_of_element)
This contiguous storage is what makes array access O(1)

2. Time complexities
~~~~~~~~~~~~~~~~~~~~

Access — O(1) — direct index-based lookup
Search — O(n) unsorted, O(log n) if sorted (binary search)
Insertion — O(n) worst case (shifting elements), O(1) if inserting at the end (amortized, dynamic array)
Deletion — O(n) worst case (shifting elements), O(1) if deleting from the end

3. Static vs. Dynamic Array
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Static Array — fixed size determined at creation, size cannot change, memory allocated once
Dynamic Array — resizable (e.g. JS arrays, Python lists, ArrayList/Vector), automatically grows by allocating a new larger block and copying elements when capacity is exceeded
Dynamic arrays trade some overhead (occasional resize costs) for flexibility

4. Max/min with minimum comparisons
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Naive: 2n comparisons (separate loop for max and min)
Optimized: process elements in pairs, compare pair elements to each other first, then compare the larger to current max and smaller to current min — about 3n/2 comparisons
js
function findMinMax(arr) {
  let min = arr[0], max = arr[0];
  for (let i = 1; i < arr.length - 1; i += 2) {
    let a = arr[i], b = arr[i + 1];
    if (a > b) { max = Math.max(max, a); min = Math.min(min, b); }
    else { max = Math.max(max, b); min = Math.min(min, a); }
  }
  if (arr.length % 2 !== 0) { // handle odd length leftover
    max = Math.max(max, arr[arr.length - 1]);
    min = Math.min(min, arr[arr.length - 1]);
  }
  return { min, max };
}

5. Reverse an array in-place (no extra space)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

js
function reverseArray(arr) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    [arr[left], arr[right]] = [arr[right], arr[left]];
    left++;
    right--;
  }
  return arr;
}
Two-pointer swap, O(n) time, O(1) space

6. Kth max/min element
~~~~~~~~~~~~~~~~~~~~~~

Simple approach: sort the array, then pick the Kth index — O(n log n)
Optimized: use a min-heap of size K for Kth largest (or max-heap for Kth smallest), or Quickselect algorithm — average O(n)
js
function kthLargest(arr, k) {
  return [...arr].sort((a, b) => b - a)[k - 1];
}

7. Sort array of 0s, 1s, 2s (Dutch National Flag)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

js
function sortColors(arr) {
  let low = 0, mid = 0, high = arr.length - 1;
  while (mid <= high) {
    if (arr[mid] === 0) { [arr[low], arr[mid]] = [arr[mid], arr[low]]; low++; mid++; }
    else if (arr[mid] === 1) { mid++; }
    else { [arr[mid], arr[high]] = [arr[high], arr[mid]]; high--; }
  }
  return arr;
}
Single pass, O(n) time, O(1) space — no generic sorting algorithm used

8. Maximum subarray sum (Kadane's Algorithm)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

js
function maxSubArray(nums) {
  let maxSum = nums[0], currentSum = nums[0];
  for (let i = 1; i < nums.length; i++) {
    currentSum = Math.max(nums[i], currentSum + nums[i]);
    maxSum = Math.max(maxSum, currentSum);
  }
  return maxSum;
}
O(n) time — at each step, decide whether to extend the current subarray or start fresh

9. Find duplicate in array of N+1 integers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using Floyd's Cycle Detection (treats array values as pointers) — O(n) time, O(1) space:
js
function findDuplicate(nums) {
  let slow = nums[0], fast = nums[0];
  do { slow = nums[slow]; fast = nums[nums[fast]]; } while (slow !== fast);
  slow = nums[0];
  while (slow !== fast) { slow = nums[slow]; fast = nums[fast]; }
  return slow;
}
Simpler alternative: use a Set to track seen numbers — O(n) time, O(n) space

10. Two Sum
~~~~~~~~~~~

js
function twoSum(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) return [map.get(complement), i];
    map.set(nums[i], i);
  }
  return [];
}
O(n) time using a hash map, instead of O(n²) brute force

11. Two-Pointer approach
~~~~~~~~~~~~~~~~~~~~~~~~

Uses two indices moving through the array (from ends inward, or both from the start) to reduce time complexity, typically avoiding nested loops
Example — checking if a sorted array has a pair summing to target:
js
function hasPairWithSum(arr, target) {
  let left = 0, right = arr.length - 1;
  while (left < right) {
    const sum = arr[left] + arr[right];
    if (sum === target) return true;
    sum < target ? left++ : right--;
  }
  return false;
}
Commonly used for: reversing arrays, pair-sum problems, removing duplicates, partitioning

12. Sliding Window technique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Maintains a "window" (subarray/substring range) that expands/contracts over the data, avoiding recomputation for overlapping subarrays
Used when the problem involves contiguous subarrays/substrings — like max sum of size K, longest substring without repeating characters
js
function maxSumSubarray(arr, k) {
  let maxSum = 0, windowSum = 0;
  for (let i = 0; i < arr.length; i++) {
    windowSum += arr[i];
    if (i >= k - 1) {
      maxSum = Math.max(maxSum, windowSum);
      windowSum -= arr[i - k + 1];
    }
  }
  return maxSum;
}
Reduces time complexity from O(n×k) brute force to O(n)

13. Three Sum (triplets summing to zero)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

js
function threeSum(nums) {
  nums.sort((a, b) => a - b);
  const result = [];
  for (let i = 0; i < nums.length - 2; i++) {
    if (i > 0 && nums[i] === nums[i - 1]) continue; // skip duplicates
    let left = i + 1, right = nums.length - 1;
    while (left < right) {
      const sum = nums[i] + nums[left] + nums[right];
      if (sum === 0) {
        result.push([nums[i], nums[left], nums[right]]);
        while (left < right && nums[left] === nums[left + 1]) left++;
        while (left < right && nums[right] === nums[right - 1]) right--;
        left++; right--;
      } else if (sum < 0) left++;
      else right--;
    }
  }
  return result;
}
Sort first, then fix one element and use two-pointer for the rest — O(n²) time

14. Trapping Rain Water
~~~~~~~~~~~~~~~~~~~~~~~

js
function trap(height) {
  let left = 0, right = height.length - 1;
  let leftMax = 0, rightMax = 0, water = 0;
  while (left < right) {
    if (height[left] < height[right]) {
      height[left] >= leftMax ? leftMax = height[left] : water += leftMax - height[left];
      left++;
    } else {
      height[right] >= rightMax ? rightMax = height[right] : water += rightMax - height[right];
      right--;
    }
  }
  return water;
}
Two-pointer approach, O(n) time, O(1) space — water trapped at each position depends on the shorter of the max walls to its left and right

15. Subarray vs. Subsequence vs. Subset
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Subarray — contiguous elements from the original array, order preserved (e.g. [2,3] from [1,2,3,4])
Subsequence — elements in the same relative order, but not necessarily contiguous (e.g. [1,3,4] from [1,2,3,4])
Subset — any combination of elements, order doesn't matter, includes the empty set (e.g. [3,1] from [1,2,3] is valid, even out of original order)