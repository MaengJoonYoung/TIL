# **Distributed Representation**

<br>

등장 횟수 기반 표현이 문서를 벡터화 했다면, 분산 표현은 **단어 자체를 벡터화 한다.** 분산 표현은 '비슷한 위치에서 등장하는 단어들은 비슷한 의미를 가진다'는 분포 가설을 기반으로 한다. 즉, **벡터로 표현되어질 단어가 주변 단어들의 분포로 인해 결정**되는 방식이다.

<br>

## **1. 원-핫 인코딩(One-hot Encoding)**

<br>

원-핫 인코딩은 단어를 벡터화하는 가장 기본적인 방법이다. 문장에 존재하는 단어 갯수 만큼 차원을 가진 영벡터를 만들고 단어의 등장 순서대로 1의 값을 배정한다. 예를 들어 '나는 학교에 간다'라는 문장을 '나는', '학교에', '간다'로 나눴다고 한다면, 원-핫 인코딩한 결과는 아래와 같다.

<br>

|나는|학교에|간다|
|--|--|--|
|[1 0 0]|[0 1 0]|[0 0 1]

<br>

하지만 원-핫 인코딩은 **단어의 유사도를 구할 수 없다는 단점이 있다.** 우리는 주로 코사인 유사도를 사용해서 단어의 유사도를 구하는데 원-핫 인코딩으로 만든 **벡터들은 내적값이 0이기 때문에 코사인 유사도도 항상 0이 된다.** 이러한 원-핫 인코딩의 단점을 해결할 수 있는 방법이 바로 **임베딩(Embedding)** 이다.

<br>
<br>

## **2. 임베딩(Embedding)**

<br>

임베딩은 차원이 일정한 벡터로 나타내는데, 벡터 내의 각 요소가 0과 1로 이뤄져있던 원-핫 인코딩과 달리 **벡터 내의 각 요소가 연속적인 값을 가진다.** 예를 들어 `[0.34 0.16 0.74]` 등의 값을 가지게 된다.

<br>

가장 널리 알려진 임베딩 방법으로는 **Word2Vec**이 있다.

<br>
<br>

## **3. Word2Vec**

<br>

Word2Vec은 말 그대로 단어를 벡터로 나타내는 방법이다. Word2Vec은 window size를 설정하여 양 옆에 단어들의 관계를 활용하기에 분포 가설을 잘 반영하고 있다.

Word2Vec에는 `CBoW`와 `Skip-gram`이라는 방법이 있다.

<br>
<br>

### *1) CBoW와 Skip-gram*

<br>

CBoW는 **주변 단어**의 정보를 기반으로 **중심 단어**의 정보를 예측하는 모델이고,

Skip-gram은 **중심 단어**의 정보를 기반으로 **주변 단어**의 정보를 예측하는 모델이다.

두 모델 중 성능이 좋은 모델은 일반적으로 Skip-gram 이다. 역전파의 관점에서도 훨씬 많은 학습을 진행하고 같은 데이터라도 Skip-gram의 경우 학습할 데이터가 더 늘어나기에 학습에 유리하다고 한다.

<br>

### *2) Word2Vec의 구조*

<br>

Skip-gram의 구조는 다음과 같다.

* 입력층
  * 원-핫 인코딩 된 벡터
* 은닉층
  * 임베딩 벡터의 차원수 만큼의 노드로 구성되어 있고 하나의 은닉층을 가진 신경망
* 출력층
  * 단어의 개수 만큼 노드를 가지고 활성화 함수로 소프트맥스 함수를 사용한다.

<br>

![skip-gram](https://user-images.githubusercontent.com/89771322/147467028-5c00d52b-b442-4b4f-a07f-a424b13c25df.png)


<br>

위 그림에서 단어의 개수는 10,000개이고 임베딩 차원은 300이다. 그렇기에 이러한 Word2Vec의 결과로 **10,000개 단어에 대한 300차원의 임베딩 벡터가 생성된다.**

이렇게 얻어진 임베딩 벡터는 **단어 간의 의미적, 문법적 관계를 잘 나타낸다.**

<br>

### *3) Keras 및 Word2Vec을 사용하여 단어 평균으로 문서 분류하기*

문장 분류를 사용하는 방법 중 가장 간단한 방법은 **문장에 있는 모든 단어의 벡터를 더한 뒤 평균을 내는 것**이다. 간단한 문제에서 좋은 성능을 보여 baseline으로 많이 사용한다.

<br>

*Keras로 진행하는 큰 틀은 다음과 같다.*

1. 단어를 담은 사전을 형성한다.
2. 각 단어를 정수로 인덱싱한다.
3. 임베딩 레이어에 입력한다.

<br>

*(아래 코드들은 데이터를 불러온 후 전처리를 완료했다는 가정하에 작성했다.)*

<br>

*gensim 설치 및 업그레이드 후 import*

`gensim`은 사전에 학습된 임베딩 벡터를 사용할 수 있는 패키지이다. 해당 패키지를 사용하여 구글 뉴스 말뭉치로 학습된 Word2Vec 벡터를 다운받아 진행.

```python
!pip install gensim --upgrade

import gensim
gensim.__version__ # 최신 버전인지 확인.
```

<br>

*구글 뉴스 말뭉치로 학습된 Word2Vec 벡터 다운받기*

```python
import gensim.downloader as api

wv = api.load('word2vec-google-news-300')
```

<br>

*필요한 라이브러리 import*

```python
import tensorflow as tf
from keras.preprocessing import sequence
from tensorflow.keras.preprocessing.sequence import pad_sequences
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Embedding, GlobalAveragePooling1D
from tensorflow.keras.preprocessing.text import Tokenizer
```

<br>

*단어를 담은 사전 형성 (keras의 Tokenizer를 학습시킨다.)*

```python
tokenizer = Tokenizer(num_words=1000)
tokenizer.fit_transform(X_train)

# 임베딩 가중치 행렬을 만들때, 모델 구성할때 사용.
vocab_size=len(tokenizer.word_index) + 1 
```

<br>

*정수 인덱싱 및 패딩*

*(기계는 문서의 길이가 모두 같으면 병렬 연산이 가능하다. 그렇기에 기계가 병렬 연산이 가능하도록 각 문서의 길의를 임의로 동일하게 맞춰주는 패딩을 진행한다.)*

```python
# 정수 인덱싱
X_train_encoded = tokenizer.texts_to_sequences(X_train)

# 패딩
X_train_padded = pad_sequences(X_train_encoded, maxlen=150, padding='post')
```

<br>

*임베딩 가중치 행렬 만들기*

```python
emb_matrix = np.zeros((vocab_size,300))

# 해당 word가 wv 안에 있으면 임베딩 값을 반환하는 함수
def get_vector(word):
    
    if word in wv:
        return wv[word]
    else:
        return None
 
for word, i in tokenizer.word_index.items():
    temp = get_vector(word)
    if temp is not None:
        emb_matrix[i] = temp
```

<br>

*모델 구성 및 학습*

```python
model = Sequential()

model.add(Embedding(vocab_size, 300, weights=[emb_matrix], input_length=150, trainable=False )) # 임베딩 레이어
model.add(GlobalAveragePooling1D()) # 단어 벡터의 평균을 구하는 층
model.add(Dense(1, activation='sigmoid')) # 출력층, 이진분류 문제

model.compile(optimizer='adam', loss='binary_crossentropy', metircs=['acc'])
model.fit(X_train, y_train, batch_size=64, epochs=10, validation_split=0.2)
```

<br>

### *4) OOV (Out of Vocabulary) 문제 :*

Word2Vec은 **기존 말뭉치에 존재하지 않는 단어에 대해서는 임베딩 벡터를 만들지 못한다**는 단점이 있다. 이렇게 **기존에 존재하지 않은 단어가 등장하는 문제를 OOV (Out of Vocabulary) 문제**라고 한다.

또한 적게 등장하는 단어에 대해서는 학습이 덜 이뤄지기 때문에 적절한 임베딩 벡터를 생성하지 못한다는 단점도 있다.

이러한 Word2Vec의 단점을 `fastText`로 해결할 수 있다.

<br>
<br>

## **4. fastText**

<br>

fastText는 철자 수준의 임베딩을 보조 정보로 사용하여 OOV 문제를 해결했다.

단어를 철자 단위로 쪼개서 임베딩을 적용하기에 기존 말뭉치에 존재하지 않는 단어에 대해서도 어느정도 유추가 가능한 것이다.

fastText는 단어의 뜻에 집중하기 보다는 생김새에 집중하는 특징이 있다. 예를 들어 nights와 fight의 유사도롤 fastText에서 살펴보면 뜻이 전혀 비슷하지 않더라도 비슷하게 생겼기에 유사도가 상당히 높게 나온다.

<br>

### *철자 단위 임베딩*

fastText는 단어의 접두사와 접미사를 인식할 수 있게 단어 앞 뒤에 '<', '>'를 붙인다. 그 후 단어를 3-6개(3-6 grams)로 잘라서 임베딩을 적용한다. 

만약 `singing`이라는 단어를 fastText를 사용하면 다음과 같이 데이터가 형성된다.

|word|Length(n)|Character n-grams|
|--|--|--|
|singing|3|<si, sin, ing, ngi, gin, ing, ng>|
|singing|4|<sin, sing, ingi, ngin, ging, ing>|
|singing|5|<sing, singi, ingin, nging, ging>|
|singing|6|<singi, singin, inging, nging>|

<br>

이렇게 생성된 n-gram 들의 임베딩 벡터를 모두 구한 뒤 `singing`
이라는 단어가 기존 말뭉치에 존재한다면, **`skip-gram`을 통해 나온 임베딩 벡터에 더한다.** 만약, **기존 말뭉치에 단어가 없다면 n-gram 들의 임베딩 벡터만으로 구성한다.**