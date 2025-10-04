# Trees

## 📖 Table of Contents
- [Binary Tree Basics](#binary-tree-basics)
- [Binary Tree Traversals](#binary-tree-traversals)
- [Binary Search Tree (BST)](#binary-search-tree-bst)
- [AVL Tree](#avl-tree)
- [Trie (Prefix Tree)](#trie-prefix-tree)
- [Segment Tree](#segment-tree)
- [Important Problems](#important-problems)

---

## Binary Tree Basics

### What is a Binary Tree?
A tree data structure where each node has at most two children (left and right).

**Types:**
- **Full Binary Tree**: Every node has 0 or 2 children
- **Complete Binary Tree**: All levels filled except possibly last, filled left to right
- **Perfect Binary Tree**: All internal nodes have 2 children, all leaves at same level
- **Balanced Binary Tree**: Height difference of left and right subtrees ≤ 1

### Node Structure

```java
// TreeNode class
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    
    // Constructor
    public TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
```

### Basic Binary Tree Operations

```java
public class BinaryTree {
    TreeNode root;
    
    public BinaryTree() {
        root = null;
    }
    
    // Insert node (level order for complete tree)
    public void insert(int val) {
        TreeNode newNode = new TreeNode(val);
        
        if (root == null) {
            root = newNode;
            return;
        }
        
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);
        
        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();
            
            if (current.left == null) {
                current.left = newNode;
                return;
            } else {
                queue.offer(current.left);
            }
            
            if (current.right == null) {
                current.right = newNode;
                return;
            } else {
                queue.offer(current.right);
            }
        }
    }
    
    // Calculate height - O(n) time
    public int height(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return 1 + Math.max(height(node.left), height(node.right));
    }
    
    // Count nodes - O(n) time
    public int countNodes(TreeNode node) {
        if (node == null) {
            return 0;
        }
        return 1 + countNodes(node.left) + countNodes(node.right);
    }
    
    // Count leaf nodes - O(n) time
    public int countLeaves(TreeNode node) {
        if (node == null) {
            return 0;
        }
        if (node.left == null && node.right == null) {
            return 1;
        }
        return countLeaves(node.left) + countLeaves(node.right);
    }
}
```

---

## Binary Tree Traversals

### 1. Inorder Traversal (Left → Root → Right)

```java
// Recursive - O(n) time, O(h) space (call stack)
public void inorderRecursive(TreeNode root) {
    if (root == null) return;
    
    inorderRecursive(root.left);       // Left
    System.out.print(root.val + " ");  // Root
    inorderRecursive(root.right);      // Right
}

// Iterative using stack - O(n) time, O(h) space
public List<Integer> inorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Stack<TreeNode> stack = new Stack<>();
    TreeNode current = root;
    
    while (current != null || !stack.isEmpty()) {
        // Go to leftmost node
        while (current != null) {
            stack.push(current);
            current = current.left;
        }
        
        // Process node
        current = stack.pop();
        result.add(current.val);
        
        // Move to right subtree
        current = current.right;
    }
    
    return result;
}
```

### 2. Preorder Traversal (Root → Left → Right)

```java
// Recursive - O(n) time, O(h) space
public void preorderRecursive(TreeNode root) {
    if (root == null) return;
    
    System.out.print(root.val + " ");  // Root
    preorderRecursive(root.left);      // Left
    preorderRecursive(root.right);     // Right
}

// Iterative - O(n) time, O(h) space
public List<Integer> preorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    
    Stack<TreeNode> stack = new Stack<>();
    stack.push(root);
    
    while (!stack.isEmpty()) {
        TreeNode node = stack.pop();
        result.add(node.val);
        
        // Push right first (so left is processed first)
        if (node.right != null) {
            stack.push(node.right);
        }
        if (node.left != null) {
            stack.push(node.left);
        }
    }
    
    return result;
}
```

### 3. Postorder Traversal (Left → Right → Root)

```java
// Recursive - O(n) time, O(h) space
public void postorderRecursive(TreeNode root) {
    if (root == null) return;
    
    postorderRecursive(root.left);     // Left
    postorderRecursive(root.right);    // Right
    System.out.print(root.val + " ");  // Root
}

// Iterative using two stacks - O(n) time, O(h) space
public List<Integer> postorderIterative(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    if (root == null) return result;
    
    Stack<TreeNode> stack1 = new Stack<>();
    Stack<TreeNode> stack2 = new Stack<>();
    
    stack1.push(root);
    
    while (!stack1.isEmpty()) {
        TreeNode node = stack1.pop();
        stack2.push(node);
        
        if (node.left != null) {
            stack1.push(node.left);
        }
        if (node.right != null) {
            stack1.push(node.right);
        }
    }
    
    while (!stack2.isEmpty()) {
        result.add(stack2.pop().val);
    }
    
    return result;
}
```

### 4. Level Order Traversal (BFS)

```java
// Level order using queue - O(n) time, O(w) space (w = max width)
public List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        List<Integer> currentLevel = new ArrayList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            currentLevel.add(node.val);
            
            if (node.left != null) {
                queue.offer(node.left);
            }
            if (node.right != null) {
                queue.offer(node.right);
            }
        }
        
        result.add(currentLevel);
    }
    
    return result;
}

// Zigzag level order
public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    Queue<TreeNode> queue = new LinkedList<>();
    queue.offer(root);
    boolean leftToRight = true;
    
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        LinkedList<Integer> currentLevel = new LinkedList<>();
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            
            // Add based on direction
            if (leftToRight) {
                currentLevel.addLast(node.val);
            } else {
                currentLevel.addFirst(node.val);
            }
            
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
        
        result.add(currentLevel);
        leftToRight = !leftToRight;
    }
    
    return result;
}
```

---

## Binary Search Tree (BST)

### What is a BST?
A binary tree where:
- Left subtree values < Node value
- Right subtree values > Node value
- Both left and right subtrees are BSTs

**Time Complexity:**
- Search, Insert, Delete: O(h) where h = height
- Balanced BST: O(log n)
- Skewed BST: O(n)

### BST Implementation

```java
public class BST {
    TreeNode root;
    
    public BST() {
        root = null;
    }
    
    // Insert - O(h) time
    public TreeNode insert(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }
        
        if (val < root.val) {
            root.left = insert(root.left, val);
        } else if (val > root.val) {
            root.right = insert(root.right, val);
        }
        
        return root;
    }
    
    // Search - O(h) time
    public boolean search(TreeNode root, int val) {
        if (root == null) {
            return false;
        }
        
        if (val == root.val) {
            return true;
        } else if (val < root.val) {
            return search(root.left, val);
        } else {
            return search(root.right, val);
        }
    }
    
    // Find minimum - O(h) time
    public TreeNode findMin(TreeNode root) {
        while (root.left != null) {
            root = root.left;
        }
        return root;
    }
    
    // Find maximum - O(h) time
    public TreeNode findMax(TreeNode root) {
        while (root.right != null) {
            root = root.right;
        }
        return root;
    }
    
    // Delete - O(h) time
    public TreeNode delete(TreeNode root, int val) {
        if (root == null) {
            return null;
        }
        
        if (val < root.val) {
            root.left = delete(root.left, val);
        } else if (val > root.val) {
            root.right = delete(root.right, val);
        } else {
            // Node found
            
            // Case 1: No children (leaf node)
            if (root.left == null && root.right == null) {
                return null;
            }
            
            // Case 2: One child
            if (root.left == null) {
                return root.right;
            }
            if (root.right == null) {
                return root.left;
            }
            
            // Case 3: Two children
            // Find inorder successor (min in right subtree)
            TreeNode successor = findMin(root.right);
            root.val = successor.val;
            root.right = delete(root.right, successor.val);
        }
        
        return root;
    }
    
    // Validate BST - O(n) time
    public boolean isValidBST(TreeNode root) {
        return isValidBSTHelper(root, null, null);
    }
    
    private boolean isValidBSTHelper(TreeNode node, Integer min, Integer max) {
        if (node == null) {
            return true;
        }
        
        // Check current node value
        if ((min != null && node.val <= min) || 
            (max != null && node.val >= max)) {
            return false;
        }
        
        // Check left and right subtrees
        return isValidBSTHelper(node.left, min, node.val) &&
               isValidBSTHelper(node.right, node.val, max);
    }
}
```

---

## AVL Tree

### What is an AVL Tree?
A self-balancing BST where the height difference between left and right subtrees is at most 1.

**Balance Factor** = height(left) - height(right)  
Valid values: -1, 0, 1

### AVL Tree Implementation

```java
class AVLNode {
    int val;
    AVLNode left;
    AVLNode right;
    int height;
    
    public AVLNode(int val) {
        this.val = val;
        this.height = 1;
    }
}

public class AVLTree {
    
    // Get height - O(1) time
    private int height(AVLNode node) {
        return node == null ? 0 : node.height;
    }
    
    // Get balance factor - O(1) time
    private int getBalance(AVLNode node) {
        return node == null ? 0 : height(node.left) - height(node.right);
    }
    
    // Update height - O(1) time
    private void updateHeight(AVLNode node) {
        node.height = 1 + Math.max(height(node.left), height(node.right));
    }
    
    // Right rotation
    private AVLNode rightRotate(AVLNode y) {
        AVLNode x = y.left;
        AVLNode T2 = x.right;
        
        // Perform rotation
        x.right = y;
        y.left = T2;
        
        // Update heights
        updateHeight(y);
        updateHeight(x);
        
        return x;  // New root
    }
    
    // Left rotation
    private AVLNode leftRotate(AVLNode x) {
        AVLNode y = x.right;
        AVLNode T2 = y.left;
        
        // Perform rotation
        y.left = x;
        x.right = T2;
        
        // Update heights
        updateHeight(x);
        updateHeight(y);
        
        return y;  // New root
    }
    
    // Insert - O(log n) time
    public AVLNode insert(AVLNode node, int val) {
        // Normal BST insertion
        if (node == null) {
            return new AVLNode(val);
        }
        
        if (val < node.val) {
            node.left = insert(node.left, val);
        } else if (val > node.val) {
            node.right = insert(node.right, val);
        } else {
            return node;  // Duplicate not allowed
        }
        
        // Update height
        updateHeight(node);
        
        // Get balance factor
        int balance = getBalance(node);
        
        // Left Left Case
        if (balance > 1 && val < node.left.val) {
            return rightRotate(node);
        }
        
        // Right Right Case
        if (balance < -1 && val > node.right.val) {
            return leftRotate(node);
        }
        
        // Left Right Case
        if (balance > 1 && val > node.left.val) {
            node.left = leftRotate(node.left);
            return rightRotate(node);
        }
        
        // Right Left Case
        if (balance < -1 && val < node.right.val) {
            node.right = rightRotate(node.right);
            return leftRotate(node);
        }
        
        return node;
    }
}
```

---

## Trie (Prefix Tree)

### What is a Trie?
A tree-like data structure for storing strings, efficient for prefix-based searches.

**Use Cases:**
- Autocomplete
- Spell checker
- IP routing
- Dictionary

```java
class TrieNode {
    TrieNode[] children;
    boolean isEndOfWord;
    
    public TrieNode() {
        children = new TrieNode[26];  // For lowercase a-z
        isEndOfWord = false;
    }
}

public class Trie {
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    // Insert word - O(m) time, m = word length
    public void insert(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            
            if (current.children[index] == null) {
                current.children[index] = new TrieNode();
            }
            
            current = current.children[index];
        }
        
        current.isEndOfWord = true;
    }
    
    // Search exact word - O(m) time
    public boolean search(String word) {
        TrieNode current = root;
        
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            
            if (current.children[index] == null) {
                return false;
            }
            
            current = current.children[index];
        }
        
        return current.isEndOfWord;
    }
    
    // Check if prefix exists - O(m) time
    public boolean startsWith(String prefix) {
        TrieNode current = root;
        
        for (char c : prefix.toCharArray()) {
            int index = c - 'a';
            
            if (current.children[index] == null) {
                return false;
            }
            
            current = current.children[index];
        }
        
        return true;
    }
    
    // Delete word - O(m) time
    public boolean delete(String word) {
        return deleteHelper(root, word, 0);
    }
    
    private boolean deleteHelper(TrieNode current, String word, int index) {
        if (index == word.length()) {
            // Reached end of word
            if (!current.isEndOfWord) {
                return false;  // Word doesn't exist
            }
            
            current.isEndOfWord = false;
            
            // Check if node has children
            return hasNoChildren(current);
        }
        
        char c = word.charAt(index);
        int charIndex = c - 'a';
        TrieNode node = current.children[charIndex];
        
        if (node == null) {
            return false;  // Word doesn't exist
        }
        
        boolean shouldDeleteChild = deleteHelper(node, word, index + 1);
        
        if (shouldDeleteChild) {
            current.children[charIndex] = null;
            return !current.isEndOfWord && hasNoChildren(current);
        }
        
        return false;
    }
    
    private boolean hasNoChildren(TrieNode node) {
        for (TrieNode child : node.children) {
            if (child != null) {
                return false;
            }
        }
        return true;
    }
}
```

---

## Segment Tree

### What is a Segment Tree?
A tree structure for storing intervals/segments, efficient for range queries.

**Use Cases:**
- Range sum queries
- Range minimum/maximum queries
- Range updates

```java
public class SegmentTree {
    private int[] tree;
    private int n;
    
    // Constructor - O(n) time
    public SegmentTree(int[] arr) {
        n = arr.length;
        tree = new int[4 * n];  // Size = 4 * n
        build(arr, 0, 0, n - 1);
    }
    
    // Build tree - O(n) time
    private void build(int[] arr, int node, int start, int end) {
        if (start == end) {
            // Leaf node
            tree[node] = arr[start];
            return;
        }
        
        int mid = start + (end - start) / 2;
        int leftChild = 2 * node + 1;
        int rightChild = 2 * node + 2;
        
        // Build left and right subtrees
        build(arr, leftChild, start, mid);
        build(arr, rightChild, mid + 1, end);
        
        // Internal node stores sum of children
        tree[node] = tree[leftChild] + tree[rightChild];
    }
    
    // Range sum query - O(log n) time
    public int query(int l, int r) {
        return queryHelper(0, 0, n - 1, l, r);
    }
    
    private int queryHelper(int node, int start, int end, int l, int r) {
        // No overlap
        if (r < start || l > end) {
            return 0;
        }
        
        // Complete overlap
        if (l <= start && end <= r) {
            return tree[node];
        }
        
        // Partial overlap
        int mid = start + (end - start) / 2;
        int leftSum = queryHelper(2 * node + 1, start, mid, l, r);
        int rightSum = queryHelper(2 * node + 2, mid + 1, end, l, r);
        
        return leftSum + rightSum;
    }
    
    // Update single element - O(log n) time
    public void update(int index, int value) {
        updateHelper(0, 0, n - 1, index, value);
    }
    
    private void updateHelper(int node, int start, int end, int index, int value) {
        if (start == end) {
            // Leaf node
            tree[node] = value;
            return;
        }
        
        int mid = start + (end - start) / 2;
        
        if (index <= mid) {
            updateHelper(2 * node + 1, start, mid, index, value);
        } else {
            updateHelper(2 * node + 2, mid + 1, end, index, value);
        }
        
        // Update current node
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }
}
```

---

## Important Problems

### 1. Maximum Depth of Binary Tree

```java
// Find maximum depth - O(n) time
public int maxDepth(TreeNode root) {
    if (root == null) {
        return 0;
    }
    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

### 2. Diameter of Binary Tree

```java
// Find diameter (longest path between any two nodes) - O(n) time
private int diameter = 0;

public int diameterOfBinaryTree(TreeNode root) {
    height(root);
    return diameter;
}

private int height(TreeNode node) {
    if (node == null) return 0;
    
    int leftHeight = height(node.left);
    int rightHeight = height(node.right);
    
    // Update diameter
    diameter = Math.max(diameter, leftHeight + rightHeight);
    
    return 1 + Math.max(leftHeight, rightHeight);
}
```

### 3. Lowest Common Ancestor (LCA)

```java
// Find LCA in binary tree - O(n) time
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) {
        return root;
    }
    
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    
    if (left != null && right != null) {
        return root;  // Both found in different subtrees
    }
    
    return left != null ? left : right;
}

// LCA in BST - O(h) time
public TreeNode lowestCommonAncestorBST(TreeNode root, TreeNode p, TreeNode q) {
    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestorBST(root.left, p, q);
    } else if (p.val > root.val && q.val > root.val) {
        return lowestCommonAncestorBST(root.right, p, q);
    } else {
        return root;
    }
}
```

### 4. Serialize and Deserialize Binary Tree

```java
// Serialize tree to string - O(n) time
public String serialize(TreeNode root) {
    if (root == null) {
        return "null";
    }
    
    return root.val + "," + serialize(root.left) + "," + serialize(root.right);
}

// Deserialize string to tree - O(n) time
public TreeNode deserialize(String data) {
    Queue<String> queue = new LinkedList<>(Arrays.asList(data.split(",")));
    return deserializeHelper(queue);
}

private TreeNode deserializeHelper(Queue<String> queue) {
    String val = queue.poll();
    
    if (val.equals("null")) {
        return null;
    }
    
    TreeNode node = new TreeNode(Integer.parseInt(val));
    node.left = deserializeHelper(queue);
    node.right = deserializeHelper(queue);
    
    return node;
}
```

### 5. Binary Tree Maximum Path Sum

```java
// Find maximum path sum - O(n) time
private int maxSum = Integer.MIN_VALUE;

public int maxPathSum(TreeNode root) {
    maxPathSumHelper(root);
    return maxSum;
}

private int maxPathSumHelper(TreeNode node) {
    if (node == null) return 0;
    
    // Get max sum from left and right (ignore negative paths)
    int left = Math.max(0, maxPathSumHelper(node.left));
    int right = Math.max(0, maxPathSumHelper(node.right));
    
    // Update global max (path through current node)
    maxSum = Math.max(maxSum, node.val + left + right);
    
    // Return max sum including current node
    return node.val + Math.max(left, right);
}
```

### 6. Construct Binary Tree from Inorder and Preorder

```java
// Build tree from inorder and preorder - O(n) time
public TreeNode buildTree(int[] preorder, int[] inorder) {
    Map<Integer, Integer> inorderMap = new HashMap<>();
    for (int i = 0; i < inorder.length; i++) {
        inorderMap.put(inorder[i], i);
    }
    
    return buildTreeHelper(preorder, 0, preorder.length - 1,
                          inorder, 0, inorder.length - 1, inorderMap);
}

private TreeNode buildTreeHelper(int[] preorder, int preStart, int preEnd,
                                 int[] inorder, int inStart, int inEnd,
                                 Map<Integer, Integer> inorderMap) {
    if (preStart > preEnd || inStart > inEnd) {
        return null;
    }
    
    TreeNode root = new TreeNode(preorder[preStart]);
    int inRoot = inorderMap.get(root.val);
    int numsLeft = inRoot - inStart;
    
    root.left = buildTreeHelper(preorder, preStart + 1, preStart + numsLeft,
                                inorder, inStart, inRoot - 1, inorderMap);
    root.right = buildTreeHelper(preorder, preStart + numsLeft + 1, preEnd,
                                 inorder, inRoot + 1, inEnd, inorderMap);
    
    return root;
}
```

### 7. Kth Smallest Element in BST

```java
// Find kth smallest - O(h + k) time
public int kthSmallest(TreeNode root, int k) {
    Stack<TreeNode> stack = new Stack<>();
    TreeNode current = root;
    int count = 0;
    
    while (current != null || !stack.isEmpty()) {
        while (current != null) {
            stack.push(current);
            current = current.left;
        }
        
        current = stack.pop();
        count++;
        
        if (count == k) {
            return current.val;
        }
        
        current = current.right;
    }
    
    return -1;
}
```

### 8. Vertical Order Traversal

```java
// Vertical order traversal - O(n log n) time
class Pair {
    TreeNode node;
    int col;
    int row;
    
    Pair(TreeNode node, int col, int row) {
        this.node = node;
        this.col = col;
        this.row = row;
    }
}

public List<List<Integer>> verticalTraversal(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;
    
    TreeMap<Integer, TreeMap<Integer, PriorityQueue<Integer>>> map = new TreeMap<>();
    Queue<Pair> queue = new LinkedList<>();
    queue.offer(new Pair(root, 0, 0));
    
    while (!queue.isEmpty()) {
        Pair pair = queue.poll();
        TreeNode node = pair.node;
        int col = pair.col;
        int row = pair.row;
        
        map.putIfAbsent(col, new TreeMap<>());
        map.get(col).putIfAbsent(row, new PriorityQueue<>());
        map.get(col).get(row).offer(node.val);
        
        if (node.left != null) {
            queue.offer(new Pair(node.left, col - 1, row + 1));
        }
        if (node.right != null) {
            queue.offer(new Pair(node.right, col + 1, row + 1));
        }
    }
    
    for (TreeMap<Integer, PriorityQueue<Integer>> rows : map.values()) {
        List<Integer> column = new ArrayList<>();
        for (PriorityQueue<Integer> pq : rows.values()) {
            while (!pq.isEmpty()) {
                column.add(pq.poll());
            }
        }
        result.add(column);
    }
    
    return result;
}
```

---

## Key Takeaways

1. **Recursion**: Natural fit for tree problems
2. **BST Property**: Exploit ordering for efficient operations
3. **Traversals**: Master all types (inorder, preorder, postorder, level)
4. **Height**: Many problems involve height calculations
5. **Balanced Trees**: AVL ensures O(log n) operations

**Practice Tip**: Draw tree diagrams and trace recursive calls!

