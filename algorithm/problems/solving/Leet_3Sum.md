# 3Sum

## 개요

리스트가 주어졌을 때, 3개의 합이 0이 되는 리스트들을 반환

```Python
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```



## 접근

1. itertools의 combinations를 이용해 **완전탐색**을 통한 풀이

2. 

## Code

1.

```PYthon
from itertools import combinations

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        result = []
        for c in combinations(nums, 3):
            if sum(c) == 0:
                if sorted(c) not in result:
                    result.append(sorted(c))        
        
        return result
```

-> 시간초과

