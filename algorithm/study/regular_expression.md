# 정규표현식 정리
Python docs : https://docs.python.org/3/library/re.html?highlight=re#module-re<br>
wikidocs : https://wikidocs.net/4308

## 메타문자
특별한 용도로 사용되는 문자들

`. ^ $ * + ? { } [ ] \ | ( )`

- `[]`

  - [] 사이의 문자들과 매치
  - `-`를 사용하면 범위를 나타냄
  - ^를 내부에 사용하면 not을 의미

  ex) [abc]라는 패턴은 a, before 등과 매치

  [0-5] : [012345], [a-zA-Z] : 알파벳 전체

  [^0-9] : 숫자가 아닌것

- `.`

  - 모든 문자를 의미

  ex) a.b 는 acb 와 매칭

  a[.]b 는 a.b와 매칭

- `*` : 반복(0번이상)을 의미

- `+` : 반복(1번이상)을 의미

- `{a,b}` : 반복을 a번이상 b번이하로({1,}은 +와 동일)

  `{a}`는 반복을 a번

  ex) ca{2,}b 는  caab와 매칭

- `?` : {0, 1}과 동일

**zerowidth assertions 메타문자**

- `|` : or로 사용

  ex) Crow|Servo : Crow123  과 매칭

- `^` : 문자열의 맨 처음, re.MULTILINE사용시 여러줄의 문자열에서 각 줄마다 적용

- `$` : 문자열의 맨 끝과 매칭 

  ex) short$ 라는 패턴은 Lift is too short 와 매칭

- `\A` : `^`와 동일하지만 re.MULTILINE 사용해도 첫번째 문자와만 매칭

- `\Z` : `$`와동일하며 re.MULTILINE 사용해도 마지막 문자와만 매칭

- `\b` : 단어구분자, `\B`는 공백 없어야됨(\b랑 반대)

  ```Python
  p = re.compile(r'\bclass\b')  # r을 써야함 '\b' = '\x08'(백스페이스) 이기때문
  # or p = re.compile('\\bclass\\b')
  print(p.search('no class at all'))  # class와 매칭
  ```

**grouping**

`()` 를 이용해 반복을 처리하거나 특정 부분의 문자열만 뽑아내기위해 사용

```Python
p = re.compile(r"(\w+)\s+\d+[-]\d+[-]\d+")
m = p.search("park 010-1234-1234")
print(m.group(1))  # park, group(0)은 매칭된 전체 문자열
```



## Re 모듈의 메소드s

- compile() : 패턴을 컴파일
- match() : 문자열의 처음부터 정규식과 매치되는지 조사
- search() : 문자열 전체 검색해 정규식과 매치되는지 조사
- findall() : 정규식과 매치되는 모든 문자열을 리스트로 반환, iterable한 객체를 리턴하려면 finditer()

```Python
import re
pattern = re.compile('a.b')
re.match(pattern, 'cabbc')  # 매칭X - 처음부터 탐색하기때문에
re.search(pattern, 'cabbbc')  # 매칭O
```

```Python
pattern = re.compile('[abc]+')
re.findall(pattern, 'qaqbcceaab')  # ['a', 'bcc', 'aab'] 반환
pattern = re.compile('[abc]')
re.findall(pattern, 'qaqbcceaab')  # ['a', 'b', 'c', 'c', 'a', 'a', 'b'] 반환
```

- pattern.sub

```Python
p = re.compile('(blue|white|red)')
p.sub('colour', 'blue socks and red shoes')  # 'colour socks and colour shoes'
p.sub('colour', 'blue socks and red shoes', count=1)  # 한번만 바꿈

```



**match 객체의 메소드**

match함수가 반환하는 객체는 다음과같은 메소드를 사용할 수 있음

- group() : 매치된 문자열 반환
- start(), end(), span() : 시작위치, 끝위치, (시작, 끝)

**compile 옵션**

- DOTALL(S) : `.`은 `\n`을 제외한 모든문자와 매칭된다. `re.compile('a.b', re.DOTALL)` 옵션을 사용하면 `\n`도 포함됨

- IGNORECASE(I) : 대소문사 관계없이 매칭

- MULTILINE(M) : 여러줄과 매칭

  ```Python
  import re
  p = re.compile("^python\s\w+", re.MULTILINE)
  
  data = """python one
  life is too short
  python two
  you need python
  python three"""
  
  print(p.findall(data))
  ```

- VERBOSE(X) : 줄단위로 구분해서 작성가능

  ```Python
  charref = re.compile(r'&[#](0[0-7]+|[0-9]+|x[0-9a-fA-F]+);')  # 복잡함
  charref = re.compile(r"""
   &[#]                # Start of a numeric entity reference
   (
       0[0-7]+         # Octal form
     | [0-9]+          # Decimal form
     | x[0-9a-fA-F]+   # Hexadecimal form
   )
   ;                   # Trailing semicolon
  """, re.VERBOSE)
  ```

**백슬래시 문제**

`\section` 이라는 것을 찾기위해서는 pattern을 `\\section`으로 바꿔야됨

## 탐색

- 긍정형 전방 탐색(`(?=...)`) - `...` 에 해당되는 정규식과 매치되어야 하며 조건이 통과되어도 문자열이 소비되지 않는다.

  `.*[.]([^b].?.?|.[^a]?.?|..?[^t]?)$` : 확장자가 bat인 파일 제외

- 부정형 전방 탐색(`(?!...)`) - `...`에 해당되는 정규식과 매치되지 않아야 하며 조건이 통과되어도 문자열이 소비되지 않는다.

  `.*[.](?!bat$).*$` : 마찬가지