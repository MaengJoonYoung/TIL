# **그래프**

<br>

![graph](https://user-images.githubusercontent.com/89771322/152749454-355561c9-ba25-4df6-b5f9-d8b7b5138744.png)<br>
[출처](https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%9E%98%ED%94%84_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0))

<br>

그래프는 위의 그림처럼 동그라미들과 이를 연결하는 선들로 이루어진 자료구조이다. 여기서 동그라미는 vertex(정점)이라 부르고 이 정점들을 연결한 선을 edge(간선)이라고 한다. 즉, **연결된 정점들간의 관계를 나타내는 자료구조이다.**

그래프는 현실세계의 많은 것들을 표현할 수 있는 자료구조 중 하나이다. 현실세계에서 수많은 개체들이 서로 관계를 맺는데, 이를 간선을 이용해 연결시킨 형태가 바로 그래프가 되기 때문이다. 이러한 그래프를 활용하여 SNS에서 친구들간의 관계를 그래프로 표현하여 적합한 친구추천을 제공할 수도 있을 것이다. 이를 그림으로 표현한다면 아래와 같다.

<br>

![Sns](https://user-images.githubusercontent.com/89771322/152781858-0b4a1209-f3f8-494d-8d2e-4c360bd395c7.jpeg)<br>
[출처](https://velog.io/@delilah/IM-1W-Tree-Graph-Binary-Search-Tree)

<br>

## **1. 그래프의 유형**

<br>

### ***방향성, 무방향성 그래프***

<br>

### *1) 방향성*
<br>

![bidirec](https://user-images.githubusercontent.com/89771322/152783185-74f399a2-5aab-4ade-ba58-cc66d9454897.jpeg)

<br>

방향성 그래프는 말 그대로 **간선들이 방향을 가진 그래프**이다. 간선의 방향은 위 그림처럼 한쪽만을 가르킬 수도 있고 양방향을 가르킬 수도 있다. 도시를 예로 든다면, 한 도시에 양방향 통행 도로와 일방통행 도로가 같이 있는 모습이라고 할 수 있다.

<br>

### *2) 무방향성*

<br>

![un](https://user-images.githubusercontent.com/89771322/152783460-56da27b3-3222-43f4-8d05-e79e9a1ec78e.jpeg)

<br>

무방향성 그래프는 **간선들이 방향을 갖고 있지 않은 그래프**로 각 연결된 정점들은 항상 상호교환관계에 있다. 즉, 양방향 도로만 있는 도시의 모습이라고 할 수 있다.

<br>

### ***가중치 그래프***

<br>

![weighted](https://user-images.githubusercontent.com/89771322/152784090-3b83f4cf-7108-4d00-b2b8-f2af3188bbbb.jpeg)

<br>

가중치 그래프는 **간선에 가중치가 포함되어 있는 그래프**이다. 서울, 대전, 부산을 그래프로 가중치 그래프로 표현하면, 가중치를 각각의 거리로 표현할 수 있을 것이다. 이러한 가중치 그래프를 활용하는 예로는 최단거리를 안내하는 네비게이션을 생각해볼 수 있다.

<br>

### ***순환, 비순환 그래프***

<br>

![Cyclinc](https://user-images.githubusercontent.com/89771322/152784850-db369289-3b7e-4411-8a8d-117d9eb8b61d.jpeg)

<br>

순환 그래프는 말 그대로 위 그림처럼 순환하는 **사이클이 있는 그래프**이고 비순환 그래프는 **사이클이 없는 그래프**이다.

<br>

## **2. 그래프의 구현**

<br>

그래프를 구현하는 방법으로는 대표적으로 인접행렬을 사용하는 방법과 인접리스트를 사용하는 방법이 있다.

<br>

### ***인접리스트***

<br>

인접리스트로 구현하는 방법은 각 정점에 인접한 정점들을 리스트로 표현하는 방법이다. 아래의 그림을 파이썬의 딕셔너리를 사용해 그래프로 구현한다면 다음과 같이 구현할 수 있다.

<br>

![1234](https://user-images.githubusercontent.com/89771322/152787272-6d8377f3-f117-40fc-bb60-8fcad2a74cda.png)<br>
[출처](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=babobigi&logNo=220479341235)

<br>

```python
class Graph:
    def __init__(self):
        self.vertices = {
            1 : {2,3},
            2 : {1,3},
            3 : {1,2,4},
            4 : {3}
        }
```

<br>

*장점:*
1. 실제로 존재하는 간선의 개수의 정보만 가지고 있다. **즉, 간선의 개수에 비례해 메모리를 차지한다. (인접행렬에 비해 메모리 측면에서 효율적이다.)**
2. 간선의 개수를 E라고 했을때, 각 노드들과 연결된 노드를 모두 방문해 보기 위해서 O(E)만큼의 시간복잡도를 가진다. (실제 연결된 노드들만 볼 수 있기 때문)

*단점:*
1. 전체 노드의 개수가 V인 경우 특정 노드 i와 j가 연결되어 있는지 확인하기 위해서 시간복잡도는 O(V)이다. (arr[i] 전체를 탐색하며 j가 있는지 확인하기 때문)

<br>

### ***인접행렬***

<br>

인접행렬은 그래프의 연결관계를 2차원 배열로 나타낸 것이다. 위의 그림을 인접행렬로 나타내면 다음과 같다.

<br>

```python
class Graph:
    def __init__(self):
        self.edges = [
            [0,1,1,0],
            [1,0,1,0],
            [1,1,0,1],
            [0,0,1,0]
        ]
```

>이때 원소의 값은 간선의 여부를 나타내거나 가중치를 나타낼 수 있다.

<br>

*장점*

1. 특정 노드간의 연결 상태를 확인하는 시간복잡도가 O(1)이다. (arr[i][j]가 0인지 1인지만 확인하면되기 때문)

*단점*
1. 간선의 개수와 상관없이 항상 V^2크기의 2차원 배열이 필요하다.
2. 각 노드들과 연결된 모든 노드를 탐색하려면 모든 배열을 다 탐색해야 한다. 즉, 시간복잡도가 O(V*V)이다.

<br>
<br>
<hr>

### *참고자료*

https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%9E%98%ED%94%84_(%EC%9E%90%EB%A3%8C_%EA%B5%AC%EC%A1%B0)<br>
https://suyeon96.tistory.com/32 <br>
https://sarah950716.tistory.com/12 <br>
https://duwjdtn11.tistory.com/515 <br>
https://shoark7.github.io/insight/rationality/relationship-between-graph-and-tree