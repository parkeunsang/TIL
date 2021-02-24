# 후위표기법

- 후위표기법이란?

  연산자(+, - 등)를 피연산자(숫자) 뒤에 표기하는 방법

- 장점

  괄호가 필요없다

- 계산법

  CAB-/ 는 다음과같이 계산된다

  1. 첫번째로 등장한 연산자를 이용해 왼쪽 피연산자2개를 계산한다. (A-B) = D
  2. 계산한 값을 해당 위치에 위치시키고, 이를 반복한다.(C/D)

  



## 중위표기법 -> 후위표기법

```
((5+2)*(3-5)+7)/(3*2+5) -> 52+35-*7+32*5+/
```

**Process**

1. 스택과 최종 결과를 담을 result(string)을 준비한다.

2. 중위표기식을 for문으로 돌며 숫자는 그대로 result에 쌓아간다

3. 우선순위가 높은 연산자 (* > +)가 후위표기식에서 먼저등장해야하므로 스택에 나중에 들어가게 쌓는다.(만약 스택에 있는 연산자가 들어가야할 연산자보다 우선순위가 같거나 높으면 pop해서 result에 쌓는다)
4. '('의 경우 스택내부에서는 우선순위가 가장 낮고, 외부에서는 우선순위가 가장 높은것으로 취급하며(즉 등장하면 무조건 스택에 쌓는다) result에는 쌓지 않는다.
5. ')'이 등장하면 '('이 등장할때까지 스택을 pop하고 result에 쌓는다.

## Code

```python
class Stack:
    """
    LIFO
    """
    def __init__(self):
        self.elements = []
        self.top = -1

    def push(self, element):
        self.elements.append(element)
        self.top += 1

    def pop(self):
        if self.elements:  # 원소의 개수가 0이 아니면
            self.top -= 1
            return self.elements.pop()
        else:
            return None

    def is_empty(self):  # 스택이 비었는지 확인
        if self.elements:
            return False
        else:
            return True

    def peek(self):
        if self.elements:
            return self.elements[self.top]
        else:
            return None


def jung_to_hu(jung):
    stack = Stack()
    result = ''
    isp = {'(': 0, '+': 1, '-': 1, '*': 2, '/': 2}  # 스택 안의 우선순위
    icp = {'(': 3, '+': 1, '-': 1, '*': 2, '/': 2}  # 스택 밖의 우선순위
    for s in jung:
        if s.isdigit():
            result += s
        else:
            if s != ')':
                if not stack.is_empty():  # 스택이 비어있지 않으면
                    if icp[s] > isp[stack.peek()]:  # 토큰이 스택의 peek보다 우선순위가 높으면
                        stack.push(s)
                    else:  # 토큰의 우선순위가 낮으면 높은것 나올때까지 pop
                        while not stack.is_empty():
                            if icp[s] <= isp[stack.peek()]:
                                popped = stack.pop()
                                result += popped
                            else:
                                break
                        stack.push(s)
                else:  # 스택이 비어있으면
                    stack.push(s)

            else:  # )이면
                while not stack.is_empty():  # (가 나올때까지 pop
                    popped = stack.pop()
                    if popped == '(':
                        break
                    result += popped

    while not stack.is_empty():
        result += stack.pop()
    return result

string = '((5+2)*(3-5)+7)/(3*2+5)'
result = jung_to_hu(string)
print(result)
```

