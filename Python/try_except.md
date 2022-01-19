# **예외 처리**

<br>

'예외 처리'에서 **예외**는 우리가 흔히 말하는 **에러**를 생각하면 된다. **즉, 프로그램이 정상적으로 작동할 수 없는 상태를 예외라고 말한다.**

python에서는 **try~except**를 사용하여 예외를 처리할 수 있다. 사실 프로그램을 예외가 발생하지 않게 확실히 만들면 예외를 처리할 필요가 없지 않나라고 생각할 수 있지만, 예외는 **외부 요인**으로 인해서 발생할 수도 있기 때문에 이러한 문제를 해결하기 위해 예외 처리를 사용하는 것이다. 

>여기서 외부 요인은 사용자가 0 이상인 숫자를 입력해야 하는 프로그램에서 실수로 0 미만인 숫자를 입력하는 등의 상황을 생각해볼 수 있다.

<br>

## **1. try, except**

<br>

try는 말 그대로 시도해보는 것이다. 즉, try 구문 안에 오류가 생길 것 같은 코드를 기입하고 한번 시도해보는 것이다. 이때 try 구문을 실행 중 오류가 생기면 except 문으로 넘어가게 된다. 구조는 다음과 같다.

<br>

```python
try:
    실행 코드(오류가 생길 것 같은)
except:
    오류 발생시 어떻게 할 것인지.
```

>*위와 같은 구조에서는 **오류 종류의 상관없이** except문을 수행한다.* 

<br>

*특정 오류 종류에 대해서만 except문을 수행하고 싶을땐 아래와 같이 작성하면 된다.*

<br>

```python
try:
    ...
except 발생 오류:

# 오류 메세지도 확인하는 경우

try:
    ...
except 발생 오류 as 메세지 변수:
    print(메세지 변수)

# except 여러번 사용 가능

try:
    ...
except 발생 오류 1 as 메세지 변수 1:
    print(메세지 변수 1)
except 발생 오류 2 as 메세지 변수 2:
    print(메세지 변수 2)

```

<br>

## **2. else**

<br>

try 문에서 else 절을 사용할 수 있다. else 절의 경우 **try 문에서 오류가 나지 않았을 시 실행된다.**

<br>

>try에서 오류가 일어나지 않으면 else 절 사용하지 말고 그냥 try 문에 다 작성하면 되는 거 아닌가? <br>
→ 의도치 않은 예외처리를 방지하기 위해서 else절을 사용한다. 예외 처리될 코드와 다음에 실행될 코드를 명시적으로 구분하기 위함.

<br>

*사용자가 숫자를 입력하면 해당 값으로 4를 나누고 결과를 반환해주는 프로그램을 예시로 들어보면 다음과 같다.*

```python
try:
    num = int(input('숫자를 입력하세요'))
    result = 4/num
except ZeroDivisionError as z:
    print(z)
else: # 오류가 발생하지 않았을 때 수행
    print(result)
```

> *물론, 이 경우에 '삼십' 처럼 문자를 입력하는 경우에 발생하는 오류는 지정하지 않았기에 문자를 입력할 경우 오류가 발생한다.*

<br>

## **3. finally**

<br>

finally는 try 문에서 오류가 발생하던 발생하지 않던 실행되는 코드이다. **오류 발생 여부와 관계없이 실행되야하는 코드**가 있는 경우 사용한다.

<br>

## 4. raise, assert

<br>

파이썬 상에서는 오류가 아니지만 코드 작성자가 오류를 정의하여 오류를 발생시키는 방법도 있다. 이를 `사용자 정의 예외 처리` 라고 하고 이때 사용하는 것이 **raise와 assert**이다.

<br>

*raise는 다음과 같이 사용할 수 있다.*

```python
ry:
    num = int(input('숫자를 입력하세요'))
    if num <= 0:
        raise ValueError # 입력값이 0보다 작으면 ValueError 발생시킴.
    result = 4/num
except:
    print('1 이상의 숫자를 입력해주세요')
else: # 오류가 발생하지 않았을 때 수행
    print(result)
```
> *사용자가 0 이하의 숫자를 입력하면 raise로 인해 오류가 발생하여 except 문을 실행한다.*

<br>

*assert의 경우 작성한 조건이 `True` 가 아닌 경우 AssertError를 발생시킨다.*

<br>

```python
try:
    num = int(input('숫자를 입력하세요'))
    assert num > 0
    result = 4/num
except:
    print('1 이상의 숫자를 입력해주세요')
else: # 오류가 발생하지 않았을 때 수행
    print(result)
```
> *위와 마찬가지로 0 이하의 숫자를 입력하면 오류를 발생시킨다.*

<br>
<br>
<hr>

### *참고자료:*
https://python.bakyeono.net/chapter-9-3.html <br>
https://wikidocs.net/30#try-finally
