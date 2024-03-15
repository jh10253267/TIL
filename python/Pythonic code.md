# Pythonic Code

## What is Pythonic Code??

Pythonic Code는 간단하게, 파이썬 특유의 문법을 활용하여 효율적으로 코드를
표현하는 기법을 말한다.

파이썬스러운 코드라고 할 수 있겠다.

그렇다면 파이썬스러운 코드라는 것이 뭘까?

예를 들어 여러 단어들을 합쳐서 하나의 문자열로 만들고 싶을 때 다음과 같이 코드를 짤 수 있다.

```python
colors = ['red', 'green', 'blue']
for s in colors :
  result += s
```

이것은 파이썬의 join() 함수로 간단하게 처리할 수 있다.

```python
colors = ['red', 'green', 'blue']
result = ''.join(colors)
```

> "Life is short, You need Python" 

간단하고 편리하다.

## Split
```python
items = 'zero one two three'.split()

print(items)

example = 'python,jquery,javascript'

print(example.split(","))

['python', 'jquery', 'javascript']

# 리스트에 있는 각 값을 a,b,c,변수로 unpacking

a, b, c = example.split(",")

example = 'cs50.gachon.edu'

# .을 기준으로 문자열 나누기 unpacking

subdomain, domain, tld = example.split(".")

```


## List Comprehension

기존의 리스트를 사용하여 간단히 다른 리스트를 만드는 기법이다.

예를 들어 일반적인 방식으론 다음과 같이 작성할 것이다.
```python
result = []
for i in range(10) :
  result.append(i)
```

그러나... 만약 다음과 같이 작성한다면?
```python
result = [i for i in range(10)]
```
한줄이면 끝난다.

일반적인 방식보다 빠르다.

이런 뉘앙스의 코드를 리스트 컴프리핸션이라고 한다.



응용해서 특정 조건에 맞는 것들만 리스트에 추가하고 싶을 때(필터링)는 다음과 같이 리스트 컴프리핸션을 응용해서 쓸 수 있다.
```python
result = [i for i in range(10) if i % 2 == 0]
```
짝수들만 리스트 안에 들어가게 된다.
모든 값이 0이고 길이가 prices인 리스트를 만들고 싶을 때answer = [0] * len(prices)

```python
word_1 = "Hello"
word_2 = "World"

# Nested for loop
#이러면 모든 알파벳의 조합을 만들 수 있다.
result = [i+j for i in word_1 for j in word_2]

printr(result)

case_1 = ["A", "B", "C"]
case_2 = ["D", "E", "A"]

result = [i+j for i in case_1 for j in case_2]
print(result)

# 필터추가 만약 i 와 j가 같다면 리스트에 포함하지 않는다.
result = [i+j for i in case_1 for j in case_2 if not(i==j)]
result.sort()

print(result)
```

```python
# 이런식으로 안에 대괄호를 넣어 감싸주면 2차원 리스트가 생성된다.
words = 'The quick brown fox jumps over the lazy dog'.split()
print(words)

stuff = [[w.upper(), w.lower(), len(w)] for w in words]

for i in stuff:
    print(i)
```

## Enumerate & Zip

Enumberate리스트의 각 요소들을 추출할 때 인덱스도 같이 추출한다.

Zip은 두 개의 List값을 병렬적으로 추출한다.

```python
alist = ['a1', 'a2', 'a3']
blist = ['b1', 'b2', 'b3']

for i, (a, b) in enumerate(zip(alist, blist)):
    print(i, a, b)

mylist = ["a", "b", "c", "d"]
list(enumerate(mylist))

# 문장을 List로 만들고 list의 index와 값을 unpacking하여 dict로 저장
{i:j for i, j enumerate('python is easy peasy, lemon squeezy'.split())}

```


