# 패턴매칭

## 개요

```Python
pattern = 'abc'
text = 'zxcasdqwe'
# text에서 pattern이 존재하는지, 존재한다면 시작 index 탐색
```

## Algorithms

- Brute Force : 한칸씩 옮겨가며 다 해보는것

- KMP : 패턴을 전처리해서 좀더 효율적으로
- 보이어-무어 : 가장 많이 쓰이는 알고리즘



### Brute Force

> O(mn)

자세한 설명은 생략



### KMP

> 평균 : O(m+n), 최악 : O(m+n)

**알고리즘 개요** : 패턴에서 각 인덱스까지의 시작과 끝을 조사해서 같은 경우가 있다면 건너뛸 때 거기까지만 건너뛰는 것

**예시**

```Python
pattern = 'abcabe'
text = 'abcabzxcaabc'
```

- pattern_preprocessed 구하기

  1. pattern[:0] 에서 겹치는지 판단(원소가 없으니 0인데 뒤에 무한루프 방지하게위해 -1 추가)

  2. pattern[:1] 에서 겹치는지 판단(원소가 한개니까 무조건 0)

  3. pattern[:2] 에서 겹치는지 판단(ab 안겹치니 0)

  4. pattern[:3] 에서 겹치는지 판단(abc 안겹치니 0)

  5. pattern[:4] 에서 겹치는지 판단(abca에서 a겹치니 1)

  6. pattern[:5] 에서 겹치는지 판단(abcab에서 ab겹치니 2)

  7. pattern[:6]에서 겹치는지 판단(abcabe에서 안겹치니 0)

     -> [-1, 0, 0, 0, 1, 2, 0] 반환

- pattern 인식

  text의 맨처음(0번째)부터 abcab까지는 일치했으나 그다음 z가 나와 일치하지 않는 문자열이라고 판단이 되었고, 다음 검사를 시작할 지점을 1번째 지점이 아닌 ab가 다시 등장한 **3**번째 지점부터 시작(**5**번째인덱스(z)에서 시작한 후 pattern_preprocessed[5] = **2**를 뺀 값)한다.

```Python
pattern = 'abc'
text = 'abwezxcabc'
```

위와같은 경우라면 pattern에서 전처리가 필요없다. 1번째 인덱스까지는 일치하고(ab) 2번째 인덱스에서 불일치가 발생하므로 다음 검사를 시작할 인덱스는 2번째가 된다.(Brute Force의 경우 다음 검사 시작인덱스는 1번째)

**Code**

```Python
def kmp(pattern, text):
    """
    text에 pattern이 존재하는지 판단(KMP)
    :param pattern: str
    :param text: str
    :return: bool(o or 1)
    """

    def preprocess_pattern(pattern):
        """
        패턴을 입력받아 전처리를 하는 과정
        해당 인덱스는 몇번 뒤로 돌아가야하는지를 의미함
        :param pattern: str
        :return: list
        """
        pattern_preprocessed = [-1]
        for i in range(1, len(pattern)):
            sub_pattern = pattern[:i]
            j = len(sub_pattern) // 2
            while j > 0:
                if sub_pattern[:j] == sub_pattern[-j:]:
                    break
                j -= 1
            pattern_preprocessed.append(j)
        return pattern_preprocessed

    pattern_preprocessed = preprocess_pattern(pattern)
    text_len = len(text)
    pattern_len = len(pattern)

    if pattern_len > text_len:  # 패턴이 더 긴 경우
        return 0

    i = 0
    while i <= text_len - pattern_len:
        j = 0
        while j < pattern_len:
            if text[i + j] != pattern[j]:
                break
            j += 1
        if j == pattern_len:  # 패턴을 찾은 경우
            return 1
        # j=1일때, pattern_preprocessed[j]는 무조건 0이므로 무한루프 안돈다.
        i += j  # 확인한 인덱스까지 건너뜀
        i -= pattern_preprocessed[j]  # 양끝이 같은 패턴이 있는경우 돌아감

    return 0
```



### 보이어-무어

> 평균 : O(m+n), 최악 : O(mn)

**알고리즘 개요** : 첫번째 인덱스부터 패턴여부를 확인하고, 

1. 만약 오른쪽 끝에 있는 문자가 불일치하고, 이 문자가 pattern에 없을경우 바로 pattern만큼 건너뜀. 
2. 만약 pattern에 있을 경우 오른쪽부터 왼쪽으로 읽으면서 가장 처음나온 위치와 text의 오른쪽문자위치를 일치시킴.
3. 만약 pattern의 끝문자와 일치한다면 차례로 해당 pattern과 text를 비교하며 패턴인식, 일치안한다면 2의 과정을 통해 점프

**예시**

```PYthon
pattern = 'abcabe'
text = 'abcabzxcewacewerq'
```

1. 처음 텍스트 비교하면 sub_text = 'abcabz'로 마지막 글자(z)가 pattern의 마지막글자(e)와 다르고 pattern에 속하지도 않기때문에 바로 건너뛰고 xcaa~ 부터 비교한다.
2. 두번째 비교에서 'zxcewa'에서 마지막 글자(a)가 pattern에 속하고 pattern에서 오른쪽으로부터 3번째에 a가 처음등장하므로 2(3-1)칸만 이동해서 'cewace'부터 살핀다.(오른쪽으로부터 3번째에 pattern과 text a가 존재)

3. 세번재 비교에서 마지막 글자(e)가 pattern의 마지막글자와 일치하므로 해당 문자열 'cewace'가 pattern과 일치하는지 살피고, 만약 다를경우 pattern의 마지막글자를 제외한 부분에서 e가 포함되는지 살피고, 없다면 과정1을, 있다면 과정2를 진행한다.

**Code**

```Python
def bomu(pattern, text):
    """
    text에 pattern이 존재하는지 판단(보이어-무어 알고리즘)
    :param pattern: str
    :param text: str
    :return: bool(0 or 1)
    """
    def isin(char, string):
        """
        오른쪽으로부터 몇번째에 해당 문자가 존재하는지 반환
        :param char: str, length is 1
        :param string: str
        :return:
        """
        for idx, s in enumerate(string[::-1]):
            if s == char:
                return idx
        else:  # 없는 경우
            return -1

    text_len = len(text)
    pattern_len = len(pattern)
    pattern_last = pattern[-1]

    i = 0
    while i <= text_len - pattern_len:

        if text[i + pattern_len - 1] != pattern_last:  # 마지막 문자가 다를 경우
            jump = isin(text[i + pattern_len - 1], pattern)  # pattern의 오른쪽에서 n번째 있는 값
            if jump == -1:  # pattern에 해당 문자가 없으면

                i += pattern_len  # 쭉 건너뛴다
            else:
                i += jump  # jump만큼 건너뛴다

        else:  # 마지막 문자가 같을 경우
            # 해당패턴이 맞는 지확인
            j = pattern_len - 2  # 마지막 문자가 같으므로 살펴볼필요가없다
            while j >= 0:
                if pattern[j] != text[i + j]:
                    break
                j -= 1
            if j == -1:  # 끝까지 다 일치할 경우
                return 1
            jump = isin(text[i + pattern_len - 1], pattern[:-1])
            if jump == -1:
                i += pattern_len
            else:
                i += jump + 1  # +1을 하는 이유는 마지막글자가 일치하므로 제외했기 때문
    return 0
```



### 나중에 속도 Check!