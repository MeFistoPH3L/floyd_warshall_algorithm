# The Floyd-Worschell algorithm for finding shortest paths between all pairs of vertices
Given a oriented or undirected weighted graph G with n vertices. It is required to find the values of all values d_{ij} - the length of the shortest path from vertex i to vertex j.

It is assumed that the graph does not contain cycles of negative weight (then the answer between some pairs of vertices may simply not exist - it will be infinitely small).

This algorithm was published simultaneously in articles by Robert Floyd and Stephen Warshall in 1962, after whom the algorithm is named today. However, in 1959 Bernard Roy published basically the same algorithm, but his publication went unnoticed.

# Algorithm description
The key idea of the algorithm is to partition the process of finding shortest paths into phases.

Before the k-th phase (k = 1 ... n), it is assumed that the distance matrix d[][] contains such shortest paths that contain only vertices from the set { 1, 2, ..., k-1 } as interior vertices (we number the vertices of the graph starting from one).

In other words, before the kth phase the value d[i][j] is equal to the length of the shortest path from vertex i to vertex j, if this path is allowed to enter only vertices with numbers less than k (the beginning and the end of the path do not count).

It is easy to see that for this property to hold for the first phase it is enough to write the adjacency matrix of the graph into the distance matrix d[][]: d[i][j] = g[i][j] - the cost of an edge from vertex i to vertex j. If there is no edge between some vertices then write "infinity" value. From a vertex to itself you should always write the value 0, it is critical for the algorithm.

Let us now be in the k-th phase, and we want to recalculate the matrix d[][] so that it meets the requirements for the k+1th phase already. Let us fix some vertices i and j. We have two fundamentally different cases:

* The shortest path from vertex i to vertex j, which is allowed to pass through vertices { 1, 2, ..., k } additionally, coincides with the shortest path allowed to pass through vertices of set { 1, 2, ..., k-1 }.
In this case, the value of d[i][j] will not change when passing from the kth to the k+1th phase.

* The "new" shortest path is better than the "old" path.
This means that the "new" shortest path goes through vertex k. Immediately note that we will not lose generality by further considering only simple paths (i.e. paths that do not pass through some vertex twice).

Then note that if we divide this "new" path by vertex k into two halves (one going i -> k and the other going k -> j), then each of these halves no longer enters vertex k. But then it turns out that the length of each of these halves was calculated at the k-1st phase or even earlier, and we just need to take the sum d[i][k] + d[k][j], it will give the length of the "new" shortest path.

Combining these two cases, we obtain that in the k-th phase we need to recalculate the lengths of shortest paths between all pairs of vertices i and j as follows:

new_d[i][j] = min (d[i][j], d[i][k] + d[k][j]);

Thus, all the work that needs to be done in the k-th phase is to go through all pairs of vertices and recalculate the length of the shortest path between them. As a result, after the nth phase the distance matrix d[i][j] will contain the length of the shortest path between i and j, or infinity if there is no path between these vertices.

The last point to make is that we can avoid creating a separate matrix new_d[][] for the time matrix of shortest paths in the k-th phase: all changes can be done immediately in the matrix d[][]. Indeed, if we improved (reduced) some value in the distance matrix, we could not thereby degrade the length of the shortest path for some other pairs of vertices processed later.

The asymptotic of the algorithm is obviously O (n^3).
