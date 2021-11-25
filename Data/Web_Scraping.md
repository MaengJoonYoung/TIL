# **Web Scraping**

<br>

오늘은 Web에서 데이터를 긁어오는 Web Scraping에 대해서 배웠다. 항상 내가 필요한 데이터가 kaggle에 정제되어 있는 채로 올라와 있다는 건 말이 안된다. 또한 웹에는 정제되지 않은 최신 데이터가 계속해서 쌓이고 있다. 그렇기에 내가 원하고 필요로 하는 데이터를 더욱 많이 얻기 위해서는 Web Scraping을 할 줄 알아야 한다. 

<br>
<hr>

<br>

## **1. HTML과 CSS**

<br>

Web Scraping을 배우는데 HTML과 CSS를 배우는 이유는 웹에서 데이터를 긁어오기 위해서는 웹의 구조가 어떻게 구성되어있는지 알아야하기 때문이다.

**HTML과 CSS 그리고 JavaScript는** 항상 붙어다닌다. HTML은 웹 페이지에 정보를 CSS는 디자인을 JavaScripts는 생명력을 불어넣어주기 때문이다. 우선 웹페이지가 어떻게 어떻게 구성되는지 알려주는 알려주는 마크업 언어인 HTML에 대해 알아보자.

<br>

### *HTML*

<br>

HTML에는 head, body, div 등 다양한 요소가 존재하고 이 요소들은 다음과 같이 태그를 통해 표현된다.

<br>

~~~html
<head>
</head>
<body>
    <div>
    </div>
</body>
~~~

<br>

요소 앞에 /가 있는 태그는 닫아주는 태그이고, 아무것도 없는 건 열어주는 태그이다. 물론 모든 태그가 다 열어주고 닫아줘야 하는 건 아니다.

<br>

### *CSS*

<br>

HTML이 정보에 치중한 언어라면 CSS는 디자인에 치중한 언어이다. 즉, 웹 페이지를 꾸며주는 역할을 한다. 사실 HTML에서도 스타일을 입혀주는 태그가 있다. 하지만 이를 사용하는 건 비효율적이기에 CSS를 사용하는 것이다. 스타일을 입혀주는 간단한 예로 폰트의 크기를 바꾸거나 색을 입히는 것들이 있다.

<br>

### *CSS Selector*

<br>

사실 HTML에는 class와 id라는 속성이 있다. class는 학교에 있는 '반'이라는 개념을 생각하고 id는 '학번'을 생각하면 이해하기 쉽다.

~~~html
<ul>
    <li class="hi">Hello</li>
    <li id="world">World</li>
    <li class="hi">!</li>
</ul>
~~~

위와 같은 경우 CSS Selector를 사용해서 "hi"라는 class에 빨간색을 입히면 'Hello' 와 '!'가 다 빨간색으로 바뀐다. 즉, class를 사용해 요소들을 그룹핑한 것이다. 사실 이는 id를 사용해서도 가능한데, 위의 작업에서 class를 id로만 바꿔주면 가능하다. 하지만 이는 약속을 깬 것이다. id는 동일한 학번을 많은 학생이 가지면 안되는 것과 같이 특정 요소에만 지정해주는 것으로 약속이 되어있다.

CSS selector를 사용한 에는 다음과 같다.

~~~css
.hi{
    color:"red";
}

#world{
    color:"blue";
}
~~~

*위와 같이 class를 선택할 때는 '.'을, id를 선택할 때는 '#'을 사용한다.*

<br>
<br>
<hr>
<br>

## **2. DOM(Document Object Model)**

<br>

DOM은 풀네임 그대로 **문서 객체 모델**이다. DOM은 HTML, XML 등 문서의 프로그래밍 인터페이스이다. ***조금 더 자세히 말하자면 HTML 문서, 태그 등을 다 객체화 하여 프로그래밍 언어로 제어할 수 있게 해주는 것이다.*** 여기서 객체(object)는 JavaScript에서 사용하는 데이터 구조 중 하나이다.

<br>

DOM은 웹 브라우저에서 개발자 도구를 열고 콘솔에 들어가면 손쉽게 사용할 수 있다. DOM은 아래와 같은 기능이 있다.

~~~javascript
getElementsbyTagName // 태그 이름을 기준으로 문서들의 요소를 가져온다.
getElementById // Id가 일치하는 요소들을 가져온다.
getElementsByClassName // clss가 일치하는 요소들을 가져온다.
querySelector // 셀렉터와 일치하는 요소 하나를 가져온다.
querySelectorAll // 셀렉터와 일치하는 요소를 다 가져온다.
~~~

<br>

*만약 엄청 큰 하나의 문자열에서 원하는 정보를 찾아야한다면 태그를 구분 짓는 법부터 시작해서 해야할 일이 산더미일 것이다. 이럴 때 DOM을 잘 활용한다면 효율적으로 데이터를 잘 긁어올 수 있을 것이다.*

<br>
<br>
<hr>
<br>

## **3. Python을 이용한 Web Scraping**

<br>
<br>

Web Scraping을 하기 위해서는 우선 웹과 소통을 해야한다. 이를 도와주는 것이 ***requests 라이브러리다.***

<br>

### *requests*

<br>

사용 방법은 다음과 같다.

<br>

~~~python
pip install requests # 설치

import requests # 라이브러리 불러오기.
url = 'https://www.naver.com/'
requests.get(url) # 요청 보내기.
# 만약 성공했다면 <Response [200]>이 뜬다.
~~~

<br>

여기서 200은 HTTP 상태코드인데 다양한 상태코드에 대한 설명은 [HTTP 상태코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status) 여기서 확인할 수 있다.

<br>

HTTP 상태코드가 200이 아닌 경우 예외를 발생시키는 resp.raise_for_status()를 통해 제대로 연결이 됐는지 확인하는 방법이 있다.

<br>

~~~python
from requests.exceptions import HTTPError

try:
    resp = requests.get(url)

    resp.raise_for_status()
except :
    print('에러! 에러!')
else:
    print('성공~!~!')

# 실패하면 에러! 에러! 나옴.
~~~

<br>

우리가 받아온 문서가 어떤 정보를 담고 있는지 확인하기 위한 방법은 다음과 같다.

<br>

~~~python
resp.text # 문자열로
resp.content # raw한 상태로
~~~

<br> 

text를 통해 HTML을 문자열로 받을 수 있고 content를 사용해서는 조금 더 raw한 상태로 받을 수 있다.

<br>

### *BeautifulSoup*

<br>

이제 이렇게 우리가 받아온 응답내용을 python에서 파싱하고 정보를 얻어내야 하는데 이때 사용하는 것이 바로 **BeautifulSoup 라이브러리다.** 여기서 파싱은 받아온 응답내용을 pythond에서도 DOM 형식처럼 가져오게 해주는 것이다.

<br>

사용방법은 아래와 같다.

<br>

~~~python
pip install beautifulsoup4 # 설치

from bs4 import BeautifulSoup # 라이브러리 불러오기.

page = requests.get(url) # 파싱할 페이지 받아오기.
soup = BeautifulSoup(page.content, 'html.parser') # 파싱. (html.parser는 파이썬에 포함된 기본 라이브러리.)
~~~

<br>

파싱을 완료했으니 이제 메서드를 활용하여 요소들을 가져오면 된다.

bs4에서 하나의 요소를 찾을때는 `find`를, 여러개의 요소를 찾을 때는 `find_all`을 사용한다. `find_all`은 조건에 맞는 결과를 리스트에 담아 보내준다.

만약 fruit라는 클래스를 가진 요소를 다 가져오고 싶으면 다음과 같이 사용한다.

<br>

```python
fruit_elements = soup.find_all(class_ = 'fruit')
```

<br>

여기서 `class`밑에 `'_'`를 추가한 이유는 추가하지 않으면 파이썬의 클래스로 인식하기 때문이다.

<br>

태그를 활용하여 조금 더 상세하게 요소를 가져올 수 있다. 'fruit'라는 class가 'div' 태그에도 있고 'li'라는 태그에도 있는데, 'div' 태그에 있는 요소만 가져오고 싶다면 다음과 같이 태그를 활용하면 된다.

<br>

```python
fruit_elements = soup.find_all('div', class_ = 'fruit')
```

<br>

원하는 요소를 가져왔으니 이제는 정보를 가져와보자. 

<br>


```html
<p class='fruit'>I am apple</p>
```

<br>

이런 HTML에서 I am apple이라는 글을 가져와 보자.

<br>

```python
fruit_elements = soup.find('p', class = 'fruit')
fruit_elements.text
# -> 'I am apple'
```

<br>

이런 방식을 정보를 가져올 수 있다.