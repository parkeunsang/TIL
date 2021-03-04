# 큐(Queue)

>  선입선출(FIFO) 구조

## 기능

- 삽입(enqueue)
- 반환(dequeue)
- 상태 체크(is_empty, is_full)

## 구현방법

- 선형큐, 원형큐, python list이용, linked list 이용 등

### 구현

효율적인 원형큐를 이용해(list에서 pop(0)은 비효율적) 구현해보았다.

**Code**

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
```

