# Graphs

Non-linear data structure consisting of a collection of vertices (nodes)

- `vertex`: object with zero or more adjacent vertices
- `edge`: connection b/w vertices
- `neighbor`: vertices connected by an edge
- `degrees`: number of edges connected to a vertex

## Directionality

- `Undirected`: Nodes connected, but no direction (no arrows)
  ![undirected graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/UndirectedGraph.PNG)
  - Vertices/Nodes = {a,b,c,d,e,f}
  - Edges = {(a,c),(a,d),(b,c),(b,f),(c,e),(d,e),(e,f)}
- `Directed`: Nodes connected, specific "nexts"
  ![directed graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/DirectedGraph.PNG)
  - Vertices = {a,b,c,d,e,f}
  - Edges = {(a,c),(b,c),(b,f),(c,e),(d,a),(d,e)(e,c)(e,f)}

## Connectedness

- `Complete`: All nodes connected to all other nodes
  ![connected graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/CompleteGraph.PNG)
- `Connected`: All nodes have at least one edge (tree is a specific type)
  ![connected graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/ConnectedGraph.PNG)
- `Disconnected`: Some nodes may not have edges
  ![disconnected graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/DisconnectedGraph.PNG)

### Cyclicality

- `Cycle`: When a node can be traversed an end back at itself
- `Acyclic`: No cycles in graph
  ![acyclic graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/threeAcyclic.png)
- `Cyclic`: At least one cycle in graph
  ![cyclic graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/cyclic.PNG)

## Graph Representation

- `Adjacency Matrix`: Displayed as 2d array (n x n boolean matrix with n vertices)
  ![adjacency matrix](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/AdjMatrix.PNG)
  - Sparse graphs have few connections (mostly 0)
  - Dense graphs have many connections (mostly 1)
- `Adjacency List`: Common representation; display of linked lists
  ![adjacency list](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/AdjList.PNG)

## Weighted Graphs

- `Weight`: Number assigned to edges
  ![weighted graph](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/weightGraph.PNG)
- Weight matrix
  ![weight matrix](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/weightMatrix.PNG)
- Weight list
  ![weight list](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/weightList.PNG)

## Traversals

- Traversals similiar to trees
- Need to track visited nodes, to handle cycles

### Breadth-First

#### Algo

1. Enqueue the declared start node into the Queue.
2. Create a loop that will run while the node still has nodes present.
3. Dequeue the first node from the queue
4. if the Dequeue‘d node has unvisited child nodes, add the unvisited children to visited set and insert them into the queue.

> Island nodes won't be visited
> Only visits nodes connect to root passed in

![breadth first](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/BreadthFirst.PNG)

#### Psueudo

```javascript
ALGORITHM BreadthFirst(vertex)
    DECLARE nodes <-- new List()
    DECLARE breadth <-- new Queue()
    DECLARE visited <-- new Set()

    breadth.Enqueue(vertex)
    visited.Add(vertex)

    while (breadth is not empty)
        DECLARE front <-- breadth.Dequeue()
        nodes.Add(front)

        for each child in front.Children
            if(child is not visited)
                visited.Add(child)
                breadth.Enqueue(child)   

    return nodes;
```

### Depth-First

#### Algo

1. Push the root node into the stack
2. Start a while loop while the stack is not empty
3. Peek at the top node in the stack
4. If the top node has unvisited children, mark the top node as visited, and then Push any unvisited children back into the stack.
5. If the top node does not have any unvisited children, Pop that node off the stack
6. repeat until the stack is empty.

![depth first](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/Depth2.PNG)
![depth first](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/depthTrav3.PNG)
![depth first](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/depthTrav4.PNG)
![depth first](https://codefellows.github.io/common_curriculum/data_structures_and_algorithms/Code_401/class-35/resources/assets/depthTrav5.PNG)

