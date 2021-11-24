# **Python 클래스**

오늘은 pythonic하게 python을 사용하는 것들에 대해 배웠다. pythonic은 python 코드를 python 답게 사용하는 것을 말한다. 이런 단어가 있는 건 아무래도 python 개발자들이 공유하는 개발 철학이 있기 때문인 것 같다. 이런 철학을 확인하는 재밌는 방법이 있는데 다음과 같다.

<br>

~~~python
import antigravity # 하늘을 나는 만화가 나옴.

import this # 파이썬 철학과 관련된 시가 출력된다.
~~~

<br>

이 둘의 내용을 대충 종합하자면 파이썬을 공부하는 데 있어서 이론들을 먼저 공부하고 시작하는 것이 아니라 **일단 시작하라는 것**과 코드를 **아름답게? 작성**하라는 것이다.

이러한 것들을과 *'파이썬의 모든 것은 객체로 이루어져 있다'* 라는 말을 기억한 채로 오늘 공부를 시작했다. ~~하지만 모든 것이 객체로 이루어져있다는 말을 아직 이해하지 못했다. 꾸준히 하다보면 언젠가 이해하게 되겠지..~~

<br/>

* * *

## **1. Class와 Instance**
<br>

파이썬의 클래스와 인스턴스를 설명할때 예시로 많이 사용하는 것이 붕어빵이라고 들었고, 이 예시가 효과가 있다고 생각한다. 그러므로 붕어빵으로 한번 클래스와 
인스턴스를 이해해보자. 

<br>

우리가 팥, 슈크림, 고구마무스가 들어간 붕어빵을 만들려고 한다고 해보자. 이때 우리는 팥 붕어빵 틀, 슈크림 붕어빵 틀, 고구마무스 붕어빵 틀로 구분하지 않고 그냥 하나의 붕어빵 틀에 재료를 다르게 해서 붕어빵을 찍어낼 거다. 당연히 이 방법이 훨씬 효율적이기 때문이다. 여기서 **붕어빵 틀이 클래스**이고 **만들어진 다양한 붕어빵이 인스턴스인 것이다.**

<br>

### ***Class는 왜 필요한가?***

<br>

클래스가 필요한 이유는 파이썬은 객체지향언어이고 객체를 다루기 위해서는 클래스에 대해서 알고 있어야 하기 때문이라고 한다. (사실 이 부분은 아직 명확하게 이해가 되지는 않았는데 파이썬을 꾸준히 공부하고 사용하면서 다시 돌아볼 생각이다.)  **객체지향언어는 대규모 프로그램의 설계를 위해 만들어졌는데, 설계자의 입장에서 했던 일을 또 반복하지 않기 위해 개발됐다고 한다.** 이 문장으로 미루어 볼때, 붕어빵 틀인 클래스를 잘 사용한다면 상당히 효율적인 프로그래밍을 할 수 있을 것이라고 생각한다. ~~(이런 것들이 pythonic한건가?)~~

<br>

### ***Class와 Instance를 만들어보자***

<br>

Class는 아래와 같이 만들 수 있다.


<br>

~~~python
class Singer:
    singer_b = BMK
~~~

<br>


이렇게 클래스를 만든 다면 아래와 같은 방식으로 클래스의 속성에 접근할 수 있다.

<br>

~~~python
Singer.singer_b # -> BMK
~~~

클래스 내에서 함수를 선언하는 것도 가능하다.

<br>


~~~python
class Singer:
    singer_b = BMK

    def print_bmk():
        print('I am BMK')
~~~

<br>


이렇게 클래스 안에서 작동하는 함수들을 메서드라고 한다. 하지만 해당 메서드는 클래스를 사용해 인스턴스를 만들지 않을때는 사용이 가능하지만, 인스턴스를 만들면 사용이 불가능하다. 인스턴스에서 해당 메서드를 사용하면 인스턴스를 인수로 넘겨주기 때문이다. 인스턴스를 만든다면 아래와 같은 방법을 활용하면 된다.

<br>


~~~python
class Singer:
    singer_b = BMK

    def print_bmk(self):
        print('I am BMK')
~~~

<br>


self는 다른 단어로 대체해도 무방하다. 이렇게 self를 넣어주고 아래와 같이 코드를 실행하면 잘 작동한다.

<br>


~~~python
singer_a = Singer() # singer_a라는 인스턴스 생성.
singer_a.print_bmk() # -> 'I am BMK'
~~~

<br>

자세히 보면 함수를 사용할때 인수값을 아무것도 넣지 않은 것을 볼 수 있다. 이는 인스턴스가 메서드나 특성을 사용할때 인스턴스에 대한 참조값을 자동으로 넘기기 때문이다. 하지만 자동으로 해당 참조값을 받지 않는데, 이러한 이유로 self를 넣어줘야 메서드가 잘 작동하게 되는 것이다.

<br>


### ***생성자 함수***

<br>

생성자 함수는 인스턴스를 만들때 자동으로 호출 되는 함수이다.

<br>

~~~python
class Singer:
    def __init__(self, singer_b = 'BMK'):
        self.singer_b = singer_b

singer_a = Singer()
print(singer_a.singer_b) # -> 'BMK'
~~~

<br>

위와 같이 생성된 인스턴스는 클래스의 생성자 함수에 따라 인스턴스의 초기 속성을 받을 수 있게 된다.

<br>

### ***@properity***

<br>

만약 클래스의 변수에 직접 접근하기 싫을 때는 getter, setter라는 기능을 사용하는데 이를 좀더 간편하고 읽기 편하게 해주는 기능으로 **@properity**가 있다.

<br>

설명을 위해 국가와 도시를 받고 연결하여 만든 핫플레이스라는 특성을 가진 말도 안되는 클래스를 생각해보자ㅎㅎ..

<br>

~~~python
class HotPlace:
    def __init__(self, country, city)
    self.country = country
    self.city = city
    self.hotplace = self.country + ' ' + self.city
~~~

<br>

여기서 도시명을 바꿔서 핫플레이스를 바꾸고자 아래와 같은 코드를 실행했다고 생각해보자.

~~~python
seoul = HotPlace(Korea, Seoul)

seoul.city = 'Busan' # 도시를 부산을 바꿔보자
print(seoul.city) # -> 'Busan'
print(seoul.hotplace) # -> 'Korea Seoul'
~~~

<br>

이 경우 생성자 함수가 실행될때 self.hotplace가 정해졌기에 도시만 바꾼다고 해서 바꿔지지 않는다. 이런 경우 메서드를 사용해서 해결할 수 있다.

<br>

~~~python
class HotPlace:
    def __init__(self, country, city)
    self.country = country
    self.city = city

    def hotplace(self)
    return = self.country + ' ' + self.city
~~~

위의 방법을 통해 아래와 같은 결과를 얻을 수 있다.

<br>

~~~python
seoul = HotPlace(Korea, Seoul)

seoul.city = 'Busan' 
print(seoul.city) # -> 'Busan'
print(seoul.hotplace) # -> 'Korea Busan'
~~~

<br>

이러한 접근 방식은 클래스의 특성이 아닌 메서드를 사용하여 접근해야 되는데, 
~~~python
@properity
def hotplace(self)
    return = self.country + ' ' + self.city
~~~
이렇게 @properity 사용하면 메서드를 클래스의 특성처럼 접근하면서 동일한 목적을 달성할 수 있다. 

<br>

여기서 더 나아가서 @properity 사용하여 hotplace를 바꿔서 city를 바꾸는 과정을 살펴보자. 이 과정을 살펴보면서 특성을 가져오거나 값을 설정할 때 사용하는 메소드를 통해서 더 섬세한 관리를 할 수 있는 @properity 기능을 알아보자.

<br>

~~~python
class HotPlace:

    def __init__(self, country, city):
    self.country = country
    self.city = city

    @properity
    def hotplace(self):
    return = self.country + ' ' + self.city

    @hotplace.setter
    def hotplace(self, new_hotplace):
    country, city = new_hotplace.split()
    self.country = country
    self.city = city


seoul = HotPlace(Korea, Seoul)

seoul.hotplace = 'Korea Busan' # setter 매소드를 특성처럼 취급해 가져옴.
print(seoul.city) # -> 'Busan'
print(seoul.hotplace) # -> 'Korea Busan'
~~~

*이렇게 변수에 직접 접근하지 않는 이유는 내부적으로 오류가 생길 가능성도 있고, 직접 변수에 접근하는 방식은 외부에 데이터가 무방비로 노출 된다는 위험도 있기 때문이라고 한다.*