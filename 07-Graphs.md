# Graphs

## 📖 Table of Contents
- [Graph Basics](#graph-basics)
- [Graph Representations](#graph-representations)
- [Graph Traversal](#graph-traversal)
- [Shortest Path Algorithms](#shortest-path-algorithms)
- [Minimum Spanning Tree](#minimum-spanning-tree)
- [Topological Sort](#topological-sort)
- [Important Problems](#important-problems)

---

## Graph Basics

### What is a Graph?
A Graph is a non-linear data structure consisting of vertices (nodes) connected by edges.

**Components:**
- **Vertex (Node)**: Basic unit
- **Edge**: Connection between two vertices
- **Weight**: Cost/value associated with edge

**Types of Graphs:**

1. **Directed Graph (Digraph)**: Edges have direction (A → B)
2. **Undirected Graph**: Edges have no direction (A - B)
3. **Weighted Graph**: Edges have weights
4. **Unweighted Graph**: All edges have same weight
5. **Cyclic Graph**: Contains at least one cycle
6. **Acyclic Graph**: No cycles (DAG = Directed Acyclic Graph)
7. **Connected Graph**: Path exists between any two vertices
8. **Disconnected Graph**: Some vertices not reachable

**Graph Terminology:**
- **Degree**: Number of edges connected to a vertex
  - Undirected: degree of vertex
  - Directed: in-degree (incoming) + out-degree (outgoing)
- **Path**: Sequence of vertices connected by edges
- **Cycle**: Path that starts and ends at same vertex
- **Connected Component**: Maximal connected subgraph

---

## Graph Representations

### 1. Adjacency Matrix

2D array where `matrix[i][j] = 1` if edge exists from i to j.

```java
public class GraphAdjacencyMatrix {
    private int[][] adjMatrix;
    private int vertices;
    
    // Constructor - O(V²) space
    public GraphAdjacencyMatrix(int vertices) {
        this.vertices = vertices;
        adjMatrix = new int[vertices][vertices];
    }
    
    // Add edge - O(1) time
    public void addEdge(int source, int dest) {
        adjMatrix[source][dest] = 1;
        // For undirected graph:
        // adjMatrix[dest][source] = 1;
    }
    
    // Add weighted edge - O(1) time
    public void addWeightedEdge(int source, int dest, int weight) {
        adjMatrix[source][dest] = weight;
    }
    
    // Remove edge - O(1) time
    public void removeEdge(int source, int dest) {
        adjMatrix[source][dest] = 0;
    }
    
    // Check if edge exists - O(1) time
    public boolean hasEdge(int source, int dest) {
        return adjMatrix[source][dest] != 0;
    }
    
    // Get all neighbors - O(V) time
    public List<Integer> getNeighbors(int vertex) {
        List<Integer> neighbors = new ArrayList<>();
        for (int i = 0; i < vertices; i++) {
            if (adjMatrix[vertex][i] != 0) {
                neighbors.add(i);
            }
        }
        return neighbors;
    }
    
    // Display graph - O(V²) time
    public void display() {
        for (int i = 0; i < vertices; i++) {
            for (int j = 0; j < vertices; j++) {
                System.out.print(adjMatrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

**Pros:**
- O(1) edge lookup
- Simple implementation
- Good for dense graphs

**Cons:**
- O(V²) space (wasteful for sparse graphs)
- O(V) to get all neighbors

### 2. Adjacency List

Array of lists where `adjList[i]` contains neighbors of vertex i.

```java
public class GraphAdjacencyList {
    private List<List<Integer>> adjList;
    private int vertices;
    
    // Constructor - O(V) space
    public GraphAdjacencyList(int vertices) {
        this.vertices = vertices;
        adjList = new ArrayList<>();
        
        for (int i = 0; i < vertices; i++) {
            adjList.add(new ArrayList<>());
        }
    }
    
    // Add edge - O(1) time
    public void addEdge(int source, int dest) {
        adjList.get(source).add(dest);
        // For undirected graph:
        // adjList.get(dest).add(source);
    }
    
    // Remove edge - O(E) time
    public void removeEdge(int source, int dest) {
        adjList.get(source).remove(Integer.valueOf(dest));
    }
    
    // Check if edge exists - O(degree) time
    public boolean hasEdge(int source, int dest) {
        return adjList.get(source).contains(dest);
    }
    
    // Get all neighbors - O(1) time
    public List<Integer> getNeighbors(int vertex) {
        return adjList.get(vertex);
    }
    
    // Display graph - O(V + E) time
    public void display() {
        for (int i = 0; i < vertices; i++) {
            System.out.print(i + " -> ");
            for (int neighbor : adjList.get(i)) {
                System.out.print(neighbor + " ");
            }
            System.out.println();
        }
    }
}
```

**Pros:**
- Space efficient for sparse graphs: O(V + E)
- O(1) to get all neighbors
- Most commonly used

**Cons:**
- O(degree) to check if edge exists

### 3. Edge List

List of all edges (source, destination, weight).

```java
class Edge {
    int source;
    int dest;
    int weight;
    
    Edge(int source, int dest, int weight) {
        this.source = source;
        this.dest = dest;
        this.weight = weight;
    }
}

public class GraphEdgeList {
    private List<Edge> edges;
    private int vertices;
    
    public GraphEdgeList(int vertices) {
        this.vertices = vertices;
        edges = new ArrayList<>();
    }
    
    public void addEdge(int source, int dest, int weight) {
        edges.add(new Edge(source, dest, weight));
    }
    
    public List<Edge> getEdges() {
        return edges;
    }
}
```

---

## Graph Traversal

### 1. Breadth-First Search (BFS)

Explore all vertices at current depth before moving to next depth.

**Use Cases:**
- Shortest path in unweighted graph
- Level order traversal
- Connected components

```java
// BFS using queue - O(V + E) time, O(V) space
public void BFS(int start) {
    boolean[] visited = new boolean[vertices];
    Queue<Integer> queue = new LinkedList<>();
    
    visited[start] = true;
    queue.offer(start);
    
    while (!queue.isEmpty()) {
        int vertex = queue.poll();
        System.out.print(vertex + " ");
        
        // Visit all neighbors
        for (int neighbor : adjList.get(vertex)) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                queue.offer(neighbor);
            }
        }
    }
}

// BFS with level tracking
public void BFSWithLevels(int start) {
    boolean[] visited = new boolean[vertices];
    Queue<Integer> queue = new LinkedList<>();
    
    visited[start] = true;
    queue.offer(start);
    int level = 0;
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        System.out.println("Level " + level + ":");
        
        for (int i = 0; i < size; i++) {
            int vertex = queue.poll();
            System.out.print(vertex + " ");
            
            for (int neighbor : adjList.get(vertex)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
        
        System.out.println();
        level++;
    }
}

// Shortest path in unweighted graph
public int shortestPath(int start, int end) {
    if (start == end) return 0;
    
    boolean[] visited = new boolean[vertices];
    Queue<Integer> queue = new LinkedList<>();
    
    visited[start] = true;
    queue.offer(start);
    int distance = 0;
    
    while (!queue.isEmpty()) {
        int size = queue.size();
        distance++;
        
        for (int i = 0; i < size; i++) {
            int vertex = queue.poll();
            
            for (int neighbor : adjList.get(vertex)) {
                if (neighbor == end) {
                    return distance;
                }
                
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }
    }
    
    return -1;  // No path found
}
```

### 2. Depth-First Search (DFS)

Explore as far as possible along each branch before backtracking.

**Use Cases:**
- Detect cycles
- Topological sorting
- Finding connected components
- Path finding

```java
// DFS recursive - O(V + E) time, O(V) space
public void DFS(int start) {
    boolean[] visited = new boolean[vertices];
    DFSHelper(start, visited);
}

private void DFSHelper(int vertex, boolean[] visited) {
    visited[vertex] = true;
    System.out.print(vertex + " ");
    
    for (int neighbor : adjList.get(vertex)) {
        if (!visited[neighbor]) {
            DFSHelper(neighbor, visited);
        }
    }
}

// DFS iterative using stack - O(V + E) time, O(V) space
public void DFSIterative(int start) {
    boolean[] visited = new boolean[vertices];
    Stack<Integer> stack = new Stack<>();
    
    stack.push(start);
    
    while (!stack.isEmpty()) {
        int vertex = stack.pop();
        
        if (!visited[vertex]) {
            visited[vertex] = true;
            System.out.print(vertex + " ");
            
            // Push neighbors (reverse order for correct traversal)
            List<Integer> neighbors = adjList.get(vertex);
            for (int i = neighbors.size() - 1; i >= 0; i--) {
                if (!visited[neighbors.get(i)]) {
                    stack.push(neighbors.get(i));
                }
            }
        }
    }
}

// Check if path exists between two vertices
public boolean hasPath(int start, int end) {
    if (start == end) return true;
    
    boolean[] visited = new boolean[vertices];
    return hasPathDFS(start, end, visited);
}

private boolean hasPathDFS(int current, int end, boolean[] visited) {
    if (current == end) return true;
    
    visited[current] = true;
    
    for (int neighbor : adjList.get(current)) {
        if (!visited[neighbor]) {
            if (hasPathDFS(neighbor, end, visited)) {
                return true;
            }
        }
    }
    
    return false;
}

// Detect cycle in directed graph
public boolean hasCycle() {
    boolean[] visited = new boolean[vertices];
    boolean[] recStack = new boolean[vertices];
    
    for (int i = 0; i < vertices; i++) {
        if (hasCycleDFS(i, visited, recStack)) {
            return true;
        }
    }
    
    return false;
}

private boolean hasCycleDFS(int vertex, boolean[] visited, boolean[] recStack) {
    if (recStack[vertex]) return true;
    if (visited[vertex]) return false;
    
    visited[vertex] = true;
    recStack[vertex] = true;
    
    for (int neighbor : adjList.get(vertex)) {
        if (hasCycleDFS(neighbor, visited, recStack)) {
            return true;
        }
    }
    
    recStack[vertex] = false;
    return false;
}
```

---

## Shortest Path Algorithms

### 1. Dijkstra's Algorithm

Find shortest paths from source to all vertices in weighted graph (non-negative weights).

```java
// Dijkstra's algorithm - O((V + E) log V) with priority queue
public int[] dijkstra(int source) {
    int[] dist = new int[vertices];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[source] = 0;
    
    // Min heap: {distance, vertex}
    PriorityQueue<int[]> pq = new PriorityQueue<>(
        (a, b) -> a[0] - b[0]
    );
    pq.offer(new int[]{0, source});
    
    boolean[] visited = new boolean[vertices];
    
    while (!pq.isEmpty()) {
        int[] current = pq.poll();
        int currDist = current[0];
        int currVertex = current[1];
        
        if (visited[currVertex]) continue;
        visited[currVertex] = true;
        
        // Relaxation
        for (Edge edge : adjListWeighted.get(currVertex)) {
            int neighbor = edge.dest;
            int newDist = currDist + edge.weight;
            
            if (newDist < dist[neighbor]) {
                dist[neighbor] = newDist;
                pq.offer(new int[]{newDist, neighbor});
            }
        }
    }
    
    return dist;
}

// Dijkstra with path reconstruction
public List<Integer> dijkstraPath(int source, int dest) {
    int[] dist = new int[vertices];
    int[] parent = new int[vertices];
    Arrays.fill(dist, Integer.MAX_VALUE);
    Arrays.fill(parent, -1);
    dist[source] = 0;
    
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
    pq.offer(new int[]{0, source});
    
    boolean[] visited = new boolean[vertices];
    
    while (!pq.isEmpty()) {
        int[] current = pq.poll();
        int currVertex = current[1];
        
        if (currVertex == dest) break;
        if (visited[currVertex]) continue;
        visited[currVertex] = true;
        
        for (Edge edge : adjListWeighted.get(currVertex)) {
            int neighbor = edge.dest;
            int newDist = dist[currVertex] + edge.weight;
            
            if (newDist < dist[neighbor]) {
                dist[neighbor] = newDist;
                parent[neighbor] = currVertex;
                pq.offer(new int[]{newDist, neighbor});
            }
        }
    }
    
    // Reconstruct path
    List<Integer> path = new ArrayList<>();
    for (int v = dest; v != -1; v = parent[v]) {
        path.add(v);
    }
    Collections.reverse(path);
    
    return path.get(0) == source ? path : new ArrayList<>();
}
```

### 2. Bellman-Ford Algorithm

Find shortest paths with negative edge weights (detects negative cycles).

```java
// Bellman-Ford - O(V * E) time
public int[] bellmanFord(int source) {
    int[] dist = new int[vertices];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[source] = 0;
    
    // Relax all edges V-1 times
    for (int i = 0; i < vertices - 1; i++) {
        for (Edge edge : edges) {
            if (dist[edge.source] != Integer.MAX_VALUE &&
                dist[edge.source] + edge.weight < dist[edge.dest]) {
                dist[edge.dest] = dist[edge.source] + edge.weight;
            }
        }
    }
    
    // Check for negative cycle
    for (Edge edge : edges) {
        if (dist[edge.source] != Integer.MAX_VALUE &&
            dist[edge.source] + edge.weight < dist[edge.dest]) {
            System.out.println("Negative cycle detected");
            return null;
        }
    }
    
    return dist;
}
```

### 3. Floyd-Warshall Algorithm

Find shortest paths between all pairs of vertices.

```java
// Floyd-Warshall - O(V³) time, O(V²) space
public int[][] floydWarshall() {
    int[][] dist = new int[vertices][vertices];
    
    // Initialize distances
    for (int i = 0; i < vertices; i++) {
        for (int j = 0; j < vertices; j++) {
            if (i == j) {
                dist[i][j] = 0;
            } else if (adjMatrix[i][j] != 0) {
                dist[i][j] = adjMatrix[i][j];
            } else {
                dist[i][j] = Integer.MAX_VALUE / 2;
            }
        }
    }
    
    // Try all intermediate vertices
    for (int k = 0; k < vertices; k++) {
        for (int i = 0; i < vertices; i++) {
            for (int j = 0; j < vertices; j++) {
                if (dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                }
            }
        }
    }
    
    return dist;
}
```

---

## Minimum Spanning Tree

### 1. Kruskal's Algorithm

```java
// Kruskal's algorithm - O(E log E) time
class UnionFind {
    int[] parent, rank;
    
    UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // Path compression
        }
        return parent[x];
    }
    
    boolean union(int x, int y) {
        int rootX = find(x);
        int rootY = find(y);
        
        if (rootX == rootY) return false;
        
        // Union by rank
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        
        return true;
    }
}

public List<Edge> kruskalMST() {
    List<Edge> mst = new ArrayList<>();
    
    // Sort edges by weight
    Collections.sort(edges, (a, b) -> a.weight - b.weight);
    
    UnionFind uf = new UnionFind(vertices);
    
    for (Edge edge : edges) {
        // Add edge if it doesn't form cycle
        if (uf.union(edge.source, edge.dest)) {
            mst.add(edge);
            
            // MST complete when V-1 edges added
            if (mst.size() == vertices - 1) {
                break;
            }
        }
    }
    
    return mst;
}
```

### 2. Prim's Algorithm

```java
// Prim's algorithm - O((V + E) log V) time
public List<Edge> primMST() {
    List<Edge> mst = new ArrayList<>();
    boolean[] inMST = new boolean[vertices];
    
    // Min heap: {weight, vertex, parent}
    PriorityQueue<int[]> pq = new PriorityQueue<>(
        (a, b) -> a[0] - b[0]
    );
    
    // Start from vertex 0
    pq.offer(new int[]{0, 0, -1});
    
    while (!pq.isEmpty() && mst.size() < vertices - 1) {
        int[] current = pq.poll();
        int weight = current[0];
        int vertex = current[1];
        int parent = current[2];
        
        if (inMST[vertex]) continue;
        
        inMST[vertex] = true;
        
        if (parent != -1) {
            mst.add(new Edge(parent, vertex, weight));
        }
        
        // Add all edges from current vertex
        for (Edge edge : adjListWeighted.get(vertex)) {
            if (!inMST[edge.dest]) {
                pq.offer(new int[]{edge.weight, edge.dest, vertex});
            }
        }
    }
    
    return mst;
}
```

---

## Topological Sort

### 1. DFS-based Topological Sort

```java
// Topological sort using DFS - O(V + E) time
public List<Integer> topologicalSort() {
    boolean[] visited = new boolean[vertices];
    Stack<Integer> stack = new Stack<>();
    
    for (int i = 0; i < vertices; i++) {
        if (!visited[i]) {
            topSortDFS(i, visited, stack);
        }
    }
    
    List<Integer> result = new ArrayList<>();
    while (!stack.isEmpty()) {
        result.add(stack.pop());
    }
    
    return result;
}

private void topSortDFS(int vertex, boolean[] visited, Stack<Integer> stack) {
    visited[vertex] = true;
    
    for (int neighbor : adjList.get(vertex)) {
        if (!visited[neighbor]) {
            topSortDFS(neighbor, visited, stack);
        }
    }
    
    stack.push(vertex);
}
```

### 2. Kahn's Algorithm (BFS-based)

```java
// Kahn's algorithm - O(V + E) time
public List<Integer> topologicalSortKahn() {
    int[] inDegree = new int[vertices];
    
    // Calculate in-degrees
    for (int i = 0; i < vertices; i++) {
        for (int neighbor : adjList.get(i)) {
            inDegree[neighbor]++;
        }
    }
    
    // Add vertices with in-degree 0
    Queue<Integer> queue = new LinkedList<>();
    for (int i = 0; i < vertices; i++) {
        if (inDegree[i] == 0) {
            queue.offer(i);
        }
    }
    
    List<Integer> result = new ArrayList<>();
    
    while (!queue.isEmpty()) {
        int vertex = queue.poll();
        result.add(vertex);
        
        // Reduce in-degree of neighbors
        for (int neighbor : adjList.get(vertex)) {
            inDegree[neighbor]--;
            if (inDegree[neighbor] == 0) {
                queue.offer(neighbor);
            }
        }
    }
    
    // Check for cycle
    if (result.size() != vertices) {
        System.out.println("Cycle detected");
        return new ArrayList<>();
    }
    
    return result;
}
```

---

## Important Problems

### 1. Number of Islands

```java
// Count islands in 2D grid - O(m * n) time
public int numIslands(char[][] grid) {
    if (grid == null || grid.length == 0) return 0;
    
    int count = 0;
    int m = grid.length;
    int n = grid[0].length;
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                count++;
                dfsIsland(grid, i, j);
            }
        }
    }
    
    return count;
}

private void dfsIsland(char[][] grid, int i, int j) {
    if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length ||
        grid[i][j] == '0') {
        return;
    }
    
    grid[i][j] = '0';  // Mark as visited
    
    // Visit all 4 directions
    dfsIsland(grid, i + 1, j);
    dfsIsland(grid, i - 1, j);
    dfsIsland(grid, i, j + 1);
    dfsIsland(grid, i, j - 1);
}
```

### 2. Clone Graph

```java
// Clone undirected graph - O(V + E) time
public Node cloneGraph(Node node) {
    if (node == null) return null;
    
    Map<Node, Node> map = new HashMap<>();
    return cloneDFS(node, map);
}

private Node cloneDFS(Node node, Map<Node, Node> map) {
    if (map.containsKey(node)) {
        return map.get(node);
    }
    
    Node clone = new Node(node.val);
    map.put(node, clone);
    
    for (Node neighbor : node.neighbors) {
        clone.neighbors.add(cloneDFS(neighbor, map));
    }
    
    return clone;
}
```

### 3. Course Schedule (Detect Cycle)

```java
// Check if can finish all courses - O(V + E) time
public boolean canFinish(int numCourses, int[][] prerequisites) {
    List<List<Integer>> graph = new ArrayList<>();
    for (int i = 0; i < numCourses; i++) {
        graph.add(new ArrayList<>());
    }
    
    for (int[] prereq : prerequisites) {
        graph.get(prereq[1]).add(prereq[0]);
    }
    
    int[] state = new int[numCourses];  // 0=unvisited, 1=visiting, 2=visited
    
    for (int i = 0; i < numCourses; i++) {
        if (hasCycleDFS(i, graph, state)) {
            return false;
        }
    }
    
    return true;
}

private boolean hasCycleDFS(int course, List<List<Integer>> graph, int[] state) {
    if (state[course] == 1) return true;   // Cycle detected
    if (state[course] == 2) return false;  // Already visited
    
    state[course] = 1;  // Mark as visiting
    
    for (int next : graph.get(course)) {
        if (hasCycleDFS(next, graph, state)) {
            return true;
        }
    }
    
    state[course] = 2;  // Mark as visited
    return false;
}
```

### 4. Network Delay Time (Dijkstra)

```java
// Find time for signal to reach all nodes - O((V + E) log V) time
public int networkDelayTime(int[][] times, int n, int k) {
    // Build graph
    List<List<int[]>> graph = new ArrayList<>();
    for (int i = 0; i <= n; i++) {
        graph.add(new ArrayList<>());
    }
    
    for (int[] time : times) {
        graph.get(time[0]).add(new int[]{time[1], time[2]});
    }
    
    // Dijkstra
    int[] dist = new int[n + 1];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[k] = 0;
    
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{k, 0});
    
    while (!pq.isEmpty()) {
        int[] current = pq.poll();
        int node = current[0];
        int time = current[1];
        
        if (time > dist[node]) continue;
        
        for (int[] next : graph.get(node)) {
            int nextNode = next[0];
            int newTime = time + next[1];
            
            if (newTime < dist[nextNode]) {
                dist[nextNode] = newTime;
                pq.offer(new int[]{nextNode, newTime});
            }
        }
    }
    
    int maxTime = 0;
    for (int i = 1; i <= n; i++) {
        if (dist[i] == Integer.MAX_VALUE) return -1;
        maxTime = Math.max(maxTime, dist[i]);
    }
    
    return maxTime;
}
```

---

## Key Takeaways

1. **BFS**: Shortest path, level order
2. **DFS**: Cycle detection, topological sort, path finding
3. **Dijkstra**: Shortest path with non-negative weights
4. **Bellman-Ford**: Handles negative weights
5. **Union-Find**: Efficient for connectivity problems

**Interview Tip**: Draw the graph first, then decide BFS vs DFS!

