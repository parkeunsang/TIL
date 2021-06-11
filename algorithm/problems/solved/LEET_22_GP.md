# Generate Parentheses

[site](https://leetcode.com/problems/generate-parentheses/)

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

`Example`

```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```



## 접근

무조건 등장한 `(`의 개수가 `)` 의 개수보다 많아야하므로 DFS에서 두개의 차이를 저장해 `)`가 등장하지 못하는 상황에서는 `(`만 등장시키며 탐색



## Code

```Python
def generateParenthesis(n):
    c1 = 0  # the number of used (
    c2 = 0  # the number of used )
    gap = 0  # c1 - c2
    dances = ''  # ex) ()()
    result = []
    stack = [(c1, c2, gap, dances)]  # the number of (, ), gap, string
    while stack:
        c1, c2, gap, dances = stack.pop()
        if c2 == n:  # finished
            result.append(dances)
            continue
        if c1 == n:
            stack.append((c1, c2+1, gap-1, dances + ')'))
        elif gap == 0:
            gap = c1 - c2
            stack.append((c1+1, c2, gap+1, dances + '('))
        else:
            stack.append((c1+1, c2, gap+1, dances + '('))
            stack.append((c1, c2+1, gap-1, dances + ')'))
    return result

print(generateParenthesis(4))
```



