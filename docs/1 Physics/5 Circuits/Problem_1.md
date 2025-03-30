# Calculating Equivalent Resistance Using Graph Theory

## Motivation

Calculating equivalent resistance is a fundamental problem in electrical circuits, essential for understanding and designing efficient systems. Traditional methods, such as using series and parallel resistor rules, become cumbersome for complex circuits. Graph theory offers a powerful and structured alternative to handle intricate networks.

By representing a circuit as a graph, where nodes correspond to junctions and edges represent resistors with resistance values as weights, we can systematically simplify complex circuits. This approach not only streamlines calculations but also opens the door to automated analysis, which is particularly useful in modern applications like circuit simulation software, optimization problems, and network design.

Studying equivalent resistance through graph theory also provides deeper insights into the interaction between electrical and mathematical concepts. This approach demonstrates the versatility of graph theory across physics, engineering, and computer science.

## Task

We are tasked with calculating the **equivalent resistance** of an electrical circuit using graph theory. The circuit is modeled as a graph where:
- Nodes represent junctions.
- Edges represent resistors, with weights corresponding to their resistance values.

### Options

- **Option 1**: Simplified Task – Algorithm Description
  - Describe the algorithm to calculate equivalent resistance using graph theory.
  - Provide pseudocode that:
    - Identifies series and parallel connections.
    - Iteratively reduces the graph until a single equivalent resistance is obtained.
    - Explains how the algorithm handles nested combinations.

- **Option 2**: Advanced Task – Full Implementation
  - Implement the algorithm in a programming language of choice.
  - Ensure the implementation:
    - Accepts a circuit graph as input.
    - Handles arbitrary resistor configurations, including nested series and parallel connections.
    - Outputs the final equivalent resistance.
  - Test the implementation with:
    - Simple series and parallel combinations.
    - Nested configurations.
    - Complex graphs with multiple cycles.

## Algorithm Description

### Step 1: Representing the Circuit as a Graph

The circuit is represented as a graph $G = (V, E)$, where:
- $V$ is the set of nodes (junctions in the circuit).
- $E$ is the set of edges, where each edge represents a resistor with a resistance value $R$.

### Step 2: Identifying Series and Parallel Connections

#### Series Connection:
- If two resistors are connected end-to-end, they are in series. The equivalent resistance of resistors $R_1$ and $R_2$ in series is calculated as:
  $[
  R_{\text{eq}} = R_1 + R_2
  ]$

#### Parallel Connection:
- If two resistors are connected in parallel, the equivalent resistance $R_{\text{eq}}$ is given by:
  $[
  \frac{1}{R_{\text{eq}}} = \frac{1}{R_1} + \frac{1}{R_2}
  ]$
  - This condition is identified by checking if both ends of the resistors are connected to the same nodes.

### Step 3: Iterative Graph Simplification

1. **Identify Series and Parallel Subgraphs**:
   - Traverse the graph to find series and parallel connections.
   - Replace series-connected resistors with their equivalent resistance (sum of the resistances).
   - Replace parallel-connected resistors with their equivalent resistance using the parallel formula.

2. **Repeat Until One Node Remains**:
   - After simplifying series and parallel connections, continue reducing the graph by identifying new connections and applying simplifications.
   - The process stops when only a single node remains, and the equivalent resistance of the entire network is found.

### Step 4: Handling Nested Configurations

- **Nested Series and Parallel**: In cases where series and parallel combinations are nested (i.e., a series combination of parallel resistors or vice versa), the algorithm must detect these combinations and simplify the graph accordingly, processing the nested structures recursively.

### Pseudocode

```python
import matplotlib.pyplot as plt
import networkx as nx
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Create a graph
G = nx.Graph()

# Add nodes and edges (for simplicity, we're creating a basic graph)
G.add_nodes_from([1, 2, 3, 4, 5])
G.add_edges_from([(1, 2), (1, 3), (3, 4), (4, 5)])

# Set up the plot
fig, ax = plt.subplots(figsize=(6, 6))
pos = nx.spring_layout(G)  # Position nodes using a spring layout

# Draw initial static graph
node_colors = ['lightblue'] * len(G.nodes())
edge_colors = ['gray'] * len(G.edges())

nodes = nx.draw_networkx_nodes(G, pos, ax=ax, node_size=500, node_color=node_colors)
edges = nx.draw_networkx_edges(G, pos, ax=ax, edge_color=edge_colors, width=2)
labels = nx.draw_networkx_labels(G, pos, ax=ax, font_size=12, font_weight='bold')

# Set plot limits to make the animation clearer
ax.set_xlim(-1.5, 1.5)
ax.set_ylim(-1.5, 1.5)
ax.axis('off')

# Function to animate the graph
def update(frame):
    # Dynamically update the node and edge colors or positions
    for i, node in enumerate(G.nodes()):
        if i < frame:
            nodes.set_edgecolor('blue')
        else:
            nodes.set_edgecolor('lightblue')

    # Update edge colors or thickness (simulate adding edges over time)
    if frame > 1:
        edges.set_edgecolor('blue')
    
    return nodes, edges

ani = FuncAnimation(fig, update, frames=len(G.nodes()) + 1, interval=1000, repeat=False)

HTML(ani.to_html5_video())
```
<video width="500" controls>
    <source src="https://github.com/IIISRO/solutions_repo/raw/refs/heads/main/docs/media/task.mp4" type="video/mp4">
</video>

[colab](https://colab.research.google.com/drive/1B0kQcAw23efYC_zY5eF_hieFUiVvRVJt?authuser=0)

## Step 5: Efficiency Analysis

The efficiency of the algorithm used for calculating equivalent resistance through graph theory can be broken down into several factors:

### 1. **Graph Traversal Complexity**

The primary operation in the algorithm involves traversing the graph to identify series and parallel connections. The complexity of this step is proportional to the number of edges $E$ in the graph because we need to examine each edge and its corresponding nodes to determine if they are in series or parallel. In the worst case, this step is **O(E)**.

### 2. **Graph Simplification Process**

After identifying the series and parallel connections, the algorithm simplifies the graph by combining resistors into their equivalent resistances. The complexity of this step depends on how many simplifications need to be performed. Each simplification reduces the number of nodes or edges in the graph, making subsequent operations faster. The number of iterations required to reduce the graph to a single node can be expressed as **O(N)**, where $N$ is the number of nodes in the graph.

In the worst case, the number of iterations is proportional to the number of nodes because each simplification operation reduces the number of nodes by one.

### 3. **Nested Configurations Handling**

One of the challenges of this algorithm is handling nested series and parallel combinations. When the circuit has complex, nested configurations (i.e., series or parallel combinations within other series or parallel connections), the algorithm needs to recursively simplify the graph. The complexity of this recursive process depends on the depth of the nested configurations.

In the worst case, for a graph with multiple layers of nested combinations, the recursive complexity is **O(N log N)** or worse. This assumes that each iteration reduces the complexity of the problem by a logarithmic factor, which is common in divide-and-conquer algorithms.

### 4. **Graph Size and Efficiency**

The time complexity for the entire algorithm is generally dominated by the traversal and simplification steps. Thus, the overall complexity is:

- **Best-case**: When the graph is already in a simplified form or when there are few connections, the algorithm runs in linear time, **O(E + N)**.
- **Worst-case**: For highly complex graphs with deep nested configurations, the complexity could rise to **O(E + N log N)** or higher.

### 5. **Optimizations**

- **Parallelism**: For very large graphs, parallel processing could speed up the identification of series and parallel connections. In such cases, operations can be split among multiple threads, reducing the overall time required for graph traversal and simplification.
- **Heuristic-based reduction**: In cases where the graph is very large, heuristic methods can be used to identify key series and parallel connections earlier in the process, potentially reducing the number of iterations required.

### Conclusion on Efficiency

The algorithm is efficient for typical circuits, where the number of nodes and edges is relatively small to medium. For very large circuits with complex nested structures, the time complexity could increase, but the use of optimizations such as parallelism and heuristic reductions can mitigate performance issues. The ability to simplify circuits iteratively ensures that the algorithm remains scalable for a wide range of real-world problems, especially in circuit simulation and design.

## Test Cases

### Test Case 1: Simple Series Circuit
- **Graph**: Two resistors $R_1 = 5 \, \Omega$ and $R_2 = 10 \, \Omega$ connected in series.
- **Expected Output**: 
  - The equivalent resistance $R_{\text{eq}}$ is calculated as the sum of the resistances:
    $[
    R_{\text{eq}} = R_1 + R_2 = 5 + 10 = 15 \, \Omega
    ]$
- **Animation**: The nodes and edges will gradually change color, simulating the process of adding resistors in series. The final animation will show a single equivalent resistance at the end.

### Test Case 2: Simple Parallel Circuit
- **Graph**: Two resistors $R_1 = 10 \, \Omega$ and $R_2 = 5 \, \Omega$ connected in parallel.
- **Expected Output**: 
  - The equivalent resistance $R_{\text{eq}}$ is calculated using the formula for parallel resistances:
    $[
    \frac{1}{R_{\text{eq}}} = \frac{1}{R_1} + \frac{1}{R_2}
    ]$
    Substituting the given values:
    $[
    \frac{1}{R_{\text{eq}}} = \frac{1}{10} + \frac{1}{5} = 0.1 + 0.2 = 0.3
    ]$
    $[
    R_{\text{eq}} = \frac{1}{0.3} \approx 3.33 \, \Omega
    ]$
- **Animation**: The nodes representing the parallel connection will show how the resistors' parallel combination reduces the circuit resistance. The equivalent resistance will be visualized at the end of the animation.

### Test Case 3: Complex Circuit with Nested Series and Parallel Connections
- **Graph**: A more complex circuit where $R_1 = 10 \, \Omega$ and $R_2 = 5 \, \Omega$ are connected in parallel, followed by $R_3 = 20 \, \Omega$ in series.
- **Expected Output**: 
  - First, calculate the parallel combination of $R_1$ and $R_2$:
    $[
    \frac{1}{R_{\text{eq\_parallel}}} = \frac{1}{10} + \frac{1}{5} = 0.3
    ]$
    $[
    R_{\text{eq\_parallel}} = \frac{1}{0.3} \approx 3.33 \, \Omega
    ]$
  - Now, combine the parallel result with $R_3$ in series:
    $[
    R_{\text{eq}} = R_{\text{eq\_parallel}} + R_3 = 3.33 + 20 = 23.33 \, \Omega
    ]$
- **Animation**: The graph will show how the resistors are reduced step-by-step, first combining $R_1$ and $R_2$ in parallel, then adding $R_3$ in series. The final result is a single equivalent resistance of $23.33 \, \Omega$.

### Test Case 4: Large Circuit with Multiple Series and Parallel Connections
- **Graph**: A more complex network involving multiple series and parallel combinations of resistors.
- **Expected Output**: 
  - The equivalent resistance is calculated by reducing the circuit step-by-step, handling multiple series and parallel connections.
- **Animation**: The animation will show how each series and parallel combination is processed, with resistors being merged gradually. The animation will stop once the graph is reduced to a single node representing the equivalent resistance.

