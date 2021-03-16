# 조합

n개의 case에서 r개를 중복없이 뽑는 것.

ex) arr = [1,2,3,4,5], r = 2일때 총 경우는 [1,2], [1,3] ,..., [4,5] 총 10개이다.

생각보다 코드로 구현하기 힘들어 python itertools의 docs를 참고해서 구현했다.

## Process

ex) arr = [1,2,3,4,5,6], r = 3

1. 먼저 오른쪽부터해서 숫자를 키워나간다.[1,2,3], [1,2,4]

2. 마지막 숫자가 끝에 다다르면 한칸왼쪽부분의 숫자를 1 증가시키고 맨 오른쪽부분은 증가시킨 숫자에서 1을 더한 숫자로 갱신한다.

   [1,2,6] -> [1,3,4]

3. 더이상 숫자를 증가시킬 수 없을때가지 반복한다.

## Code

```Python
def combination(n, r):
    indices = list(range(r))
    result = [tuple(indices)]  # 최종 반환값
    while True:
        # 마지막값(오른쪽)을 1씩 더해주는 파트 ex) [0,1,2] -> [0,1,3]...
        for i in reversed(range(r)):
            if indices[i] != i + n - r:  # 끝이 가장 마지막값이 아니면 증가시킬 인데스 i를 채택
                break
        else:
            break

        indices[i] += 1
        # 끝에 도달했으면 처음값들을 1씩 더해주는 파트 ex) [0,1,5] -> [1,2,3]
        for j in range(i+1, r):
            indices[j] = indices[j-1] + 1
        result.append(tuple(indices))
    return result
```

