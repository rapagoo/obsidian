## 포인터의 정의
포인터는 데이터의 주소를 저장하는 주소 변수이다.
데이터 자체를 저장하는 변수가 아니다.
포인터는 데이터에 간접적으로 접근할 때 사용된다.
간접적으로 접근하는 방식이 왜 필요할까?
메모리는 세 구역으로 나뉜다. heap, stack, code section
프로그램은 stack 과 code 에 직접적으로 접근이 가능하다.
heap에는 직접 접근이 불가능하다. 
그래서 heap 에 접근하려면 포인터를 사용한다.
포인터를 사용하는 이유 중 하나는 heap 메모리에 접근하기 위해서이다.
포인터는 프로그램 외부에 있는 자원에 접근할 때 용이하다.
예를 들어 하드 디스크에 있는 정보에 접근할 때도 포인터를 사용한다.
또 하나는 parameter passing 이 있다.
이렇게 포인터의 주 용도는 1. heap에 접근 2. 외부 자원에 접근 3. parameter passing 이다.

# Section 4: Introduction
## 40. Stack vs Heap Memory
static vs dynamic memory allocation에 대해 설명한다
이 강의에서는 모든 메모리를 64KB로 가정하고 진행한다. 0~65535
main memory는 세가지로 나뉜다. heap, stack, code section
하드 디스크에 있는 프로그램의 코드가 먼저 code section에 로드된다.
그리고 나서 cpu가 그 프로그램을 실행한다.
이제 stack 과 heap이 어떻게 동작하는지 알아보자.
프로그램 내에 선언한 변수들은 stack에 배정된다.
컴파일 타임에 메모리 배정이 결정되는 것이 static memory allocation 이다.
컴파일 타임에 결정되는 것을 static 이라고 부른다.
## 41. Stack vs Heap. Continued...
heap에 접근하는 방법은? 포인터를 사용한다
heap 메모리는 자원처럼 다뤄져야 한다.
그래서 메모리가 필요 없으면 자원 할당을 해제해야 한다.
포인터 변수를 stack에 할당하고 그 포인터 변수를 활용해 heap 메모리에 자원을 생성해 이용하는 것이 dynamic memory allocation 이다.
## 42. Physical vs Logical Data Structures
data structure에는 두가지 종류가 있다.
physical / logical
physical 의 예로는 array 와 linked list 가 있다.
이들은 메모리가 어떻게 정렬되어야 하는지 정의하기 때문에, 실제로 저장하기 때문에 physical 이다.
logical 의 예로는 stack, queue, tree, graph, hash table이 있다.
logical 은 데이터의 삽입과 삭제를 어떤 규율에 따라 처리할 것인지 정하는 것이다.
이 규칙이 data structure 에 따라 결정된다.
logical data structure 는 physical data structure 에 의해 구현된다.
## 43.ADT
abstract data type - 추상 자료형
내부적으로 어떻게 작동하는지는 우리가 걱정할 필요가 없다. 추상 자료형이니까
## 44. Time and Space Complexity
n 이란 그 알고리즘이 처리하는 입력 데이터의 크기이다
예를 들어 배열이라면, n은 원소의 개수이다
for loop의 시간 복잡도는 O(n) 이다. 
코드의 절차를 이해하면 시간 복잡도에 대한 이해는 쉽다
nested for loop 라면 시간 복잡도는 O(n^2) 가 된다.
특정 알고리즘을 수학적으로 계산했을 때, 시간 복잡도는 다항식에서 최고차항(가장 큰 차수)의
영향을 받아 결정된다.
O(log n) 은 매번 선택할 때마다 남은 데이터가 반씩 줄어서,전체 데이터를 다 확인하지 않아도 
금방 답을 찾는 경우에 나타나는 시간 복잡도이다.
## 45. Time and Space Complexity from Code
첫 번째 예시
```C++
void swap(x, y)
{
	int t;
	t = x;
	x = y;
	y = t;
}
```
이 코드에서는 항상 3번의 연산이 실행되고, 입력과 상관없이 연산 횟수가 동일하다.
그러므로, 이 함수의 시간 복잡도는 상수 시간 복잡도, O(1)이다.
***
두 번째 예시
```C++
int sum(int A[], int n)
{
	int s, i;
	s = 0;
	for(i=0;i<n;i++)
	{
		s=s+A[i];
	}
	return s;
}
```
for 문이 한 개 있고, 0부터 n-1까지 반복되며 반복마다 연산 하나가 실행된다.
전체 반복 횟수가 n번이므로 전체 시간 복잡도는 O(1) x n = O(n)이 된다.
***
세 번째 예시
```C++
void add(int n)
{
	int i, j;
	for (i=0;i<n;i++)
	{
		for (j=0;j<n;j++)
		{
			c[i][j]=a[i][j]+b[i][j];
		}
	}
}
```
바깥쪽 for loop가 n번 반복하고, 그 각각의 반복마다 for loop가 또 n번 반복된다.
그래서 총 연산 횟수는 `n*n` 에 비례하여 시간 복잡도는 `O(n^2)` 가 된다.
# Section 5: Recursion
## 46. How Recursion Works(Tracing)
```C++
void fun1(int n)
{
	if (n>0)
	{
		printf("%d", n);
		fun1(n-1);
	}
}

void main()
{
	int x=3;
	fun1(x);
}
```
재귀 추적 트리를 이용해 호출 과정을 시각적으로 이해해 본다.
```
fun1(3)
| 
+-- printf("3")
| 
+-- fun1(2) 호출
	|
	+-- printf("2")
	|
	+-- fun1(1) 호출
		|
		+-- printf("1")
		|
		+-- fun1(0) 호출
			| +-- (n > 0 조건이 거짓이므로 아무것도 안 하고 종료)
```
이런 식으로 진행이 된다는 것을 알 수가 있다.
```C++
void fun2(int n) 
{
	if (n > 0)
	{ 
		fun2(n - 1); // 재귀 호출을 먼저 하고
		printf("%d", n); // 출력을 나중에 합니다.
		}
	}
```
만약 이러한 함수를 재귀 호출했다면?
printf 가 재귀 호출 다음에 있기 때문에, 가장 깊은 곳까지 호출되었다가 다시 되돌아오면서 출력이 일어난다. 그래서 출력값은 123.
## 47. Generalising Recursion
```C++
void fun(int n)
{
	if (n>0)
	{
		1.________
		2.fun(n-1)
		3.________
	}
}
```
이런 식으로 재귀함수가 있다면, 1번 부분은 Calling time에 실행되고, 3번은
Returning time에 실행된다.
재귀함수가 반복문과 다른 점은, 반복문은 ascending phase 만 존재하지만
재귀함수는 descending phase가 존재한다.
## 48.How Recursion uses Stack
재귀함수가 어떻게 stack에서 동작하는지 살펴보았다.
재귀함수는 n이 주어지면 n+1 번 stack에서 호출된다.
시간복잡도는 O(n) 이 된다.
## 49. Recurrence Relation - Time Complexity of Recursion
