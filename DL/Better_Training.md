# **더 나은 학습을 위한 방법**

<br>

## **1. 학습률 감소/계획법 (Learning rate Decay/Scheduling)**

<br>

### *학습률 :*
경사하강법이 산을 내려가는 과정이라면 학습률은 **보폭을 결정**하는 것이다. 즉, 학습률은 해당 가중치에서의 손실함수 기울기를 **가중치 갱신에 얼마나 반영**할지 결정한다.

<br>

![lr](https://user-images.githubusercontent.com/89771322/146884908-94a56911-e41b-48f0-a0f3-8cc1074a7d3b.jpeg)

<br>

왼쪽 그림과 같이 학습률이 너무 작으면 최적점까지 이르기에 너무 오래걸리고 주어진 iteration 내에 최적점에 이르지 못할 수도 있다. 물론 epochs가 충분하다면 안정적으로 최적점에 이를 수도 있다.

반대로 오른쪽 그림과 같이 학습률이 너무 크면 경사하강 과정에서 발산하기에 최적점에 이르기 힘들다.

그렇기에 적절한 학습률을 설정하는 것은 중요하다.

<br>
<br>

### *학습률 감소 (Learning rate Decay)*

<br>

학습률 감소는 처음에는 큰 보폭으로 산을 내려오다가 도착지에 가까워지면 점차 보폭을 줄이는 방식이라고 할 수 있다. 즉, 처음에는 큰 학습률로 학습하다가 점점 학습률을 줄이는 방식이다.

학습률 감소는 Adagrad, RMSprop, Adam과 같은 옵티마이저에 이미 구현이어있어서 아래와 같은 방식으로 편하게 사용 가능하다.

<br>

~~~python
model.compile(optimizer=tf.keras.optimizers.Adam(lr=0.001, beta_1 = 0.89),
             loss='sparse_categorical_crossentropy',
             metrics=['accuracy']))
~~~

<br>

학습률 감소의 경우 애매한 지점에서 학습률이 감소하기 시작할 경우 최적점에 이르는 데 너무 오래 걸릴수도 있다는 문제가 있다.

<br>
<br>

### *학습률 계획법 (Learning rate Scheduling)*

<br>

학습률 계획법은 0 혹은 아주 작은 학습률 부터 일정 step 까지 warm up 스텝을 가진 후 학습률이 커졌다가 다시 줄어드는 방법이다. 학습률 계획법은 **초반에 작은 학습률로 시작해 Global Minima로 갈 수 있도록 방향을 잘 잡게 해주는 장점이 있다.** 하지만 언제까지 학습률을 키우고 언제부터 다시 줄일지를 사용자가 다 설정해야 한다는 문제가 있다. 즉, 설정해야할 하이퍼파라미터가 늘어난다는 단점이 있다.

<br>
<br>

## **2. 가중치 초기화 (Weight Initialization)**

<br>

초기 가중치를 어떻게 설정하느냐는 모델의 성능에 영향을 미치기에 매우 중요한 문제이다.

표준편차가 1인 정규분포로 가중치를 초기화할 경우 활성화 값이 대부분 0과 1 사이에 위치하기에 잘 사용하지 않는다.

이러한 문제를 해결할 수 있는 것이 바로 **Xavier 초기화 (Xavier initialization)** 이다.

<br>

### *Xavier 초기화 (Xavier initialization)* :

Xavier 초기화의 경우 노드의 값이 달라질때 표준편차 값이 달라진다. 표준편차가 고정된 정규분포로 가중치를 초기화 했을 경우보다 활성화 값이 고르게 나타난다. Xavier 초기화는 활성화 함수가 S**igmoid인 경우 잘 작동**하지만, 활성화 함수가 **ReLU인 경우 층을 지날수록 활성화 값이 고르지 않다**는 문제가 있다.

<br>

### *He 초기화 :*
He 초기화는 활성화 함수가 ReLU일때 층을 지나도 활성화 값이 고르게 분포한다. 그렇기에 활성화 함수가 ReLU일 경우 He 초기화를 추천하고 Sigmoid일 경우 Xavier 초기화를 추천하는 것으로 알고 있다. **하지만 가중치 초기화 방법은 많고 신경망의 구조에 따라 다를 수 있기에 정답이 정해져 있는 건 아니다.**


<br>
<br>

## **3. 과적합 방지를 위한 방법**

<br>

### *1) 가중치 감소 (Weight Decay) :*

과적합은 주로 가중치가 커질때 발생한다. 그렇기에 **손실함수에 가중치 값이 과도하게 커지지 않을 조건을 추가하는 방식으로 과적합을 방지한다.**

<br>

* *단점*

  * Weight Decay 방법은 매우 강력한 제약이기에 모델의 성능을 저하시키는 요인이 될수도 있다. 그렇기에 유의해서 사용해야 한다. 

<br>

* *사용방법*

  * 사용방법은 Weight Decay를 사용하고 싶은 층에 `regularizer` 파라미터를 추가하면 된다.

<br>

~~~python
model.add(Dense(128, kernel_regularizer=regularizers.l2(0.001), # L2 Regularization
      activity_regularizer=regularizers.l1(0.001)) # L1 Regularization)
~~~

<br>

### *2) Dropout :*

Dropout은 **매 Iteration 마다 각 층에서 설정한 비율만큼의 노드를 끄고 학습을 진행하는 것이다.** 매번 다른 노드가 학습되면서 전체 가중치의 과적합을 방지할 수 있다. 즉, Iteration 마다 다르게 학습되기에 앙상블 효과와 비슷한 효과를 낸다고 볼 수도 있다. 이때 비율을 너무 낮게 설정하면, underfitting이 발생한다. 

<br>

* *단점*

  * 어느 비율만큼의 노드를 끄고 어느 층에 학습해야 할지 사용자가 설정해야하기에 **하이퍼파라미터가 늘어난다**
  * 모든 노드를 사용해서 학습을 진행하지 않기에 학습에 필요한 epochs가 증가하여 **학습시간이 증가한다**

<br>

* *사용방법*
  * 사용하고 싶은 층 다음에 Dropout 함수를 추가한다. (Keras에서 사용시)

<br>

~~~python
model.add(Dense(100, activation='relu'))
model.add(Dropout(0.2)) # 100*0.2 만큼의 노드를 끄고 학습한다.
~~~

<br>

### *3) Early Stopping (조기 종료) :*

<br>

![early](https://user-images.githubusercontent.com/89771322/146904911-2398ed1a-4047-422a-a577-bc3a9ad813ec.png)

<br>

그림에서 볼 수 있듯이 일정 지점을 지나면 학습 데이터셋에 대한 에러는 감소하지만 검증 데이터셋에 대한 오류는 증가하는 걸 볼 수 있다. Early Stopping은 **검증 데이터셋의 오류를 모니터링 하여 정해준 횟수만큼 검증 데이터셋의 오류가 감소하지 않으면 학습을 멈추는 방법**이다.

<br>

* *단점*
  * 검증 데이터셋의 오류가 감소하지 않는 걸 얼마만큼 참을지 정해줘야 한다. 즉, 하이퍼파라미터가 증가한다는 단점이 있다.
  * 모델이 잘 학습되지 않다가 어느 순간 갑자기 학습을 하는 경우가 있는데 이러한 경우를 놓칠 수 있다.

<br>

* *사용방법*

<br>

~~~python
# 파라미터 저장경로 설정.
checkpoint_filepath = 'FMbest.hdf5'

# min_delta=0 -> 0미만의 변경은 개선이 없는 것으로 간주.
# patience=10 -> 10번동안 변화 없으면 멈춘다.
early_stop = keras.callbacks.EarlyStopping(monitor='val_loss', min_delta=0, patience=10, verbose=1)

# save_best_only=True -> best 모델만 저장.
# save_weights_only=True -> 가중치만 저장.
# save_freq='epoch' -> 모델의 저장 단위는 epoch
save_best = keras.callbacks.ModelCheckpoint(filepath=checkpoint_filepath, monitor='val_loss', verbose=1, save_best_only=True,
    save_weights_only=True, mode='auto', save_freq='epoch', options=None)

# 모델 학습.
model.fit(X_train, y_train, batch_size=32, epochs=50, verbose=1, 
          validation_data=(X_test,y_test), 
          callbacks=[early_stop, save_best])

# 베스트 모델 불러오기
model.load_weights(checkpoint_filepath)
~~~