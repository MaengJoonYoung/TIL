# **Perceptron**

<br>

## **1. Perceptron 이란?**

<br>

*퍼셉트론(Perceptron)은 신경망을 이루는 가장 기본 단위로 여러개의 신호를 입력으로 받고 하나의 신호를 출력하는 구조이다.*

<br>

[Perceptrons](https://user-images.githubusercontent.com/89771322/146639543-1b423a99-0e85-451f-9e19-25f168c80b38.jpeg)

<br>

*위의 그림에서 처럼 입력으로 들어온 여러개의 신호에 가중치를 곱하여 가중합을 계산하고, 활성화 함수가 계산된 가중합을 얼마만큼의 신호로 출력할지 결정하여 출력한다.*

<br>
<br>
<hr>
<br>
<br>

## **2. 활성화 함수**

<br>

가중합을 얼마만큼의 신호로 출력할지 결정하는 활성화 함수는 여러가지가 있다. 오늘 내가 배운 활성화 함수는 총 4가지로 Step function, Sigmoid function, ReLU function, Softmax function 이다.

<br>

### *1) 계단 함수 (Step function)*

<br>

[step functions](https://user-images.githubusercontent.com/89771322/146639798-1fd35668-6cf1-49b6-b3d9-7a4cdabc9300.png)

<br>

계단 함수는 입력값이 임계점을 넘으면 1을 출력하고 넘지 않으면 0을 출력하는 함수이다.

계단 함수는 임계점에서는 미분이 불가능하고 나머지 지점에서는 미분값이 0이 되는데, 이로 인해 신경망이 경사 하강법으로 학습하는 데 어려움이 있다. **즉, 계단 함수를 활성화 함수로 사용하면 학습에 어려움이 있다.**

<br>

### *2) 시그모이드 함수 (Sigmoid function)*

<br>

[sigmoid](https://user-images.githubusercontent.com/89771322/146639913-6af8868a-54dd-4144-9804-5e16a60e4484.png)

<br>

위에서 말한 계단 함수의 단점을 보완하기 위해 사용하는 활성화 함수가 바로 시그모이드 함수이다.

시그모이드 함수는 임계값에서 작은 부분은 0에 가까워지고 큰 부분은 1에 가까워진다. 그림에서 볼 수 있듯이 계단 함수에 비해 그래프가 부드럽다. **즉, 모든 지점에서 미분이 가능하다.**

*하지만 시그모이드 함수의 미분값은 0 ~ 0.25의 값을 가지기 때문에 층이 깊어지면 기울기 소실 문제가 발생한다.*

<br>

### *3) ReLU function*

<br>

[relu](https://user-images.githubusercontent.com/89771322/146639979-7885b44b-b0ec-4e1a-af64-ad453453573e.png)

<br>

위에서 말한 시그모이드 함수의 문제점을 해결하기 위해 등장한 것이 바로 ReLU function 이다. ReLU function의 경우 미분값이 0이거나 1이기에 기울기 소실 문제가 발생하지 않는다. *(기울기 소실 문제로 딥러닝으로 복잡한 문제를 해결하는데 어려움을 겪었는데 이를 ReLU functions이 잘 해결해줬다고 한다.)*

ReLU function은 음수인 값은 모두 0으로 출력하고 양수인 값은 값 그대로 출력하는 활성화 함수이다.

<br>

### *4) 소프트맥스 함수 (Softmax function)*

<br>

소프트맥스 함수의 경우 다중 분류 문제를 해결할 때 사용 된다.

소프트맥스 함수는 가중합 값을 입력하면 해당 데이터가 각 클래스에 속할 확률을 반환한다. 예를 들어 3가지의 클래스가 있는 경우, 가중합을 입력값으로 주면 해당 데이터가 1일 경우의 확률, 2일 경우의 확률, 3일 경우의 확률을 반환하고 해당 값들을 모두 더하면 1이된다.

<br>
<br>
<hr>
<br>
<br>

## **3. 인공 신경망**

<br>

[layer](https://user-images.githubusercontent.com/89771322/146640841-6f43585f-4fd6-494e-b678-3549061ea680.jpeg)

<br>

인공 신경망은 실제 사람의 신경계를 모사하여 만든 계산 모델로 **입력층, 은닉층, 출력층** 으로 이루어져 있다. 

*딥러닝은 이러한 인공 신경망 층을 깊게 쌓은 것인데, 층을 깊게 쌓는 이유는 하나의 층으로 해결할 수 없던 복잡한 문제를 해결할 수 있기 때문이다.* 이렇게 여러개의 층으로 쌓아 구축한 신경망을 **다충 퍼셉트론 신경망(Multi-Layer Perceptron, MLP)** 이라고 한다.

<br>

### *입력층*
: 입력층은 데이터셋이 입력되는 층으로 데이터셋의 특성의 개수가 노드의 개수가 된다. 또한 입력층은 계산을 수행하지 않고 단순히 값을 입력하는 역할을 하기 때문에 층을 셀때 포함되지 않는다.

<br>

### *은닉층*
: 입력층과 출력층 사이에 있는 층으로 입력된 신호가 가중치, 편향과 연산이 이뤄지는 층이다. 노드 개수의 경우는 자유롭게 설정한다. 일반적으로 딥러닝이라고 하면 2개 이상의 은닉층을 가지고 있는 것을 말한다. 

<br>

### *출력층*
: 은닉층에서 연산이 된 값들이 출력되는 층으로 **해결하고자 하는 문제에 따라 알맞게 출력층을 설계하는 것이 중요하다.**

<br>

* 이진분류 :
  
  이진분류의 경우 활성화 함수는 시그모이드 함수로 설정하고 출력층 노드는 1개로 설정한다.

* 다중분류 :
  
    다중 분류의 경우 활성화 함수로는 소프트맥스 함수를 사용하고 출력층 노드의 개수는 클래스 개수로 설정한다.

* 회귀 :
  
   회귀 문제의 경우 일반적으로 활성화 함수를 지정해주지 않고, 출력하고자 하는 특성값의 개수만큼 노드를 설정한다.

<br>
<br>
<hr>
<br>
<br>

## **4. 손글씨 MNIST 예제**

<br>

0 ~ 9 까지를 손으로 그린 이미지를 분류하는 문제로 **다중분류 문제**이다. 

***항상 지금 내가 해결하려는 문제가 이진 분류인지 회귀인지 등을 먼저 파악하는 것이 중요하다.***

<br>

*우선 필요한 패키지와 라이브러리를 불러온다.*

<br>

~~~python
import pandas as pd 
import tensorflow as tf
~~~

<br>

*데이터를 불러오고 train과 test 데이터로 나눈다.*

<br>

~~~python
mnist = tf.keras.datasets.mnist

(x_train, y_train), (x_test, y_test) = mnist.load_data()
~~~

<br>

*이미지 데이터를 정규화 해준다.*

<br>

~~~python
x_train, x_test = x_train / 255.0, x_test / 255.0
~~~

<br>

*이제 모델을 구성하고 컴파일한 후 학습시킨다.*

<br>

~~~python
model = tf.keras.models.Sequential([
      tf.keras.layers.Flatten(input_shape=(28,28)),
      tf.keras.layers.Dense(100, activation='relu'), # 은닉층으로 활성화 함수로는 ReLU 함수를 사용했다.
      tf.keras.layers.Dense(10, activation='softmax') # 출력층으로 10개의 클래스이기에 노드의 개수는 10으로 설정했고, 활성화 함수로는 Softmax를 사용했다.
])
~~~

<br>

*Flatten의 경우는 2차원 데이터를 1차원 데이터로 펴주는 역할을 한다. MLP는 2차원 데이터를 처리할 수 없기 때문에 이러한 과정을 거쳐야 한다.*

<br>

~~~python
# 신경망에서 사용할 옵티마이저, 손실함수, 지표를 설정한다.

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])
~~~

<br>

~~~python
model.fit(x_train, y_train, epochs=5) # epochs로 학습횟수 설정.
~~~

<br>

*결과는 아래와 같이 확인할 수 있다.*

<br>

~~~python
model.evaluate(x_test, y_test, verbose=2)
~~~
