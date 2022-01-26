# **재귀 함수**

<br>

재귀 함수는 **함수를 정의할때 자기 자신을 호출하여 정의하는 것을 말한다.** 이렇게 자기 자신을 호출하는 방식을 통해 **반복작업을 수행할 수 있다.**

<br>

## **1. 재귀 함수 규칙**

<br>

*재귀 함수를 사용할때 아래 규칙들을 잘 지켜야 한다.*

<br>

### *1) base case(기본 케이스 또는 조건)이 있어야 한다.*

<br>

base case는 **반복 작업을 종료하는 조건**이라고 할 수 있다. 재귀 함수는 자기 자신을 호출하면서 반복 작업을 수행하는데 이 반복 작업을 끝내줄 종료 조건이 있어야 무한 반복에 빠지는 걸 방지할 수 있다. 그렇기에 base case는 반드시 있어야 한다. 만약, base case가 없다면 아래 그림처럼 무한 반복에 빠지게 될 것이다.

<br>

![재귀 무한반복](https://user-images.githubusercontent.com/89771322/151159884-12a99c35-b6cd-4405-99fe-2336c57d4d05.jpeg)

<br>

또한 재귀 함수는 함수의 호출과 관계되는 지역변수와 매개변수가 저장되는 메모리 공간인 스택을 사용하는데, 무한 반복에 빠지면 스택 오버 플로우 문제가 발생하게 된다. 즉, 스택에 메모리가 너무 많이 쌓여서 흘러 넘치게 된다.

파이썬의 경우 이렇게 무한 반복에 빠지지 않더라도 **재귀 함수의 호출을 1,000번까지로 제한한다.** 이 횟수를 초과하면 `RecursionError`가 발생하기 때문에 이를 유념하고 재귀를 사용해야 한다. *(제한 횟수를 바꾸고 싶다면 아래와 같은 코드를 사용해 제한 횟수를 수정할 수 있다.)*

```python
import sys
sys.setrecursionlimit(횟수)
```

<br>

*base case의 예는 다음과 같다.*

<br>

```python
# 1부터 num까지 합하는 함수
def sum_num(num):
    
    # base case : num이 1 이하가 되면 1을 반환하고 작업을 종료.
    if num <= 1:
        return 1
    # 추가 조건
    else:
        return num + oneto100(num-1)
```

<br>

### *2) 반드시 자기 자신을 호출한다.*

<br>

당연한 말이지만 재귀는 자기 자신을 호출하여 반복작업을 수행하는 방법이기 때문에, **반드시 자기 자신을 호출해야한다.**

<br>

## **2. 재귀와 반복문**

<br>

재귀 함수는 위에서도 말했듯 반복 작업을 수행한다. **즉, 재귀를 사용해 해결가능한 문제는 반복문으로도 해결이 가능하고 그 역도 성립한다.**

재귀는 반복문에 비해 **처리 속도가 느리다.** 또한 재귀는 재귀의 깊이가 너무 깊어지면 **스택 오버 플로우가 발생하여 프로그램이 비정상적으로 중단될 수 있지만,** 반복문의 경우 지역 변수들이 호출될때 **한번만 메모리에 할당 되기에 재귀와 같은 비효율이 발생하지 않는다.**

***그렇다면 재귀 함수를 사용하는 이유는 무엇일까?***

재귀 함수를 사용하는 경우 똑같은 문제를 반복문으로 풀었을때 보다 **코드의 길이도 줄어들고 변수의 수도 줄어들어 반복문에 비해 가독성이 좋다.** 또한 변수가 줄어든다는 것은 변경 가능한 상태가 줄어든다는 말이다. 즉, 프로그램에 오류가 발생할 가능성이 줄어든다는 얘기다.

<br>

각자 가지고 있는 장단점이 확실하기 때문에, 무조건 어느 하나가 좋은 방법이다 라고 말하기는 힘들 것 같다. ***다만 이러한 재귀와 반복문의 특징을 잘 파악하여 알맞는 상황에 적절히 사용하는 것이 중요할 것 같다.***

<br>
<br>
<hr>

### *참고자료*

https://tecoble.techcourse.co.kr/post/2020-04-30-iteration_vs_recursion/ <br>
https://hazel-developer.tistory.com/173 <br>
http://www.tcpschool.com/c/c_memory_structure