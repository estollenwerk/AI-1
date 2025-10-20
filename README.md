from collections import deque

class Graph:
    def __init__(self, adjacency_list, time):
        self.adjacency_list = adjacency_list
        self.time = time

    def get_neighbors(self, v):
        return self.adjacency_list[v]

    def h(self, n):
        if 2 <= self.time <= 7:
            H ={
                'A': 12, 'B1': 8, 'B2': 4, 'B3': 4, 'B4': 4, 'B5': 6, 'B6': 4,
                'C1': 4, 'C2': 4, 'C3': 4, 'C4': 4, 'C5': 6, 'C6': 4,
                'D1': 4, 'D2': 4, 'D3': 4, 'D4': 4, 'D5': 6, 'D6': 4,
                'E1': 4, 'E2': 4, 'E3': 4, 'E4': 4, 'E5': 6, 'E6': 4
            }
        else:
            H = {
                'A': 1, 'B1': 2, 'B2': 2, 'B3': 2, 'B4': 2, 'B5': 1, 'B6': 2,
                'C1': 2, 'C2': 2, 'C3': 2, 'C4': 2, 'C5': 1, 'C6': 2,
                'D1': 2, 'D2': 2, 'D3': 2, 'D4': 2, 'D5': 1, 'D6': 2,
                'E1': 2, 'E2': 2, 'E3': 2, 'E4': 2, 'E5': 1, 'E6': 2
            }

        return H[n]

    def a_star_algorithm(self, start_node, stop_node):
        open_list = set([start_node])
        closed_list = set([])

        g = {start_node: 0}
        parents = {start_node: start_node}

        while open_list:
            n = None
            for v in open_list:
                if n is None or g[v] + self.h(v) < g[n] + self.h(n):
                    n = v

            if n is None:
                print('Path does not exist!')
                return None

            if n == stop_node:
                reconst_path = []
                while parents[n] != n:
                    reconst_path.append(n)
                    n = parents[n]
                reconst_path.append(start_node)
                reconst_path.reverse()
                print('Path found: {}'.format(reconst_path))
                return reconst_path

            for (m, weight) in self.get_neighbors(n):
                if m not in open_list and m not in closed_list:
                    open_list.add(m)
                    parents[m] = n
                    g[m] = g[n] + weight
                else:
                    if g.get(m, float('inf')) > g[n] + weight:
                        g[m] = g[n] + weight
                        parents[m] = n
                        if m in closed_list:
                            closed_list.remove(m)
                            open_list.add(m)

            open_list.remove(n)
            closed_list.add(n)

        print('Path does not exist!')
        return None

# ✅ Move this outside the class
adjacency_list = {
    'A': [('B1', 3.5)],
    'B1': [('B2', 1), ('C1', 3.5)],
    'B2': [('B3', 1), ('C2', 3.5)],
    'B3': [('B4', 1), ('C3', 3.5)],
    'B4': [('B5', 1), ('C4', 3.5)],
    'B5': [('B6', 1), ('C5', 3.5)],
    'B6': [('C6', 3.5)],
    'C1': [('C2', 1), ('D1', 3.5)],
    'C2': [('C3', 1), ('D2', 3.5)],
    'C3': [('C4', 1), ('D3', 3.5)],
    'C4': [('C5', 1), ('D4', 3.5)],
    'C5': [('C6', 1), ('D5', 3.5)],
    'C6': [('D6', 3.5)],
    'D1': [('D2', 1), ('E1', 3.5)],
    'D2': [('D3', 1), ('E2', 3.5)],
    'D3': [('D4', 1), ('E3', 3.5)],
    'D4': [('D5', 1), ('E4', 3.5)],
    'D5': [('D6', 1), ('E5', 3.5)],
    'D6': [('E6', 3.5)],
    'E1': [('E2', 1)],
    'E2': [('E3', 1),],
    'E3': [('E4', 1),],
    'E4': [('E5', 1),],
    'E5': [('E6', 1),],
    'E6': []
}
time = int(input("What's the time? (Enter an integer 1–4): "))
graph1 = Graph(adjacency_list, time)
graph1.a_star_algorithm('A', 'E5')
time = int(input("What's the time? (Enter an integer 1–4): "))
graph2= Graph(adjacency_list, time)
graph2.a_star_algorithm('A', 'E5')
