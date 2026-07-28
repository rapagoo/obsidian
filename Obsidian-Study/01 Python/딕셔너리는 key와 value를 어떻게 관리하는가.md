# 딕셔너리는 key와 value를 어떻게 관리하는가?

## 한 줄 설명

딕셔너리는 의미를 나타내는 `key`와 실제 값인 `value`를 한 쌍으로 저장한다.

## 왜 필요한가

리스트의 위치만으로는 값이 무엇을 뜻하는지 알기 어렵다. 딕셔너리는 `"이름"`, `"나이"`처럼 이름을 붙여 데이터의 의미를 분명하게 만든다.

## 핵심 코드

```python
person = {"이름": "나예호", "나이": 20, "MBTI": "ENFJ"}

print(person["MBTI"])
print(person.get("전화번호"))  # None

for key, value in person.items():
    print(key, value)
```

## 꼭 기억할 점

- `dic[key]`는 없는 키일 때 `KeyError`를 발생시킨다.
- `dic.get(key)`는 없는 키일 때 기본적으로 `None`을 반환한다.
- `keys()`, `values()`, `items()`로 키·값·쌍을 순회할 수 있다.
- `in`은 기본적으로 값이 아니라 키의 존재를 검사한다.

## 관련 노트

- [[API 요청과 응답은 어떻게 동작하는가]]
- [[Series와 DataFrame은 어떻게 다른가]]

## 출처

- `day0720.ipynb`
- `인공지능_프로그래밍_3일차_온디1기.pdf`, 2~20쪽

