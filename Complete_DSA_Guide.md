# Complete Data Structures and Algorithms Guide (Java)

## 📚 Table of Contents

### Part I: Data Structures
1. [Arrays and Strings](#1-arrays-and-strings)
2. [Linked Lists](#2-linked-lists)
3. [Stacks and Queues](#3-stacks-and-queues)
4. [Trees](#4-trees)
5. [Heaps](#5-heaps)
6. [Hashing](#6-hashing)
7. [Graphs](#7-graphs)

### Part II: Algorithms
8. [Sorting Algorithms](#8-sorting-algorithms)
9. [Searching Algorithms](#9-searching-algorithms)
10. [Dynamic Programming](#10-dynamic-programming)
11. [Greedy Algorithms](#11-greedy-algorithms)
12. [Backtracking](#12-backtracking)
13. [Divide and Conquer](#13-divide-and-conquer)
14. [Bit Manipulation](#14-bit-manipulation)

### Part III: Advanced Topics
15. [String Algorithms](#15-string-algorithms)
16. [Math and Number Theory](#16-math-and-number-theory)
17. [Common Patterns](#17-common-patterns)
18. [Complexity Analysis](#18-complexity-analysis)

---

## 1. Arrays and Strings

### 1.1 Arrays Basics

**What is an Array?**
An array is a linear data structure that stores elements of the same type in contiguous memory locations.

**Key Properties:**
- Fixed size (in Java)
- Indexed from 0
- O(1) access time
- Elements stored contiguously in memory

**Array Declaration and Initialization:**

```java
public class ArrayBasics {
    public static void main(String[] args) {
        // Method 1: Declare and allocate memory
        int[] arr1 = new int[5];  // Array of size 5, initialized to 0
        
        // Method 2: Declare and initialize
        int[] arr2 = {1, 2, 3, 4, 5};
        
        // Method 3: Using new keyword with values
        int[] arr3 = new int[]{10, 20, 30, 40, 50};
        
        // 2D Array
        int[][] matrix = new int[3][4];  // 3 rows, 4 columns
        
        // 2D Array with initialization
        int[][] matrix2 = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        
        // Print array length
        System.out.println("Array length: " + arr2.length);
        System.out.println("Matrix rows: " + matrix2.length);
        System.out.println("Matrix cols: " + matrix2[0].length);
    }
}
```

### 1.2 Common Array Operations

**1. Traversal:**

```java
// Linear traversal - O(n) time
public void traverseArray(int[] arr) {
    // Method 1: For loop
    for (int i = 0; i < arr.length; i++) {
        System.out.print(arr[i] + " ");
    }
    
    // Method 2: Enhanced for loop (for-each)
    for (int num : arr) {
        System.out.print(num + " ");
    }
    
    // Method 3: Using streams (Java 8+)
    Arrays.stream(arr).forEach(System.out::println);
}
```

**2. Insertion:**

```java
// Insert element at specific position - O(n) time
public int[] insertElement(int[] arr, int element, int position) {
    // Create new array with size + 1
    int[] newArr = new int[arr.length + 1];
    
    // Copy elements before position
    for (int i = 0; i < position; i++) {
        newArr[i] = arr[i];
    }
    
    // Insert new element
    newArr[position] = element;
    
    // Copy remaining elements
    for (int i = position; i < arr.length; i++) {
        newArr[i + 1] = arr[i];
    }
    
    return newArr;
}
```

**3. Deletion:**

```java
// Delete element at specific position - O(n) time
public int[] deleteElement(int[] arr, int position) {
    // Create new array with size - 1
    int[] newArr = new int[arr.length - 1];
    
    // Copy elements before position
    for (int i = 0; i < position; i++) {
        newArr[i] = arr[i];
    }
    
    // Copy elements after position
    for (int i = position + 1; i < arr.length; i++) {
        newArr[i - 1] = arr[i];
    }
    
    return newArr;
}
```

**4. Searching:**

```java
// Linear Search - O(n) time
public int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;  // Return index if found
        }
    }
    return -1;  // Return -1 if not found
}

// Binary Search - O(log n) time (requires sorted array)
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
```

**5. Reversing an Array:**

```java
// Reverse array in-place - O(n) time, O(1) space
public void reverseArray(int[] arr) {
    int left = 0;
    int right = arr.length - 1;
    
    while (left < right) {
        // Swap elements
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        
        left++;
        right--;
    }
}
```

**6. Rotation:**

```java
// Rotate array to right by k positions - O(n) time, O(1) space
public void rotateArray(int[] arr, int k) {
    int n = arr.length;
    k = k % n;  // Handle k > n
    
    // Reverse entire array
    reverse(arr, 0, n - 1);
    
    // Reverse first k elements
    reverse(arr, 0, k - 1);
    
    // Reverse remaining elements
    reverse(arr, k, n - 1);
}

// Helper method to reverse array from start to end index
private void reverse(int[] arr, int start, int end) {
    while (start < end) {
        int temp = arr[start];
        arr[start] = arr[end];
        arr[end] = temp;
        start++;
        end--;
    }
}
```

### 1.3 Two Pointers Technique (Detailed)

**What is Two Pointers?**
Two pointers technique uses two pointers to traverse an array or string from different positions, typically from both ends or at different speeds.

**When to Use Two Pointers:**
- Sorted array problems
- Finding pairs/triplets that sum to target
- Removing duplicates
- Palindrome problems
- Container problems

**Pattern 1: Opposite Direction (Converging)**

```java
// Two Sum in Sorted Array - O(n) time, O(1) space
public int[] twoSum(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left < right) {
        int sum = nums[left] + nums[right];
        
        if (sum == target) {
            return new int[]{left, right};  // Found pair
        } else if (sum < target) {
            left++;  // Need larger sum
        } else {
            right--;  // Need smaller sum
        }
    }
    
    return new int[]{};  // No solution found
}

// Container With Most Water - O(n) time, O(1) space
public int maxArea(int[] height) {
    int left = 0;
    int right = height.length - 1;
    int maxArea = 0;
    
    while (left < right) {
        // Calculate area with current left and right
        int width = right - left;
        int h = Math.min(height[left], height[right]);
        int area = width * h;
        
        // Update maximum area
        maxArea = Math.max(maxArea, area);
        
        // Move pointer with smaller height
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxArea;
}

// Valid Palindrome - O(n) time, O(1) space
public boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        // Skip non-alphanumeric characters
        while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
            left++;
        }
        while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
            right--;
        }
        
        // Compare characters (case-insensitive)
        if (Character.toLowerCase(s.charAt(left)) != 
            Character.toLowerCase(s.charAt(right))) {
            return false;
        }
        
        left++;
        right--;
    }
    
    return true;
}
```

**Pattern 2: Same Direction (Fast and Slow)**

```java
// Remove Duplicates from Sorted Array - O(n) time, O(1) space
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    
    int slow = 0;  // Points to position for next unique element
    
    for (int fast = 1; fast < nums.length; fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];  // Move unique element to slow position
        }
    }
    
    return slow + 1;  // Length of array with unique elements
}

// Move Zeros to End - O(n) time, O(1) space
public void moveZeroes(int[] nums) {
    int slow = 0;  // Points to position for next non-zero element
    
    // Move all non-zero elements to front
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != 0) {
            nums[slow] = nums[fast];
            slow++;
        }
    }
    
    // Fill remaining positions with zeros
    while (slow < nums.length) {
        nums[slow] = 0;
        slow++;
    }
}

// Remove Element - O(n) time, O(1) space
public int removeElement(int[] nums, int val) {
    int slow = 0;  // Points to position for next element != val
    
    for (int fast = 0; fast < nums.length; fast++) {
        if (nums[fast] != val) {
            nums[slow] = nums[fast];
            slow++;
        }
    }
    
    return slow;  // New length
}
```

**Pattern 3: Three Pointers**

```java
// Three Sum - O(n²) time, O(1) space
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(nums);  // Sort array first
    
    for (int i = 0; i < nums.length - 2; i++) {
        // Skip duplicates for first element
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        
        int left = i + 1;
        int right = nums.length - 1;
        
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            
            if (sum == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                
                // Skip duplicates for second and third elements
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;
                
                left++;
                right--;
            } else if (sum < 0) {
                left++;  // Need larger sum
            } else {
                right--;  // Need smaller sum
            }
        }
    }
    
    return result;
}

// Sort Colors (Dutch National Flag) - O(n) time, O(1) space
public void sortColors(int[] nums) {
    int left = 0;      // Boundary for 0s
    int right = nums.length - 1;  // Boundary for 2s
    int current = 0;   // Current element being processed
    
    while (current <= right) {
        if (nums[current] == 0) {
            // Swap with left boundary
            swap(nums, left, current);
            left++;
            current++;
        } else if (nums[current] == 2) {
            // Swap with right boundary
            swap(nums, current, right);
            right--;
            // Don't increment current, need to check swapped element
        } else {
            // Element is 1, just move forward
            current++;
        }
    }
}

private void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

### 1.4 Sliding Window Technique (Detailed)

**What is Sliding Window?**
Sliding window technique maintains a window of elements and slides it through the array/string to solve problems efficiently.

**When to Use Sliding Window:**
- Subarray/substring problems
- Fixed or variable window size
- Maximum/minimum in window
- Problems involving consecutive elements

**Pattern 1: Fixed Size Window**

```java
// Maximum Sum of K Consecutive Elements - O(n) time, O(1) space
public int maxSumK(int[] arr, int k) {
    if (arr.length < k) return -1;
    
    // Calculate sum of first window
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    
    // Slide window
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];  // Add new element, remove old
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}

// Average of K Consecutive Elements - O(n) time, O(1) space
public double[] findAverages(int[] arr, int k) {
    double[] result = new double[arr.length - k + 1];
    double windowSum = 0;
    
    // Calculate sum of first window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    result[0] = windowSum / k;
    
    // Slide window
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        result[i - k + 1] = windowSum / k;
    }
    
    return result;
}
```

**Pattern 2: Variable Size Window**

```java
// Longest Substring Without Repeating Characters - O(n) time, O(min(m,n)) space
public int lengthOfLongestSubstring(String s) {
    Set<Character> window = new HashSet<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        
        // If character already in window, shrink from left
        while (window.contains(c)) {
            window.remove(s.charAt(left));
            left++;
        }
        
        // Add current character to window
        window.add(c);
        
        // Update maximum length
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

// Minimum Window Substring - O(n) time, O(m) space where m = charset size
public String minWindow(String s, String t) {
    Map<Character, Integer> need = new HashMap<>();
    Map<Character, Integer> window = new HashMap<>();
    
    // Count characters needed
    for (char c : t.toCharArray()) {
        need.put(c, need.getOrDefault(c, 0) + 1);
    }
    
    int left = 0, right = 0;
    int valid = 0;  // Number of characters with correct count
    int start = 0, len = Integer.MAX_VALUE;
    
    while (right < s.length()) {
        char c = s.charAt(right);
        right++;
        
        // Update window
        if (need.containsKey(c)) {
            window.put(c, window.getOrDefault(c, 0) + 1);
            if (window.get(c).equals(need.get(c))) {
                valid++;
            }
        }
        
        // Shrink window when valid
        while (valid == need.size()) {
            // Update result
            if (right - left < len) {
                start = left;
                len = right - left;
            }
            
            char d = s.charAt(left);
            left++;
            
            // Update window
            if (need.containsKey(d)) {
                if (window.get(d).equals(need.get(d))) {
                    valid--;
                }
                window.put(d, window.get(d) - 1);
            }
        }
    }
    
    return len == Integer.MAX_VALUE ? "" : s.substring(start, start + len);
}

// Longest Substring with At Most K Distinct Characters - O(n) time, O(k) space
public int lengthOfLongestSubstringKDistinct(String s, int k) {
    Map<Character, Integer> charCount = new HashMap<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        
        // Shrink window if more than k distinct characters
        while (charCount.size() > k) {
            char leftChar = s.charAt(left);
            charCount.put(leftChar, charCount.get(leftChar) - 1);
            if (charCount.get(leftChar) == 0) {
                charCount.remove(leftChar);
            }
            left++;
        }
        
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

**Pattern 3: Sliding Window with Deque**

```java
// Sliding Window Maximum - O(n) time, O(k) space
public int[] maxSlidingWindow(int[] nums, int k) {
    if (nums.length == 0 || k == 0) return new int[]{};
    
    int n = nums.length;
    int[] result = new int[n - k + 1];
    Deque<Integer> deque = new ArrayDeque<>();  // Store indices
    
    for (int i = 0; i < n; i++) {
        // Remove elements outside window
        while (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
            deque.pollFirst();
        }
        
        // Remove smaller elements from rear
        while (!deque.isEmpty() && nums[deque.peekLast()] < nums[i]) {
            deque.pollLast();
        }
        
        // Add current element
        deque.offerLast(i);
        
        // Add to result if window is complete
        if (i >= k - 1) {
            result[i - k + 1] = nums[deque.peekFirst()];
        }
    }
    
    return result;
}
```

### 1.5 String Basics

**What is a String?**
A String in Java is a sequence of characters. It's an object of the String class.

**Key Properties:**
- Immutable (cannot be changed after creation)
- Stored in String Pool for memory efficiency
- Can be created using literals or new keyword

**String Creation:**

```java
public class StringBasics {
    public static void main(String[] args) {
        // Method 1: String literal (preferred)
        String s1 = "Hello";
        
        // Method 2: Using new keyword
        String s2 = new String("Hello");
        
        // From char array
        char[] chars = {'H', 'e', 'l', 'l', 'o'};
        String s3 = new String(chars);
        
        // String concatenation
        String s4 = "Hello" + " " + "World";
        
        // String length
        System.out.println("Length: " + s1.length());
    }
}
```

**StringBuilder vs StringBuffer:**

```java
// StringBuilder - NOT thread-safe, FASTER (use in single-threaded)
public String useStringBuilder() {
    StringBuilder sb = new StringBuilder();
    
    // Append operations
    sb.append("Hello");
    sb.append(" ");
    sb.append("World");
    
    // Insert at position
    sb.insert(5, ",");
    
    // Delete range
    sb.delete(5, 6);
    
    // Reverse
    sb.reverse();
    
    // Convert to String
    return sb.toString();
}

// StringBuffer - Thread-safe, SLOWER (use in multi-threaded)
public String useStringBuffer() {
    StringBuffer sbf = new StringBuffer();
    sbf.append("Thread");
    sbf.append(" Safe");
    return sbf.toString();
}
```

### 1.6 Important Array/String Problems

**1. Two Sum Problem:**

```java
// Find two numbers that add up to target - O(n) time, O(n) space
public int[] twoSum(int[] nums, int target) {
    // Use HashMap to store number and its index
    Map<Integer, Integer> map = new HashMap<>();
    
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        
        // Check if complement exists in map
        if (map.containsKey(complement)) {
            return new int[]{map.get(complement), i};
        }
        
        // Store current number and index
        map.put(nums[i], i);
    }
    
    return new int[]{};  // No solution found
}
```

**2. Best Time to Buy and Sell Stock:**

```java
// Find maximum profit from stock prices - O(n) time, O(1) space
public int maxProfit(int[] prices) {
    if (prices.length == 0) return 0;
    
    int minPrice = prices[0];  // Track minimum price seen so far
    int maxProfit = 0;         // Track maximum profit
    
    for (int i = 1; i < prices.length; i++) {
        // Calculate profit if we sell today
        int profit = prices[i] - minPrice;
        
        // Update maximum profit
        maxProfit = Math.max(maxProfit, profit);
        
        // Update minimum price
        minPrice = Math.min(minPrice, prices[i]);
    }
    
    return maxProfit;
}
```

**3. Trapping Rain Water:**

```java
// Calculate trapped rain water - O(n) time, O(1) space
public int trap(int[] height) {
    if (height.length == 0) return 0;
    
    int left = 0;
    int right = height.length - 1;
    int leftMax = 0;
    int rightMax = 0;
    int water = 0;
    
    while (left < right) {
        if (height[left] < height[right]) {
            // Process left side
            if (height[left] >= leftMax) {
                leftMax = height[left];
            } else {
                water += leftMax - height[left];
            }
            left++;
        } else {
            // Process right side
            if (height[right] >= rightMax) {
                rightMax = height[right];
            } else {
                water += rightMax - height[right];
            }
            right--;
        }
    }
    
    return water;
}
```

**4. Product of Array Except Self:**

```java
// Calculate product of all elements except self - O(n) time, O(1) space
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    
    // Calculate left products
    result[0] = 1;
    for (int i = 1; i < n; i++) {
        result[i] = result[i - 1] * nums[i - 1];
    }
    
    // Calculate right products and multiply with left
    int rightProduct = 1;
    for (int i = n - 1; i >= 0; i--) {
        result[i] = result[i] * rightProduct;
        rightProduct *= nums[i];
    }
    
    return result;
}
```

---

## 2. Linked Lists

### 2.1 Introduction

**What is a Linked List?**
A Linked List is a linear data structure where elements (nodes) are not stored in contiguous memory locations. Each node contains data and a reference (pointer) to the next node.

**Advantages over Arrays:**
- Dynamic size (can grow/shrink)
- Easy insertion/deletion at any position (O(1) if position known)
- No memory wastage

**Disadvantages:**
- No random access (O(n) to access element)
- Extra memory for storing references
- Not cache-friendly

### 2.2 Types of Linked Lists

1. **Singly Linked List**: Each node points to the next node
2. **Doubly Linked List**: Each node points to both next and previous nodes
3. **Circular Linked List**: Last node points back to the first node

### 2.3 Singly Linked List Implementation

**Node Structure:**

```java
// Node class for Singly Linked List
class Node {
    int data;        // Data stored in node
    Node next;       // Reference to next node
    
    // Constructor
    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}
```

**Complete Implementation:**

```java
public class SinglyLinkedList {
    private Node head;  // Head of the list
    
    // Constructor
    public SinglyLinkedList() {
        this.head = null;
    }
    
    // 1. Insert at beginning - O(1) time
    public void insertAtBeginning(int data) {
        Node newNode = new Node(data);
        newNode.next = head;  // Point new node to current head
        head = newNode;       // Update head
    }
    
    // 2. Insert at end - O(n) time
    public void insertAtEnd(int data) {
        Node newNode = new Node(data);
        
        // If list is empty
        if (head == null) {
            head = newNode;
            return;
        }
        
        // Traverse to last node
        Node current = head;
        while (current.next != null) {
            current = current.next;
        }
        
        // Link last node to new node
        current.next = newNode;
    }
    
    // 3. Insert at specific position - O(n) time
    public void insertAtPosition(int data, int position) {
        // If position is 0, insert at beginning
        if (position == 0) {
            insertAtBeginning(data);
            return;
        }
        
        Node newNode = new Node(data);
        Node current = head;
        
        // Traverse to position-1
        for (int i = 0; i < position - 1 && current != null; i++) {
            current = current.next;
        }
        
        // If position is invalid
        if (current == null) {
            System.out.println("Invalid position");
            return;
        }
        
        // Insert node
        newNode.next = current.next;
        current.next = newNode;
    }
    
    // 4. Delete from beginning - O(1) time
    public void deleteFromBeginning() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        head = head.next;  // Move head to next node
    }
    
    // 5. Delete from end - O(n) time
    public void deleteFromEnd() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        
        // If only one node
        if (head.next == null) {
            head = null;
            return;
        }
        
        // Traverse to second last node
        Node current = head;
        while (current.next.next != null) {
            current = current.next;
        }
        
        // Remove last node
        current.next = null;
    }
    
    // 6. Delete specific node - O(n) time
    public void deleteNode(int data) {
        if (head == null) return;
        
        // If head needs to be deleted
        if (head.data == data) {
            head = head.next;
            return;
        }
        
        // Find node to delete
        Node current = head;
        while (current.next != null && current.next.data != data) {
            current = current.next;
        }
        
        // If node not found
        if (current.next == null) {
            System.out.println("Node not found");
            return;
        }
        
        // Delete node
        current.next = current.next.next;
    }
    
    // 7. Search for element - O(n) time
    public boolean search(int data) {
        Node current = head;
        while (current != null) {
            if (current.data == data) {
                return true;
            }
            current = current.next;
        }
        return false;
    }
    
    // 8. Get length of list - O(n) time
    public int getLength() {
        int count = 0;
        Node current = head;
        while (current != null) {
            count++;
            current = current.next;
        }
        return count;
    }
    
    // 9. Display list - O(n) time
    public void display() {
        if (head == null) {
            System.out.println("List is empty");
            return;
        }
        
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }
    
    // 10. Reverse the list - O(n) time, O(1) space
    public void reverse() {
        Node prev = null;
        Node current = head;
        Node next = null;
        
        while (current != null) {
            next = current.next;    // Store next node
            current.next = prev;    // Reverse link
            prev = current;         // Move prev forward
            current = next;         // Move current forward
        }
        
        head = prev;  // Update head
    }
}
```

### 2.4 Important Linked List Problems

**1. Find Middle Element:**

```java
// Find middle element using slow and fast pointers - O(n) time
public Node findMiddle(Node head) {
    if (head == null) return null;
    
    Node slow = head;
    Node fast = head;
    
    // Fast moves 2 steps, slow moves 1 step
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;  // Slow will be at middle
}
```

**2. Detect Cycle (Floyd's Algorithm):**

```java
// Detect if linked list has a cycle - O(n) time, O(1) space
public boolean hasCycle(Node head) {
    if (head == null) return false;
    
    Node slow = head;
    Node fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            return true;  // Cycle detected
        }
    }
    
    return false;  // No cycle
}

// Find the start of the cycle
public Node detectCycleStart(Node head) {
    if (head == null) return null;
    
    Node slow = head;
    Node fast = head;
    
    // Detect cycle
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            // Cycle found, find start
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;  // Start of cycle
        }
    }
    
    return null;  // No cycle
}
```

**3. Merge Two Sorted Lists:**

```java
// Merge two sorted linked lists - O(m+n) time
public Node mergeTwoLists(Node l1, Node l2) {
    // Dummy node to simplify logic
    Node dummy = new Node(0);
    Node current = dummy;
    
    // Compare and merge
    while (l1 != null && l2 != null) {
        if (l1.data <= l2.data) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    // Attach remaining nodes
    if (l1 != null) {
        current.next = l1;
    }
    if (l2 != null) {
        current.next = l2;
    }
    
    return dummy.next;
}
```

**4. Remove Nth Node From End:**

```java
// Remove nth node from end - O(n) time, O(1) space
public Node removeNthFromEnd(Node head, int n) {
    Node dummy = new Node(0);
    dummy.next = head;
    
    Node first = dummy;
    Node second = dummy;
    
    // Move first n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        first = first.next;
    }
    
    // Move both until first reaches end
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    
    // Remove nth node
    second.next = second.next.next;
    
    return dummy.next;
}
```

---

*[This is the beginning of the comprehensive guide. Due to length constraints, I'll continue with the remaining sections in subsequent parts. The complete guide will include all data structures and algorithms with detailed explanations, code examples, and important problems.]*
