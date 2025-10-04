# Time and Space Complexity Guide

## 📖 Table of Contents
- [Big O Notation](#big-o-notation)
- [Common Complexities](#common-complexities)
- [Time Complexity Analysis](#time-complexity-analysis)
- [Space Complexity Analysis](#space-complexity-analysis)
- [Master Theorem](#master-theorem)
- [Amortized Analysis](#amortized-analysis)

---

## Big O Notation

### What is Big O?
Describes upper bound of algorithm's growth rate as input size increases.

**Properties:**
- **O(1)**: Constant - Best
- **O(log n)**: Logarithmic - Excellent
- **O(n)**: Linear - Good
- **O(n log n)**: Linearithmic - Fair
- **O(n²)**: Quadratic - Bad
- **O(2ⁿ)**: Exponential - Very Bad
- **O(n!)**: Factorial - Worst

### Big O Rules

1. **Drop Constants**: O(2n) → O(n)
2. **Drop Non-Dominant Terms**: O(n² + n) → O(n²)
3. **Different Inputs Use Different Variables**: O(a + b), O(a * b)
4. **Amortized Time**: Average time over sequence of operations

---

## Common Complexities

### Complexity Chart

```
O(1) < O(log n) < O(√n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)
```

### Growth Rates

| n | O(1) | O(log n) | O(n) | O(n log n) | O(n²) | O(2ⁿ) |
|---|------|----------|------|------------|-------|--------|
| 10 | 1 | 3 | 10 | 30 | 100 | 1,024 |
| 100 | 1 | 7 | 100 | 700 | 10,000 | 1.27×10³⁰ |
| 1,000 | 1 | 10 | 1,000 | 10,000 | 1,000,000 | - |
| 10,000 | 1 | 13 | 10,000 | 130,000 | 100,000,000 | - |

---

## Time Complexity Analysis

### O(1) - Constant Time

```java
// Array access
public int getElement(int[] arr, int index) {
    return arr[index];  // O(1)
}

// Hash map get/put
public void hashMapOps(Map<String, Integer> map) {
    map.put("key", 1);    // O(1)
    int val = map.get("key");  // O(1)
}

// Math operations
public int add(int a, int b) {
    return a + b;  // O(1)
}
```

### O(log n) - Logarithmic Time

```java
// Binary search
public int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    
    while (left <= right) {  // O(log n) iterations
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

// Binary tree operations (balanced)
public boolean searchBST(TreeNode root, int val) {
    while (root != null) {  // O(log n) height
        if (root.val == val) return true;
        else if (val < root.val) root = root.left;
        else root = root.right;
    }
    return false;
}
```

### O(n) - Linear Time

```java
// Array traversal
public int sum(int[] arr) {
    int total = 0;
    for (int num : arr) {  // O(n)
        total += num;
    }
    return total;
}

// String operations
public boolean contains(String s, char c) {
    for (char ch : s.toCharArray()) {  // O(n)
        if (ch == c) return true;
    }
    return false;
}
```

### O(n log n) - Linearithmic Time

```java
// Merge sort
public void mergeSort(int[] arr) {
    // Dividing: log n levels
    // Merging at each level: n work
    // Total: O(n log n)
}

// Quick sort (average)
public void quickSort(int[] arr) {
    // Average case: O(n log n)
}

// Heap operations
public void heapSort(int[] arr) {
    // Build heap: O(n)
    // n deletions: O(n log n)
}
```

### O(n²) - Quadratic Time

```java
// Nested loops
public int[][] multiplyMatrix(int[][] a, int[][] b) {
    int n = a.length;
    int[][] result = new int[n][n];
    
    for (int i = 0; i < n; i++) {           // O(n)
        for (int j = 0; j < n; j++) {       // O(n)
            for (int k = 0; k < n; k++) {   // O(n)
                result[i][j] += a[i][k] * b[k][j];
            }
        }
    }
    // Total: O(n³)
    return result;
}

// Bubble sort
public void bubbleSort(int[] arr) {
    for (int i = 0; i < arr.length; i++) {        // O(n)
        for (int j = 0; j < arr.length - i - 1; j++) {  // O(n)
            if (arr[j] > arr[j + 1]) {
                swap(arr, j, j + 1);
            }
        }
    }
    // Total: O(n²)
}
```

### O(2ⁿ) - Exponential Time

```java
// Naive Fibonacci
public int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);  // O(2ⁿ)
}

// Generate all subsets
public List<List<Integer>> subsets(int[] nums) {
    // 2ⁿ subsets
    // O(n * 2ⁿ) with copying
}
```

### O(n!) - Factorial Time

```java
// Generate all permutations
public List<List<Integer>> permute(int[] nums) {
    // n! permutations
    // O(n * n!) with copying
}
```

---

## Space Complexity Analysis

### O(1) - Constant Space

```java
// Variables only
public int sum(int a, int b) {
    int result = a + b;  // O(1) space
    return result;
}

// In-place operations
public void reverse(int[] arr) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        swap(arr, left++, right--);
    }
    // O(1) space - no extra array
}
```

### O(log n) - Logarithmic Space

```java
// Recursion with binary division
public int binarySearchRecursive(int[] arr, int target, int left, int right) {
    if (left > right) return -1;
    
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) 
        return binarySearchRecursive(arr, target, mid + 1, right);
    else 
        return binarySearchRecursive(arr, target, left, mid - 1);
    
    // O(log n) recursive calls on stack
}
```

### O(n) - Linear Space

```java
// Array/List storage
public int[] copyArray(int[] arr) {
    int[] copy = new int[arr.length];  // O(n) space
    System.arraycopy(arr, 0, copy, 0, arr.length);
    return copy;
}

// Hash map
public Map<Integer, Integer> frequency(int[] arr) {
    Map<Integer, Integer> map = new HashMap<>();  // O(n) space
    for (int num : arr) {
        map.put(num, map.getOrDefault(num, 0) + 1);
    }
    return map;
}

// Recursion depth
public void dfs(TreeNode root) {
    if (root == null) return;
    dfs(root.left);
    dfs(root.right);
    // O(n) space in worst case (skewed tree)
    // O(log n) space for balanced tree
}
```

### O(n²) - Quadratic Space

```java
// 2D array
public int[][] createMatrix(int n) {
    int[][] matrix = new int[n][n];  // O(n²) space
    return matrix;
}

// DP table
public int lcs(String s1, String s2) {
    int[][] dp = new int[s1.length() + 1][s2.length() + 1];  // O(m*n)
    // ...
    return dp[s1.length()][s2.length()];
}
```

---

## Master Theorem

### Recurrence Relations

For recurrences of form: **T(n) = aT(n/b) + f(n)**

Where:
- a = number of subproblems
- n/b = size of each subproblem
- f(n) = cost of divide and combine

### Cases

1. **Case 1**: If f(n) = O(n^c) where c < log_b(a)  
   Then T(n) = Θ(n^(log_b(a)))

2. **Case 2**: If f(n) = Θ(n^c * log^k(n)) where c = log_b(a)  
   Then T(n) = Θ(n^c * log^(k+1)(n))

3. **Case 3**: If f(n) = Ω(n^c) where c > log_b(a)  
   Then T(n) = Θ(f(n))

### Examples

```java
// Merge Sort: T(n) = 2T(n/2) + O(n)
// a=2, b=2, f(n)=n
// log_2(2) = 1, c=1
// Case 2: T(n) = O(n log n)

// Binary Search: T(n) = T(n/2) + O(1)
// a=1, b=2, f(n)=1
// log_2(1) = 0, c=0
// Case 2: T(n) = O(log n)

// Strassen Matrix: T(n) = 7T(n/2) + O(n²)
// a=7, b=2, f(n)=n²
// log_2(7) ≈ 2.81, c=2
// Case 1: T(n) = O(n^2.81)
```

---

## Amortized Analysis

### Concept
Average time taken per operation over sequence of operations.

### Dynamic Array Example

```java
// ArrayList add operation
public class DynamicArray {
    private int[] arr;
    private int size;
    private int capacity;
    
    public void add(int val) {
        if (size == capacity) {
            // Resize: O(n) time
            capacity *= 2;
            int[] newArr = new int[capacity];
            System.arraycopy(arr, 0, newArr, 0, size);
            arr = newArr;
        }
        
        arr[size++] = val;  // O(1) time
    }
    
    // Amortized O(1) per add operation
    // Even though occasional resize is O(n)
}
```

### Analysis Methods

1. **Aggregate Method**: Total cost / number of operations
2. **Accounting Method**: Assign cost to each operation
3. **Potential Method**: Define potential function

---

## Data Structure Complexities

| Data Structure | Access | Search | Insertion | Deletion | Space |
|----------------|--------|--------|-----------|----------|-------|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Dynamic Array | O(1) | O(n) | O(1)* | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1) | O(1) | O(n) |
| Stack | O(n) | O(n) | O(1) | O(1) | O(n) |
| Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| Hash Table | - | O(1)* | O(1)* | O(1)* | O(n) |
| Binary Search Tree | O(log n)* | O(log n)* | O(log n)* | O(log n)* | O(n) |
| AVL Tree | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Heap | - | O(n) | O(log n) | O(log n) | O(n) |
| Trie | - | O(k) | O(k) | O(k) | O(n*k) |

*Amortized or average case

---

## Algorithm Complexities

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n+k) |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| DFS | - | O(V+E) | O(V+E) | O(V) |
| BFS | - | O(V+E) | O(V+E) | O(V) |
| Dijkstra | - | O((V+E) log V) | O((V+E) log V) | O(V) |

---

## Key Takeaways

1. **Identify Loops**: Each nested loop multiplies complexity
2. **Recursion**: Analyze tree of recursive calls
3. **Master Theorem**: For divide-and-conquer recurrences
4. **Space**: Consider both auxiliary and input space
5. **Best vs Average vs Worst**: Know the difference

**Interview Tip**: Always analyze both time and space complexity!

