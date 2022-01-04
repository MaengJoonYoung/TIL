# **결정트리 (Decision Tree)**

<br>

결정트리는 스무고개와 비슷하다. 왜냐하면 결정트리는 데이터의 특성들의 수치를 바탕으로 **질문을 던지며 정답을 찾아가는 과정**이기 때문이다. **즉, 결정트리는 데이터를 분활해가는 알고리즘**이고 **회귀와 분석에서 모두 사용 가능하다.**

## **1. 결정트리 (Decision Tree)**

<br>

<img width="270" alt="dt" src="https://user-images.githubusercontent.com/89771322/148063687-8f06b3a1-c486-449f-8019-8fe6d38da7f1.png">

<br>

결정트리는 그림에서 처럼 질문을 던지고 매인지 펭귄인지 등의 정답을 찾아가게 된다. 

이때 맨 위에 있는 '날개가 있나요?'라는 박스를 **뿌리 노드 (Root Node)**, '날 수 있나요?'와 '지느러미가 있나요?'를 **중간 노드 (Internal Node)**, 맨 밑에 있는 매, 펭귄, 돌고래, 곰 등을 말단 노드 (Leaf Node)라고 한다. 마지막으로 화살표들을 **엣지 (Edge)** 라고 한다.

<br>
<br>

## **2. 결정트리 학습**

<br>

결정트리도 일반적인 모데들과 같이 비용함수를 정의하고 비용함수를 최소화 하는 방향으로 학습을 진행하는데, 이때 비용함수를 최소화 하는 문제는 **노드를 어떻게 분할하는가**와 연결되어 있다.

<br>

### ***지니 불순도 (Gini Impurity) & 엔트로피 (Entropy)***

<br>

결정트리에서 사용하는 비용함수로는 주로 **지니 불순도**와 **엔트로피**가 있다. 지니 불순도와 엔트로피 둘다 한 노드에 있는 샘플이 다 같은 클래스일 경우 0이 된다. 즉, 다른 클래스가 섞이면 섞일 수록 불순도가 증가하는 것이다.

예를 들어 내가 가진 동전 지갑에 전부 100원짜리 동전만 있다면 불순도가 낮은 것이고, 100원, 500원, 50원, 10원 등의 동전이 마구 섞여 있다면 불순도가 높은 것이다.

<br>

![불순도](https://user-images.githubusercontent.com/89771322/148065912-87e8b805-429a-409d-bf11-e48d8e4ba798.png)

<br>

위 그림의 경우 2번 항아리의 불순도는 높고 나머지 1번, 2번 항아리의 불순도는 0이 되어 순수하다고 표현한다.

<br>

엔트로피가 더 균형잡힌 트리를 만들다고는 하지만 지니 불순도의 계산이 더 빠르기에 sklearn에서는 지니 불순도를 디폴트 값으로 사용한다. (또한 둘의 성능 차이도 그렇게 크지 않다고 한다..)

<br>

*정리하자면, 결정트리는 비용함수를 최소화하는 방향으로 학습을 진행하는데, 이때 사용하는 비용함수로는 **지니 불순도와 엔트로피**가 있다. 즉, 각 분할점에서 **불순도를 가장 낮춰주는 질문을 선택**하는 방식으로 학습을 진행하고 이는 곧 **정보획득**이 큰 질문을 선택한다는 것이다.*

> *정보획득은 특정 특성을 사용했을 때 엔트로피 감소량을 말한다. 정보획득은 **분할전 노드의 불순도 - 분할 후 자식 노드들의 불순도** 로 계산한다.*

<br>
<br>

## **3. 결정트리 실습**

<br>

사이킷런 DesicionTreeClassifier 를 사용해 결정트리를 구현하는 방법은 다음과 같다.

<br>

```python
from category_encoders import OrdinalEncoder
from sklearn.tree import DecisionTreeClassifier
from sklearn.pipeline import make_pipeline

# 비용함수로 엔트로피 사용.
pipe = make_pipeline(OrinalEncoder(),
DecisionTreeClassifier(random_state=1, criterion='entropy'))
```

<br>

### *과적합을 방지하기 위해 자주 사용하는 하이퍼파라미터*

<br>

* min_samples_split
  * 노드를 분할하기 위해 최소 몇개의 샘플이 있어야 하는지
  * 작을 수록 분할 노드가 많아져서 과적합 가능성이 높아진다.
* min_samples_leaf
  * 리프노드에 최소한 몇개의 샘플이 있어야 하는지
  * 클 수록 과적합 가능성 낮아진다.
* max_depth
  * 트리의 깊이를 결정한다.
  * 깊을 수록 과적합 가능성이 높아진다.

<br>
<br>

## **4. 특성중요도 (feature importance)**

<br>

결정트리에서는 특성중요도를 구할 수가 있는데, 상위에 있는 노드일 수록 특성중요도가 높게 나온다. 즉, 정보이득이 큰 특성의 특성중요도가 높다.

특성중요도를 확인하는 방법은 다음과 같다.

<br>

*named_steps 사용하여 파이프라인 내 결정트리에 접근.*

```python
dt = pipe.named_steps['decisiontreeclassifier']
```

<br>

*특성 중요도 확인*

```python
import pandas as pd
import matplotlib.pyplot as plt

# 파이프라인 내 인코더에 접근.
encoder = pipe.named_steps['ordinalencoder']
# X_val -> 검증 데이터
encoded_columns = encoder.transform(X_val).columns

# 특성 중요도 시각화.
importances = pd.Series(dt.feature_importances_, encoded_columns)
plt.figure(figsize=(10,30))
importances.sort_values().plot.barh();
```

위의 과정을 거치면 다음과 같은 결과를 얻을 수 있다.

<br>

![특성 중요도](https://user-images.githubusercontent.com/89771322/148070561-bcbeb365-0ddb-450e-b3d6-a6de56d5f60c.jpeg)