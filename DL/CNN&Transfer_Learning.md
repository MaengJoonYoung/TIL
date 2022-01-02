# **CNN & Transfer Learning**

<br>

## **1. 합성곱 신경망 (Convolutional Neural Network)**

<br>

합성곱 신경망 CNN은 이미지 분류에서 주목받는 신경망이다. MLP의 경우 **입력값을 Flatten한 후 연산하기 때문에 이미지 데이터의 공간적 특성을 잘 파악하지 못한다.** 하지만 CNN은 **이미지 데이터의 공간적 특성을 잘 보존하여 학습하기에** 이미지 분류에서 주목받고 있다.

CNN은 특징을 추출하는 부분(합성곱 층과 풀링 층)과 분리를 위한 신경망으로 크게 두가지 부분으로 나눌 수 있다.

<br>

### ***1-1. 합성곱과 풀링***

<br>

### *1) 합성곱(Convolution)*

<br>

합성곱 층에서는 합성곱 필터가 이미지 데이터를 읽어나가며 합성곱 연산을 실시하여 이미지의 특징을 읽어나간다.

<br>

![합성곱](https://user-images.githubusercontent.com/89771322/147874192-c6b027e2-93c2-4158-891a-7859bd673ffc.gif)


<br>

위 그림 처럼 `5x5` 이미지를 `3x3`필터가 **슬라이딩하며 합성곱 연산을 진행**한다. 오른쪽에 보이는 사각형은 feature map인데, 필터의 값과 이미지 데이터의 픽셀 값을 곱해서 다 더한 값을 출력되는 feature map에 하나씩 배정한다. **즉, 합성곱 연산을 통해 나오는 출력값은 바로 feture map이다.**

<br>

### *2) 패딩 (Padding)*

<br>

위 그림을 보면 출력되는 Feature map의 크기가 **원본 데이터에 비해 작고** 각 코너에 있는 값의 경우 각 코너에 있다는 이유 만으로 **연산이 한번만 이뤄지는** 걸 볼 수 있다. **즉 정보손실이 일어날 수 있다는 것이다. 이러한 문제를 해결하기 위해 사용하는 것이 바로 패딩이고, 주로 zero padding을 사용한다.**

<br>

![zero padding](https://user-images.githubusercontent.com/89771322/147874470-f7da948d-5fd8-4959-82cf-d98adfdd1498.gif)

<br>

이렇게 zero padding을 사용하면 **원본 이미지의 크기를 그대로 유지하고 각 코너에 있는 데이터에 연산을 여러번 진행할 수 있다.**

<br>

### *3) 스트라이드 (Stride)*

<br>

스트라이드는 필터가 어느정도 간격으로 이동할지 결정한다. 앞서 본 필터들은 다 `stride=1`인 상태이다. 스트라이드가 크면 클 수록 feature map의 크기는 작아진다.

즉, 스트라이드와 패딩 모두 Feature map의 크기에 영향을 주는 요소이다.

필터, 패딩 그리고 스트라이드의 변화에 따라 Feature map의 크기가 얼마나 변하는 지는 다음과 같다.

<br>


$$
N_{\text{out}} = \bigg[\frac{N_{\text{in}} + 2p - k}{s}\bigg] + 1
$$


<br>


$N_{\text{out}}$ : 출력되는 이미지의 크기(=피처 수) <br/>
$N_{\text{in}}$ : 입력되는 이미지의 크기(=피처 수) <br/>
$k$ : 필터의 크기 <br/>
$p$ : 패딩 값 <br/>
$s$ : 스트라이드 값


<br>

### *4) 풀링 (Pooling)*

<br>

풀링은 다운 샘플링에 목적이 있는 것으로, 좋은 정보만 추려내어 feature map의 차원을 줄이는 역할을 한다. 이를 통해 **연산 속도에서 이득**을 가지게 되고 **과대적합을 방지**한다.

또한 풀링층은 **가중치가 없고 채널 수가 변하지 않는다.**

<br>

![pooling](https://user-images.githubusercontent.com/89771322/147876259-aa6bfebf-9a9c-40d0-acca-3651d7d8924d.png)

<br>

그림은 `2x2` 풀링의 최대 풀링과 평균 풀링이다. 최대 풀링은 범위 내에서 **가장 큰 값을 가져오고** 평균 풀링은 범위 내에 있는 **모든 요소의 평균을 가져온다.** 일반적으로 각 이미지의 특징을 최대로 보존하기 위해 max pooling을 많이 사용한다.

<br>

### ***1-2. 완전 연결 신경망 (Fully Connected Layer)***

<br>

합성곱 층과 풀링 층으로 특징을 추출한 후에 **분류를 수행하기 위한 신경망**을 구성해야 한다. 분류를 수행하기 위해 완전 연결 신경망을 **우리가 풀고자 하는 문제에 맞게 설계하면 된다.**

<br>

### ***1-3. CNN 학습***

<br>

CNN은 합성곱 층에 있는 **Filter에 학습을 하는 가중치가 있다.** CNN은 Filter를 **랜덤**으로 가진 뒤 loss를 줄이는 방향으로 가중치를 학습한다.


<br>

CNN은 입력에 가까운 층일 수록 필터가 **저수준의 특성을 뽑고** 출력과 가까운 층일 수록 **고수준의 특성을 뽑게된다.**

<br>

예를 들어 `2x2` 필터의 경우 첫번째 Feature map의 값 하나는 원본 이미지의 `2x2` 만큼의 정보를 가지고 있다. 즉, 필터가 한번 추출한 정보를 담고 있다. 하지만 그 다음 층의 Feature map의 값 하나는 원본 데이터에서 `2x2` 필터가 4번 추출한 정보를 담고 있다. 그렇기에 출력 층에 가까울 수록 고수준의 특성을 뽑게된다.

<br>

### ***1-4. CNN 모델 구성***

<br>

*(데이터 불러오기 및 전처리를 거쳤다고 가정)*

<br>

*필요한 라이브러리 import*

```python
import numpy as np
import tensorflow as tf
from tensorflow.keras.layers import Dense, Conv2D, MaxPooling2D, Flatten
from tensorflow.keras.models import Sequential
```

<br>

*모델 구성*

```python
model = Sequential()

# Conv2D(필터 갯수, (필터크기), padiing='same'은 입출력 크기 같게)
model.add(Conv2D(32, (3,3), padding='same', activation='relu'))
# MaxPooling2D(풀링 크기)
model.add(MaxPooling2D(2,2))
model.add(Conv2D(64, (3,3), padding='same', activation='relu'))
model.add(MaxPooling2D(2,2))
model.add(Conv2D(64, (3,3), padding='same', activation='relu'))
model.add(Flatten())
# 분류를 위한 신경망
model.add(Dense(128, activation='relu'))
model.add(Dense(10, activation='softmax'))
```

*컴파일 이후 학습*

```python
model.compile(optimizer='adam',
loss='binary_crossentropy',
metrics=['acc'])

model.fit(X_train, y_train,
batch_size=128,
validation_data=(X_val, y_val),
epochs=10))
```

<br>
<br>

## **2. 전이 학습 (Transfer Learning)**

<br>

전이 학습은 대량의 데이터를 사전에 학습한 모델의 **가중치를 그대로 가져온 뒤 분류를 위한 신경망만 설계하여 사용하는 것**을 말한다.

사전 학습 모델은 대량의 데이터를 학습하며 여러 데이터의 일반적인 특성을 많이 학습하였기에 어떠한 데이터를 넣어도 준수한 성능을 보인다는 특징을 가진다.

일반적으로 사전 학습된 **가중치는 고정**해두고 **완전 연결 신경망 부분만 학습을 진행**하기에 빠르게 좋은 결과를 얻을 수 있다는 장점이 있다.

<br>

이미지 분류를 위해 사용하는 사전 모델은 **VGG, GoogLeNet, ResNet, MobileNet, EfficientNet** 등이 있다.

<br>

### ***ResNet 실습***

<br>

*(데이터 불러오기 및 전처리를 거쳤다고 가정)*

<br>

*필요한 라이브러리 import 및 모델 불러오기*

~~~python
import numpy as np

from tensorflow.keras.applications.resnet50 import ResNet50
from tensorflow.keras.applications.resnet50 import preprocess_input, decode_predictions

from tensorflow.keras.layers import Dense, GlobalAveragePooling2D
from tensorflow.keras.models import Model

# imagenet 데이터를 학습한 가중치를 가져오고, include_top = False -> 분류를 위한 신경망 부분은 가져오지 않는다.
resnet = ResNet50(weights='imagenet', include_top=False)
~~~

<br>

*사전 학습된 가중치 고정*

```python
for layer in resnet.layers:
    layer.trainable = False
```

<br>

*모델 구성*

```python
x = resnet.output
x = GlobalAveragePooling2D()(x)
x = Dense(1024, activation='relu')(x)
predictions = Dense(1, activation='sigmoid')(x) # 출력층 설계
model = Model(resnet.input, predictions)
```

<br>

*컴파일 및 학습*

```python
model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['acc'])

model.fit(train, epochs=5, validation_data=val)
```