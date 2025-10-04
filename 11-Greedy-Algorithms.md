# Greedy Algorithms

## 📖 Table of Contents
- [Greedy Concept](#greedy-concept)
- [Classic Greedy Problems](#classic-greedy-problems)
- [Interval Problems](#interval-problems)
- [Optimization Problems](#optimization-problems)

---

## Greedy Concept

### What is Greedy Algorithm?
Make locally optimal choice at each step hoping to find global optimum.

**When to Use Greedy:**
1. Problem has optimal substructure
2. Greedy choice property (local optimum leads to global optimum)

**Greedy vs DP:**
- Greedy: Makes choice without looking back
- DP: Considers all possibilities

---

## Classic Greedy Problems

### 1. Activity Selection

```java
// Select maximum non-overlapping activities - O(n log n) time
class Activity {
    int start, end;
    Activity(int start, int end) {
        this.start = start;
        this.end = end;
    }
}

public int maxActivities(Activity[] activities) {
    // Sort by end time
    Arrays.sort(activities, (a, b) -> a.end - b.end);
    
    int count = 1;
    int lastEnd = activities[0].end;
    
    for (int i = 1; i < activities.length; i++) {
        if (activities[i].start >= lastEnd) {
            count++;
            lastEnd = activities[i].end;
        }
    }
    
    return count;
}
```

### 2. Fractional Knapsack

```java
// Maximize value with fractional items - O(n log n) time
class Item {
    int value, weight;
    Item(int value, int weight) {
        this.value = value;
        this.weight = weight;
    }
}

public double fractionalKnapsack(Item[] items, int capacity) {
    // Sort by value/weight ratio
    Arrays.sort(items, (a, b) -> 
        Double.compare((double)b.value/b.weight, (double)a.value/a.weight));
    
    double totalValue = 0;
    int remaining = capacity;
    
    for (Item item : items) {
        if (remaining >= item.weight) {
            // Take whole item
            totalValue += item.value;
            remaining -= item.weight;
        } else {
            // Take fraction
            totalValue += item.value * ((double)remaining / item.weight);
            break;
        }
    }
    
    return totalValue;
}
```

### 3. Huffman Coding

```java
// Build Huffman tree for compression - O(n log n) time
class HuffmanNode implements Comparable<HuffmanNode> {
    char ch;
    int freq;
    HuffmanNode left, right;
    
    public int compareTo(HuffmanNode other) {
        return this.freq - other.freq;
    }
}

public HuffmanNode buildHuffmanTree(char[] chars, int[] freq) {
    PriorityQueue<HuffmanNode> pq = new PriorityQueue<>();
    
    // Create leaf nodes
    for (int i = 0; i < chars.length; i++) {
        HuffmanNode node = new HuffmanNode();
        node.ch = chars[i];
        node.freq = freq[i];
        pq.offer(node);
    }
    
    // Build tree
    while (pq.size() > 1) {
        HuffmanNode left = pq.poll();
        HuffmanNode right = pq.poll();
        
        HuffmanNode parent = new HuffmanNode();
        parent.freq = left.freq + right.freq;
        parent.left = left;
        parent.right = right;
        
        pq.offer(parent);
    }
    
    return pq.poll();
}
```

---

## Interval Problems

### 1. Merge Intervals

```java
// Merge overlapping intervals - O(n log n) time
public int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) return intervals;
    
    // Sort by start time
    Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
    
    List<int[]> result = new ArrayList<>();
    int[] current = intervals[0];
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] <= current[1]) {
            // Overlap, merge
            current[1] = Math.max(current[1], intervals[i][1]);
        } else {
            // No overlap, add current and start new
            result.add(current);
            current = intervals[i];
        }
    }
    
    result.add(current);
    return result.toArray(new int[result.size()][]);
}
```

### 2. Non-overlapping Intervals

```java
// Minimum removals to make non-overlapping - O(n log n) time
public int eraseOverlapIntervals(int[][] intervals) {
    if (intervals.length == 0) return 0;
    
    // Sort by end time
    Arrays.sort(intervals, (a, b) -> a[1] - b[1]);
    
    int count = 0;
    int end = intervals[0][1];
    
    for (int i = 1; i < intervals.length; i++) {
        if (intervals[i][0] < end) {
            count++;  // Remove current interval
        } else {
            end = intervals[i][1];
        }
    }
    
    return count;
}
```

### 3. Minimum Arrows to Burst Balloons

```java
// Minimum arrows to burst all balloons - O(n log n) time
public int findMinArrowShots(int[][] points) {
    if (points.length == 0) return 0;
    
    // Sort by end point
    Arrays.sort(points, (a, b) -> Integer.compare(a[1], b[1]));
    
    int arrows = 1;
    int end = points[0][1];
    
    for (int i = 1; i < points.length; i++) {
        if (points[i][0] > end) {
            // Need new arrow
            arrows++;
            end = points[i][1];
        }
    }
    
    return arrows;
}
```

---

## Optimization Problems

### 1. Gas Station

```java
// Find starting gas station for circular tour - O(n) time
public int canCompleteCircuit(int[] gas, int[] cost) {
    int totalGas = 0;
    int currentGas = 0;
    int start = 0;
    
    for (int i = 0; i < gas.length; i++) {
        int diff = gas[i] - cost[i];
        totalGas += diff;
        currentGas += diff;
        
        if (currentGas < 0) {
            // Can't start from start to i, try from i+1
            start = i + 1;
            currentGas = 0;
        }
    }
    
    return totalGas >= 0 ? start : -1;
}
```

### 2. Assign Cookies

```java
// Assign cookies to maximize content children - O(n log n) time
public int findContentChildren(int[] g, int[] s) {
    Arrays.sort(g);  // Greed factor
    Arrays.sort(s);  // Cookie size
    
    int child = 0;
    int cookie = 0;
    
    while (child < g.length && cookie < s.length) {
        if (s[cookie] >= g[child]) {
            child++;
        }
        cookie++;
    }
    
    return child;
}
```

### 3. Jump Game II

```java
// Minimum jumps to reach end - O(n) time, O(1) space
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

### 4. Partition Labels

```java
// Partition string into maximum parts - O(n) time
public List<Integer> partitionLabels(String s) {
    // Find last occurrence of each character
    int[] last = new int[26];
    for (int i = 0; i < s.length(); i++) {
        last[s.charAt(i) - 'a'] = i;
    }
    
    List<Integer> result = new ArrayList<>();
    int start = 0;
    int end = 0;
    
    for (int i = 0; i < s.length(); i++) {
        end = Math.max(end, last[s.charAt(i) - 'a']);
        
        if (i == end) {
            result.add(end - start + 1);
            start = i + 1;
        }
    }
    
    return result;
}
```

### 5. Queue Reconstruction by Height

```java
// Reconstruct queue based on height - O(n² ) time
public int[][] reconstructQueue(int[][] people) {
    // Sort by height desc, k asc
    Arrays.sort(people, (a, b) -> 
        a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
    
    List<int[]> result = new ArrayList<>();
    
    for (int[] person : people) {
        result.add(person[1], person);
    }
    
    return result.toArray(new int[result.size()][]);
}
```

---

## Key Takeaways

1. **Greedy**: Make locally optimal choice
2. **Sorting**: Often first step in greedy solutions
3. **Proof**: Must prove greedy choice leads to optimal solution
4. **Not Always Optimal**: Greedy doesn't work for all problems
5. **Intervals**: Common pattern in greedy problems

**Interview Tip**: Consider greedy when problem involves optimization with constraints!

