# 오목판정

## 문제

[Link](https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AXaSUPYqPYMDFASQ)



## 개요

주어진 문자 (., o) 를 통해 오목인지 여부를 판정하는것



## 접근

- 오목판의 최대 길이가 100이므로 **완전탐색** 으로 접근

- 가로, 세로, 우하향대각선, 우상향대각선 총 4가지 case에 대해 해당 list가 오목을 포함하고있으면 'YES'반환, 그렇지않으면 'NO'반환 하는 함수로 판별



## Code

```PYthon
def is_omok(arr):
    """
    1차원 배열을 받아서 오목인지 판정
    :param arr: list
    :return: bool
    """
    count = 0
    for s in arr:
        if s == 'o':
            count += 1
        else:
            count = 0
        if count == 5:
            return 1
    return 0


def travel_arr(arr, n):
    # 가로 탐색
    for li in arr:
        if is_omok(li):
            return 'YES'

    # 세로 탐색
    for i in range(n):
        li = []
        for j in range(n):
            li.append(arr[j][i])
        if is_omok(li):
            return 'YES'

    # 오른쪽 밑으로가는 대각 탐색
    # 0, 95 / 1, 96 / 2, 97 / 3, 98 / 4, 99
    # ...
    # 0, 0 / ... / 99, 99
    for i in range(n-4):
        j = 0
        li = []
        while i < n:
            li.append(arr[j][i])
            i += 1
            j += 1
        if is_omok(li):
            return 'YES'
    # 1, 0 / 2, 1 / ... /99, 98
    # ...
    # 95, 0 / ... / 99, 4
    for j in range(n-4):
        i = 0
        li = []
        while j < n:
            li.append(arr[j][i])
            i += 1
            j += 1
        if is_omok(li):
            return 'YES'
    # 왼쪽 밑으로가는 대각 탐색
    # 0,4 / 1,3 / 2,2 / 3,1 / 4,0
    # ...
    # 0,99 / ... / 99,0
    for i in range(4, n):
        j = 0
        li = []
        while i >= 0:
            li.append(arr[j][i])
            i -= 1
            j += 1

        if is_omok(li):
            return 'YES'
    # 1,99 / 2,98 / ... / 99,1
    # ...
    # 95,99 / 96,98 / ... / 99,95
    for j in range(1, n):
        i = n - 1
        li = []
        while j < n:
            li.append(arr[j][i])
            i -= 1
            j += 1
        if is_omok(li):
            return 'YES'
    return 'NO'


for tc in range(int(input())):
    n = int(input())
    input_arr = []
    for i in range(n):
        input_arr.append(list(input()))

    print('#{} {}'.format(tc+1, travel_arr(input_arr, n)))

```



## 후기

- 길이가 k인 1차원 배열을 받아 탐색하며 오목인지를 판정하는 함수인 **is_omok**에 가로, 세로, 대각선 전체의 list를 넣어서 판별하는것보다 그냥 (x,y) 좌표를 잡아서 +4까지 오목인지를 보는것이 훨씬 간편한 방법인듯
- DFS를 통한 탐색도 가능(by 박정웅님)