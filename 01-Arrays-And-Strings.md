# Arrays and Strings

## 📖 Table of Contents
- [Arrays Basics](#arrays-basics)
- [Common Array Operations](#common-array-operations)
- [String Basics](#string-basics)
- [Common String Operations](#common-string-operations)
- [Important Problems](#important-problems)

---

## Arrays Basics

### What is an Array?
An array is a linear data structure that stores elements of the same type in contiguous memory locations.

**Key Properties:**
- Fixed size (in Java)
- Indexed from 0
- O(1) access time
- Elements stored contiguously in memory

### Array Declaration and Initialization

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

---

## Common Array Operations

### 1. Traversal

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

### 2. Insertion

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

### 3. Deletion

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

### 4. Searching

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

### 5. Reversing an Array

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

### 6. Rotation

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

### 7. Finding Maximum and Minimum

```java
// Find max and min - O(n) time
public int[] findMaxMin(int[] arr) {
    if (arr.length == 0) return new int[]{};
    
    int max = arr[0];
    int min = arr[0];
    
    for (int i = 1; i < arr.length; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
        if (arr[i] < min) {
            min = arr[i];
        }
    }
    
    return new int[]{max, min};
}
```

---

## String Basics

### What is a String?
A String in Java is a sequence of characters. It's an object of the String class.

**Key Properties:**
- Immutable (cannot be changed after creation)
- Stored in String Pool for memory efficiency
- Can be created using literals or new keyword

### String Creation

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

### StringBuilder vs StringBuffer

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

---

## Common String Operations

### 1. Character Access

```java
// Access characters - O(1) per character
public void accessCharacters(String s) {
    // Get character at index
    char ch = s.charAt(0);
    
    // Convert to char array
    char[] charArray = s.toCharArray();
    
    // Iterate through characters
    for (int i = 0; i < s.length(); i++) {
        System.out.println(s.charAt(i));
    }
    
    // Enhanced for loop
    for (char c : s.toCharArray()) {
        System.out.println(c);
    }
}
```

### 2. String Comparison

```java
// Compare strings
public void compareStrings() {
    String s1 = "Hello";
    String s2 = "Hello";
    String s3 = new String("Hello");
    
    // == compares references
    System.out.println(s1 == s2);        // true (same reference in pool)
    System.out.println(s1 == s3);        // false (different references)
    
    // equals() compares content
    System.out.println(s1.equals(s3));   // true
    
    // compareTo() - lexicographic comparison
    System.out.println(s1.compareTo(s2)); // 0 (equal)
    System.out.println("abc".compareTo("abd")); // negative (abc < abd)
    
    // equalsIgnoreCase() - ignore case
    System.out.println("Hello".equalsIgnoreCase("hello")); // true
}
```

### 3. Substring Operations

```java
// Extract substrings - O(n) time
public void substringOperations(String s) {
    // substring(beginIndex) - from beginIndex to end
    String sub1 = s.substring(2);
    
    // substring(beginIndex, endIndex) - from beginIndex to endIndex-1
    String sub2 = s.substring(2, 5);
    
    // Check if string contains substring
    boolean contains = s.contains("ell");
    
    // Find index of substring
    int index = s.indexOf("lo");        // First occurrence
    int lastIndex = s.lastIndexOf("l"); // Last occurrence
    
    // Check prefix and suffix
    boolean startsWith = s.startsWith("He");
    boolean endsWith = s.endsWith("lo");
}
```

### 4. String Modification (Creates New String)

```java
// String modifications - O(n) time
public void stringModifications(String s) {
    // Convert case
    String upper = s.toUpperCase();
    String lower = s.toLowerCase();
    
    // Trim whitespace
    String trimmed = "  Hello  ".trim();
    
    // Replace characters/strings
    String replaced = s.replace('l', 'L');
    String replacedStr = s.replace("Hello", "Hi");
    
    // Replace first occurrence with regex
    String replaceFirst = s.replaceFirst("l", "L");
    
    // Replace all with regex
    String replaceAll = s.replaceAll("l+", "L");
    
    // Split string
    String[] parts = "a,b,c,d".split(",");
    
    // Join strings
    String joined = String.join("-", "a", "b", "c"); // "a-b-c"
}
```

### 5. String Reversal

```java
// Reverse string - O(n) time
public String reverseString(String s) {
    // Method 1: Using StringBuilder
    return new StringBuilder(s).reverse().toString();
    
    // Method 2: Using character array
    // char[] chars = s.toCharArray();
    // int left = 0, right = chars.length - 1;
    // while (left < right) {
    //     char temp = chars[left];
    //     chars[left] = chars[right];
    //     chars[right] = temp;
    //     left++;
    //     right--;
    // }
    // return new String(chars);
}
```

### 6. Palindrome Check

```java
// Check if string is palindrome - O(n) time
public boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}

// Check palindrome ignoring non-alphanumeric - O(n) time
public boolean isPalindromeAlphanumeric(String s) {
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

### 7. Anagram Check

```java
// Check if two strings are anagrams - O(n) time
public boolean isAnagram(String s1, String s2) {
    if (s1.length() != s2.length()) {
        return false;
    }
    
    // Method 1: Using sorting - O(n log n)
    char[] chars1 = s1.toCharArray();
    char[] chars2 = s2.toCharArray();
    Arrays.sort(chars1);
    Arrays.sort(chars2);
    return Arrays.equals(chars1, chars2);
    
    // Method 2: Using frequency count - O(n)
    // int[] count = new int[26];
    // for (char c : s1.toCharArray()) {
    //     count[c - 'a']++;
    // }
    // for (char c : s2.toCharArray()) {
    //     count[c - 'a']--;
    // }
    // for (int i : count) {
    //     if (i != 0) return false;
    // }
    // return true;
}
```

---

## Important Problems

### 1. Two Sum Problem

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

### 2. Best Time to Buy and Sell Stock

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

### 3. Container With Most Water

```java
// Find two lines that form container with most water - O(n) time
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
```

### 4. Longest Substring Without Repeating Characters

```java
// Find length of longest substring without repeating chars - O(n) time
public int lengthOfLongestSubstring(String s) {
    // Use set to track characters in current window
    Set<Character> set = new HashSet<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        
        // If character already exists, remove characters from left
        while (set.contains(c)) {
            set.remove(s.charAt(left));
            left++;
        }
        
        // Add current character
        set.add(c);
        
        // Update maximum length
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

### 5. Trapping Rain Water

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

### 6. Product of Array Except Self

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

### 7. Sliding Window Maximum

```java
// Find maximum in each sliding window - O(n) time using deque
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

---

## Key Takeaways

1. **Arrays**: Fixed size, O(1) access, but O(n) insertion/deletion
2. **Strings**: Immutable in Java, use StringBuilder for modifications
3. **Two Pointers**: Common technique for array/string problems
4. **Sliding Window**: Efficient for substring/subarray problems
5. **HashMap**: Quick lookups for counting and complement finding

**Time to Practice**: Master these fundamentals before moving to advanced topics!

