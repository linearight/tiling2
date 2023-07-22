import numpy as np
import matplotlib.pyplot as plt
import networkx as nx
from hopcroftkarp import HopcroftKarp
import matplotlib.colors as mcolors
n = 6
def chessboard(n):
    # Create an n x n grid
    grid = np.zeros((n, n))

    # Color the cells of the chessboard
    for i in range(n):
        for j in range(n):
            if (i + j) % 2 == 0:
                grid[i, j] = 1
    return(grid)

block = [(4,1),(4,2),(1,2),(3,3),(5,5),(5,4), (2,4),((3,4))] #n=6
# block = [] #n=4
# block = [(0,0),(1,2),(4,4),(4,5),(3,7),(3,5),(2,3),(0,1)] #n=8




grid = chessboard(n)

left_nodes = []
right_nodes = []
graph = {}
for x in range(len(block)):
    grid[block[x]] = 0.5
graph_edge = []

for i in range(n):
    for j in range(int(n/2)):
        l = 2*j + i%2
        graph[str(n*i+l)] = set([])
print(graph)
for i in range(n):
    for j in range(int(n/2)):
        l = 2*j + i%2
        r = 2*j + (i+1)%2
        left_nodes.append(n*i + l)
        right_nodes.append(n*i + r)
        if grid[i,l] != 0.5:
            if l > 0 and l < n-1:
                if grid[i,l-1] == 0:
                    graph_edge.append(((n*i + l-1), (n*i + l)))
                    graph[str(n * i + l)].add(n*i + l-1)
                if grid[i,l+1] == 0:
                    graph_edge.append(((n*i + l+1), (n*i + l)))
                    graph[str(n * i + l)].add(n * i + l + 1)
            elif l == 0:
                if grid[i, l + 1] == 0:
                    graph_edge.append(((n * i + l + 1), (n * i + l)))
                    graph[str(n * i + l)].add(n * i + l + 1)
            else:
                if grid[i, l - 1] == 0:
                    graph_edge.append(((n * i + l - 1), (n * i + l)))
                    graph[str(n * i + l)].add(n * i + l - 1)

            if i >= 1 and i <= n-2:
                if grid[i-1,l] == 0:
                    graph_edge.append(((n*(i-1) + l), (n*i + l)))
                    graph[str(n * i + l)].add(n * (i-1) + l )
                if grid[i+1,l] == 0:
                    graph_edge.append(((n*(i+1) + l), (n*i + l)))
                    graph[str(n * i + l)].add(n * (i+1) + l )
            elif i == 0:
                if grid[i + 1, l] == 0:
                    graph_edge.append(((n * (i+1) + l), (n * i + l)))
                    graph[str(n * i + l)].add(n * (i+1) + l )
            else:
                if grid[i - 1, l] == 0:
                    graph_edge.append(((n * (i - 1) + l), (n * i + l)))
                    graph[str(n * i + l)].add(n * (i-1) + l )

print(graph_edge)
print(graph)
print(left_nodes, right_nodes)

def show_chessboard(grid):
    cmap = plt.cm.get_cmap('Reds')
    cmap.set_bad(color='white')

    # Create a figure and axis
    fig, ax = plt.subplots()
    fig.set_size_inches(8,8)
    # Plot the grid
    ax.imshow(grid, cmap=cmap)

    # Remove the ticks and labels
    ax.set_xticks([])
    ax.set_yticks([])


def plot_graph(edges, plotpos):
    # Create an empty graph
    G = nx.Graph()
    G.add_nodes_from(left_nodes)

    # Add nodes to the right side
    G.add_nodes_from(right_nodes)


    # Create a layout for the graph
    pos = {}
    pos.update((node, (1, index)) for index, node in enumerate(left_nodes))
    pos.update((node, (10, index)) for index, node in enumerate(right_nodes))

    G.add_edges_from(edges)

    # Draw the graph
    nx.draw(G, pos = pos, with_labels=True, node_color='lightblue', node_size=500, edge_color='gray')

show_chessboard(grid)
plot_graph(graph_edge, 567)
plt.tight_layout()
plt.show()

graph = HopcroftKarp(graph).maximum_matching(keys_only=True)
print(graph)

for i in range(n):
    for j in range(int(n/2)):
        l = 2*j + i%2
        t1 = n*i + l
        if grid[i,l] != 0.5:
            grid[i,l] = 1 + t1
            grid[graph[str(t1)]//n, graph[str(t1)]%n] = 1 + t1
for x in range(len(block)):
    grid[block[x]] = 0

G = nx.Graph()

# Sort the keys and values based on their numerical order (higher numbers lower)
sorted_keys = sorted(graph.keys(), key=int)
sorted_values = sorted(graph.values(), key=int)

# Add nodes to the left side (keys)
for index, key in enumerate(sorted_keys):
    G.add_node(key, pos=(0, index))

# Add nodes to the right side (values)
for index, value in enumerate(sorted_values):
    G.add_node(value, pos=(1, index))

# Create edges between corresponding key-value nodes
for key, value in graph.items():
    G.add_edge(key, value)

# Create positions for nodes based on their 'pos' attribute
pos = nx.get_node_attributes(G, 'pos')

# Draw the graph
nx.draw(G, pos, with_labels=True, node_size=800, node_color='lightblue', edge_color='gray')

# Show the graph
plt.show()

print(grid)
print(list(mcolors.CSS4_COLORS.keys()))
fuck = list(mcolors.CSS4_COLORS.keys())

fig, ax = plt.subplots()
fig.set_size_inches(8,8)




ax.imshow(grid, cmap='Reds')


ax.set_xticks([])
ax.set_yticks([])

plt.show()



