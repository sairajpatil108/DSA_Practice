# Heaps and Priority Queues

## 📖 Table of Contents
- [Heap Basics](#heap-basics)
- [Min Heap Implementation](#min-heap-implementation)
- [Max Heap Implementation](#max-heap-implementation)
- [Heap Operations](#heap-operations)
- [Heap Sort](#heap-sort)
- [Important Problems](#important-problems)

---

## Heap Basics

### What is a Heap?
A Heap is a complete binary tree that satisfies the heap property:
- **Max Heap**: Parent ≥ Children (root is maximum)
- **Min Heap**: Parent ≤ Children (root is minimum)

**Properties:**
- Complete binary tree (filled left to right)
- Usually implemented using array
- Parent at index i, children at 2i+1 and 2i+2
- Height = log(n)

**Time Complexities:**
- Insert: O(log n)
- Delete (extract): O(log n)
- Get Min/Max: O(1)
- Heapify: O(log n)
- Build Heap: O(n)

**Array Representation:**
```
For node at index i:
- Left child: 2 * i + 1
- Right child: 2 * i + 2
- Parent: (i - 1) / 2
```

---

## Min Heap Implementation

```java
public class MinHeap {
    private int[] heap;
    private int size;       // Current number of elements
    private int capacity;   // Maximum size
    
    // Constructor
    public MinHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.heap = new int[capacity];
    }
    
    // Get parent index
    private int parent(int i) {
        return (i - 1) / 2;
    }
    
    // Get left child index
    private int leftChild(int i) {
        return 2 * i + 1;
    }
    
    // Get right child index
    private int rightChild(int i) {
        return 2 * i + 2;
    }
    
    // Swap two elements
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    // Insert element - O(log n) time
    public void insert(int value) {
        if (size == capacity) {
            System.out.println("Heap is full");
            return;
        }
        
        // Insert at end
        heap[size] = value;
        int current = size;
        size++;
        
        // Heapify up (bubble up)
        while (current > 0 && heap[current] < heap[parent(current)]) {
            swap(current, parent(current));
            current = parent(current);
        }
    }
    
    // Get minimum (root) - O(1) time
    public int getMin() {
        if (size == 0) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap[0];
    }
    
    // Extract minimum - O(log n) time
    public int extractMin() {
        if (size == 0) {
            throw new IllegalStateException("Heap is empty");
        }
        
        if (size == 1) {
            size--;
            return heap[0];
        }
        
        // Store root value
        int min = heap[0];
        
        // Move last element to root
        heap[0] = heap[size - 1];
        size--;
        
        // Heapify down from root
        heapifyDown(0);
        
        return min;
    }
    
    // Heapify down - O(log n) time
    private void heapifyDown(int i) {
        int smallest = i;
        int left = leftChild(i);
        int right = rightChild(i);
        
        // Find smallest among node and children
        if (left < size && heap[left] < heap[smallest]) {
            smallest = left;
        }
        
        if (right < size && heap[right] < heap[smallest]) {
            smallest = right;
        }
        
        // If smallest is not current node
        if (smallest != i) {
            swap(i, smallest);
            heapifyDown(smallest);  // Recursively heapify
        }
    }
    
    // Delete specific element - O(log n) time
    public void delete(int index) {
        if (index >= size) {
            return;
        }
        
        // Replace with last element
        heap[index] = heap[size - 1];
        size--;
        
        // Heapify down or up based on value
        if (index > 0 && heap[index] < heap[parent(index)]) {
            // Heapify up
            while (index > 0 && heap[index] < heap[parent(index)]) {
                swap(index, parent(index));
                index = parent(index);
            }
        } else {
            // Heapify down
            heapifyDown(index);
        }
    }
    
    // Build heap from array - O(n) time
    public void buildHeap(int[] arr) {
        size = arr.length;
        capacity = arr.length;
        heap = arr;
        
        // Start from last non-leaf node and heapify down
        for (int i = (size - 2) / 2; i >= 0; i--) {
            heapifyDown(i);
        }
    }
    
    // Check if heap is empty
    public boolean isEmpty() {
        return size == 0;
    }
    
    // Get size
    public int size() {
        return size;
    }
    
    // Display heap
    public void display() {
        System.out.print("Heap: ");
        for (int i = 0; i < size; i++) {
            System.out.print(heap[i] + " ");
        }
        System.out.println();
    }
}
```

---

## Max Heap Implementation

```java
public class MaxHeap {
    private int[] heap;
    private int size;
    private int capacity;
    
    public MaxHeap(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.heap = new int[capacity];
    }
    
    private int parent(int i) { return (i - 1) / 2; }
    private int leftChild(int i) { return 2 * i + 1; }
    private int rightChild(int i) { return 2 * i + 2; }
    
    private void swap(int i, int j) {
        int temp = heap[i];
        heap[i] = heap[j];
        heap[j] = temp;
    }
    
    // Insert - O(log n) time
    public void insert(int value) {
        if (size == capacity) {
            System.out.println("Heap is full");
            return;
        }
        
        heap[size] = value;
        int current = size;
        size++;
        
        // Heapify up
        while (current > 0 && heap[current] > heap[parent(current)]) {
            swap(current, parent(current));
            current = parent(current);
        }
    }
    
    // Get maximum - O(1) time
    public int getMax() {
        if (size == 0) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap[0];
    }
    
    // Extract maximum - O(log n) time
    public int extractMax() {
        if (size == 0) {
            throw new IllegalStateException("Heap is empty");
        }
        
        if (size == 1) {
            size--;
            return heap[0];
        }
        
        int max = heap[0];
        heap[0] = heap[size - 1];
        size--;
        
        heapifyDown(0);
        
        return max;
    }
    
    // Heapify down - O(log n) time
    private void heapifyDown(int i) {
        int largest = i;
        int left = leftChild(i);
        int right = rightChild(i);
        
        if (left < size && heap[left] > heap[largest]) {
            largest = left;
        }
        
        if (right < size && heap[right] > heap[largest]) {
            largest = right;
        }
        
        if (largest != i) {
            swap(i, largest);
            heapifyDown(largest);
        }
    }
}
```

---

## Heap Operations

### Using Java's PriorityQueue

```java
import java.util.PriorityQueue;
import java.util.Collections;

public class PriorityQueueExamples {
    
    // Min Heap (default)
    public void minHeapExample() {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        
        // Insert elements
        minHeap.offer(30);
        minHeap.offer(10);
        minHeap.offer(20);
        minHeap.offer(5);
        
        // Get minimum (peek) - O(1)
        System.out.println("Min: " + minHeap.peek());  // 5
        
        // Extract minimum (poll) - O(log n)
        System.out.println("Extracted: " + minHeap.poll());  // 5
        
        // Size
        System.out.println("Size: " + minHeap.size());  // 3
    }
    
    // Max Heap
    public void maxHeapExample() {
        PriorityQueue<Integer> maxHeap = 
            new PriorityQueue<>(Collections.reverseOrder());
        
        maxHeap.offer(30);
        maxHeap.offer(10);
        maxHeap.offer(20);
        maxHeap.offer(5);
        
        System.out.println("Max: " + maxHeap.peek());  // 30
        System.out.println("Extracted: " + maxHeap.poll());  // 30
    }
    
    // Custom comparator
    public void customComparatorExample() {
        // Heap of arrays sorted by first element
        PriorityQueue<int[]> pq = new PriorityQueue<>(
            (a, b) -> a[0] - b[0]
        );
        
        pq.offer(new int[]{3, 5});
        pq.offer(new int[]{1, 2});
        pq.offer(new int[]{2, 4});
        
        int[] min = pq.poll();  // {1, 2}
        System.out.println(min[0] + ", " + min[1]);
    }
    
    // Heap of custom objects
    class Student {
        String name;
        int marks;
        
        Student(String name, int marks) {
            this.name = name;
            this.marks = marks;
        }
    }
    
    public void customObjectExample() {
        // Sort by marks (ascending)
        PriorityQueue<Student> pq = new PriorityQueue<>(
            (a, b) -> a.marks - b.marks
        );
        
        pq.offer(new Student("Alice", 85));
        pq.offer(new Student("Bob", 90));
        pq.offer(new Student("Charlie", 80));
        
        Student topStudent = pq.poll();
        System.out.println(topStudent.name + ": " + topStudent.marks);
    }
}
```

---

## Heap Sort

```java
public class HeapSort {
    
    // Heap sort - O(n log n) time, O(1) space
    public void heapSort(int[] arr) {
        int n = arr.length;
        
        // Step 1: Build max heap - O(n)
        for (int i = n / 2 - 1; i >= 0; i--) {
            heapify(arr, n, i);
        }
        
        // Step 2: Extract elements one by one - O(n log n)
        for (int i = n - 1; i > 0; i--) {
            // Move current root to end
            swap(arr, 0, i);
            
            // Heapify reduced heap
            heapify(arr, i, 0);
        }
    }
    
    // Heapify subtree - O(log n) time
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
            swap(arr, i, largest);
            heapify(arr, n, largest);
        }
    }
    
    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

---

## Important Problems

### 1. Kth Largest Element

```java
// Find kth largest element - O(n log k) time, O(k) space
public int findKthLargest(int[] nums, int k) {
    // Use min heap of size k
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    
    for (int num : nums) {
        minHeap.offer(num);
        
        // Keep only k largest elements
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    
    return minHeap.peek();
}

// Alternative: Using max heap - O(n + k log n)
public int findKthLargestMaxHeap(int[] nums, int k) {
    PriorityQueue<Integer> maxHeap = 
        new PriorityQueue<>(Collections.reverseOrder());
    
    for (int num : nums) {
        maxHeap.offer(num);
    }
    
    // Remove k-1 elements
    for (int i = 0; i < k - 1; i++) {
        maxHeap.poll();
    }
    
    return maxHeap.peek();
}
```

### 2. Merge K Sorted Lists

```java
// Merge k sorted linked lists - O(n log k) time
public ListNode mergeKLists(ListNode[] lists) {
    if (lists == null || lists.length == 0) {
        return null;
    }
    
    // Min heap based on node values
    PriorityQueue<ListNode> minHeap = new PriorityQueue<>(
        (a, b) -> a.val - b.val
    );
    
    // Add first node from each list
    for (ListNode list : lists) {
        if (list != null) {
            minHeap.offer(list);
        }
    }
    
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (!minHeap.isEmpty()) {
        // Get smallest node
        ListNode node = minHeap.poll();
        current.next = node;
        current = current.next;
        
        // Add next node from same list
        if (node.next != null) {
            minHeap.offer(node.next);
        }
    }
    
    return dummy.next;
}
```

### 3. Top K Frequent Elements

```java
// Find k most frequent elements - O(n log k) time
public int[] topKFrequent(int[] nums, int k) {
    // Count frequencies
    Map<Integer, Integer> freqMap = new HashMap<>();
    for (int num : nums) {
        freqMap.put(num, freqMap.getOrDefault(num, 0) + 1);
    }
    
    // Min heap based on frequency
    PriorityQueue<Integer> minHeap = new PriorityQueue<>(
        (a, b) -> freqMap.get(a) - freqMap.get(b)
    );
    
    for (int num : freqMap.keySet()) {
        minHeap.offer(num);
        
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    
    // Extract results
    int[] result = new int[k];
    for (int i = 0; i < k; i++) {
        result[i] = minHeap.poll();
    }
    
    return result;
}
```

### 4. Find Median from Data Stream

```java
// Maintain median with two heaps - O(log n) insert, O(1) median
class MedianFinder {
    private PriorityQueue<Integer> maxHeap;  // Lower half
    private PriorityQueue<Integer> minHeap;  // Upper half
    
    public MedianFinder() {
        maxHeap = new PriorityQueue<>(Collections.reverseOrder());
        minHeap = new PriorityQueue<>();
    }
    
    // Add number - O(log n)
    public void addNum(int num) {
        // Add to max heap first
        maxHeap.offer(num);
        
        // Balance: move largest from max to min
        minHeap.offer(maxHeap.poll());
        
        // Keep max heap size >= min heap size
        if (maxHeap.size() < minHeap.size()) {
            maxHeap.offer(minHeap.poll());
        }
    }
    
    // Find median - O(1)
    public double findMedian() {
        if (maxHeap.size() > minHeap.size()) {
            return maxHeap.peek();
        }
        return (maxHeap.peek() + minHeap.peek()) / 2.0;
    }
}
```

### 5. K Closest Points to Origin

```java
// Find k closest points - O(n log k) time
public int[][] kClosest(int[][] points, int k) {
    // Max heap based on distance
    PriorityQueue<int[]> maxHeap = new PriorityQueue<>(
        (a, b) -> (b[0] * b[0] + b[1] * b[1]) - 
                  (a[0] * a[0] + a[1] * a[1])
    );
    
    for (int[] point : points) {
        maxHeap.offer(point);
        
        if (maxHeap.size() > k) {
            maxHeap.poll();
        }
    }
    
    int[][] result = new int[k][2];
    for (int i = 0; i < k; i++) {
        result[i] = maxHeap.poll();
    }
    
    return result;
}
```

### 6. Kth Smallest Element in Sorted Matrix

```java
// Find kth smallest in n x n matrix - O(k log n) time
public int kthSmallest(int[][] matrix, int k) {
    int n = matrix.length;
    
    // Min heap with {value, row, col}
    PriorityQueue<int[]> minHeap = new PriorityQueue<>(
        (a, b) -> a[0] - b[0]
    );
    
    // Add first element from each row
    for (int i = 0; i < Math.min(n, k); i++) {
        minHeap.offer(new int[]{matrix[i][0], i, 0});
    }
    
    // Extract k-1 elements
    int result = 0;
    for (int i = 0; i < k; i++) {
        int[] curr = minHeap.poll();
        result = curr[0];
        int row = curr[1];
        int col = curr[2];
        
        // Add next element from same row
        if (col + 1 < n) {
            minHeap.offer(new int[]{matrix[row][col + 1], row, col + 1});
        }
    }
    
    return result;
}
```

### 7. Task Scheduler

```java
// Schedule tasks with cooldown - O(n) time
public int leastInterval(char[] tasks, int n) {
    // Count frequencies
    int[] freq = new int[26];
    for (char task : tasks) {
        freq[task - 'A']++;
    }
    
    // Max heap for frequencies
    PriorityQueue<Integer> maxHeap = 
        new PriorityQueue<>(Collections.reverseOrder());
    
    for (int f : freq) {
        if (f > 0) {
            maxHeap.offer(f);
        }
    }
    
    int time = 0;
    
    while (!maxHeap.isEmpty()) {
        List<Integer> temp = new ArrayList<>();
        
        // Process n+1 tasks (one cycle)
        for (int i = 0; i <= n; i++) {
            if (!maxHeap.isEmpty()) {
                int f = maxHeap.poll();
                if (f > 1) {
                    temp.add(f - 1);
                }
            }
            
            time++;
            
            // If all tasks done, break
            if (maxHeap.isEmpty() && temp.isEmpty()) {
                break;
            }
        }
        
        // Add back remaining tasks
        for (int f : temp) {
            maxHeap.offer(f);
        }
    }
    
    return time;
}
```

### 8. Reorganize String

```java
// Reorganize string so no adjacent chars are same - O(n log k) time
public String reorganizeString(String s) {
    // Count frequencies
    Map<Character, Integer> freqMap = new HashMap<>();
    for (char c : s.toCharArray()) {
        freqMap.put(c, freqMap.getOrDefault(c, 0) + 1);
    }
    
    // Max heap based on frequency
    PriorityQueue<Character> maxHeap = new PriorityQueue<>(
        (a, b) -> freqMap.get(b) - freqMap.get(a)
    );
    maxHeap.addAll(freqMap.keySet());
    
    StringBuilder result = new StringBuilder();
    
    while (maxHeap.size() >= 2) {
        // Get two most frequent chars
        char first = maxHeap.poll();
        char second = maxHeap.poll();
        
        result.append(first);
        result.append(second);
        
        // Decrease frequencies
        freqMap.put(first, freqMap.get(first) - 1);
        freqMap.put(second, freqMap.get(second) - 1);
        
        // Add back if frequency > 0
        if (freqMap.get(first) > 0) {
            maxHeap.offer(first);
        }
        if (freqMap.get(second) > 0) {
            maxHeap.offer(second);
        }
    }
    
    // Handle remaining character
    if (!maxHeap.isEmpty()) {
        char last = maxHeap.poll();
        if (freqMap.get(last) > 1) {
            return "";  // Not possible
        }
        result.append(last);
    }
    
    return result.toString();
}
```

### 9. Meeting Rooms II

```java
// Find minimum meeting rooms required - O(n log n) time
public int minMeetingRooms(int[][] intervals) {
    if (intervals.length == 0) return 0;
    
    // Sort by start time
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    
    // Min heap for end times
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    minHeap.offer(intervals[0][1]);
    
    for (int i = 1; i < intervals.length; i++) {
        // If room available, reuse it
        if (intervals[i][0] >= minHeap.peek()) {
            minHeap.poll();
        }
        
        // Add current meeting's end time
        minHeap.offer(intervals[i][1]);
    }
    
    return minHeap.size();
}
```

---

## Key Takeaways

1. **Heap**: Efficient for finding min/max and k-th elements
2. **Min Heap**: Use for k largest problems
3. **Max Heap**: Use for k smallest problems
4. **Two Heaps**: Powerful pattern for median and balanced problems
5. **Priority Queue**: Java's built-in heap implementation

**Interview Tip**: Consider heap when you see "k-th", "top k", "median", or "merge k"!

