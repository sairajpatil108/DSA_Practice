# Searching Algorithms

## 📖 Table of Contents
- [Linear Search](#linear-search)
- [Binary Search](#binary-search)
- [Binary Search Variations](#binary-search-variations)
- [Ternary Search](#ternary-search)
- [Exponential Search](#exponential-search)
- [Important Problems](#important-problems)

---

## Linear Search

**Concept**: Search element sequentially from start to end.

**Time**: O(n)  
**Space**: O(1)

```java
// Linear Search - O(n) time, O(1) space
public int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;  // Return index if found
        }
    }
    return -1;  // Not found
}

// Linear search in 2D array
public int[] linearSearch2D(int[][] matrix, int target) {
    for (int i = 0; i < matrix.length; i++) {
        for (int j = 0; j < matrix[i].length; j++) {
            if (matrix[i][j] == target) {
                return new int[]{i, j};
            }
        }
    }
    return new int[]{-1, -1};
}
```

---

## Binary Search

**Concept**: Search in sorted array by repeatedly dividing search space in half.

**Time**: O(log n)  
**Space**: O(1) iterative, O(log n) recursive  
**Prerequisite**: Array must be sorted

```java
// Binary Search - Iterative - O(log n) time, O(1) space
public int binarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        
        if (arr[mid] == target) {
            return mid;  // Found
        } else if (arr[mid] < target) {
            left = mid + 1;  // Search right half
        } else {
            right = mid - 1;  // Search left half
        }
    }
    
    return -1;  // Not found
}

// Binary Search - Recursive - O(log n) time, O(log n) space
public int binarySearchRecursive(int[] arr, int target) {
    return binarySearchHelper(arr, target, 0, arr.length - 1);
}

private int binarySearchHelper(int[] arr, int target, int left, int right) {
    if (left > right) {
        return -1;  // Not found
    }
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        return binarySearchHelper(arr, target, mid + 1, right);
    } else {
        return binarySearchHelper(arr, target, left, mid - 1);
    }
}
```

---

## Binary Search Variations

### 1. Find First Occurrence

```java
// Find first occurrence of target - O(log n) time
public int findFirst(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;  // Continue searching left
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

### 2. Find Last Occurrence

```java
// Find last occurrence of target - O(log n) time
public int findLast(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            left = mid + 1;  // Continue searching right
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

### 3. Count Occurrences

```java
// Count occurrences of target - O(log n) time
public int countOccurrences(int[] arr, int target) {
    int first = findFirst(arr, target);
    if (first == -1) return 0;
    
    int last = findLast(arr, target);
    return last - first + 1;
}
```

### 4. Find Ceiling (Smallest element >= target)

```java
// Find ceiling - O(log n) time
public int findCeiling(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] >= target) {
            result = mid;
            right = mid - 1;  // Try to find smaller ceiling
        } else {
            left = mid + 1;
        }
    }
    
    return result;
}
```

### 5. Find Floor (Largest element <= target)

```java
// Find floor - O(log n) time
public int findFloor(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] <= target) {
            result = mid;
            left = mid + 1;  // Try to find larger floor
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

### 6. Search in Rotated Sorted Array

```java
// Search in rotated sorted array - O(log n) time
public int searchRotated(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        }
        
        // Check which half is sorted
        if (arr[left] <= arr[mid]) {
            // Left half is sorted
            if (arr[left] <= target && target < arr[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {
            // Right half is sorted
            if (arr[mid] < target && target <= arr[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

### 7. Find Peak Element

```java
// Find peak element (greater than neighbors) - O(log n) time
public int findPeakElement(int[] arr) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] > arr[mid + 1]) {
            // Peak is on left side or mid itself
            right = mid;
        } else {
            // Peak is on right side
            left = mid + 1;
        }
    }
    
    return left;
}
```

### 8. Search in 2D Matrix (Sorted)

```java
// Search in 2D matrix (each row sorted) - O(log(m*n)) time
public boolean searchMatrix(int[][] matrix, int target) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return false;
    }
    
    int m = matrix.length;
    int n = matrix[0].length;
    int left = 0;
    int right = m * n - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int row = mid / n;
        int col = mid % n;
        int midValue = matrix[row][col];
        
        if (midValue == target) {
            return true;
        } else if (midValue < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return false;
}

// Search in 2D matrix (rows and columns sorted)
public boolean searchMatrix2(int[][] matrix, int target) {
    if (matrix.length == 0 || matrix[0].length == 0) {
        return false;
    }
    
    int row = 0;
    int col = matrix[0].length - 1;
    
    // Start from top-right corner
    while (row < matrix.length && col >= 0) {
        if (matrix[row][col] == target) {
            return true;
        } else if (matrix[row][col] > target) {
            col--;  // Move left
        } else {
            row++;  // Move down
        }
    }
    
    return false;
}
```

### 9. Find Minimum in Rotated Sorted Array

```java
// Find minimum in rotated sorted array - O(log n) time
public int findMin(int[] arr) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] > arr[right]) {
            // Minimum is in right half
            left = mid + 1;
        } else {
            // Minimum is in left half or mid
            right = mid;
        }
    }
    
    return arr[left];
}
```

### 10. Binary Search on Answer

```java
// Find square root using binary search - O(log n) time
public int sqrt(int x) {
    if (x == 0) return 0;
    
    int left = 1;
    int right = x;
    int result = 0;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (mid <= x / mid) {  // Avoid overflow
            result = mid;
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

// Koko eating bananas
public int minEatingSpeed(int[] piles, int h) {
    int left = 1;
    int right = 0;
    for (int pile : piles) {
        right = Math.max(right, pile);
    }
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (canFinish(piles, mid, h)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}

private boolean canFinish(int[] piles, int speed, int h) {
    int hours = 0;
    for (int pile : piles) {
        hours += (pile + speed - 1) / speed;  // Ceiling division
    }
    return hours <= h;
}
```

---

## Ternary Search

**Concept**: Divide search space into three parts (for unimodal functions).

**Time**: O(log₃ n)  
**Space**: O(1)

```java
// Ternary Search - O(log n) time
public int ternarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left <= right) {
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] == target) {
            return mid1;
        }
        if (arr[mid2] == target) {
            return mid2;
        }
        
        if (target < arr[mid1]) {
            right = mid1 - 1;
        } else if (target > arr[mid2]) {
            left = mid2 + 1;
        } else {
            left = mid1 + 1;
            right = mid2 - 1;
        }
    }
    
    return -1;
}

// Find maximum in unimodal array
public int findMaximum(int[] arr) {
    int left = 0;
    int right = arr.length - 1;
    
    while (right - left > 2) {
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] < arr[mid2]) {
            left = mid1;
        } else {
            right = mid2;
        }
    }
    
    int max = arr[left];
    for (int i = left + 1; i <= right; i++) {
        max = Math.max(max, arr[i]);
    }
    
    return max;
}
```

---

## Exponential Search

**Concept**: Find range where element exists, then binary search.

**Time**: O(log n)  
**Space**: O(1)

```java
// Exponential Search - O(log n) time
public int exponentialSearch(int[] arr, int target) {
    if (arr[0] == target) {
        return 0;
    }
    
    // Find range for binary search
    int i = 1;
    while (i < arr.length && arr[i] <= target) {
        i *= 2;
    }
    
    // Binary search in range [i/2, min(i, n-1)]
    return binarySearchHelper(arr, target, i / 2, 
                              Math.min(i, arr.length - 1));
}
```

---

## Important Problems

### 1. Find First and Last Position

```java
// Find first and last position - O(log n) time
public int[] searchRange(int[] nums, int target) {
    int first = findFirst(nums, target);
    if (first == -1) {
        return new int[]{-1, -1};
    }
    
    int last = findLast(nums, target);
    return new int[]{first, last};
}
```

### 2. Find K Closest Elements

```java
// Find k closest elements to x - O(log n + k) time
public List<Integer> findClosestElements(int[] arr, int k, int x) {
    int left = 0;
    int right = arr.length - k;
    
    // Binary search for best window start
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (x - arr[mid] > arr[mid + k] - x) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    List<Integer> result = new ArrayList<>();
    for (int i = left; i < left + k; i++) {
        result.add(arr[i]);
    }
    
    return result;
}
```

### 3. Median of Two Sorted Arrays

```java
// Find median of two sorted arrays - O(log(min(m,n))) time
public double findMedianSortedArrays(int[] nums1, int[] nums2) {
    // Ensure nums1 is smaller
    if (nums1.length > nums2.length) {
        return findMedianSortedArrays(nums2, nums1);
    }
    
    int m = nums1.length;
    int n = nums2.length;
    int left = 0;
    int right = m;
    
    while (left <= right) {
        int partition1 = (left + right) / 2;
        int partition2 = (m + n + 1) / 2 - partition1;
        
        int maxLeft1 = (partition1 == 0) ? Integer.MIN_VALUE : 
                       nums1[partition1 - 1];
        int minRight1 = (partition1 == m) ? Integer.MAX_VALUE : 
                        nums1[partition1];
        
        int maxLeft2 = (partition2 == 0) ? Integer.MIN_VALUE : 
                       nums2[partition2 - 1];
        int minRight2 = (partition2 == n) ? Integer.MAX_VALUE : 
                        nums2[partition2];
        
        if (maxLeft1 <= minRight2 && maxLeft2 <= minRight1) {
            // Found correct partition
            if ((m + n) % 2 == 0) {
                return (Math.max(maxLeft1, maxLeft2) + 
                        Math.min(minRight1, minRight2)) / 2.0;
            } else {
                return Math.max(maxLeft1, maxLeft2);
            }
        } else if (maxLeft1 > minRight2) {
            right = partition1 - 1;
        } else {
            left = partition1 + 1;
        }
    }
    
    throw new IllegalArgumentException();
}
```

### 4. Capacity To Ship Packages Within D Days

```java
// Find minimum capacity to ship packages - O(n log(sum)) time
public int shipWithinDays(int[] weights, int days) {
    int left = 0;
    int right = 0;
    
    for (int weight : weights) {
        left = Math.max(left, weight);  // At least max weight
        right += weight;                // At most sum of all
    }
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (canShip(weights, days, mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}

private boolean canShip(int[] weights, int days, int capacity) {
    int daysNeeded = 1;
    int currentLoad = 0;
    
    for (int weight : weights) {
        if (currentLoad + weight > capacity) {
            daysNeeded++;
            currentLoad = 0;
        }
        currentLoad += weight;
    }
    
    return daysNeeded <= days;
}
```

### 5. Split Array Largest Sum

```java
// Split array to minimize largest sum - O(n log(sum)) time
public int splitArray(int[] nums, int m) {
    int left = 0;
    int right = 0;
    
    for (int num : nums) {
        left = Math.max(left, num);
        right += num;
    }
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (canSplit(nums, m, mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    
    return left;
}

private boolean canSplit(int[] nums, int m, int maxSum) {
    int splits = 1;
    int currentSum = 0;
    
    for (int num : nums) {
        if (currentSum + num > maxSum) {
            splits++;
            currentSum = 0;
        }
        currentSum += num;
    }
    
    return splits <= m;
}
```

### 6. Java's Built-in Binary Search

```java
import java.util.Arrays;
import java.util.Collections;

public class JavaBinarySearch {
    
    public void primitiveArrays() {
        int[] arr = {1, 3, 5, 7, 9};
        
        // Binary search - returns index if found, negative if not
        int index = Arrays.binarySearch(arr, 5);  // 2
        
        // Search in range
        int index2 = Arrays.binarySearch(arr, 0, 3, 3);  // 1
        
        // If not found: -(insertion point) - 1
        int notFound = Arrays.binarySearch(arr, 4);  // -3
    }
    
    public void objectArrays() {
        String[] arr = {"apple", "banana", "cherry"};
        
        // Binary search
        int index = Arrays.binarySearch(arr, "banana");
        
        // Custom comparator
        Arrays.binarySearch(arr, "banana", String.CASE_INSENSITIVE_ORDER);
    }
    
    public void listBinarySearch() {
        List<Integer> list = Arrays.asList(1, 3, 5, 7, 9);
        
        // Binary search on list
        int index = Collections.binarySearch(list, 5);
        
        // With comparator
        Collections.binarySearch(list, 5, Collections.reverseOrder());
    }
}
```

---

## Key Takeaways

1. **Binary Search**: O(log n), requires sorted array
2. **Variations**: First/last occurrence, ceiling/floor, rotated array
3. **Binary Search on Answer**: Search for optimal value in range
4. **2D Search**: Treat as 1D or start from corner
5. **Template**: Always use `left + (right - left) / 2` to avoid overflow

**Interview Tip**: If problem involves "minimum/maximum to satisfy condition", consider binary search on answer!

