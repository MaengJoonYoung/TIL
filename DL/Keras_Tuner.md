# **Keras Tuner**

<br>

*Keras에서는 Keras Tuner를 사용하여 하이퍼파라미터를 튜닝할 수 있다. 튜너의 종류로는 RandomSearch, Hyperband, BayesianOptimization 등이 있다.*

<br>

## *Hyperband :*
Hyperband는 하이퍼파라미터의 조합 전체를 한번 학습 후 성능이 좋지 않은 조합을 일정 비율 버리고 남은 하이퍼파라미터들을 학습량(epoch)
을 더 늘려서 학습 후 다시 일정 비율만큼을 버리고를 반복하면서 최적의 조합을 찾는 방식이다.

<br>

Hyperband를 사용하여 하이퍼파리미터를 튜닝하는 과정은 다음과 같다.

<br>

1. *kerastuner 설치.*
2. *모델을 제작하고 튜닝할 하이퍼파라미터의 범위를 설정하는 함수를 만든다.*
3. *하이퍼파라미터를 튜닝할 튜너를 지정한다.*
4. *이전의 학습의 출력을 지우는 콜백함수를 지정한다. (선택사항)*
5. *튜닝 시작!*
6. *최적의 조합 불러와서 다시 학습*

<br>

### *코드로 구현하기*

<br>

*1) kerastuner 설치*

~~~python
# 설치후 import
!pip install -U keras-tuner
import kerastuner as kt
~~~

<br>

~~~python
# 필요한 라이브러리 import
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras.layers import Dense
from tensorflow.keras.models import Sequential
import IPython
~~~

<br>

*2) 모델을 제작하고 튜닝할 하이퍼파라미터의 범위를 설정하는 함수를 만든다.*
~~~python
# 이진 분류 문제로 가정
def model_builder(hp):
    model = Sequential()

    # 은닉층 노드 수 32~512까지 32단위로 끊어가면서 설정
    hp_units = hp.Int('units', min_value=32, max_value=512, step=32)

    model.add(Dense(hp_units, activation='relu')))
    model.add(Dense(hp_units, activation='relu')))
    model.add(Dense(1, activation='sigmoid'))

    # 학습률 범위 0.01, 0.001, 0.0001로 설정
    hp_learning_rate = hp.Choice('learning_rate', values=[1e-2, 1e-3, 1e-4])

    model.compile(optimizer=keras.optimizers.Adam(learning_rate=hp_learning_rate),
    loss=keras.losses.BinaryCrossentropy(from_logits=True),
    metrics=['accuracy'])

    return model
~~~

<br>

함수를 정의할때 `hp`를 사용했는데, 이는 keras의 HyperParameters class로 해당 클래스의 여러 method로 하이퍼파라미터 범위 등을 설정할 수 있다. 자세한 방법은 [HyperParameters class](https://keras.io/api/keras_tuner/hyperparameters/)에 나와 있다,

<br>

*3) 하이퍼파라미터를 튜닝할 튜너를 지정한다.*

~~~python
tuner = kt.Hyperband(model_builder,
objective='val_accuracy',
max_epochs=30,
factir=3,
directory='my dir',
project_name='test')
~~~

<br>

*4) 이전의 학습의 출력을 지우는 콜백함수를 지정한다.*
~~~python
class ClearTrainingOutput(keras.callbacks.Callback):
  def on_train_end(*args, **kwargs):
    IPython.display.clear_output(wait = True)
~~~~

<br>

*5) 튜닝 시작*

~~~python
tuner.search(X_train_scaled, y_train, epochs=30, batch_size=50, validation_data=(X_test_scaled, y_test), callbacks=[ClearTrainingOutput()])

best_hps = tuner.get_best_hyperparameters(num_trials=1)[0]
~~~

<br>

*6) 최적의 조합 불러와서 다시 학습*

~~~python
model = tuner.hypermodel.build(best_hps)
model.fit(X_train_scaled, y_train, validation_data=(X_test_scaled, y_test))
~~~
