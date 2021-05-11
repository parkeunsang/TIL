# 다익스트라 알고리즘

## 개요

그래프에서 시작노드(X)와 특정 노드들 사이의 최단거리를 구하는 알고리즘

## Process

1. 처음에 시작 노드에서 갈 수 있는 노드들까지의 거리를 구한다
2. 가장 짧은 거리에 있는 노드를 방문한다.
3. 방문한 노드에서 다시 갈수있는 노드들까지의 거리를 구한다
4. 시작노드(X)에서 갈 수 있는 거리가 가장 짧은 노드를 방문하다
5. 3~4를 반복한다.

핵심은 

- 새로 방문한 노드에서 갈 수 있는 노드들의 거리를 새로 갱신(기존의 거리vs새로 방문한 노드를 통해 가는 거리)하는것
- 시작점에서 갈 수 있는 거리가 가장 짧은 노드를 택하는것(heap queue를 사용)



## Code

백준 10282 해킹 문제의 풀이에서 다익스트라 알고리즘을 사용했다.

```Python
import heapq
for tc in range(int(input())):
    n, d, c = map(int, input().split())
    link = {}
    # for i in range(1, n+1):  # 메모리초과
    #     link[i] = []
    #     link_cost[i] = {}
    for _ in range(d):
        a, b, s = map(int, input().split())
        if link.get(b):
            link[b][a] = s
        else:
            link[b] = {a: s}
    dist = [1000*10000+1] * (n+1)
    dist[c] = 0
    visited = [0] * (n+1)
    hq = [(0, c)]
    while hq:
        cost, start = heapq.heappop(hq)
        if link.get(start) and not visited[start]:  # 해당지점 방문안했으면
            for end in link[start].keys():  # 거리 갱신
                dist_new = dist[start] + link[start][end]
                if dist_new < dist[end]:  # 길이가 길면 굳이 넣을필요없다.(어차피 hq에 들어가있음)
                    dist[end] = dist_new
                    heapq.heappush(hq, (dist_new, end))
            visited[start] = 1
    dist = [x for x in dist if x != 10000001]
    print(f'{len(dist)} {max(dist)}')
```

