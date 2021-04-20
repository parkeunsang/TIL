# Merge sort

## 개요

리스트를 반반으로 계속 분할해나간 후 다시 병합하는 과정에서 정렬을 하는 방법



## Process

1. 리스트를 midian index를 기준으로 반으로 쪼개어 나간다
2. 더이상 쪼갤수가 없으면 쪼개진 2개의 리스트의 원소를 각각 살펴보며 순서대로 정렬한다
3. 이 과정을 반복한다(divide and conquer)

시간복잡도의 경우 병합하는 과정에서 n의 상수배만큼 걸리고, 쪼개는 횟수가 logn과 비례해 총 시간복잡도는 O(nlogn)이다.



## Code

```Python
def merge_sort(arr, start, end):
    if start == end:  # 재귀를 빠져나오는 부분
        return start, end
    else:
        # divide
        mid = (end + start + 1) // 2
        start1, end1 = merge_sort(arr, start, mid-1)
        start2, end2 = merge_sort(arr, mid, end)
        return merge(arr, start1, end1, start2, end2)


def merge(arr, start1, end1, start2, end2):
    # conquer
    # start1 = end1인경우 아무것도 바뀔게없다
    global count
    idx1 = start1  # 왼쪽 부분 arr
    idx2 = start2
    merged_list = []
    while idx1 <= end1 and idx2 <= end2:
        if arr[idx1] <= arr[idx2]:
            merged_list.append(arr[idx1])
            idx1 += 1
        else:
            merged_list.append(arr[idx2])
            idx2 += 1
    if idx1 == end1 + 1:  # 왼쪽 arr가 바닥났을 경우
        merged_list.extend(arr[idx2:end2+1])
    else:
        merged_list.extend(arr[idx1:end1+1])
    arr[start1:end2+1] = merged_list  # 원본 리스트 변경
    return start1, end2  # 합친 리스트의 처음과 끝 인덱스


a = [1,3,4,9,1,6,4,4,6,791,43,1,432,69,34]
merge_sort(a, 0, len(a) - 1)
print(a)

```

