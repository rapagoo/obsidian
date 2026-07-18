# Python 코딩 테스트 학습 노트

코딩 테스트 문제를 풀거나 Python을 사용하면서 새롭게 알게 된 문법, 함수, 활용법을 정리하는 문서입니다.

## 1. 문자열(String)

### 1.1 문자열 분리: `split()`

문자열을 구분자 기준으로 나누어 리스트로 반환합니다.

```python
문자열.split(구분자, 최대_분리_횟수)
```

#### `split()`과 `split(' ')`의 차이

- `split()`: 연속된 공백을 하나의 구분자로 처리하며, 문자열 앞뒤의 공백도 무시합니다.
- `split(' ')`: 스페이스 한 칸을 정확한 구분자로 사용하므로 연속된 공백 사이에 빈 문자열이 생길 수 있습니다.

```python
text = "Python   is   fun"

print(text.split())
# ['Python', 'is', 'fun']

print(text.split(' '))
# ['Python', '', '', 'is', '', '', 'fun']
```

구분자와 최대 분리 횟수를 지정할 수도 있습니다.

```python
data = "apple,banana,cherry,orange"
result = data.split(',', 2)

print(result)
# ['apple', 'banana', 'cherry,orange']
```

코딩 테스트에서 공백으로 구분된 입력을 받을 때는 보통 인자 없는 `split()`을 사용합니다.

### 1.2 문자열 결합: `join()`

문자열 리스트의 요소들을 지정한 구분자로 연결합니다.

```python
words = ['Python', 'is', 'fun']

print(' '.join(words))   # Python is fun
print('-'.join(words))   # Python-is-fun
print(''.join(words))    # Pythonisfun
```

`join()`은 연결할 구분자 문자열을 기준으로 호출한다는 점에 주의합니다.

### 1.3 문자열 정제와 변환

#### `strip()`

문자열 앞뒤의 공백 또는 지정한 문자를 제거합니다. 문자열 중간에 있는 문자는 제거하지 않습니다.

```python
text = "  hello  "
print(text.strip())  # hello

value = "---hello---"
print(value.strip('-'))  # hello
```

왼쪽만 제거하려면 `lstrip()`, 오른쪽만 제거하려면 `rstrip()`을 사용합니다.

#### `replace()`

문자열 안의 특정 문자열을 다른 문자열로 교체합니다. 원본 문자열은 바뀌지 않고 새 문자열을 반환합니다.

```python
text = "Hello World"
result = text.replace("World", "Python")

print(result)  # Hello Python
```

#### `upper()` / `lower()`

문자열을 모두 대문자 또는 소문자로 변환합니다.

```python
text = "Python"

print(text.upper())  # PYTHON
print(text.lower())  # python
```

### 1.4 인덱스와 값을 함께 순회: `enumerate()`

반복 가능한 객체를 순회하면서 인덱스와 값을 동시에 얻을 수 있습니다.

```python
text = "hello"

for index, char in enumerate(text):
    print(f"Index: {index}, Value: {char}")
```

인덱스를 1부터 시작하고 싶다면 `start` 값을 지정합니다.

```python
for index, char in enumerate(text, start=1):
    print(index, char)
```

## 2. 입출력(I/O)

### 2.1 기본 입력: `input()`

`input()`은 한 줄을 문자열로 입력받고, 마지막 줄바꿈 문자는 제거합니다.

```python
name = input()
a, b = map(int, input().split())
```

### 2.2 빠른 입력: `sys.stdin.readline()`

입력량이 많은 문제에서는 `input()`보다 빠른 `sys.stdin.readline()`을 자주 사용합니다.

```python
import sys

a, b = map(int, sys.stdin.readline().split())
```

`split()`은 공백과 줄바꿈을 함께 처리하므로 숫자 입력을 나눌 때 별도로 `strip()`을 호출하지 않아도 됩니다.

한 줄 전체를 문자열로 사용할 때는 끝에 포함된 줄바꿈 문자(`\n`)를 제거해야 할 수 있습니다.

```python
import sys

text = sys.stdin.readline().rstrip('\n')
```

일반적으로 `rstrip()`을 인자 없이 호출하면 오른쪽의 모든 공백까지 제거됩니다. 입력값 끝의 공백을 보존해야 한다면 `rstrip('\n')`처럼 제거할 문자를 명시합니다.

## 3. 순서 뒤집기

### 3.1 리스트 자체를 뒤집기: `reverse()`

`list.reverse()`는 리스트 요소의 순서를 **원본 리스트에서 직접** 뒤집습니다. 새로운 리스트를 만들지 않으며 반환값은 `None`입니다.

```python
numbers = [1, 2, 3, 4]
result = numbers.reverse()

print(numbers)  # [4, 3, 2, 1]
print(result)   # None
```

따라서 아래처럼 반환값을 변수에 저장하면 뒤집힌 리스트가 아니라 `None`이 저장됩니다.

```python
numbers = [1, 2, 3]
numbers = numbers.reverse()  # 잘못된 사용

print(numbers)  # None
```

`reverse()`는 리스트의 메서드이므로 문자열이나 튜플에는 사용할 수 없습니다.

### 3.2 역순 반복자 만들기: `reversed()`

`reversed()`는 원본을 변경하지 않고 요소를 역순으로 꺼내는 **반복자(iterator)**를 반환합니다.

```python
numbers = [1, 2, 3, 4]
result = reversed(numbers)

print(numbers)        # [1, 2, 3, 4] (원본 유지)
print(list(result))   # [4, 3, 2, 1]
```

결과가 필요하면 목적에 맞게 `list()`, `tuple()`, `join()` 등으로 변환합니다.

```python
numbers = [1, 2, 3]
reversed_numbers = list(reversed(numbers))
print(reversed_numbers)  # [3, 2, 1]

text = "python"
reversed_text = ''.join(reversed(text))
print(reversed_text)  # nohtyp
```

반복자이므로 `for`문에서는 변환 없이 바로 사용할 수 있습니다.

```python
for number in reversed([1, 2, 3]):
    print(number)
```

### 3.3 핵심 차이

| 구분 | `reverse()` | `reversed()` |
| --- | --- | --- |
| 형태 | 리스트 메서드 | 내장 함수 |
| 원본 변경 | 변경함 | 변경하지 않음 |
| 반환값 | `None` | 역순 반복자 |
| 사용 대상 | 리스트 | 리스트, 문자열, 튜플, `range` 등 |

새로운 역순 리스트가 필요할 때는 슬라이싱도 사용할 수 있습니다.

```python
numbers = [1, 2, 3, 4]
reversed_numbers = numbers[::-1]

print(reversed_numbers)  # [4, 3, 2, 1]
print(numbers)           # [1, 2, 3, 4]
```

- 원본 리스트 자체를 바꿀 때: `numbers.reverse()`
- 원본을 유지하며 역순으로 순회할 때: `reversed(numbers)`
- 역순으로 복사된 리스트가 바로 필요할 때: `numbers[::-1]`

## 4. 모든 요소에 함수 적용하기: `map()`

`map()`은 반복 가능한 객체의 각 요소에 같은 함수를 적용합니다.

```python
map(함수, 반복_가능한_객체)
```

`map()`의 결과는 리스트가 아니라 **반복자(iterator)**입니다. 결과 전체가 필요하면 `list()`, `tuple()` 등으로 변환합니다.

```python
numbers = ['1', '2', '3']
result = map(int, numbers)

print(result)        # map 객체
print(list(result))  # [1, 2, 3]
```

### 4.1 코딩 테스트 입력 처리

공백으로 구분된 여러 정수를 입력받을 때 가장 자주 사용합니다.

```python
a, b = map(int, input().split())
```

처리 과정은 다음과 같습니다.

```python
# 입력: 10 20
input()                # "10 20"
input().split()        # ['10', '20']
map(int, input().split())  # 각 문자열에 int 적용
```

입력 개수가 정해져 있지 않아 리스트로 보관하려면 `list()`로 변환합니다.

```python
numbers = list(map(int, input().split()))
```

구조 분해 할당을 할 때는 `map` 객체를 리스트로 변환하지 않아도 됩니다.

```python
a, b, c = map(int, input().split())
```

단, 입력값의 개수가 변수 개수와 다르면 `ValueError`가 발생합니다.

### 4.2 직접 만든 함수 적용하기

```python
def square(number):
    return number ** 2

numbers = [1, 2, 3, 4]
squares = list(map(square, numbers))

print(squares)  # [1, 4, 9, 16]
```

간단한 처리는 `lambda`와 함께 사용할 수도 있습니다.

```python
numbers = [1, 2, 3, 4]
squares = list(map(lambda number: number ** 2, numbers))

print(squares)  # [1, 4, 9, 16]
```

같은 결과를 리스트 컴프리헨션으로 표현하면 다음과 같습니다.

```python
squares = [number ** 2 for number in numbers]
```

복잡한 조건이나 계산이 포함되면 리스트 컴프리헨션이 더 읽기 쉬울 수 있습니다.

### 4.3 여러 반복 객체 함께 처리하기

`map()`에 반복 가능한 객체를 여러 개 전달하면 함수에도 여러 인자가 전달됩니다.

```python
def add(a, b):
    return a + b

left = [1, 2, 3]
right = [10, 20, 30]

result = list(map(add, left, right))
print(result)  # [11, 22, 33]
```

길이가 서로 다르면 가장 짧은 객체의 길이까지만 처리합니다.

```python
left = [1, 2, 3]
right = [10, 20]

result = list(map(lambda a, b: a + b, left, right))
print(result)  # [11, 22]
```

### 4.4 반복자 사용 시 주의점

`map` 객체는 한 번 꺼낸 요소를 다시 사용할 수 없습니다.

```python
result = map(int, ['1', '2', '3'])

print(list(result))  # [1, 2, 3]
print(list(result))  # []
```

결과를 여러 번 사용해야 한다면 처음부터 리스트로 저장합니다.

```python
result = list(map(int, ['1', '2', '3']))
```
