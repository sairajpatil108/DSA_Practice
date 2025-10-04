# Dynamic Programming

## 📖 Table of Contents
- [DP Basics](#dp-basics)
- [DP Approaches](#dp-approaches)
- [Common DP Patterns](#common-dp-patterns)
- [Classic DP Problems](#classic-dp-problems)
- [DP on Strings](#dp-on-strings)
- [DP on Trees](#dp-on-trees)
- [Advanced DP](#advanced-dp)

---

## DP Basics

### What is Dynamic Programming?
DP is an optimization technique that solves complex problems by breaking them into simpler subproblems and storing results to avoid redundant computation.

**When to Use DP:**
1. **Optimal Substructure**: Optimal solution contains optimal solutions to subproblems
2. **Overlapping Subproblems**: Same subproblems solved multiple times

**Types:**
1. **Memoization (Top-Down)**: Recursion + caching
2. **Tabulation (Bottom-Up)**: Iterative + table

**Steps to Solve DP:**
1. Define state (what represents a subproblem)
2. Define recurrence relation
3. Identify base cases
4. Decide computation order
5. Optimize space if possible

---

## DP Approaches

### 1. Fibonacci - Classic Example

```java
// Naive Recursion - O(2ⁿ) time, O(n) space
public int fibNaive(int n) {
    if (n <= 1) return n;
    return fibNaive(n - 1) + fibNaive(n - 2);
}

// Memoization (Top-Down DP) - O(n) time, O(n) space
public int fibMemo(int n) {
    int[] memo = new int[n + 1];
    Arrays.fill(memo, -1);
    return fibMemoHelper(n, memo);
}

private int fibMemoHelper(int n, int[] memo) {
    if (n <= 1) return n;
    
    if (memo[n] != -1) {
        return memo[n];  // Return cached result
    }
    
    memo[n] = fibMemoHelper(n - 1, memo) + fibMemoHelper(n - 2, memo);
    return memo[n];
}

// Tabulation (Bottom-Up DP) - O(n) time, O(n) space
public int fibTab(int n) {
    if (n <= 1) return n;
    
    int[] dp = new int[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    return dp[n];
}

// Space Optimized - O(n) time, O(1) space
public int fibOptimized(int n) {
    if (n <= 1) return n;
    
    int prev2 = 0;
    int prev1 = 1;
    
    for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    
    return prev1;
}
```

---

## Common DP Patterns

### 1. Climbing Stairs

```java
// Climb n stairs, can climb 1 or 2 steps - O(n) time, O(1) space
public int climbStairs(int n) {
    if (n <= 2) return n;
    
    int prev2 = 1;
    int prev1 = 2;
    
    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    
    return prev1;
}

// With k steps allowed
public int climbStairsK(int n, int k) {
    int[] dp = new int[n + 1];
    dp[0] = 1;
    
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= k && j <= i; j++) {
            dp[i] += dp[i - j];
        }
    }
    
    return dp[n];
}
```

### 2. House Robber

```java
// Rob houses, can't rob adjacent - O(n) time, O(1) space
public int rob(int[] nums) {
    if (nums.length == 0) return 0;
    if (nums.length == 1) return nums[0];
    
    int prev2 = 0;
    int prev1 = 0;
    
    for (int num : nums) {
        int curr = Math.max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    
    return prev1;
}

// House Robber II (circular)
public int robCircular(int[] nums) {
    if (nums.length == 1) return nums[0];
    
    // Either rob first house (exclude last) or rob last (exclude first)
    return Math.max(
        robHelper(nums, 0, nums.length - 2),
        robHelper(nums, 1, nums.length - 1)
    );
}

private int robHelper(int[] nums, int start, int end) {
    int prev2 = 0;
    int prev1 = 0;
    
    for (int i = start; i <= end; i++) {
        int curr = Math.max(prev1, prev2 + nums[i]);
        prev2 = prev1;
        prev1 = curr;
    }
    
    return prev1;
}
```

### 3. Coin Change

```java
// Minimum coins to make amount - O(n * amount) time
public int coinChange(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, amount + 1);  // Initialize with impossible value
    dp[0] = 0;  // Base case
    
    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i) {
                dp[i] = Math.min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    
    return dp[amount] > amount ? -1 : dp[amount];
}

// Number of ways to make amount - O(n * amount) time
public int coinChangeWays(int[] coins, int amount) {
    int[] dp = new int[amount + 1];
    dp[0] = 1;  // One way to make 0
    
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            dp[i] += dp[i - coin];
        }
    }
    
    return dp[amount];
}
```

---

## Classic DP Problems

### 1. 0/1 Knapsack

```java
// 0/1 Knapsack - O(n * W) time, O(n * W) space
public int knapsack(int[] weights, int[] values, int W) {
    int n = weights.length;
    int[][] dp = new int[n + 1][W + 1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (weights[i - 1] <= w) {
                // Max of including or excluding current item
                dp[i][w] = Math.max(
                    dp[i - 1][w],  // Exclude
                    dp[i - 1][w - weights[i - 1]] + values[i - 1]  // Include
                );
            } else {
                dp[i][w] = dp[i - 1][w];  // Can't include
            }
        }
    }
    
    return dp[n][W];
}

// Space Optimized - O(W) space
public int knapsackOptimized(int[] weights, int[] values, int W) {
    int[] dp = new int[W + 1];
    
    for (int i = 0; i < weights.length; i++) {
        // Traverse backward to avoid using updated values
        for (int w = W; w >= weights[i]; w--) {
            dp[w] = Math.max(dp[w], dp[w - weights[i]] + values[i]);
        }
    }
    
    return dp[W];
}
```

### 2. Longest Common Subsequence (LCS)

```java
// LCS - O(m * n) time, O(m * n) space
public int longestCommonSubsequence(String s1, String s2) {
    int m = s1.length();
    int n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[m][n];
}

// Space Optimized - O(n) space
public int lcsOptimized(String s1, String s2) {
    int m = s1.length();
    int n = s2.length();
    int[] prev = new int[n + 1];
    int[] curr = new int[n + 1];
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                curr[j] = prev[j - 1] + 1;
            } else {
                curr[j] = Math.max(prev[j], curr[j - 1]);
            }
        }
        int[] temp = prev;
        prev = curr;
        curr = temp;
    }
    
    return prev[n];
}
```

### 3. Longest Increasing Subsequence (LIS)

```java
// LIS - O(n²) time, O(n) space
public int lengthOfLIS(int[] nums) {
    if (nums.length == 0) return 0;
    
    int[] dp = new int[nums.length];
    Arrays.fill(dp, 1);  // Each element is a subsequence of length 1
    int maxLength = 1;
    
    for (int i = 1; i < nums.length; i++) {
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                dp[i] = Math.max(dp[i], dp[j] + 1);
            }
        }
        maxLength = Math.max(maxLength, dp[i]);
    }
    
    return maxLength;
}

// Binary Search approach - O(n log n) time
public int lengthOfLISBinarySearch(int[] nums) {
    List<Integer> tails = new ArrayList<>();
    
    for (int num : nums) {
        int pos = Collections.binarySearch(tails, num);
        
        if (pos < 0) {
            pos = -(pos + 1);
        }
        
        if (pos == tails.size()) {
            tails.add(num);
        } else {
            tails.set(pos, num);
        }
    }
    
    return tails.size();
}
```

### 4. Maximum Subarray (Kadane's Algorithm)

```java
// Maximum sum subarray - O(n) time, O(1) space
public int maxSubArray(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}

// Maximum product subarray
public int maxProduct(int[] nums) {
    int maxSoFar = nums[0];
    int maxEndingHere = nums[0];
    int minEndingHere = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        int temp = maxEndingHere;
        maxEndingHere = Math.max(nums[i], 
                        Math.max(maxEndingHere * nums[i], 
                                minEndingHere * nums[i]));
        minEndingHere = Math.min(nums[i], 
                        Math.min(temp * nums[i], 
                                minEndingHere * nums[i]));
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    
    return maxSoFar;
}
```

### 5. Partition Equal Subset Sum

```java
// Check if array can be partitioned into two equal sum subsets
// O(n * sum) time
public boolean canPartition(int[] nums) {
    int sum = 0;
    for (int num : nums) {
        sum += num;
    }
    
    // If sum is odd, can't partition
    if (sum % 2 != 0) return false;
    
    int target = sum / 2;
    boolean[] dp = new boolean[target + 1];
    dp[0] = true;  // Can make sum 0
    
    for (int num : nums) {
        for (int j = target; j >= num; j--) {
            dp[j] = dp[j] || dp[j - num];
        }
    }
    
    return dp[target];
}
```

### 6. Unique Paths

```java
// Count unique paths in grid - O(m * n) time
public int uniquePaths(int m, int n) {
    int[][] dp = new int[m][n];
    
    // Initialize first row and column
    for (int i = 0; i < m; i++) dp[i][0] = 1;
    for (int j = 0; j < n; j++) dp[0][j] = 1;
    
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }
    
    return dp[m - 1][n - 1];
}

// Space Optimized - O(n) space
public int uniquePathsOptimized(int m, int n) {
    int[] dp = new int[n];
    Arrays.fill(dp, 1);
    
    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++) {
            dp[j] += dp[j - 1];
        }
    }
    
    return dp[n - 1];
}

// With obstacles
public int uniquePathsWithObstacles(int[][] grid) {
    int m = grid.length;
    int n = grid[0].length;
    
    if (grid[0][0] == 1) return 0;
    
    int[] dp = new int[n];
    dp[0] = 1;
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == 1) {
                dp[j] = 0;
            } else if (j > 0) {
                dp[j] += dp[j - 1];
            }
        }
    }
    
    return dp[n - 1];
}
```

### 7. Jump Game

```java
// Check if can reach end - O(n) time, O(1) space
public boolean canJump(int[] nums) {
    int maxReach = 0;
    
    for (int i = 0; i < nums.length; i++) {
        if (i > maxReach) return false;
        maxReach = Math.max(maxReach, i + nums[i]);
    }
    
    return true;
}

// Minimum jumps to reach end - O(n) time
public int jump(int[] nums) {
    int jumps = 0;
    int currentEnd = 0;
    int farthest = 0;
    
    for (int i = 0; i < nums.length - 1; i++) {
        farthest = Math.max(farthest, i + nums[i]);
        
        if (i == currentEnd) {
            jumps++;
            currentEnd = farthest;
        }
    }
    
    return jumps;
}
```

---

## DP on Strings

### 1. Edit Distance

```java
// Minimum operations to convert s1 to s2 - O(m * n) time
public int minDistance(String s1, String s2) {
    int m = s1.length();
    int n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    
    // Base cases
    for (int i = 0; i <= m; i++) dp[i][0] = i;  // Delete all
    for (int j = 0; j <= n; j++) dp[0][j] = j;  // Insert all
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                dp[i][j] = dp[i - 1][j - 1];  // No operation needed
            } else {
                dp[i][j] = 1 + Math.min(
                    dp[i - 1][j],      // Delete
                    Math.min(
                        dp[i][j - 1],  // Insert
                        dp[i - 1][j - 1]  // Replace
                    )
                );
            }
        }
    }
    
    return dp[m][n];
}
```

### 2. Longest Palindromic Subsequence

```java
// Longest palindromic subsequence - O(n²) time
public int longestPalindromeSubseq(String s) {
    int n = s.length();
    int[][] dp = new int[n][n];
    
    // Single character is palindrome of length 1
    for (int i = 0; i < n; i++) {
        dp[i][i] = 1;
    }
    
    // Fill table diagonally
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            
            if (s.charAt(i) == s.charAt(j)) {
                dp[i][j] = dp[i + 1][j - 1] + 2;
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

### 3. Longest Palindromic Substring

```java
// Expand around center - O(n²) time, O(1) space
public String longestPalindrome(String s) {
    if (s == null || s.length() == 0) return "";
    
    int start = 0;
    int maxLength = 0;
    
    for (int i = 0; i < s.length(); i++) {
        // Odd length palindrome
        int len1 = expandAroundCenter(s, i, i);
        // Even length palindrome
        int len2 = expandAroundCenter(s, i, i + 1);
        
        int len = Math.max(len1, len2);
        
        if (len > maxLength) {
            maxLength = len;
            start = i - (len - 1) / 2;
        }
    }
    
    return s.substring(start, start + maxLength);
}

private int expandAroundCenter(String s, int left, int right) {
    while (left >= 0 && right < s.length() && 
           s.charAt(left) == s.charAt(right)) {
        left--;
        right++;
    }
    return right - left - 1;
}
```

### 4. Word Break

```java
// Check if string can be segmented - O(n²) time
public boolean wordBreak(String s, List<String> wordDict) {
    Set<String> dict = new HashSet<>(wordDict);
    boolean[] dp = new boolean[s.length() + 1];
    dp[0] = true;  // Empty string
    
    for (int i = 1; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
            if (dp[j] && dict.contains(s.substring(j, i))) {
                dp[i] = true;
                break;
            }
        }
    }
    
    return dp[s.length()];
}
```

---

## DP on Trees

### 1. Diameter of Binary Tree

```java
// Maximum distance between any two nodes
private int diameter = 0;

public int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

private int height(TreeNode node) {
    if (node == null) return 0;
    
    int left = height(node.left);
    int right = height(node.right);
    
    diameter = Math.max(diameter, left + right);
    
    return 1 + Math.max(left, right);
}
```

### 2. House Robber III (Tree)

```java
// Rob houses in binary tree
public int rob(TreeNode root) {
    int[] result = robHelper(root);
    return Math.max(result[0], result[1]);
}

// Returns {robRoot, notRobRoot}
private int[] robHelper(TreeNode node) {
    if (node == null) return new int[]{0, 0};
    
    int[] left = robHelper(node.left);
    int[] right = robHelper(node.right);
    
    // If rob current node, can't rob children
    int robCurrent = node.val + left[1] + right[1];
    
    // If not rob current, take max from children
    int notRobCurrent = Math.max(left[0], left[1]) + 
                        Math.max(right[0], right[1]);
    
    return new int[]{robCurrent, notRobCurrent};
}
```

---

## Advanced DP

### 1. Matrix Chain Multiplication

```java
// Minimum multiplications needed - O(n³) time
public int matrixChainOrder(int[] dims) {
    int n = dims.length - 1;  // Number of matrices
    int[][] dp = new int[n][n];
    
    // len is chain length
    for (int len = 2; len <= n; len++) {
        for (int i = 0; i < n - len + 1; i++) {
            int j = i + len - 1;
            dp[i][j] = Integer.MAX_VALUE;
            
            for (int k = i; k < j; k++) {
                int cost = dp[i][k] + dp[k + 1][j] + 
                          dims[i] * dims[k + 1] * dims[j + 1];
                dp[i][j] = Math.min(dp[i][j], cost);
            }
        }
    }
    
    return dp[0][n - 1];
}
```

### 2. Palindrome Partitioning II

```java
// Minimum cuts for palindrome partitioning - O(n²) time
public int minCut(String s) {
    int n = s.length();
    boolean[][] isPalin = new boolean[n][n];
    int[] cuts = new int[n];
    
    // Build palindrome table
    for (int i = 0; i < n; i++) {
        int minCuts = i;  // Max cuts
        
        for (int j = 0; j <= i; j++) {
            if (s.charAt(j) == s.charAt(i) && 
                (i - j <= 1 || isPalin[j + 1][i - 1])) {
                isPalin[j][i] = true;
                minCuts = (j == 0) ? 0 : Math.min(minCuts, cuts[j - 1] + 1);
            }
        }
        
        cuts[i] = minCuts;
    }
    
    return cuts[n - 1];
}
```

---

## Key Takeaways

1. **Identify State**: What represents a subproblem?
2. **Recurrence**: How to compute state from smaller states?
3. **Base Cases**: What are the simplest cases?
4. **Optimization**: Can space be reduced?
5. **Practice Patterns**: Knapsack, LCS, LIS, Matrix, Tree DP

**Interview Tip**: Draw small examples, identify pattern, then generalize!

