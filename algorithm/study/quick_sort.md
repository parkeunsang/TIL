# Quick sort

## 개요

- 평균적으로 가장 빠른 알고리즘(O(nlogn)), but 최악의 경우 시간 복잡도는 O(n^2)
- 불안정 정렬(같은 값일 경우 위치순서를 보장하지 못함)
- 분할 정복(divide and conquer)

**Process**

1. 피벗 설정
2. 해당 피벗의 위치 정확히 찾기(피벗기준 왼쪽은 자기보다작은것, 오른쪽은 자기보다 큰것)
3. 피벗 좌, 우 리스트를 대상으로 이를 반복(분할 정복) 후 결합
4. 리스트의 크기가 1이하일때 중단

적절한 피벗을 찾는 방법은 여러가지가 있으며 여기선는 Hoare partition과 Lomuto partition 두가지를 살펴보고자 한다.

`Hoare partition` : 인덱스들의 초기 위치를 설정하고 left(리스트 가장 왼쪽), right(리스트 가장 오른쪽) 피벗을 리스트 가장 왼쪽의 값이나 중간 위치의 값 등으로 설정한 다음 left인덱스와 right인덱스를 pivot과 비교해가며 pivot의 왼쪽에는 pivot보다 작은값들을, 오른쪽에는 pivot보다 큰 값들을 위치시킨다.

`Lomuto partition` : left를 가장 왼쪽에서 1을 뺀 idx, right를 가장 왼쪽 idx로 설정한 다음 피벗의 위치를 가장 오른쪽 인덱스로잡는다. 만약 right인덱스에서 값이 피봇보다 작으면 left를 1 증가시키고, left와 right의 값을 교환한다. 이과정에서 right왼쪽의 값들은 pivot보다 작아지게 된다. 이과정을 다 끝내고 pivot과 left+1(+1을 하는 이유는 리스트의 길이가 2일때 계속 swap이발생하기떄문이다.)과의 자리를 바꾸어주면 된다.

## Code

**Partition**

```Python
# Lomuto partition
def partition_l(arr, start, end):
    i = start - 1
    for j in range(start, end):
        if arr[j] < arr[end]:  # <= 는 시간초과남. swap과정이 많아져서 그런듯
            i += 1
            arr[i], arr[j] = arr[j], arr[i]  # j를 왼쪽편으로 보낸다. 이때 1 증가된 i는 무조건 arr[end]보다 크다(거쳐왔으니까)
    arr[i+1], arr[end] = arr[end], arr[i+1]  # i+1자리에 정답 위치
    return i + 1


# Hoare partition v1, 피봇 위치가 중간
def partition_h(arr, start, end):
    pivot = (start + end) // 2
    left = start
    right = end
    while left < right:
        while arr[left] < arr[pivot] and left < right:
            left += 1
        while arr[right] >= arr[pivot] and left < right:
            right -= 1
        if left < right:
            if left == pivot:
                pivot = right
            arr[left], arr[right] = arr[right], arr[left]

    arr[pivot], arr[right] = arr[right], arr[pivot]  # 피봇과 수왑
    return right


# Hoare partition v2, 피봇 위치가 맨 왼쪽
def partition_h(arr, start, end):
    pivot = arr[start]
    left = start
    right = end

    while left <= right:
        if arr[left] <= pivot:  # left가 피봇보다 커지는 지점에 위치
            left += 1
        if arr[right] >= pivot:  # right가 피봇보다 작아지는 지점에 위치
            right -= 1
        if left < right:
            arr[left], arr[right] = arr[right], arr[left]
    arr[start], arr[right] = arr[right], arr[start]
    return right
```

**병합**

```Python
def quick_sort(arr, start, end):
    if start < end:
        mid = partition_l(arr, start, end)  # mid위치는 옳은 위치임
        quick_sort(arr, start, mid-1)
        quick_sort(arr, mid+1, end)


a = [1,3,4,9,1,6,4,4,6,791,43,1,432,69,34]
quick_sort(a, 0, len(a) - 1)
print(a)

```







