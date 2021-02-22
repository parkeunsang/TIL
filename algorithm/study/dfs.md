# DFS

- 비선형 구조인 그래프로 표현된 자료들을 빠짐없이 탐색하는것
- 깊이우선탐색으로 스택(stack)을 사용한다.



## Process

1. 시작점 v 방문
2. 방문하지 않은 정점 w가 있으면 v를 스택에 push하고, w를 방문 후 w를 v로 취급하며 이를 반복
3. v에서 갈수있는 곳중 방문하지 않은 정점이 없으면(다 방문했으면) 스택을 pop해서 가장 마지막 방문 정점을 v로 해서 이를 반복
4. 스택이 공백이 될 때까지 반복



## Code

```PYthon
class Stack:
    ~
graph = {'a':['b','c'], 'b':['a','d'], 'c':['a'], 'd':['b']}
s = Stack()
def dfs(v):
	visited = [v]
    s.push(v)
    while not s.is_empty():
        v = s.pop()
        for element in graph[v]:
            if element not in visited:
                visited.append(element)
                s.push(element)
                break
```

