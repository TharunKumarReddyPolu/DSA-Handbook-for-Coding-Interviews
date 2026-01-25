# Graph Data Structure and Algorithms 🌐

## Introduction
A graph is a non-linear data structure consisting of vertices (nodes) and edges that connect these vertices. Graphs are used to represent networks, relationships, and various real-world problems from social networks to computer networks.

## Prerequisites & Related Topics

### Prerequisites
- [Arrays](arrays.md) (for adjacency list/matrix representation)
- [Queue](queue.md) (essential for BFS traversal)
- [Stack](stack.md) (for iterative DFS)
- [Recursion](recursion.md) (for recursive DFS)
- [Hashing](hashing.md) (for visited set, adjacency map)

### Related Topics
- **Builds on**: [Tree](tree.md) (trees are special graphs), [Linked List](linked-list.md) (adjacency lists)
- **Algorithms use**: [Heap](heap-pq.md) (Dijkstra's), [Dynamic Programming](dynamic-programming.md) (shortest paths)
- **Traversal relates to**: [Backtracking](backtracking.md) (graph exploration)
- **See also**: [Matrix](matrix.md) (grid-based graph problems)

## Pattern Recognition Guide

### 🎯 When to Use Graph Algorithms
**Keywords in problem**: "connected", "path", "network", "nodes/vertices", "edges", "reachable"

**Use Graphs when you see**:
- Entities with relationships/connections
- Network or map traversal problems
- Dependency relationships (prerequisites)
- Finding shortest/longest paths
- Detecting cycles or connectivity

### 🔑 Problem Indicators
| Pattern | Keywords/Clues | Example Problems |
|---------|---------------|------------------|
| BFS | "shortest path (unweighted)", "level order", "minimum steps" | Word Ladder, Rotting Oranges |
| DFS | "all paths", "connected components", "explore all" | Number of Islands, Clone Graph |
| Topological Sort | "prerequisites", "ordering", "dependencies", "schedule" | Course Schedule, Alien Dictionary |
| Union-Find | "connected components", "groups", "union", "redundant" | Number of Provinces, Redundant Connection |
| Dijkstra | "shortest path (weighted)", "minimum cost", "cheapest" | Network Delay Time, Cheapest Flights |
| Cycle Detection | "detect cycle", "circular dependency", "valid tree" | Course Schedule, Graph Valid Tree |

### ❌ When NOT to Use
- Data is strictly hierarchical with single parent → use [Tree](tree.md)
- Simple sequential processing → use [Arrays](arrays.md)
- Grid without complex connectivity → may use simpler [Matrix](matrix.md) traversal
- Problem doesn't involve relationships between entities

## Core Concepts

### Important Terminologies
- **Vertex/Node**: Basic unit of a graph
- **Edge**: Connection between two vertices
- **Directed Graph**: Edges have direction
- **Undirected Graph**: Edges are bidirectional
- **Weighted Graph**: Edges have weights/costs
- **Path**: Sequence of vertices connected by edges
- **Cycle**: Path that starts and ends at same vertex
- **DAG**: Directed Acyclic Graph (no cycles)
- **Connected Component**: Subset of connected vertices
- **Degree**: Number of edges connected to a vertex
- **Tree**: Connected graph with no cycles
- **Bipartite Graph**: Vertices can be split into two sets
- **Strongly Connected**: Every vertex reachable from every other
- **Adjacency**: Direct connection between vertices

### Graph Representations
1. **Adjacency Matrix**:
   - 2D array where matrix[i][j] represents edge from i to j
   - Space: O(V²)
   - Good for dense graphs

2. **Adjacency List**:
   - Array of lists where list[i] contains vertices adjacent to i
   - Space: O(V + E)
   - Good for sparse graphs

3. **Edge List**:
   - List of all edges in graph
   - Space: O(E)
   - Good for algorithms that process all edges

### Time Complexity Analysis
| Operation           | Adjacency List | Adjacency Matrix |
|--------------------|----------------|------------------|
| Add Vertex         | O(1)           | O(V²)            |
| Add Edge           | O(1)           | O(1)             |
| Remove Vertex      | O(V + E)       | O(V²)            |
| Remove Edge        | O(E)           | O(1)             |
| Query Edge         | O(V)           | O(1)             |
| Find Neighbors     | O(V)           | O(V)             |
| Storage Space      | O(V + E)       | O(V²)            |

## Implementation

### Basic Graph using Adjacency List

**Pseudocode:**
```
Graph structure:
- Use dictionary/map where key=vertex, value=list of neighbors
- add_vertex(v): if v not in graph, create empty list for v
- add_edge(v1, v2): add v2 to v1's list; if undirected, add v1 to v2's list
- get_vertices(): return all keys
- get_edges(): collect all (v, neighbor) pairs from adjacency lists
```

```python
class Graph:
    def __init__(self, directed=False):
        self.graph = {}
        self.directed = directed
    
    def add_vertex(self, vertex):
        if vertex not in self.graph:
            self.graph[vertex] = []
    
    def add_edge(self, v1, v2):
        if v1 not in self.graph:
            self.add_vertex(v1)
        if v2 not in self.graph:
            self.add_vertex(v2)
        
        self.graph[v1].append(v2)
        if not self.directed:
            self.graph[v2].append(v1)
    
    def get_vertices(self):
        return list(self.graph.keys())
    
    def get_edges(self):
        edges = []
        for vertex in self.graph:
            for neighbor in self.graph[vertex]:
                if not self.directed and (neighbor, vertex) not in edges:
                    edges.append((vertex, neighbor))
                else:
                    edges.append((vertex, neighbor))
        return edges
```

```java
class Graph {
    Map<Integer, List<Integer>> graph = new HashMap<>();
    boolean directed;
    
    Graph(boolean directed) { this.directed = directed; }
    
    void addVertex(int v) { graph.putIfAbsent(v, new ArrayList<>()); }
    
    void addEdge(int v1, int v2) {
        addVertex(v1); addVertex(v2);
        graph.get(v1).add(v2);
        if (!directed) graph.get(v2).add(v1);
    }
    
    Set<Integer> getVertices() { return graph.keySet(); }
    
    List<int[]> getEdges() {
        List<int[]> edges = new ArrayList<>();
        Set<String> seen = new HashSet<>();
        for (int v : graph.keySet()) {
            for (int neighbor : graph.get(v)) {
                String key = directed ? v + "-" + neighbor : Math.min(v, neighbor) + "-" + Math.max(v, neighbor);
                if (!seen.contains(key)) { edges.add(new int[]{v, neighbor}); seen.add(key); }
            }
        }
        return edges;
    }
}
```

```cpp
class Graph {
    unordered_map<int, vector<int>> graph;
    bool directed;
public:
    Graph(bool directed = false) : directed(directed) {}
    
    void addVertex(int v) { if (!graph.count(v)) graph[v] = {}; }
    
    void addEdge(int v1, int v2) {
        addVertex(v1); addVertex(v2);
        graph[v1].push_back(v2);
        if (!directed) graph[v2].push_back(v1);
    }
    
    vector<int> getVertices() {
        vector<int> vertices;
        for (auto& [v, _] : graph) vertices.push_back(v);
        return vertices;
    }
    
    vector<pair<int, int>> getEdges() {
        vector<pair<int, int>> edges;
        set<pair<int, int>> seen;
        for (auto& [v, neighbors] : graph) {
            for (int neighbor : neighbors) {
                auto key = directed ? make_pair(v, neighbor) : make_pair(min(v, neighbor), max(v, neighbor));
                if (!seen.count(key)) { edges.push_back({v, neighbor}); seen.insert(key); }
            }
        }
        return edges;
    }
};
```

### Weighted Graph Implementation

**Pseudocode:**
```
Weighted Graph structure:
- Use nested dictionary: graph[v1][v2] = weight
- add_edge(v1, v2, weight): graph[v1][v2] = weight
- For undirected: also set graph[v2][v1] = weight
- get_weight(v1, v2): return graph[v1][v2] or infinity if not exists
```

```python
class WeightedGraph:
    def __init__(self, directed=False):
        self.graph = {}
        self.directed = directed
    
    def add_vertex(self, vertex):
        if vertex not in self.graph:
            self.graph[vertex] = {}
    
    def add_edge(self, v1, v2, weight):
        if v1 not in self.graph:
            self.add_vertex(v1)
        if v2 not in self.graph:
            self.add_vertex(v2)
        
        self.graph[v1][v2] = weight
        if not self.directed:
            self.graph[v2][v1] = weight
```

```java
class WeightedGraph {
    Map<Integer, Map<Integer, Integer>> graph = new HashMap<>();
    boolean directed;
    
    WeightedGraph(boolean directed) { this.directed = directed; }
    
    void addVertex(int v) { graph.putIfAbsent(v, new HashMap<>()); }
    
    void addEdge(int v1, int v2, int weight) {
        addVertex(v1); addVertex(v2);
        graph.get(v1).put(v2, weight);
        if (!directed) graph.get(v2).put(v1, weight);
    }
    
    int getWeight(int v1, int v2) {
        return graph.getOrDefault(v1, new HashMap<>()).getOrDefault(v2, Integer.MAX_VALUE);
    }
}
```

```cpp
class WeightedGraph {
    unordered_map<int, unordered_map<int, int>> graph;
    bool directed;
public:
    WeightedGraph(bool directed = false) : directed(directed) {}
    
    void addVertex(int v) { if (!graph.count(v)) graph[v] = {}; }
    
    void addEdge(int v1, int v2, int weight) {
        addVertex(v1); addVertex(v2);
        graph[v1][v2] = weight;
        if (!directed) graph[v2][v1] = weight;
    }
    
    int getWeight(int v1, int v2) {
        if (graph.count(v1) && graph[v1].count(v2)) return graph[v1][v2];
        return INT_MAX;
    }
};
```

## Common Algorithms

### 1. Depth-First Search (DFS)

**Pseudocode:**
```
1. Mark start vertex as visited
2. Process start vertex (print, add to result, etc.)
3. For each neighbor of start:
   a. If neighbor not visited:
      - Recursively call DFS on neighbor
```

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    
    visited.add(start)
    print(start, end=' ')
    
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

```java
public void dfs(Map<Integer, List<Integer>> graph, int start, Set<Integer> visited) {
    visited.add(start);
    System.out.print(start + " ");
    for (int neighbor : graph.getOrDefault(start, new ArrayList<>())) {
        if (!visited.contains(neighbor)) {
            dfs(graph, neighbor, visited);
        }
    }
}
```

```cpp
void dfs(unordered_map<int, vector<int>>& graph, int start, unordered_set<int>& visited) {
    visited.insert(start);
    cout << start << " ";
    for (int neighbor : graph[start]) {
        if (visited.find(neighbor) == visited.end()) {
            dfs(graph, neighbor, visited);
        }
    }
}
```

### 2. Breadth-First Search (BFS)

**Pseudocode:**
```
1. Create visited set, add start to it
2. Create queue, enqueue start
3. While queue not empty:
   a. Dequeue vertex, process it
   b. For each neighbor of vertex:
      - If not visited: mark visited, enqueue neighbor
```

```python
from collections import deque

def bfs(graph, start):
    visited = set([start])
    queue = deque([start])
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=' ')
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

```java
public void bfs(Map<Integer, List<Integer>> graph, int start) {
    Set<Integer> visited = new HashSet<>();
    Queue<Integer> queue = new LinkedList<>();
    visited.add(start);
    queue.offer(start);
    while (!queue.isEmpty()) {
        int vertex = queue.poll();
        System.out.print(vertex + " ");
        for (int neighbor : graph.getOrDefault(vertex, new ArrayList<>())) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.offer(neighbor);
            }
        }
    }
}
```

```cpp
void bfs(unordered_map<int, vector<int>>& graph, int start) {
    unordered_set<int> visited;
    queue<int> q;
    visited.insert(start);
    q.push(start);
    while (!q.empty()) {
        int vertex = q.front(); q.pop();
        cout << vertex << " ";
        for (int neighbor : graph[vertex]) {
            if (visited.find(neighbor) == visited.end()) {
                visited.insert(neighbor);
                q.push(neighbor);
            }
        }
    }
}
```

### 3. Dijkstra's Shortest Path

**Pseudocode:**
```
1. Initialize distances: all vertices = infinity, start = 0
2. Create min-heap priority queue with (0, start)
3. While priority queue not empty:
   a. Pop (current_dist, current_vertex)
   b. If current_dist > distances[current_vertex]: skip (stale entry)
   c. For each neighbor with edge weight:
      - new_dist = current_dist + weight
      - If new_dist < distances[neighbor]:
        Update distances[neighbor], push (new_dist, neighbor) to queue
4. Return distances array
```

```python
import heapq

def dijkstra(graph, start):
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        current_distance, current_vertex = heapq.heappop(pq)
        
        if current_distance > distances[current_vertex]:
            continue
        
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances
```

```java
public int[] dijkstra(int[][] graph, int start) {
    int n = graph.length;
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{start, 0});
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int u = curr[0], d = curr[1];
        if (d > dist[u]) continue;
        for (int v = 0; v < n; v++) {
            if (graph[u][v] > 0 && dist[u] + graph[u][v] < dist[v]) {
                dist[v] = dist[u] + graph[u][v];
                pq.offer(new int[]{v, dist[v]});
            }
        }
    }
    return dist;
}
```

```cpp
vector<int> dijkstra(vector<vector<pair<int,int>>>& graph, int start) {
    int n = graph.size();
    vector<int> dist(n, INT_MAX);
    dist[start] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, start});
    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : graph[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

### 4. Topological Sort

**Pseudocode:**
```
1. Create visited set and result stack
2. For each vertex in graph:
   a. If not visited: call DFS on it
3. DFS(vertex):
   a. Mark vertex as visited
   b. For each neighbor: if not visited, DFS(neighbor)
   c. After processing all neighbors: push vertex to stack
4. Return reversed stack (or pop elements one by one)
```

```python
def topological_sort(graph):
    visited = set()
    stack = []
    
    def dfs(vertex):
        visited.add(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                dfs(neighbor)
        
        stack.append(vertex)
    
    for vertex in graph:
        if vertex not in visited:
            dfs(vertex)
    
    return stack[::-1]
```

```java
public List<Integer> topologicalSort(Map<Integer, List<Integer>> graph) {
    Set<Integer> visited = new HashSet<>();
    Stack<Integer> stack = new Stack<>();
    for (int vertex : graph.keySet())
        if (!visited.contains(vertex)) dfs(graph, vertex, visited, stack);
    Collections.reverse(stack);
    return new ArrayList<>(stack);
}
private void dfs(Map<Integer, List<Integer>> graph, int v, Set<Integer> visited, Stack<Integer> stack) {
    visited.add(v);
    for (int neighbor : graph.getOrDefault(v, new ArrayList<>()))
        if (!visited.contains(neighbor)) dfs(graph, neighbor, visited, stack);
    stack.push(v);
}
```

```cpp
vector<int> topologicalSort(unordered_map<int, vector<int>>& graph) {
    unordered_set<int> visited;
    vector<int> result;
    function<void(int)> dfs = [&](int v) {
        visited.insert(v);
        for (int neighbor : graph[v])
            if (!visited.count(neighbor)) dfs(neighbor);
        result.push_back(v);
    };
    for (auto& [vertex, _] : graph)
        if (!visited.count(vertex)) dfs(vertex);
    reverse(result.begin(), result.end());
    return result;
}
```

### 5. Union-Find (Disjoint Set)

**Pseudocode:**
```
Initialize: parent[v] = v for all vertices, rank[v] = 0

Find(x) with path compression:
1. If parent[x] != x: parent[x] = Find(parent[x])
2. Return parent[x]

Union(x, y) with union by rank:
1. rootX = Find(x), rootY = Find(y)
2. If rootX == rootY: return (already same set)
3. Attach smaller rank tree under larger rank tree
4. If ranks equal: increment rank of new root
```

```python
class UnionFind:
    def __init__(self, vertices):
        self.parent = {v: v for v in vertices}
        self.rank = {v: 0 for v in vertices}
    
    def find(self, item):
        if self.parent[item] != item:
            self.parent[item] = self.find(self.parent[item])
        return self.parent[item]
    
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
```

```java
class UnionFind {
    int[] parent, rank;
    UnionFind(int n) {
        parent = new int[n]; rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    void union(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
    }
}
```

```cpp
class UnionFind {
    vector<int> parent, rank;
public:
    UnionFind(int n) : parent(n), rank(n, 0) {
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    void unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
    }
};
```

```python
    # Continuation of union method
    def union(self, x, y):
        rootX = self.find(x)
        rootY = self.find(y)
        
        if rootX != rootY:
            if self.rank[rootX] < self.rank[rootY]:
                rootX, rootY = rootY, rootX
            self.parent[rootY] = rootX
            if self.rank[rootX] == self.rank[rootY]:
                self.rank[rootX] += 1
```

## Common Patterns

### 1. Cycle Detection

**Pseudocode:**
```
For directed graph:
1. Use visited set and recursion_stack set
2. DFS(vertex):
   a. Add to visited and recursion_stack
   b. For each neighbor:
      - If not visited and DFS(neighbor) returns true: return true
      - If neighbor in recursion_stack: return true (back edge = cycle)
   c. Remove from recursion_stack, return false
3. Call DFS for each unvisited vertex
```

```python
def has_cycle(graph):
    visited = set()
    rec_stack = set()
    
    def dfs(vertex):
        visited.add(vertex)
        rec_stack.add(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                if dfs(neighbor):
                    return True
            elif neighbor in rec_stack:
                return True
        
        rec_stack.remove(vertex)
        return False
    
    for vertex in graph:
        if vertex not in visited:
            if dfs(vertex):
                return True
    return False
```

### 2. Connected Components

**Pseudocode:**
```
1. Create visited set and components list
2. For each vertex:
   a. If not visited:
      - Create new component list
      - DFS from vertex, adding each visited node to component
      - Add component to components list
3. Return components list
```

```python
def find_connected_components(graph):
    visited = set()
    components = []
    
    def dfs(vertex, component):
        visited.add(vertex)
        component.append(vertex)
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                dfs(neighbor, component)
    
    for vertex in graph:
        if vertex not in visited:
            component = []
            dfs(vertex, component)
            components.append(component)
    
    return components
```

## Edge Cases to Consider
1. Empty graph
2. Single vertex
3. Disconnected components
4. Self-loops
5. Parallel edges
6. Negative weights
7. Cycles
8. Dense vs sparse graphs
9. Directed vs undirected
10. Bipartite properties

## Common Pitfalls
1. Not handling disconnected components
2. Incorrect cycle detection
3. Stack overflow in DFS
4. Not considering edge weights
5. Assuming undirected graph
6. Memory issues with adjacency matrix
7. Not handling self-loops

## Practice Problems by Difficulty

### Easy
1. [Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/) (LC #997)
2. [Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph/) (LC #1791)
3. [Number of Islands](https://leetcode.com/problems/number-of-islands/) (LC #200)

### Medium
1. [Course Schedule](https://leetcode.com/problems/course-schedule/) (LC #207)
2. [Clone Graph](https://leetcode.com/problems/clone-graph/) (LC #133)
3. [Redundant Connection](https://leetcode.com/problems/redundant-connection/) (LC #684)
4. [Network Delay Time](https://leetcode.com/problems/network-delay-time/) (LC #743)

### Hard
1. [Critical Connections in a Network](https://leetcode.com/problems/critical-connections-in-a-network/) (LC #1192)
2. [Word Ladder](https://leetcode.com/problems/word-ladder/) (LC #127)
3. [Bus Routes](https://leetcode.com/problems/bus-routes/) (LC #815)

## Real-World Applications
1. **Social Networks**: Friend connections
2. **Computer Networks**: Network topology
3. **GPS Navigation**: Road networks
4. **Recommendation Systems**: User-item relationships
5. **Compiler Design**: Control flow analysis
6. **Biology**: Protein interaction networks
7. **Circuit Design**: Electronic circuits

## Advanced Topics
1. **Advanced Graph Algorithms**:
   - A* Search
   - Bellman-Ford
   - Floyd-Warshall
2. **Network Flow**:
   - Ford-Fulkerson
   - Max-Flow Min-Cut
3. **Graph Coloring**:
   - Vertex coloring
   - Edge coloring
4. **Planar Graphs**
5. **Graph Isomorphism**
6. **Spectral Graph Theory**

## Important Resources
1. [Graph Theory Basics](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/)
2. [Network Flow Tutorial](https://www.geeksforgeeks.org/max-flow-problem-introduction/)
3. [Advanced Graph Algorithms](https://cp-algorithms.com/graph/basic-graph.html)
4. [Graph Applications](https://www.geeksforgeeks.org/applications-of-graph-data-structure/)
5. [Graph Visualization Tools](https://graphviz.org/)

## ❓ FAQ Section

**Q: When should I use adjacency list vs adjacency matrix?**
A: Use adjacency list for sparse graphs (E << V²) - it's more space efficient O(V+E). Use adjacency matrix for dense graphs or when you need O(1) edge lookup. Matrix is also better when you frequently check if an edge exists.

**Q: How do I detect a cycle in a graph?**
A: For undirected graphs: DFS with parent tracking or Union-Find. For directed graphs: DFS with three colors (white/gray/black) or check if back edge exists. A back edge points to an ancestor in the DFS tree.

**Q: When should I use Dijkstra vs BFS vs Bellman-Ford?**
A: BFS: unweighted graphs (all edges = 1). Dijkstra: weighted graphs with non-negative edges. Bellman-Ford: graphs with negative edges (also detects negative cycles). For negative edges, Dijkstra fails!

**Q: What's the difference between topological sort and regular DFS?**
A: Topological sort is DFS that adds nodes to result AFTER processing all descendants (post-order). It only works on DAGs (Directed Acyclic Graphs). Regular DFS can work on any graph.

**Q: How do I choose between DFS and BFS for graph traversal?**
A: Use BFS for shortest path in unweighted graphs, level-by-level processing, or when solution is likely near the start. Use DFS for path finding, cycle detection, topological sort, or when you need to explore as far as possible before backtracking.

## Interview Tips
1. Clarify graph properties
2. Choose appropriate representation
3. Consider time/space complexity
4. Handle edge cases explicitly
5. Draw examples
6. Consider optimization opportunities
7. Test with small graphs first
8. Discuss trade-offs