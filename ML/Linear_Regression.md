# **선형회귀**

<br>

우리가 데이터로 훈련시킨 머신러닝 모델은 우리가 알고 있는 값 즉, 우리가 훈련시킨 데이터를 잘 예측하는 데 목적이 있는 게 아니라 **새로운 테스트 데이터를 잘 맞추는 데 목적이 있다.** 그리고 이러한 목적을 달성하는 가장 간단한 모델은 바로 **선**이다. 


 아래 그림과 같이 우리가 가진 데이터를 표에 점의 형태로 다 뿌려놓고 이 점들을 가장 잘 설명하는 선을 그리는 방법을 바로 선형회귀라고 한다.

 <br>

 ![linear](https://user-images.githubusercontent.com/89771322/153598922-0307cb86-6f96-4ac1-aec6-8569774b4277.png)<br>
 [출처](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95_%ED%9A%8C%EA%B7%80)

 <br>

 ***그렇다면 데이터를 가장 잘 설명하는 선은 무엇일까?***

 <br>

 위 그림에서 볼 수 있듯이 선 과 실제 데이터 간에는 차이가 존재한다. 이러한 **잔차들을 다 제곱하여 합한 것**을 **RSS(residual sum of squares) 혹은 SSE(Sum of Square Error)**라고 말하고 **이 값을 최소화 하는 선을 가장 데이터를 잘 설명하는 선이라고 할 수 있을 것이다.**

 즉, **RSS는 회귀모델의 비용함수**가 되고 이 비용함수를 **최소화하는 과정을 학습이라고 한다.**
 >RSS를 최소화하는 방법을 OLS라고 한다.

 <br>

 ## **1. 단순, 다중 선형회귀**

 <br>

우리가 예측하고자 하는 값은 종속변수이고, 이 종속변수에 영향을 미치는 변수를 독립변수라고 한다. **하나의 독립변수만을 가지고 예측을 수행하는 모델을 단순 선형회귀 모델**이라고 하고 **하나 이상의 독립변수로 예측을 수행하는 모델을 다중 선형회귀 모델**이라고 한다.

`sklearn`을 통해 우리는 쉽게 선형회귀 모델을 만들 수 있다.

<br>

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
# 다중 선형회귀 모델의 경우 독립변수를 여러개 지정해주면 된다.
feature = ['독립변수'] 
target = ['종속변수'] # 예측하고자 하는 값
# df 라는 데이터프레임이 있을 때 특성 테이블과 타겟 벡터를 나눠준다.
X_train = df[feature]
y_train = df[target]

# 모델 학습.
model.fit(X_train, y_train)
```

<br>

### ***계수***

<br>

우리가 선형회귀로 만든 선은 중고등학교 수학시간에 많이 봤던 식처럼 다음과 같이 표현된다.

$y=a+bx$

여기서 b는 x가 한단위 변할때 y에 얼마나 영향을 미치는지를 나타내는데 우리가 만든 모델에서 계수를 확인하는 방법은 다음과 같다.

```python
# 계수 확인
model.coef_
# 절편 확인
model.intercept_
```
> 만약 계수가 10이 나왔다면 독립변수가 1단위 변할때 종속변수는 10만큼 변한다는 것이다.

<br>

## **2. 평가지표**

<br>

선형회귀 모델의 성능을 평가하는 지표는 다양한 것들이 있다.

<br>

### ***1) MAE(Mean Absolute of Errors)***

<br>

![mae](https://user-images.githubusercontent.com/89771322/153604049-e209cc04-5443-4e3b-9b95-40b458f58d38.jpeg)

<br>

MAE는 평균절대 오차로 잔차의 절대값에 대한 평균을 나타낸다.

<br>

*장점*
1. 지표가 직관적이고 예측하고자 하는 값과 단위가 같다.
2. MSE보다 이상치에 민감하게 반응하지 않는다.

*단점*
1. 절대값을 취하기 때문에 모델이 실제 값보다 낮거나 높게 예측했는 지 알 수 없다.
2. 예측값의 스케일의 영향을 받는다. (1,000 단위의 값을 예측하는 것과 1단위의 값을 예측하는 모델의 경우 1% 정도의 오차가 있다고 했을때 MAE값은 크게 차이가 난다.)

<br>

### ***2) MSE(Mean Square of Errors)***

<br>

![mse](https://user-images.githubusercontent.com/89771322/153605072-0f4b2d54-4aed-478c-ba3a-784917e5ea8a.png)

<br>

MSE는 평균제곱오차로 잔차의 제곱에 대한 평균이다.

<br>

*장점*
1. MAE와 마찬가지로 직관적이다.

*단점*
1. 잔차를 제곱하기에 이상치에 민감하고, 1미만의 에러는 더 작아지고 그 반대의 경우는 에러가 더 커진다.
2. 예측하고자 하는 값과 단위가 다르다.
3. 모델이 실제 값보다 낮거나 높게 예측했는 지 알 수 없다.

<br>

### ***3) RMSE(Root Mean Squared Error)***

<br>

RMSE는 MSE에 루트를 씌운 값이다. MSE는 제곱을 하기에 실제 오류 평균보다 더 커지는 특성이 있는데 이를 보완하는 방법으로 RMSE가 있다. 

RMSE는 **이상치에 큰 패널티를 주는 이점**이 있다.


<br>
<br>
<hr>

### *참고자료*

https://partrita.github.io/posts/regression-error/<br>
https://data101.oopy.io/mae-vs-rmse <br>
https://velog.io/@tyhlife/%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%ED%9A%8C%EA%B7%80-%EB%AA%A8%EB%8D%B8%EC%9D%98-%ED%8F%89%EA%B0%80-%EC%A7%80%ED%91%9C <br>
https://velog.io/@dlskawns/Linear-Regression-%EC%84%A0%ED%98%95%ED%9A%8C%EA%B7%80%EC%9D%98-%ED%8F%89%EA%B0%80-%EC%A7%80%ED%91%9C-MAE-MSE-RMSE-R-Squared-%EC%A0%95%EB%A6%AC <br>
https://ko.wikipedia.org/wiki/%EB%8F%85%EB%A6%BD%EB%B3%80%EC%88%98%EC%99%80_%EC%A2%85%EC%86%8D%EB%B3%80%EC%88%98 <br>
https://hleecaster.com/ml-linear-regression-concept/