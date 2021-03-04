# BFS

- 비선형 구조인 그래프로 표현된 자료들을 빠짐없이 탐색하는것
- 깊이우선탐색으로 큐(queue)를 사용한다.



## Process

1. 시작점 v 방문
2. v에서 갈 수 있는 노드들 중 방문하지 않은 노드들을 queue에 push
3. queue의 원소들을 pop하며 v로 저장하고, 2의 과정을 반복
4. queue가 공백이 될 때 까지 반복



## Code

```Python
class Queue:
    """
    원형큐
    """
    def __init__(self, n):
        """
        Create queue
        """
        self.n = n  # 큐의 크기는 n이지만 들어갈 수 있는 원소의 개수는 n-1개
        self.queue = [0] * n
        self.front = 0
        self.rear = 0

    def enqueue(self, element):
        if not self.is_full():
            self.rear = (self.rear + 1) % self.n
            self.queue[self.rear] = element
        else:
            raise Exception('Queue is full')

    def dequeue(self):
        if not self.is_empty():
            self.front = (self.front + 1) % self.n
            return self.queue[self.front]
        else:
            raise Exception('Queue is empty')

    def qpeek(self):
        if not self.is_empty():
            return self.queue[(self.front + 1) % self.n]
        else:
            raise Exception('Queue is Empty')

    def is_empty(self):
        return self.front == self.rear

    def is_full(self):
        return (self.rear + 1) % self.n == self.front


def bfs(graph, v, n):
    queue = Queue(n)
    queue.enqueue(v)
    visited = []  # 방문여부

    while not queue.is_empty():  # 큐가 빌 때 까지
        v = queue.dequeue()
        visited.append(v)
        print(v, end=' ')
        for element in graph[v]:
            if element not in visited:
                queue.enqueue(element)
                visited.append(element)


input_arr = [1, 2, 1, 3, 2, 4, 2, 5,
             4, 6, 5, 6, 6, 7, 3, 7,]
graph = [[] for _ in range(max(input_arr) + 1)]  # 인덱스로 접근
for i in range(len(input_arr) // 2):
    node1 = input_arr[2*i]
    node2 = input_arr[2*i+1]
    graph[node1].append(node2)  # 양방향
    graph[node2].append(node1)

bfs(graph, 1, 100)

```

