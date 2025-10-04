# Common Patterns and Techniques

## 📖 Table of Contents
- [Two Pointers](#two-pointers)
- [Sliding Window](#sliding-window)
- [Fast and Slow Pointers](#fast-and-slow-pointers)
- [Merge Intervals](#merge-intervals)
- [Cyclic Sort](#cyclic-sort)
- [Top K Elements](#top-k-elements)
- [Binary Search Patterns](#binary-search-patterns)
- [Tree Patterns](#tree-patterns)
- [Graph Patterns](#graph-patterns)
- [DP Patterns](#dp-patterns)

---

## Two Pointers

### Pattern
Use two pointers to iterate through data structure simultaneously.

**When to Use:**
- Sorted array problems
- Finding pairs/triplets
- Removing duplicates
- Palindrome problems

```java
// Example: Two Sum in Sorted Array
public int[] twoSum(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            return new int[]{left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    
    return new int[]{};
}

// Example: Three Sum
public List<List<Integer>> threeSum(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(nums);
    
    for (int i = 0; i < nums.length - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        
        int left = i + 1;
        int right = nums.length - 1;
        
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum == 0) {
                result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                while (left < right && nums[left] == nums[left + 1]) left++;
                while (left < right && nums[right] == nums[right - 1]) right--;
                left++;
                right--;
            } else if (sum < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
    
    return result;
}
```

---

## Sliding Window

### Pattern
Maintain a window and slide it through array/string.

**When to Use:**
- Subarray/substring problems
- Fixed or variable window size
- Maximum/minimum in window

```java
// Fixed Size Window: Maximum sum of k elements
public int maxSum(int[] arr, int k) {
    int windowSum = 0;
    
    // First window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    
    // Slide window
    for (int i = k; i < arr.length; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = Math.max(maxSum, windowSum);
    }
    
    return maxSum;
}

// Variable Size Window: Longest substring with k distinct chars
public int longestSubstringKDistinct(String s, int k) {
    Map<Character, Integer> charCount = new HashMap<>();
    int left = 0;
    int maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        charCount.put(c, charCount.getOrDefault(c, 0) + 1);
        
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

---

## Fast and Slow Pointers

### Pattern
Two pointers moving at different speeds.

**When to Use:**
- Cycle detection
- Middle of linked list
- Palindrome linked list

```java
// Detect cycle in linked list
public boolean hasCycle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}

// Find middle of linked list
public ListNode findMiddle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    return slow;
}
```

---

## Merge Intervals

### Pattern
Sort intervals and merge overlapping ones.

**When to Use:**
- Overlapping intervals
- Meeting rooms
- Calendar problems

```java
// Merge overlapping intervals
public int[][] merge(int[][] intervals) {
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    List<int[]> result = new ArrayList<>();
    int[] current = intervals[0];
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= current[1]) {
            current[1] = Math.max(current[1], intervals[i][1]);
        } else {
            result.add(current);
            current = intervals[i];
        }
    }
    
    result.add(current);
    return result.toArray(new int[result.size()][]);
}

// Insert interval
public int[][] insert(int[][] intervals, int[] newInterval) {
    List<int[]> result = new ArrayList<>();
    int i = 0;
    
    // Add intervals before newInterval
    while (i < intervals.length && intervals[i][1] < newInterval[0]) {
        result.add(intervals[i++]);
    }
    
    // Merge overlapping intervals
    while (i < intervals.length && intervals[i][0] <= newInterval[1]) {
        newInterval[0] = Math.min(newInterval[0], intervals[i][0]);
        newInterval[1] = Math.max(newInterval[1], intervals[i][1]);
        i++;
    }
    result.add(newInterval);
    
    // Add remaining intervals
    while (i < intervals.length) {
        result.add(intervals[i++]);
    }
    
    return result.toArray(new int[result.size()][]);
}
```

---

## Cyclic Sort

### Pattern
Sort array where numbers are in range [1, n].

**When to Use:**
- Array elements in range [1, n]
- Finding missing/duplicate numbers

```java
// Cyclic sort
public void cyclicSort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correctIndex = nums[i] - 1;
        if (nums[i] != nums[correctIndex]) {
            swap(nums, i, correctIndex);
        } else {
            i++;
        }
    }
}

// Find missing number
public int findMissingNumber(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        if (nums[i] < nums.length && nums[i] != nums[nums[i]]) {
            swap(nums, i, nums[i]);
        } else {
            i++;
        }
    }
    
    for (i = 0; i < nums.length; i++) {
        if (nums[i] != i) {
            return i;
        }
    }
    
    return nums.length;
}
```

---

## Top K Elements

### Pattern
Use heap to find top k elements.

**When to Use:**
- Finding k largest/smallest
- Frequency-based problems

```java
// Top k frequent elements
public int[] topKFrequent(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    for (int num : nums) {
        freq.put(num, freq.getOrDefault(num, 0) + 1);
    }
    
    PriorityQueue<Integer> heap = new PriorityQueue<>(
        (a, b) -> freq.get(a) - freq.get(b)
    );
    
    for (int num : freq.keySet()) {
        heap.offer(num);
        if (heap.size() > k) {
            heap.poll();
        }
    }
    
    int[] result = new int[k];
    for (int i = 0; i < k; i++) {
        result[i] = heap.poll();
    }
    
    return result;
}
```

---

## Binary Search Patterns

### Pattern
Search in sorted/rotated arrays.

**When to Use:**
- Sorted array search
- Finding boundaries
- Binary search on answer

```java
// Template for finding left boundary
public int findLeftBoundary(int[] nums, int target) {
    int left = 0;
    int right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] >= target) {
            if (nums[mid] == target) {
                result = mid;
            }
            right = mid - 1;
        } else {
            left = mid + 1;
        }
    }
    
    return result;
}
```

---

## Tree Patterns

### Pattern
Common tree traversal patterns.

**DFS Patterns:**
1. Pre-order: Root → Left → Right
2. In-order: Left → Root → Right
3. Post-order: Left → Right → Root

**BFS Pattern:**
Level-order traversal

```java
// DFS Template
public void dfs(TreeNode node) {
    if (node == null) return;
    
    // Pre-order: process node here
    
    dfs(node.left);
    
    // In-order: process node here
    
    dfs(node.right);
    
    // Post-order: process node here
}

// BFS Template
public void bfs(TreeNode root) {
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        for (int i = 0; i < size; i++) {
            TreeNode node = queue.poll();
            // Process node
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
}
```

---

## Graph Patterns

### Pattern
Graph traversal and search.

```java
// DFS Template (Graph)
public void dfs(int node, boolean[] visited, List<List<Integer>> graph) {
    visited[node] = true;
    
    for (int neighbor : graph.get(node)) {
        if (!visited[neighbor]) {
            dfs(neighbor, visited, graph);
        }
    }
}

// BFS Template (Graph)
public void bfs(int start, List<List<Integer>> graph) {
    boolean[] visited = new boolean[graph.size()];
    Queue<Integer> queue = new LinkedList<>();
    
    visited[start] = true;
    queue.offer(start);
    
    while (!queue.isEmpty()) {
        int node = queue.poll();
        
        for (int neighbor : graph.get(node)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}
```

---

## DP Patterns

### Pattern
Common DP patterns.

**1. Linear DP:** dp[i] depends on dp[i-1]  
**2. Grid DP:** dp[i][j] depends on neighbors  
**3. Knapsack:** Include/exclude pattern  
**4. LCS:** Two sequence comparison  

```java
// DP Template
public int dp(/* parameters */) {
    // 1. Define state
    int[] dp = new int[n];
    
    // 2. Initialize base cases
    dp[0] = /* base case */;
    
    // 3. Fill dp table
    for (int i = 1; i < n; i++) {
        dp[i] = /* recurrence relation */;
    }
    
    // 4. Return result
    return dp[n - 1];
}
```

---

## Problem Recognition Guide

| Pattern | Keywords | Example Problems |
|---------|----------|------------------|
| Two Pointers | Sorted array, pairs | Two Sum, Container With Water |
| Sliding Window | Subarray, substring | Longest Substring, Max Sum |
| Fast/Slow | Cycle, middle | Linked List Cycle, Middle |
| Merge Intervals | Overlapping | Merge Intervals, Meeting Rooms |
| Cyclic Sort | [1,n] range | Find Missing, Find Duplicate |
| Top K | Largest/smallest k | Top K Frequent, Kth Largest |
| Binary Search | Sorted, search | Binary Search, Search Rotated |
| BFS | Level-order, shortest | Tree Level, Shortest Path |
| DFS | All paths, backtrack | Permutations, N-Queens |
| DP | Optimal, count ways | Fibonacci, Knapsack, LCS |
| Greedy | Optimal choice | Activity Selection, Huffman |
| Backtracking | Generate all | Subsets, Combinations |

---

## Key Takeaways

1. **Identify Pattern**: Look for keywords in problem
2. **Choose Template**: Use appropriate pattern template
3. **Adapt Solution**: Modify template for specific problem
4. **Optimize**: Consider time/space improvements
5. **Test**: Verify with examples and edge cases

**Interview Tip**: Practice recognizing patterns quickly!

