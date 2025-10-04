# Sorting Algorithms

## 📖 Table of Contents
- [Comparison-Based Sorting](#comparison-based-sorting)
- [Non-Comparison-Based Sorting](#non-comparison-based-sorting)
- [Sorting Comparison Table](#sorting-comparison-table)
- [Important Problems](#important-problems)

---

## Comparison-Based Sorting

### 1. Bubble Sort

**Concept**: Repeatedly swap adjacent elements if they're in wrong order.

**Time**: O(n²) worst/average, O(n) best  
**Space**: O(1)  
**Stable**: Yes

```java
// Bubble Sort - O(n²) time, O(1) space
public void bubbleSort(int[] arr) {
    int n = arr.length;
    
    for (int i = 0; i < n - 1; i++) {
        boolean swapped = false;
        
        // Last i elements are already sorted
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        
        // If no swaps, array is sorted
        if (!swapped) break;
    }
}
```

---

### 2. Selection Sort

**Concept**: Find minimum element and place it at beginning.

**Time**: O(n²) all cases  
**Space**: O(1)  
**Stable**: No (but can be made stable)

```java
// Selection Sort - O(n²) time, O(1) space
public void selectionSort(int[] arr) {
    int n = arr.length;
    
    for (int i = 0; i < n - 1; i++) {
        // Find minimum element in remaining array
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        
        // Swap minimum with first element
        int temp = arr[minIndex];
        arr[minIndex] = arr[i];
        arr[i] = temp;
    }
}
```

---

### 3. Insertion Sort

**Concept**: Build sorted array one element at a time.

**Time**: O(n²) worst/average, O(n) best  
**Space**: O(1)  
**Stable**: Yes

```java
// Insertion Sort - O(n²) time, O(1) space
public void insertionSort(int[] arr) {
    int n = arr.length;
    
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        // Move elements greater than key one position ahead
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}
```

---

### 4. Merge Sort

**Concept**: Divide array into halves, sort recursively, and merge.

**Time**: O(n log n) all cases  
**Space**: O(n)  
**Stable**: Yes

```java
// Merge Sort - O(n log n) time, O(n) space
public void mergeSort(int[] arr) {
    if (arr.length < 2) return;
    
    mergeSortHelper(arr, 0, arr.length - 1);
}

private void mergeSortHelper(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        // Sort first and second halves
        mergeSortHelper(arr, left, mid);
        mergeSortHelper(arr, mid + 1, right);
        
        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}

private void merge(int[] arr, int left, int mid, int right) {
    // Calculate sizes of subarrays
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temp arrays
    int[] leftArr = new int[n1];
    int[] rightArr = new int[n2];
    
    // Copy data to temp arrays
    for (int i = 0; i < n1; i++) {
        leftArr[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        rightArr[j] = arr[mid + 1 + j];
    }
    
    // Merge temp arrays back
    int i = 0, j = 0, k = left;
    
    while (i < n1 && j < n2) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            i++;
        } else {
            arr[k] = rightArr[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining elements
    while (i < n1) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    
    while (j < n2) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
}
```

---

### 5. Quick Sort

**Concept**: Choose pivot, partition array, sort recursively.

**Time**: O(n log n) average, O(n²) worst  
**Space**: O(log n)  
**Stable**: No (but can be made stable)

```java
// Quick Sort - O(n log n) average, O(n²) worst
public void quickSort(int[] arr) {
    quickSortHelper(arr, 0, arr.length - 1);
}

private void quickSortHelper(int[] arr, int low, int high) {
    if (low < high) {
        // Partition the array
        int pivotIndex = partition(arr, low, high);
        
        // Sort elements before and after partition
        quickSortHelper(arr, low, pivotIndex - 1);
        quickSortHelper(arr, pivotIndex + 1, high);
    }
}

// Lomuto partition scheme
private int partition(int[] arr, int low, int high) {
    int pivot = arr[high];  // Choose last element as pivot
    int i = low - 1;        // Index of smaller element
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            // Swap arr[i] and arr[j]
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    
    // Swap arr[i+1] and arr[high] (pivot)
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    
    return i + 1;
}

// Hoare partition scheme (alternative)
private int partitionHoare(int[] arr, int low, int high) {
    int pivot = arr[low];
    int i = low - 1;
    int j = high + 1;
    
    while (true) {
        do {
            i++;
        } while (arr[i] < pivot);
        
        do {
            j--;
        } while (arr[j] > pivot);
        
        if (i >= j) {
            return j;
        }
        
        // Swap arr[i] and arr[j]
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

// Quick sort with random pivot (to avoid worst case)
private int randomPartition(int[] arr, int low, int high) {
    Random rand = new Random();
    int randomIndex = low + rand.nextInt(high - low + 1);
    
    // Swap random element with last element
    int temp = arr[randomIndex];
    arr[randomIndex] = arr[high];
    arr[high] = temp;
    
    return partition(arr, low, high);
}
```

---

### 6. Heap Sort

**Concept**: Build max heap, extract max repeatedly.

**Time**: O(n log n) all cases  
**Space**: O(1)  
**Stable**: No

```java
// Heap Sort - O(n log n) time, O(1) space
public void heapSort(int[] arr) {
    int n = arr.length;
    
    // Build max heap - O(n)
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
    // Extract elements one by one - O(n log n)
    for (int i = n - 1; i > 0; i--) {
        // Move current root to end
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        
        // Heapify reduced heap
        heapify(arr, i, 0);
    }
}

private void heapify(int[] arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    if (largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        
        heapify(arr, n, largest);
    }
}
```

---

### 7. Shell Sort

**Concept**: Generalized insertion sort with decreasing gap.

**Time**: O(n log n) to O(n²) depending on gap sequence  
**Space**: O(1)  
**Stable**: No

```java
// Shell Sort - O(n log n) average
public void shellSort(int[] arr) {
    int n = arr.length;
    
    // Start with large gap, reduce by half
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // Do gapped insertion sort
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;
            
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            
            arr[j] = temp;
        }
    }
}
```

---

## Non-Comparison-Based Sorting

### 1. Counting Sort

**Concept**: Count occurrences of each element.

**Time**: O(n + k) where k = range  
**Space**: O(k)  
**Stable**: Yes

```java
// Counting Sort - O(n + k) time, O(k) space
public void countingSort(int[] arr) {
    if (arr.length == 0) return;
    
    // Find range
    int max = arr[0];
    int min = arr[0];
    for (int num : arr) {
        max = Math.max(max, num);
        min = Math.min(min, num);
    }
    
    int range = max - min + 1;
    int[] count = new int[range];
    int[] output = new int[arr.length];
    
    // Count occurrences
    for (int num : arr) {
        count[num - min]++;
    }
    
    // Cumulative count
    for (int i = 1; i < range; i++) {
        count[i] += count[i - 1];
    }
    
    // Build output array (traverse backward for stability)
    for (int i = arr.length - 1; i >= 0; i--) {
        output[count[arr[i] - min] - 1] = arr[i];
        count[arr[i] - min]--;
    }
    
    // Copy output to original array
    System.arraycopy(output, 0, arr, 0, arr.length);
}
```

---

### 2. Radix Sort

**Concept**: Sort digit by digit using counting sort.

**Time**: O(d * (n + k)) where d = digits, k = base  
**Space**: O(n + k)  
**Stable**: Yes

```java
// Radix Sort - O(d * n) time
public void radixSort(int[] arr) {
    if (arr.length == 0) return;
    
    // Find maximum to know number of digits
    int max = arr[0];
    for (int num : arr) {
        max = Math.max(max, num);
    }
    
    // Do counting sort for each digit
    for (int exp = 1; max / exp > 0; exp *= 10) {
        countingSortByDigit(arr, exp);
    }
}

private void countingSortByDigit(int[] arr, int exp) {
    int n = arr.length;
    int[] output = new int[n];
    int[] count = new int[10];  // Base 10
    
    // Count occurrences
    for (int num : arr) {
        int digit = (num / exp) % 10;
        count[digit]++;
    }
    
    // Cumulative count
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }
    
    // Build output array
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }
    
    // Copy to original array
    System.arraycopy(output, 0, arr, 0, n);
}
```

---

### 3. Bucket Sort

**Concept**: Distribute elements into buckets, sort each bucket.

**Time**: O(n + k) average, O(n²) worst  
**Space**: O(n + k)  
**Stable**: Yes (if insertion sort used)

```java
// Bucket Sort - O(n) average
public void bucketSort(float[] arr) {
    if (arr.length == 0) return;
    
    int n = arr.length;
    
    // Create buckets
    List<List<Float>> buckets = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        buckets.add(new ArrayList<>());
    }
    
    // Distribute elements into buckets
    for (float num : arr) {
        int bucketIndex = (int) (n * num);
        buckets.get(bucketIndex).add(num);
    }
    
    // Sort individual buckets
    for (List<Float> bucket : buckets) {
        Collections.sort(bucket);
    }
    
    // Concatenate all buckets
    int index = 0;
    for (List<Float> bucket : buckets) {
        for (float num : bucket) {
            arr[index++] = num;
        }
    }
}

// Bucket sort for integers
public void bucketSortInt(int[] arr) {
    if (arr.length == 0) return;
    
    int max = arr[0];
    int min = arr[0];
    for (int num : arr) {
        max = Math.max(max, num);
        min = Math.min(min, num);
    }
    
    int bucketCount = arr.length;
    int bucketRange = (max - min) / bucketCount + 1;
    
    List<List<Integer>> buckets = new ArrayList<>();
    for (int i = 0; i < bucketCount; i++) {
        buckets.add(new ArrayList<>());
    }
    
    for (int num : arr) {
        int bucketIndex = (num - min) / bucketRange;
        buckets.get(bucketIndex).add(num);
    }
    
    int index = 0;
    for (List<Integer> bucket : buckets) {
        Collections.sort(bucket);
        for (int num : bucket) {
            arr[index++] = num;
        }
    }
}
```

---

## Sorting Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable | Notes |
|-----------|------|---------|-------|-------|--------|-------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Simple, rarely used |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No | Simple, minimizes swaps |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Good for small/nearly sorted |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | Consistent performance |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Usually fastest in practice |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No | In-place, not stable |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | Yes | For integers in range |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n+k) | Yes | For fixed-length keys |
| Bucket Sort | O(n+k) | O(n+k) | O(n²) | O(n+k) | Yes | For uniformly distributed |

---

## Important Problems

### 1. Sort Colors (Dutch National Flag)

```java
// Sort array with values 0, 1, 2 - O(n) time, O(1) space
public void sortColors(int[] nums) {
    int low = 0;
    int mid = 0;
    int high = nums.length - 1;
    
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums, low, mid);
            low++;
            mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else {
            swap(nums, mid, high);
            high--;
        }
    }
}

private void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

### 2. Merge Sorted Arrays

```java
// Merge two sorted arrays - O(m + n) time
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1;      // Last element of nums1
    int j = n - 1;      // Last element of nums2
    int k = m + n - 1;  // Last position
    
    // Merge from end
    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[k] = nums1[i];
            i--;
        } else {
            nums1[k] = nums2[j];
            j--;
        }
        k--;
    }
    
    // Copy remaining elements from nums2
    while (j >= 0) {
        nums1[k] = nums2[j];
        j--;
        k--;
    }
}
```

### 3. Kth Largest Element (Quick Select)

```java
// Find kth largest - O(n) average, O(n²) worst
public int findKthLargest(int[] nums, int k) {
    return quickSelect(nums, 0, nums.length - 1, nums.length - k);
}

private int quickSelect(int[] nums, int left, int right, int k) {
    if (left == right) return nums[left];
    
    int pivotIndex = partition(nums, left, right);
    
    if (k == pivotIndex) {
        return nums[k];
    } else if (k < pivotIndex) {
        return quickSelect(nums, left, pivotIndex - 1, k);
    } else {
        return quickSelect(nums, pivotIndex + 1, right, k);
    }
}
```

### 4. Sort Linked List

```java
// Sort linked list using merge sort - O(n log n) time
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) {
        return head;
    }
    
    // Find middle
    ListNode mid = findMiddle(head);
    ListNode left = head;
    ListNode right = mid.next;
    mid.next = null;
    
    // Sort halves
    left = sortList(left);
    right = sortList(right);
    
    // Merge
    return mergeLists(left, right);
}

private ListNode findMiddle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head.next;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
}

private ListNode mergeLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    current.next = (l1 != null) ? l1 : l2;
    
    return dummy.next;
}
```

### 5. Largest Number

```java
// Arrange numbers to form largest number - O(n log n) time
public String largestNumber(int[] nums) {
    // Convert to strings
    String[] strs = new String[nums.length];
    for (int i = 0; i < nums.length; i++) {
        strs[i] = String.valueOf(nums[i]);
    }
    
    // Sort with custom comparator
    Arrays.sort(strs, (a, b) -> (b + a).compareTo(a + b));
    
    // Handle leading zeros
    if (strs[0].equals("0")) {
        return "0";
    }
    
    // Concatenate
    StringBuilder sb = new StringBuilder();
    for (String str : strs) {
        sb.append(str);
    }
    
    return sb.toString();
}
```

### 6. Custom Sort String

```java
// Sort string based on custom order - O(n) time
public String customSortString(String order, String s) {
    int[] count = new int[26];
    
    // Count characters in s
    for (char c : s.toCharArray()) {
        count[c - 'a']++;
    }
    
    StringBuilder result = new StringBuilder();
    
    // Add characters in order
    for (char c : order.toCharArray()) {
        while (count[c - 'a'] > 0) {
            result.append(c);
            count[c - 'a']--;
        }
    }
    
    // Add remaining characters
    for (char c = 'a'; c <= 'z'; c++) {
        while (count[c - 'a'] > 0) {
            result.append(c);
            count[c - 'a']--;
        }
    }
    
    return result.toString();
}
```

### 7. Java's Built-in Sorting

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;

public class JavaSorting {
    
    public void primitiveArrays() {
        int[] arr = {5, 2, 8, 1, 9};
        
        // Sort ascending - O(n log n)
        Arrays.sort(arr);
        
        // Sort range
        Arrays.sort(arr, 0, 3);  // Sort first 3 elements
    }
    
    public void objectArrays() {
        Integer[] arr = {5, 2, 8, 1, 9};
        
        // Sort ascending
        Arrays.sort(arr);
        
        // Sort descending
        Arrays.sort(arr, Collections.reverseOrder());
        
        // Custom comparator
        Arrays.sort(arr, (a, b) -> b - a);
    }
    
    public void listSorting() {
        List<Integer> list = Arrays.asList(5, 2, 8, 1, 9);
        
        // Sort ascending
        Collections.sort(list);
        
        // Sort descending
        Collections.sort(list, Collections.reverseOrder());
        
        // Java 8+ stream sorting
        list.stream().sorted().collect(Collectors.toList());
    }
    
    public void customObjects() {
        class Student {
            String name;
            int marks;
            
            Student(String name, int marks) {
                this.name = name;
                this.marks = marks;
            }
        }
        
        List<Student> students = new ArrayList<>();
        
        // Sort by marks
        Collections.sort(students, (a, b) -> a.marks - b.marks);
        
        // Sort by name
        Collections.sort(students, Comparator.comparing(s -> s.name));
        
        // Multiple criteria
        Collections.sort(students, 
            Comparator.comparing((Student s) -> s.marks)
                     .thenComparing(s -> s.name));
    }
}
```

---

## Key Takeaways

1. **Quick Sort**: Best for general-purpose sorting (in-place, fast average)
2. **Merge Sort**: Best when stability needed (stable, consistent O(n log n))
3. **Heap Sort**: Best when memory is constrained (in-place, O(n log n))
4. **Counting/Radix**: Best for integers in known range (O(n) possible)
5. **Insertion Sort**: Best for small or nearly sorted arrays

**Interview Tip**: Know time/space complexity and when to use each algorithm!

