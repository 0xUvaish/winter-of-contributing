## REVERSE DELETE ALGORITHM FOR MINIMUM SPANNING TREE

The reverse-delete algorithm is an algorithm in graph theory used to obtain a minimum spanning tree from a given connected, edge-weighted graph.

In this Algorithm we study about the graph in which we can identify how reverse delete algorithm works to obtain a minimum spanning tree from a given connected, edge-weighted graph.Reverse delete algorithm is opposite to kruskal algorithm. In kruskal algorithm we solve the graph in increasing order and in reverse delete algorithm we solve the graph in decreasing order.Kruskal’s algorithm starts with an empty graph and adds edges while the Reverse-Delete algorithm starts with the original graph and deletes edges from it. If the graph is disconnected, this algorithm will find a minimum spanning tree for each disconnected part of the graph.This algorithm is a greedy algorithm, choosing the best choice given any situation.

The algorithm works as follows:<br>
 ***Start with a full graph, (V,E) and begin deleting edges in order of decreasing cost as long as deleting it does not disconnect the graph.***

### :arrow_right:Algorithm:-
```
Initially E is the set of all edges in G(Graph)
T is E (* T will store edges of a MST *)
  while E is not empty do
    choose i ∈ E of largest cost
    if removing i does not disconnect T then
        remove i from T
return the set T
```
<hr></hr>

### :arrow_right:Example:-<br>
Consider the below graph:<br>
![Graph](https://user-images.githubusercontent.com/72224843/141634707-d4fc0d62-c91b-4573-bf58-33cfd8a38c24.png)
&nbsp;&nbsp; &nbsp;
![download-removebg-preview](https://user-images.githubusercontent.com/72224843/141615338-5cf5d86e-403d-483b-b8cc-6de93df6b9f6.png)&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp; &nbsp;
![MST-1](https://user-images.githubusercontent.com/72224843/141635036-8d3868a7-da41-4844-bf33-5fa9cf566fca.png)
<br>

**Step 1:-** Delete the edge 3-4 which does not disconnect the graph, so the edge can be removed.<br>
![Reverse-Graph-1](https://user-images.githubusercontent.com/72224843/141635177-32b9bf7c-287e-46e5-999a-da194fca0059.png)
<br>
**Step 2:-** Next select the edge 5-6 with weight 11. Since, deleting the edge 5-6 does not disconnect the graph, so the edge can be removed.</br>
![Reverse-Graph-2](https://user-images.githubusercontent.com/72224843/141635397-7e1066c9-0707-4bea-8821-750822e02627.png)
<br>
**Step 3:-** Delete the edge 1-3 which does not disconnect the graph, so the edge can be removed.<br>
![Reverse-Graph-3](https://user-images.githubusercontent.com/72224843/141635966-ecb907d8-477f-4440-a0ac-465d923969a3.png)
<br>
**Step 4:-** Next select the edge 4-6 with weight 9. Deleting this edge will result in the graph being disconnected, as Node 6 gets seperated. So, we do not delete the edge.<br>
![Reverse-Graph-4](https://user-images.githubusercontent.com/72224843/141636129-9b00744c-849a-4a87-96a7-458fd257cacb.png)
<br>
**Step 5:-** Next select the edge 1-2 with weight 8. Since, deleting the edge 1-2 does not disconnect the graph, so the edge can be removed.</br>
![Reverse-Graph-5](https://user-images.githubusercontent.com/72224843/141636310-753d5f0c-ead0-40f0-b762-c37bfb19eef3.png)
</br>
**Step 6:-** Next select the edge 4-5 with weight 8. Since, deleting the edge 4-5 does not disconnect the graph, so the edge can be removed.</br>
![MST-1](https://user-images.githubusercontent.com/72224843/141636481-238da6d9-5010-43e4-a6a3-4a3d9e224ebf.png)
<br>
***Minimum Spanning Tree(final graph returned by the algorithm)***<br>
<hr></hr>

### :arrow_right:Implementation of Reverse Delete Algorithm (C++ Code):-<br>
```
#include <bits/stdc++.h>
using namespace std;

// Adding u to v nodes to the graph and weight between them
void AddWeightedEdge(unordered_map<int, unordered_set<int>> &graph, vector<pair<int, pair<int, int>>> &edges, int u, int v, int w)
{
    graph[u].insert(v);
    graph[v].insert(u);
    edges.push_back({w, {u, v}});
}

// DFS on the node
void DFS(unordered_map<int, unordered_set<int>> &graph, vector<bool> &visited, int node)
{
    visited[node] = 1;
    for (auto neighbour : graph[node])
    {
        if (!visited[neighbour])
        {
            DFS(graph, visited, neighbour);
        }
    }
}

// Checking if the graph is connected after removing the edge
bool checkConnected(unordered_map<int, unordered_set<int>> &graph, int V)
{
    // vector to keep track of visited and initialized to 0
    vector<bool> visited(V, 0);

    // Starting DFS from node 0
    DFS(graph, visited, 0);

    // Checking if each node is visited. If a node is not visited that means the graph is disconnected and we return 0
    // The removed edge needs to be added again
    for (int i = 0; i < V; i++)
        if (visited[i] == 0)
            return 0;

    // If all nodes are visited removing the edge does keep the graph connected
    return 1;
}

void reverseDeleteMST(unordered_map<int, unordered_set<int>> &graph, vector<pair<int, pair<int, int>>> &edges, int V)
{
    // Sorting the edges
    sort(edges.begin(), edges.end());

    // Weight of the MST
    int mstWeight = 0;
    int u, v;

    for (int i = edges.size() - 1; i >= 0; i--)
    {
        u = edges[i].second.first;
        v = edges[i].second.second;

        // Deleting the edge from the graph
        graph[u].erase(v);
        graph[v].erase(u);

        // Check if the graph is connected after removing the edge
        if (checkConnected(graph, V) == 0)
        {
            // Re-adding the edge as the graph gets disconnected
            graph[u].insert(v);
            graph[v].insert(u);

            cout << "Edge : " << u << " " << v << "\n";
            mstWeight += edges[i].first;
        }
    }
    cout << "Weight of MST is : " << mstWeight << "\n";
}

int main()
{
    int V = 7;
    unordered_map<int, unordered_set<int>> graph;
    vector<pair<int, pair<int, int>>> edges;

    // Adding nodes to the graph and edges
    // Same graph as the one in the example
    AddWeightedEdge(graph, edges, 0, 1, 7);
    AddWeightedEdge(graph, edges, 0, 3, 5);
    AddWeightedEdge(graph, edges, 1, 2, 8);
    AddWeightedEdge(graph, edges, 1, 4, 7);
    AddWeightedEdge(graph, edges, 1, 3, 9);
    AddWeightedEdge(graph, edges, 3, 4, 15);
    AddWeightedEdge(graph, edges, 3, 5, 6);
    AddWeightedEdge(graph, edges, 2, 4, 5);
    AddWeightedEdge(graph, edges, 4, 5, 8);
    AddWeightedEdge(graph, edges, 4, 6, 9);
    AddWeightedEdge(graph, edges, 5, 6, 11);

    // Calling reverse Delete MST
    reverseDeleteMST(graph, edges, V);
}
```
```
OUTPUT:-
Edge : 4 6
Edge : 1 4
Edge : 0 1
Edge : 3 5
Edge : 2 4
Edge : 0 3
Weight of MST is : 39
```

**Time Complexity:** O( E * Log(E) + E * (V+E))

**Space Complexity:** O(V+E)
<hr></hr>

### :arrow_right:Applications of Reverse Delete Algorithm for MST are:

1) Used to find the Minimum Spanning Tree using a greedy approach<br>
2) Designing Local Area Networks<br>
3) Image registration and segmentation (minimum spanning tree-based segmentation).<br>
4) Handwriting recognition of mathematical expressions.<br>
Etc.<br>
<hr></hr>

### :arrow_right:References:-

https://www.geeksforgeeks.org/reverse-delete-algorithm-minimum-spanning-tree/<br>
https://iq.opengenus.org/reverse-delete-algorithm/<br>
