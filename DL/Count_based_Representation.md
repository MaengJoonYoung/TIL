# **횟수 기반의 표현(Count-based Representation)**

<br>

자연어 처리를 위해 텍스트를 그대로 컴퓨터에게 넘겨주면 컴퓨터는 처리를 하지 못한다. 컴퓨터가 일을 하게 만들기 위해서 우리는 **텍스트를 벡터화 해서 넘겨줘야 하는데**, 이러한 벡터화 방법 중 오늘은 **단어가 문장에서 등장한 횟수를 기반으로 벡터화**하는 방법에 대해 배웠다.

<br>

*이렇게 등장 횟수를 기반으로 벡터화할 경우 문법 정보를 고려하지 않고 단어의 순서를 고려하지 않는다거나 등의 다양한 문제가 있다.*

<br>

### *자연어 처리에서 사용되는 용어*
* 말뭉치 (Corpus) : 특정한 목적을 가지고 수집한 텍스트 데이터
* 문서(Document) : 문장(Sentence)들의 집합
* 문장(Sentence) : 여러 개의 토큰(단어, 형태소 등)으로 구성된 문자열.
* 어휘집합(Vocabulary) : 코퍼스에 있는 모든 문서, 문장을 토큰화한 후 중복을 제거한 토큰의 집합


<br>
<br>

## **1. 텍스트 전처리**

<br>

텍스트 데이터에서 범용적으로 사용되는 전처리 방법들이 있다. 대소문자 통일, 불용어 제거, 통계적 트리밍, 어간추출 혹은 표제어 추츨 등 이 있다. 

이러한 전처리 방법들은 각각 단점이 있기에 모든 데이터에 다 사용해야 되는 것은 아니다. 자신이 가진 데이터와 해결해야 하는 문제를 잘 파악하고 유용한 것들만 선택해서 잘 사용해야 한다.

<br>

### *그렇다면 전처리 방법들이 단점을 가지고 있는데 전처리를 왜 해야될까?*

<br>

이는 차원의 저주와 관련이 있다. 특성의 개수가 선형적으로 늘어날때 동일한 설명력을 갖기 위한 인스턴스는 기하급수적으로 증가한다는 것이 바로 차원의 저주다. 

***단어의 등장횟수를 기반으로 벡터화를 진행하면 각 문장의 단어 종류가 특성의 개수(차원)가 된다. 그렇기에 전처리를 통해 불필요한 특성들을 제거해주는 것이다.***

<br>
<br>

### *Spacy를 이용한 전처리 :*

**Spacy 라이브러리**와 **파이썬 정규식**을 사용하여 대소문자 통일 후 구두점이나 특수문자를 제거할 수 있다.

<br>

*라이브러리 import 후 모델 불러오기.*

~~~python
import spacy
from spacy.tokenizer import Tokenizer

nlp = spacy.load("en_core_web_sm")
tokenizer = Tokenizer(nlp.vocab)
~~~

<br>

*전처리*


~~~python
import re

tokens = []

for doc in tokenizer.pipe(df['text']):
    doc_tokens = [re.sub(r"[^a-z0-9]", '', token.text.lower() for token in doc)] # 대소문자 통일 후 정규식 사용해서 소문자와 숫자 빼고 다 제거. 
    tokens.append(doc_tokens)

df['tokens'] = tokens
~~~

<br>

*대소문자 제거나 특수문자 제거 등은 대문자와 특수문자가 주는 정보를 잃는 다는 단점이 있다.*

<br>

### *불용어 처리*

<br>

텍스트 데이터를 다룰때 'I', 'and', 'a', 'an' 등 별 의미가 없는 것들을 불용어라고 한다. Spacy 라이브러리에 있는 불용어를 이용해 이런 불용어를 처리할 수 있다. 물론 다른 NLP 라이브러리들에서도 대명사, 관사, 접속사 등을 포함한 일반적인 불용어가 내장되어 있다. 

<br>

*기본 불용어를 사용하여 불용어 처리*


~~~python
tokens = []

for doc in tokenizer.pipe(df['text']):
    doc_tokens = []

    for token in doc:
        # 불용어와 구두점이 아니면 저장.
        if (token.is_stop == False) & (token.is_punct == False):
            doc_tokens.append(token.text.lower())

    tokens.append(doc_tokens)

df['tokens'] = tokens
~~~

<br>

*불용어에 특정 단어 추가해서 제거*


~~~python
# 불용어에 단어 추가. (기본 불용어 타입이 set이기에 union 사용.)
STOP_WORDS = nlp.Defaults.stop_words.union(['soccer', 'baseball'])

tokens = []

for doc in tokenizer.pipe(df['text']):
    doc_tokens = []

    for token in doc:
        if token.text.lower() not in STOP_WORDS:
            doc_tokens.append(token.text.lower())
    tokens.append(doc_tokens)

df['tokens'] = tokens
~~~

<br>

*불용어 처리의 경우 문장의 문법적인 정보를 잃는다는 단점이 있다. 예를 들어 '나는 밥을 먹었다'에서 불용어를 제거하면 '나 밥 먹'이 된다. 그렇기에 챗봇같은 경우에 불용어 제거는 적절하지 않을 수 있다.*

<br>

### *어간 추출 (Stemming), 표제어 추출 (Lemmatization)*

<br>

어간추출은 'es', 'ing' 's' 등을 제거하여 어간만 남기고 표제어추출은 단어를 기본 사전형 단어 형태인 표제어로 바꾼다. 그렇기에 표제어추출 방법이 시간이 더 오래 걸린다.

예를 들어보자면, 'wolves'의 경우 어간 추출을 사용하면 'wolv'로, 표제어 추출을 사용하면 'wolf'로 변형된다.

<br>

*어간 추출*

~~~python
# nltk 사용.
from nlk.stem import PorterStemmer

ps = PorterStemmer()

tokens = []

for doc in tokenizer.pipe(df['text']):
    doc_tokens = []

    for token in doc:
        doc_tokens.append(ps.stem(token))
    tokens.append(doc_tokens)

df['stems'] = tokens
~~~

<br>

*어간 추출의 경우 빠르기 때문에 간단히 사용할때 많이 사용되고 속도가 중요한 검색 분야에서 많이 사용된다.*

<br>

*표제어 추출*

~~~python
# Spacy 사용

def get_lemmas(text):

    lemmas = []

    doc = nlp(text)

    for token in doc:
        if ((token.is_stop == False) and (token.is_punct == False)) and (token.pos_ != 'PRON'):
            lemmas.append(token.lemma_)
    
    return lemmas

df['lemmas'] = df['text'].apply(get_lemmas)
~~~

<br>

*표제어 추출의 경우 위에서 봤듯이 복수 정보 등이 사라진다는 단점이 있다.*

<br>
<br>

## **2. 벡터화**

<br>

### *Bag-of-Words (BoW) :*

가장 간단한 벡터화 방법으로 단어의 빈도수만으로 벡터화를 진행한다.

<br>

~~~python
from sklearn.feature_extraction.text import CountVectorizer

# 단어 최대 300개
count_vect = CountVectorizer(stop_words='english', max_features = 300)

dtm_bow = count_vect.fit_transform(df['text'])

# 벡터화를 진행하면 문서-단어 행렬로 바뀌는데, 0을 표현하지 않는 행렬로 나오기에 todense() 해줌.
dtm_bow = pd.DataFrame(dtm_bow.todense(), columns=count_vect.get_feature_names())
~~~

<br>

위에서 나오는 데이터 프레임의 예는 다음과 같다.

||word1|word2|
|--|--|--|
|0|1|0|
|1|1|1|

<br>

*Bow의 경우 많이 등장하는 단어의 영향이 커진다는 단점이 있다. 의미있는 단어는 모든 문서에 등장하는 단어가 아니라 특정 문서에만 등장하는 단어일 확률이 높기 때문이다.*

*우리가 군대에서 선임이 오늘 메뉴 뭐냐고 물어보면 맨날 나오는 '흰 밥'이라고 대답하지 않고 가끔 나오는 '소야'라고 답하는 것과 같은 맥락이다.*

<br>

### *TF-IDF(Term Frequency-Inverse Document Frequency) :*
TF_IDF의 수식은 `TF*IDF`이다. TF는 특정 문서에서 단어가 등장한 빈도수이고 IDF는 역문서 빈도로 해당 단어가 많은 문서에서 등장할 수록 0에 가까워진다. (모든 문서에 등장할 경우 0) 

~~~python
tfidf = TfidfVectorizer(stop_words='english', max_features=100)

dtm_tfidf = tfidf.fit_transform(sentences_lst)

dtm_tfidf = pd.DataFrame(dtm_tfidf.todense(), columns=tfidf.get_feature_names())
dtm_tfidf
~~~

<br>

위 데이터 프레임의 예는 다음과 같다.

<br>

||word1|word2|
|--|--|--|
|0|0.32|0.56|
|1|0.64|0.75|