# Backtracking

## 📖 Table of Contents
- [Backtracking Concept](#backtracking-concept)
- [Template](#backtracking-template)
- [Permutations and Combinations](#permutations-and-combinations)
- [Subset Problems](#subset-problems)
- [Board Problems](#board-problems)
- [Classic Problems](#classic-problems)

---

## Backtracking Concept

### What is Backtracking?
Try all possibilities by building solution incrementally and backtrack when solution is invalid.

**Key Idea:** Explore → Check → Backtrack

**When to Use:**
- Generate all possible solutions
- Find one valid solution
- Count number of solutions

---

## Backtracking Template

```java
public void backtrack(State state, List<Solution> result) {
    // Base case: check if valid solution
    if (isValid(state)) {
        result.add(new Solution(state));
        return;
    }
    
    // Try all possible choices
    for (Choice choice : getAllChoices(state)) {
        // Make choice
        makeChoice(state, choice);
        
        // Recurse
        backtrack(state, result);
        
        // Undo choice (backtrack)
        undoChoice(state, choice);
    }
}
```

---

## Permutations and Combinations

### 1. Permutations

```java
// Generate all permutations - O(n * n!) time
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, new ArrayList<>(), new boolean[nums.length], result);
    return result;
}

private void backtrack(int[] nums, List<Integer> current, 
                       boolean[] used, List<List<Integer>> result) {
    // Base case
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        
        // Make choice
        current.add(nums[i]);
        used[i] = true;
        
        // Recurse
        backtrack(nums, current, used, result);
        
        // Undo choice
        current.remove(current.size() - 1);
        used[i] = false;
    }
}

// Permutations with duplicates
public List<List<Integer>> permuteUnique(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(nums);  // Sort to handle duplicates
    backtrack(nums, new ArrayList<>(), new boolean[nums.length], result);
    return result;
}

private void backtrack(int[] nums, List<Integer> current, 
                       boolean[] used, List<List<Integer>> result) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        
        // Skip duplicates
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
            continue;
        }
        
        current.add(nums[i]);
        used[i] = true;
        
        backtrack(nums, current, used, result);
        
        current.remove(current.size() - 1);
        used[i] = false;
    }
}
```

### 2. Combinations

```java
// Generate all combinations - O(2ⁿ) time
public List<List<Integer>> combine(int n, int k) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(1, n, k, new ArrayList<>(), result);
    return result;
}

private void backtrack(int start, int n, int k, 
                       List<Integer> current, List<List<Integer>> result) {
    if (current.size() == k) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int i = start; i <= n; i++) {
        current.add(i);
        backtrack(i + 1, n, k, current, result);
        current.remove(current.size() - 1);
    }
}

// Combination Sum (can reuse elements)
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(candidates);
    backtrack(candidates, target, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] candidates, int target, int start,
                       List<Integer> current, List<List<Integer>> result) {
    if (target == 0) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    if (target < 0) return;
    
    for (int i = start; i < candidates.length; i++) {
        current.add(candidates[i]);
        backtrack(candidates, target - candidates[i], i, current, result);
        current.remove(current.size() - 1);
    }
}
```

---

## Subset Problems

### 1. Subsets

```java
// Generate all subsets - O(2ⁿ) time
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, int start, 
                       List<Integer> current, List<List<Integer>> result) {
    result.add(new ArrayList<>(current));
    
    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);
        backtrack(nums, i + 1, current, result);
        current.remove(current.size() - 1);
    }
}

// Subsets with duplicates
public List<List<Integer>> subsetsWithDup(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(int[] nums, int start, 
                       List<Integer> current, List<List<Integer>> result) {
    result.add(new ArrayList<>(current));
    
    for (int i = start; i < nums.length; i++) {
        // Skip duplicates
        if (i > start && nums[i] == nums[i - 1]) {
            continue;
        }
        
        current.add(nums[i]);
        backtrack(nums, i + 1, current, result);
        current.remove(current.size() - 1);
    }
}
```

### 2. Partition to K Equal Sum Subsets

```java
// Check if can partition into k equal subsets - O(k * 2ⁿ) time
public boolean canPartitionKSubsets(int[] nums, int k) {
    int sum = 0;
    for (int num : nums) sum += num;
    
    if (sum % k != 0) return false;
    
    int target = sum / k;
    Arrays.sort(nums);
    
    return backtrack(nums, nums.length - 1, new int[k], target);
}

private boolean backtrack(int[] nums, int index, int[] groups, int target) {
    if (index < 0) {
        // All numbers placed
        return true;
    }
    
    int num = nums[index];
    
    for (int i = 0; i < groups.length; i++) {
        if (groups[i] + num <= target) {
            groups[i] += num;
            
            if (backtrack(nums, index - 1, groups, target)) {
                return true;
            }
            
            groups[i] -= num;
        }
        
        // Optimization: if current group is empty, skip other empty groups
        if (groups[i] == 0) break;
    }
    
    return false;
}
```

---

## Board Problems

### 1. N-Queens

```java
// Place N queens on NxN board - O(N!) time
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], '.');
    }
    
    backtrack(board, 0, result);
    return result;
}

private void backtrack(char[][] board, int row, List<List<String>> result) {
    if (row == board.length) {
        result.add(construct(board));
        return;
    }
    
    for (int col = 0; col < board.length; col++) {
        if (isValid(board, row, col)) {
            board[row][col] = 'Q';
            backtrack(board, row + 1, result);
            board[row][col] = '.';
        }
    }
}

private boolean isValid(char[][] board, int row, int col) {
    // Check column
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') return false;
    }
    
    // Check diagonal (top-left)
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }
    
    // Check diagonal (top-right)
    for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
        if (board[i][j] == 'Q') return false;
    }
    
    return true;
}

private List<String> construct(char[][] board) {
    List<String> result = new ArrayList<>();
    for (char[] row : board) {
        result.add(new String(row));
    }
    return result;
}
```

### 2. Sudoku Solver

```java
// Solve Sudoku - O(9^m) where m = empty cells
public void solveSudoku(char[][] board) {
    backtrack(board);
}

private boolean backtrack(char[][] board) {
    for (int row = 0; row < 9; row++) {
        for (int col = 0; col < 9; col++) {
            if (board[row][col] == '.') {
                for (char c = '1'; c <= '9'; c++) {
                    if (isValid(board, row, col, c)) {
                        board[row][col] = c;
                        
                        if (backtrack(board)) {
                            return true;
                        }
                        
                        board[row][col] = '.';
                    }
                }
                return false;
            }
        }
    }
    return true;
}

private boolean isValid(char[][] board, int row, int col, char c) {
    for (int i = 0; i < 9; i++) {
        // Check row
        if (board[row][i] == c) return false;
        
        // Check column
        if (board[i][col] == c) return false;
        
        // Check 3x3 box
        int boxRow = 3 * (row / 3) + i / 3;
        int boxCol = 3 * (col / 3) + i % 3;
        if (board[boxRow][boxCol] == c) return false;
    }
    return true;
}
```

### 3. Word Search

```java
// Search word in 2D board - O(m * n * 4^L) where L = word length
public boolean exist(char[][] board, String word) {
    for (int i = 0; i < board.length; i++) {
        for (int j = 0; j < board[0].length; j++) {
            if (backtrack(board, word, 0, i, j)) {
                return true;
            }
        }
    }
    return false;
}

private boolean backtrack(char[][] board, String word, int index, int row, int col) {
    if (index == word.length()) {
        return true;
    }
    
    if (row < 0 || row >= board.length || col < 0 || col >= board[0].length ||
        board[row][col] != word.charAt(index)) {
        return false;
    }
    
    char temp = board[row][col];
    board[row][col] = '#';  // Mark as visited
    
    boolean found = backtrack(board, word, index + 1, row + 1, col) ||
                    backtrack(board, word, index + 1, row - 1, col) ||
                    backtrack(board, word, index + 1, row, col + 1) ||
                    backtrack(board, word, index + 1, row, col - 1);
    
    board[row][col] = temp;  // Restore
    
    return found;
}
```

---

## Classic Problems

### 1. Generate Parentheses

```java
// Generate valid parentheses - O(4ⁿ/√n) time
public List<String> generateParenthesis(int n) {
    List<String> result = new ArrayList<>();
    backtrack(result, new StringBuilder(), 0, 0, n);
    return result;
}

private void backtrack(List<String> result, StringBuilder current,
                       int open, int close, int n) {
    if (current.length() == 2 * n) {
        result.add(current.toString());
        return;
    }
    
    if (open < n) {
        current.append('(');
        backtrack(result, current, open + 1, close, n);
        current.deleteCharAt(current.length() - 1);
    }
    
    if (close < open) {
        current.append(')');
        backtrack(result, current, open, close + 1, n);
        current.deleteCharAt(current.length() - 1);
    }
}
```

### 2. Letter Combinations of Phone Number

```java
// Generate letter combinations - O(4ⁿ) time
private static final String[] KEYS = {
    "", "", "abc", "def", "ghi", "jkl",
    "mno", "pqrs", "tuv", "wxyz"
};

public List<String> letterCombinations(String digits) {
    List<String> result = new ArrayList<>();
    if (digits.isEmpty()) return result;
    
    backtrack(digits, 0, new StringBuilder(), result);
    return result;
}

private void backtrack(String digits, int index, 
                       StringBuilder current, List<String> result) {
    if (index == digits.length()) {
        result.add(current.toString());
        return;
    }
    
    String letters = KEYS[digits.charAt(index) - '0'];
    
    for (char c : letters.toCharArray()) {
        current.append(c);
        backtrack(digits, index + 1, current, result);
        current.deleteCharAt(current.length() - 1);
    }
}
```

### 3. Palindrome Partitioning

```java
// Partition string into palindrome substrings - O(n * 2ⁿ) time
public List<List<String>> partition(String s) {
    List<List<String>> result = new ArrayList<>();
    backtrack(s, 0, new ArrayList<>(), result);
    return result;
}

private void backtrack(String s, int start, 
                       List<String> current, List<List<String>> result) {
    if (start == s.length()) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int end = start + 1; end <= s.length(); end++) {
        String substring = s.substring(start, end);
        
        if (isPalindrome(substring)) {
            current.add(substring);
            backtrack(s, end, current, result);
            current.remove(current.size() - 1);
        }
    }
}

private boolean isPalindrome(String s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s.charAt(left++) != s.charAt(right--)) {
            return false;
        }
    }
    
    return true;
}
```

---

## Key Takeaways

1. **Three Steps**: Make choice → Recurse → Undo choice
2. **Pruning**: Add conditions to skip invalid branches
3. **State Management**: Use visited arrays or modify in-place
4. **Optimization**: Sort input to enable early termination
5. **Time Complexity**: Often exponential

**Interview Tip**: Draw recursion tree to visualize backtracking!

