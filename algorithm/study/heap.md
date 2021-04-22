# Heap

>  힙은 부모노드가 자식노드보다 항상 작거나(최소힙) 큰(최대힙) 완전이진트리이다.



## 구현

힙은 리스트로 쉽게 구현할 수 있다. 리스트의 첫 인덱스의 시작을 1이라고하면, 특정 노드의 인덱스의 2배는 왼쪽자식노드, 2배+1은 오른쪽 자식노드가 된다. 

원소를 삽입(push)하는 경우 마지막 리프노드에 삽입 후 부모노드와 비교해가며 조건을 만족할때까지 swap해가며 위로 이동시키면 된다.

원소를 삭제(pop)하는 경우 루트노드를 pop하고 마지막 리프노드를 루트노드에 위치시켜 왼쪽, 오른쪽자식노드와 비교해가며 힙의 성질이 만족될때까지 swap해나가며 아래로 이동시킨다.

## Code

```Python
class Heap:
    def __init__(self):
        self.elements = [0]
        self.n = 0  # 원소의 개수

    def swap(self, x, y):
        self.elements[x], self.elements[y] = self.elements[y], self.elements[x]

    def push(self, x):
        """
        :param x: (distance, node)
        :return: None
        """
        self.elements.append(x)
        self.n += 1
        idx = self.n
        while idx > 1:
            next_idx = idx // 2
            if self.elements[idx][0] < self.elements[next_idx][0]:
                self.swap(idx, next_idx)
                idx = next_idx
            else:
                break

    def pop(self):
        if self.n == 0:
            print('Empty heap')
            return
        if self.n == 1:  # 원소가 1개라 다시힙을 만들필요없음
            self.n -= 1
            return self.elements.pop()
        self.n -= 1
        popped = self.elements[1]  # pop된 값
        # 다시 정렬
        self.elements[1] = self.elements.pop()  # 마지막 원소를 꼭대기로 위치
        idx = 1
        while True:
            left_idx = idx * 2
            right_idx = idx * 2 + 1
            if left_idx > self.n:  # 자식이 없으면
                break
            if right_idx > self.n:  # 오른쪽 자식노드가 없으면
                if self.elements[idx][0] > self.elements[left_idx][0]:
                    self.swap(idx, left_idx)
                    idx = left_idx
                break  # 마지막 리프노드이므로 어차피 자식이 없다.
            else:  # 자식이 둘다 있으면
                if self.elements[left_idx][0] > self.elements[right_idx][0]:  # 오른쪽 자식이 더 작으면
                    if self.elements[idx] > self.elements[right_idx]:
                        self.swap(idx, right_idx)
                        idx = right_idx
                    else:  # 부모가 더 작으므로
                        break
                else:  # 왼쪽 자식이 더 작으면
                    if self.elements[idx][0] > self.elements[left_idx][0]:
                        self.swap(idx, left_idx)
                        idx = left_idx
                    else:  # 부모가 더 작으므로
                        break

        return popped
```

