# Ass3
Ass3_220
"""Q1"""
import sys
import functools
from collections import defaultdict
class AdjacencyLists:
    def __init__(self):
        self.graph = defaultdict(list)
        self.current_szie = 0
        self.graph_num = 0
    def empty(self): self.graph = defaultdict(list)
    def order(self):
        return len(self.graph)
    def isArc(self, i, j):
        if not ( 0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        return j in self.graph[i]

    def addArc(self, i, j):
        if not (0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        if not self.isArc(i, j):
            self.graph[i].append(j)
    def addVertices(self, n):
        if not 0 <= n:
            raise ValueError('Argument is negative!')
        for i in range(n): self.graph[i] = []
    def read(self, stream):
        self.empty()
        self.graph_num += 1
        order = int(stream.readline().strip())
        self.addVertices(order)
        for i in range(order):
            for neighbours in map(int, stream.readline().strip().split()):
                self.addArc(i, neighbours)
        if len(self.graph) == 0:
            raise ValueError("something")

    def DFSUtil(self, v, visited):
        visited[v] = True
        self.current_szie += 1
        for i in self.graph[v]:
            if visited[i] == False:
                self.DFSUtil(i, visited)
    def DFS(self, v):
        self.current_szie = 0
        visited = [False] * (len(self.graph))
        self.DFSUtil(v, visited)
    def get_largest_c(self):
        largest = 0
        for k in self.graph.keys():
            self.DFS(k)
            if self.current_szie > largest:
                largest = self.current_szie
        return largest
text_input = sys.stdin
graph = AdjacencyLists()
while(True):
    try:
        graph.read(text_input)
        print("Graph " + str(graph.graph_num) +" has a component of order " + str(graph.get_largest_c()) + ".")
    except:
        break
        
"""Q2"""
import sys
import functools

class AdjacencyLists:
    def __init__(self):
        self._adj = []
    def empty(self): self._adj = []

    def order(self):
        return len(self._adj)

    def size(self):
        return functools.reduce(lambda x,y: x + y,  map(len, self._adj),0 )

    def addVertices(self, n):
        if not 0 <= n:
            raise ValueError('Argument is negative!')
        for i in range(n): self._adj.append([])

    def removeVertex(self, i):
        if not 0 <= i < self.order():
            raise ValueError('Arguments out of bounds')
        del self._adj[i]
        for otherVertex in range(self.order()):
            current = self._adj[otherVertex]
            try:
                current.remove(i)
            except ValueError:
                pass
            for j in range(len(current)):
                if current[j] > i: current[j] -= 1

    def addArc(self, i, j):
        if not (0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        if not self.isArc(i, j):
            self._adj[i].append(j)

    def removeArc(self, i, j):
        if not (0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        try:
            self._adj[i].remove(j)
        except ValueError:
            pass
    def addEdge(self, i, j):
        self.addArc(i,j);self.addArc(j,i)

    def removeEdge(self, i, j):
        self.removeArc(i, j); self.removeArc(j, i)

    def isArc(self, i, j):
        if not ( 0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        return j in self._adj[i]

    def isEdge(self, i, j):
        return self.isArc(i, j)and self.isArc(j, i)

    def degree(self, i):
        if not 0 <= i < self.order():
            raise ValueError('Argument out of bounds')
        return len(self._adj[i])

    def inDegree(self, i):
        if not 0 <= i < self.order():
            raise ValueError('Argument out of bounds')
        retval = 0
        for j in range(self.order()):
            if self.isArc(j, i): retval += 1
        return retval

    def neighbors(self, i):
        if not 0 <= i < self.order():
            raise ValueError('Argument out of bounds')

        return sorted(self._adj[i])


    def read(self, stream):
        self.empty()
        order = int(stream.readline().strip())
        self.addVertices(order)
        for i in range(order):
            for neighbours in map(int, stream.readline().strip().split()):
                self.addArc(i, neighbours)
    def matrices(self):
        matrix = [[0]*self.order() for i in range(self.order())]
        for i in range(len(matrix)):
            for k in self._adj[i]:
                matrix[i][k] = 1
        print(self.order())
        for line in range(len(matrix)):
            for ind in matrix[line]:
                print(ind, end = ' ')
            print()
text_input = sys.stdin
graph = AdjacencyLists()
while(True):
    try:
        graph.read(text_input)
        graph.matrices()
    except:
        break


"""Q3"""
import sys
import functools
from collections import defaultdict
class AdjacencyLists:
    def __init__(self):
        self.graph = defaultdict(list)
        self.current_szie = 0
        self.graph_num = 0
    def empty(self): self.graph = defaultdict(list)
    def order(self):
        return len(self.graph)
    def isArc(self, i, j):
        if not ( 0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        return j in self.graph[i]

    def addArc(self, i, j):
        if not (0 <= i < self.order() and 0 <= j < self.order()):
            raise ValueError('Arguments out of bounds')
        if not self.isArc(i, j):
            self.graph[i].append(j)
    def addVertices(self, n):
        if not 0 <= n:
            raise ValueError('Argument is negative!')
        for i in range(n): self.graph[i] = []
    def read(self, stream):
        self.empty()
        self.graph_num += 1
        order = int(stream.readline().strip())
        self.addVertices(order)
        for i in range(order):
            for neighbours in map(int, stream.readline().strip().split()):
                self.addArc(i, neighbours)
        if len(self.graph) == 0:
            raise ValueError("something")
    def get_diameter(self):
        l = []
        for k in self.graph.keys():
            l.append(self.BFS(k))
        return max(l)
    def BFS(self, s):
        visited = [False] * (len(self.graph))
        queue = []
        a_dict = {}
        for i in range(len(self.graph)): a_dict[i] = 0
        queue.append(s)
        visited[s] = True
        while queue:
            s = queue.pop(0)
            for i in self.graph[s]:
                if visited[i] == False:
                    queue.append(i)
                    visited[i] = True
                    a_dict[i] = a_dict[s] + 1
        if False in visited:
            return False
        return max(a_dict.values())


text_input = sys.stdin
graph = AdjacencyLists()
while(True):
    try:
        graph.read(text_input)
        diameter = graph.get_diameter()
        if diameter:
            print("Graph " + str(graph.graph_num) + " has diameter " + str(diameter) + ".")
        else:
            print('Graph '+ str(graph.graph_num) + " is disconnected.")
    except:
        break
