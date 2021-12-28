# **RNN & LSTM**

<br>

신경망 언어 모델은 횟수 기반이 아니라 임베딩 벡터를 사용하기때문에 기존 말뭉치에 등장하지 않는 단어라도 의미적, 문법적으로 유사한 단어라면 선택될 수 있다. **RNN**은 인공 신경망 언어 모델에서 사용하는 **순환 신경망**이다.

<br>

## **1. RNN (Recurrent Neural Network)**

<br>

RNN은 **Sequential Data(연속형 데이터)를 다루기 위해 등장**했다. 연속형 데이터는 **순서 정보가 있는 데이터**를 말한다. 시계열 데이터, 자연어 등이 연속형 데이터에 속한다.

<br>

### *RNN 구조*

<br>

![rnn](https://user-images.githubusercontent.com/89771322/147561164-868164af-a74e-4748-8c41-63172e85fcc2.png)

<br>

RNN은 Input 값으로 두가지를 받는다.

1. 현재 time step의 단어 임베딩 값.
2. 이전 time step의 은닉값. 

그림을 보면 은닉층에서 나온 출력값이 다음 은닉층에 입력값으로 들어가는 것을 볼 수 있다. 이렇게 출**력값이 다시 입력값으로 들어가는 특성** 때문에 순환 신경망이라는 이름이 붙었다.

<br>

이렇게 각 time step의 정보를 담은 은닉값들이 계속 입력으로 들어가면서 결국, 모든 time step의 정보를 담은 하나의 벡터가 출력된다.

RNN은 이론적으로 어떤 길이의 sequential 데이터라도 처리할 수 있다고 한다. ***(이론적으로..!)***

<br>

### *RNN 단점*


<br>

RNN은 역전파 과정에서 활성화 함수인 탄젠트 함수의 미분값을 전달하는데, 이 과정에서 **기울기 소실/폭발**이 일어날 수 있다는 치명적인 단점이 있다.

그렇기에 이론적으로는 어떤 길이의 sequential 데이터라도 처리할 수 있다지만, 길이가 길어지면 기울기 소실 및 폭발이 일어날 수도 있다.

<br>

이러한 RNN의 단점을 보완하기 위해 고안된 것이 바로 **장단기 기억망 (Long-Short Term Memory, LSTM)** 이다.

<br>
<br>

## **2. LSTM**

<br>

### *LSTM 구조*

<br>

![Lstm](https://user-images.githubusercontent.com/89771322/147564224-bf46c982-c09b-4e5e-8ff0-48f73f4d8bd7.png)

<br>

LSTM은 그림에서 보이는 두개의 직선 중 위에 보이는 직선인 cell state로 기울기 소실 문제를 해결할 수 있다.

<br>

* **cell state**
  * cell state는 forget gate와 input gate를 통해 값을 받고, cell state를 거친 값들이 output gate를 거쳐 출력된다.
  * cell state는 활성화 함수를 거치지 않기 때문에 역전파 과정에서 기울기 소실이 일어나지 않는 장점이 있다.
  
  <br>

    * **forget gate**
      * 이전의 정보를 얼만큼 반영할지 결정하는 gate이다.
      * 두개의 입력값을 받고 이를 시그모이드 함수를 통해 0~1 사이의 값을 반환한다
      * 만약 값이 0 이라면 이전 정보를 모두 잊고, 1이면 이전 정보를 기억하는 원리이다.
  
  <br>

    * **input gate**
      * 지금 들어온 정보를 얼만큼 반영할지 결정하는 gate
      * forget gate와 마찬가지로 활성화 함수로 시그모이드를 사용하기에 0~1 사이의 값이 나온다
      * 만약 값이 1로 나오면 탄젠트 함수를 거쳐 cell state에 더해지게 된다.
  
  <br>

    * **ouput gate**
      * 두 정보를 계산하여 나온 출력 정보를 얼마나 전달할지 결정하는 gate

<br>

### *GRU (Gated Recurrent Unit) :*
GRU는 LSMT를 간소화 한 버전으로 forget gate와 input gate를 하나의 gate에서 관리하고 cell state도 사라졌다.

<br>

![Gru](https://user-images.githubusercontent.com/89771322/147566678-f8639f98-66a5-42ec-addc-208aba20e9bd.png)

<br>
<br>

## **3. LSMT 코드**

<br>

*필요한 라이브러리 import*

```python
import tensorflow as tf
import numpy as np

from tensorflow.keras.preprocessing import sequence
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Embedding, Input, LSTM
from keras.preprocessing.text import Tokenizer
```

<br>

*Tokenizer 설정*

```python
# 파라미터 설정
num_words = 3000
max_len = 400

# Tokenizer 설정 및 사전 형성
tokenizer = Tokenizer(num_words=num_words)
tokenizer.fit_on_texts(X_train)
```

<br>

*정수 인덱싱 및 패딩*

```python
# 정수 인덱싱
X_train_encoded = tokenizer.texts_to_sequences(X_train)

# 패딩
X_train_padded = sequence.pad_sequences(X_train_encoded, maxlen=max_len, padding='post')
y_train = np.array(y_train)
```

<br>

*모델 구성 및 학습*

```python
model = Sequential()
model.add(Input(shape=(max_len,)))
model.add(Embedding(num_words, 50, input_length=max_len))
model.add(LSMT(64, dropout=0.2))
model.add(Desne(1, activation='sigmoid'))

model.compile(loss='binary_crossentropy',
              optimizer='adam', 
              metrics=['acc'])

model.fit(model.fit(X_train, y_train, batch_size=128, epochs=10, validation_split=0.2)
```
<br>
<br>

## **4. Attention**

<br>

LSTM과 GRU가 장기 의존성 문제를 개선했다고 해도 문장이 길어지면 모든 단어의 정보를 하나의 벡터에 담기는 힘들어진다. 이러한 문제를 해결하기 위해 등장한 방법이 바로 Attention 이다.

<br>

![attention](https://user-images.githubusercontent.com/89771322/147570260-c6cda103-52a7-4d24-85fb-c3fe51f91399.png)

<br>

Attention은 디코더의 hidden state 벡터와 인코더에서 넘어온 N개의 hidden state 벡터와의 **연관성을 계산**하는데 이 결과를 Attention의 가중치로 사용한다. 과정은 다음과 같다.

<br>

1. 디코더의 hidden state 벡터와 인코더에서 넘어온 N개의 hidden state 벡터를 준비한다.
2. 각각의 벡터를 내적한 값을 구한다.
3. 이렇게 구한 값에 소프트맥스 함수를 취한다.
4. 소프트맥스를 취해서 나온 값이 인코더에서 넘어온 있는 hidden state 벡터를 곱한다.
5. 이 벡터를 모두 더해준다. (그림에서 디코더에 있는 hidden state 벡터와 인코더에서 넘어온 hidden state 벡터 중 연관성이 높은 벡터의 정보가 많이 남은 걸 볼수 있다.)
6. 5번 과정을 통해 생성된 벡터와 디코더에 있는 벡터를 사용해서 출력 단어를 결정한다.

<br>

이러한 과정을 통해 디코더가 인코더에 있는 **모든 단어의 정보를 활용**하면서 장기 의존성 문제를 해결하는 것이다.

<br>
<br>
<br>
<hr>
<br>

### 참고자료 :
https://analysisbugs.tistory.com/106