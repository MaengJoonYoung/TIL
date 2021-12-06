# **Scheduling**

<br>

## **1. 시간 표기 방법**

<br>

*스케줄링에 대해 말하기 위해서는 우선 컴퓨터에서 시간을 표기하는 방법을 먼저 얘기해야 한다. 또한 우리가 사용하는 서비스에서 시간은 자주 사용되는데, 요즘은 전세계의 서비스를 전세계인이 쓰기 때문에 시간을 어떻게 표기해야하는지는 무시할 수 없는 문제라고 한다.*

<br>

### *UTC*
: UTC는 협정 세계시로 영국을 기준으로 한 시간이다. UTC로 시차를 표현할 수 있는데 **'UTC±0'** 을 기준으로 표현한다. 우리나라의 표준 시간은 **'KST'** 인데 표기할 때 **UTC+09**로 표현한다. 즉, UTC에서 9시간이 차이가나는 것이다. UTC는 그리니치 평균 시라고 하는 GMT와 초의 소숫점 단위에서만 차이가 나기에 UTC와 GMT는 혼용되기도 한다.

<br>

### *ISO 8601*
: ISO 8601은 날짜와 시간과 관련된 데이터 교환을 다루는 국제 표준이다. 위에서 다룬 UTC, KST는 어떤 시간 기준을 사용할지와 관련된 개념이라면 ISO 8601은 이러한 시간을 어떻게 표현하냐의 문제이다.

기본표기법은 '20211206'처럼 날짜를 표기하는 것인데, 다양한 확장 표기법이 존재하고 다음과 같이 표기할 수 있다.
~~~
2021-12-06T23:20:30Z
~~~
가운데 'T'는 시간과 날짜를 구분하는 것이고 맨 뒤에 'Z'는 타임존을 의미한다.

<br>

### *Unix Time*
: 'Epoch Time'이라고도 말하며 영국의 1970년 1월 1일 0시 0분 0초를 기준으로 +1초나 -1초를 하여 시간을 표현한다. 터미널에 `date +%s`를 입력하면 Unix Time을 볼 수 있는데 지금 내 터미널 창에는 ***'1638790638'*** 가 나온다 즉, 영국의 1970년 1월 1일 0시 0분 0초에서 1638790638초가 흘렀다는 것이다. 

앞에서 다룬 ISO 8601은 사람이 보기 편한 방식이고 Unix Time은 컴퓨터가 인식하기 편한 방식이다. 그렇기에 Unix Time의 경우 특정 시점들 간의 차이를 계산하거나 하는 연산에 중점을 뒀을 때 사용한다.

<br>
<br>
<hr>
<br>

## **2. APScheduler**

<br>

스케줄링을 배우기 전까지는 스크레이핑을 통해 데이터를 모았고, 코드를 작성하여 한번에 모든 데이터가 수집되게 할 수 있었다. 하지만, 특정 날짜나 시점 혹은 일정한 시간마다 데이터를 모아야할 경우가 있는데 이럴때 사용할 수 있는게 바로 스케줄링 도구이다.

오늘은 그 중에서 Python의 라이브러리인 APScheduler에 대해 배웠고 사용 순서는 다음과 같다.

*스케줄러 선언 → 스케줄러에 job 선언 → 스케줄러 시작*

<br>

~~~python
# 스케줄러 선언

from apscheduler.schedulers.blocking import BlockingScheduler

scheduler = BlockingScheduler({'apscheduler.timzone':'UTC'}) # UTC+0을 기준으로 선언.

# 스케줄러에 사용될 job 선언하기

def hello():
    print('안녕하세요, 여러분 저는 5초마다 실행됩니다.')

scheduler.add_job(func=hello, trigger='interval', seconds=5)

# 스케줄러 시작하기.

scheduler.start() # 5초마다 프린트 실행.
~~~

<br>

위에서는 `trigger`를 `interval`로 설정했는데 `interval`을 포함한 3가지 방법이 있다. 

<br>

~~~python
date # 특정 시점을 지정하여 사용 

interval # 내가 설정한 시간마다 반복하여 실행

 cron # cron 문법을 기반으로 하여 실행
~~~

<br>

또한 위에서는 `BlockingScheduler`을 사용했는데 이 외에도 다양한 스케줄러가 있다. `BlockingScheduler`의 경우 스케줄러 자체가 프로그램의 목적이 되는 경우 사용을 한다. 아래와 같은 스케줄러 들은 다른 어플리케이션을 실행하는 게 목적이고 스케줄러는 부가적인 기능인 경우에 사용한다. 

* `BackgroundScheduler`
* `AsyncIOScheduler`
* `GeventScheduler`
* `TornadoScheduler`
* `TwistedScheduler`
* `QtScheduler`