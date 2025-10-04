# Divide and Conquer

## 📖 Table of Contents
- [Concept](#concept)
- [Classic Problems](#classic-problems)
- [Advanced Applications](#advanced-applications)

---

## Concept

### What is Divide and Conquer?
Break problem into smaller subproblems, solve recursively, combine results.

**Steps:**
1. **Divide**: Break into smaller subproblems
2. **Conquer**: Solve subproblems recursively
3. **Combine**: Merge solutions

---

## Classic Problems

### 1. Merge Sort (covered in Sorting)

### 2. Quick Sort (covered in Sorting)

### 3. Binary Search (covered in Searching)

### 4. Maximum Subarray (Kadane's vs D&C)

```java
// Divide and Conquer approach - O(n log n) time
public int maxSubArray(int[] nums) {
    return maxSubArrayHelper(nums, 0, nums.length - 1);
}

private int maxSubArrayHelper(int[] nums, int left, int right) {
    if (left == right) {
        return nums[left];
    }
    
    int mid = left + (right - left) / 2;
    
    int leftMax = maxSubArrayHelper(nums, left, mid);
    int rightMax = maxSubArrayHelper(nums, mid + 1, right);
    int crossMax = maxCrossingSum(nums, left, mid, right);
    
    return Math.max(Math.max(leftMax, rightMax), crossMax);
}

private int maxCrossingSum(int[] nums, int left, int mid, int right) {
    int leftSum = Integer.MIN_VALUE;
    int sum = 0;
    
    for (int i = mid; i >= left; i--) {
        sum += nums[i];
        leftSum = Math.max(leftSum, sum);
    }
    
    int rightSum = Integer.MIN_VALUE;
    sum = 0;
    
    for (int i = mid + 1; i <= right; i++) {
        sum += nums[i];
        rightSum = Math.max(rightSum, sum);
    }
    
    return leftSum + rightSum;
}
```

### 5. Closest Pair of Points

```java
// Find closest pair of points - O(n log n) time
class Point {
    int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }
}

public double closestPair(Point[] points) {
    Arrays.sort(points, (a, b) -> a.x - b.x);
    return closestPairHelper(points, 0, points.length - 1);
}

private double closestPairHelper(Point[] points, int left, int right) {
    if (right - left <= 3) {
        return bruteForce(points, left, right);
    }
    
    int mid = left + (right - left) / 2;
    Point midPoint = points[mid];
    
    double leftMin = closestPairHelper(points, left, mid);
    double rightMin = closestPairHelper(points, mid + 1, right);
    
    double minDist = Math.min(leftMin, rightMin);
    
    // Find points in strip
    List<Point> strip = new ArrayList<>();
    for (int i = left; i <= right; i++) {
        if (Math.abs(points[i].x - midPoint.x) < minDist) {
            strip.add(points[i]);
        }
    }
    
    return Math.min(minDist, stripClosest(strip, minDist));
}

private double bruteForce(Point[] points, int left, int right) {
    double min = Double.MAX_VALUE;
    for (int i = left; i <= right; i++) {
        for (int j = i + 1; j <= right; j++) {
            min = Math.min(min, distance(points[i], points[j]));
        }
    }
    return min;
}

private double distance(Point p1, Point p2) {
    return Math.sqrt((p1.x - p2.x) * (p1.x - p2.x) + 
                     (p1.y - p2.y) * (p1.y - p2.y));
}

private double stripClosest(List<Point> strip, double d) {
    double min = d;
    strip.sort((a, b) -> a.y - b.y);
    
    for (int i = 0; i < strip.size(); i++) {
        for (int j = i + 1; j < strip.size() && 
             (strip.get(j).y - strip.get(i).y) < min; j++) {
            min = Math.min(min, distance(strip.get(i), strip.get(j)));
        }
    }
    
    return min;
}
```

### 6. Count Inversions

```java
// Count inversions in array - O(n log n) time
public int countInversions(int[] arr) {
    return mergeSortAndCount(arr, 0, arr.length - 1);
}

private int mergeSortAndCount(int[] arr, int left, int right) {
    int count = 0;
    
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        count += mergeSortAndCount(arr, left, mid);
        count += mergeSortAndCount(arr, mid + 1, right);
        count += mergeAndCount(arr, left, mid, right);
    }
    
    return count;
}

private int mergeAndCount(int[] arr, int left, int mid, int right) {
    int[] leftArr = Arrays.copyOfRange(arr, left, mid + 1);
    int[] rightArr = Arrays.copyOfRange(arr, mid + 1, right + 1);
    
    int i = 0, j = 0, k = left, count = 0;
    
    while (i < leftArr.length && j < rightArr.length) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k++] = leftArr[i++];
        } else {
            arr[k++] = rightArr[j++];
            count += (mid + 1) - (left + i);
        }
    }
    
    while (i < leftArr.length) {
        arr[k++] = leftArr[i++];
    }
    
    while (j < rightArr.length) {
        arr[k++] = rightArr[j++];
    }
    
    return count;
}
```

### 7. Power Function

```java
// Calculate x^n - O(log n) time
public double myPow(double x, int n) {
    long N = n;
    if (N < 0) {
        x = 1 / x;
        N = -N;
    }
    
    return fastPow(x, N);
}

private double fastPow(double x, long n) {
    if (n == 0) return 1.0;
    
    double half = fastPow(x, n / 2);
    
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * x;
    }
}
```

---

## Key Takeaways

1. **Divide**: Break into smaller problems
2. **Conquer**: Solve recursively
3. **Combine**: Merge solutions
4. **Master Theorem**: Analyze time complexity
5. **Examples**: Merge sort, Quick sort, Binary search

**Interview Tip**: Look for problems that can be broken into similar subproblems!

