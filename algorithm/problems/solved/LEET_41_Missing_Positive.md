# 41. First Missing Positive

## 문제 개요

정렬되지않은 정수 원소를 가지고있는 길이가 n인 리스트 `nums`가 주어지고, 1이상인 정수 중 포함되지 않는 가장 작은 정수를 반환하는 문제

시간복잡도는 O(n), 공간복잡도는 O(1)로 제한



## 접근

생각나는 가장 쉬운방법은 정렬을 이용하는 방법이었으며, 이때의 시간복잡도는 O(nlogn)으로 주어진 조건을 만족하지 못한다. 그래서 다른 방법을 계속 생각해보았지만 실패했다. 값을 다 더해서 빈 값을 찾는 방법도 생각해봤는데 실패했다.

그래서 다른 사람의 인덱스를 활용하는 아이디어를 참고해 풀었다. for문을 돌면서 양수 값이면 해당 값의 인덱스와 그 값이 같은지를 체크하고, 다르면 swap하는 방식으로 풀이할 수 있었다. 이 기가막힌 아이디어때문에 난이도가 Hard인것같다.



## Code

```Python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)  # can max
        idx = 0
        while idx < n:
            i = nums[idx]
            if i > 0 and i <= n and nums[i-1] != i:
                nums[idx], nums[i-1] = nums[i-1], nums[idx]
                idx -= 1
            idx += 1
        for i in range(n):
            if nums[i] != i+1:
                return i+1
        return n+1  
```

