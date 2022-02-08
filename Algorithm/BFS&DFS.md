# **BFS & DFS**

그래프나 트리처럼 노드들의 연결되어 있을때 각 노드들을 한번씩 탐색하는 것을 **순회**라고 한다.

그래프를 순회하는 방법은 크게 **BFS(너비 우선 탐색)와 DFS(깊이 우선 탐색)** 가 있다.

<br>

## **1. BFS(Breadth-First Search)**

<br>

![bfs](https://user-images.githubusercontent.com/89771322/153000497-d261b603-7b87-4d96-a481-f641049964cd.gif)<br>
[출처](https://developer-mac.tistory.com/64)

<br>

BFS는 **시작 하는 정점에서 인접한 정점들을 먼저 탐색하는 방법**으로 이루어진다. 위 그림의 경우 1번 노드와 인접한 2, 3, 4를 탐색하고 이후에 2와 인접한 5를 3과 인접한 6,7을 4와 인접한 8을 탐색하는 순서로 진행된다. 위의 그림을 트리구조라고 한다면, **층에 있는 노드를 모두 탐색한 후 층을 하나씩 내려간다**고 생각할 수 있다.

<br>

### ***구현***

<br>

BFS는 FIFO구조를 가진 queue를 사용하여 구현할 수 있다. 큐를 사용하여 구현하면 순회하는 과정은 다음과 같다.

1. 시작 정점을 큐에 인큐한다.(삽입한다)
2. 큐에서 디큐(추출)
3. 꺼내진 정점과 인접한 모든 정점을 큐에 인큐한다.
4. 디큐했던 정점을 방문처리한다.
5. 큐에 아무것도 남지 않을때 까지 반복한다.

<br>

```python
from collections import deque

def bfs(start_node, bfs_graph):
    queue = deque() # 덱(양방향 큐)을 사용한다.
    queue.append(start_node) # 시작점을 먼저 인큐
    bfs_list = []

    while queue:
        cur_node = queue.popleft() # 큐에서 디큐한 정점을 현재 정점으로 사용.
        for i in bfs_graph[cur_node]:
            if i not in bfs_list: # bfs_list에 없다는 건 아직 방문하지 않았다는 것.
                queue.append(i)
            if cur_node not in bfs_list:
                bfs_list.append(cur_node)
    return bfs_list
```
> 큐는 선입선출의 구조이기 때문에 디큐를 진행하기 위해서는 `pop(0)`을 사용해야 한다. 하지만 deque의 `popleft()`의 수행시간이 `pop(0)`을 사용했을 때 보다 훨씬 빠르기 때문에 deque를 이용해서 구현한 것이다.

<br>

## **2. DFS(Depth-First Search)**

<br>

![dfs](https://user-images.githubusercontent.com/89771322/153003523-d8621414-1ff5-4959-b1de-fe9ac6a9ac69.gif)<br>
[출처](https://developer-mac.tistory.com/64)

<br>

DFS는 시작 정점에서 인접한 노드를 파악한 후 특정 노드를 선택하여 최대 깊이까지 내려간다. 이후 더 이상 내려갈 곳이 없으면 다시 올라와서 파악해뒀던 다른 인접 노드의 최대 깊이까지 내려가는 방식으로 진행된다. 즉, **최대 깊이까지 내려가면 옆으로 이동하여 다시 반복하는 형식**이다.

이때 다시 올라가는 과정을 backtracking이라고 한다.

<br>

### ***구현***

<br>

DFS는 BFS를 구현했던 방식에서 큐를 스택으로만 바꾸면 구현이 가능하고 재귀를 활용해서도 구현이 가능하다. 재귀를 사용한 방법은 아래와 같다.

```python
def dfs(node, dfs_graph, dfs_list=[]):
    dfs_list.append(node)

    for i in dfs_graph[node]:
        if i not in dfs_list:
            dfs_recur(i,dfs_graph,dfs_list)
    return dfs_list
```
> 인접 노드부터 함수를 재귀적으로 호출한뒤 더 이상 호출하지 못하면 함수가 종료되고 다시 첫번째 시작 정점으로 돌아와 다른 인접노드를 재귀적으로 호출하는 방식으로 진행된다.

<br>

## **3. BFS vs DFS**

<br>

![bfs_dfs](https://user-images.githubusercontent.com/89771322/153008218-5224a40a-23b6-4626-81e5-fbbb50a070c6.png)<br>
[출처](https://velog.io/@elma98/210620.-Today-I-LearnedTIL-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0BFS-DFS)

<br>


BFS와 DFS는 그래프와 트리의 구조에 따라 서로의 장단점이 역전되기도 하기 때문에 단순히 비교할 수는 없다. 즉, 자신이 어떤 자료구조에서 순회를 사용할 것인지를 파악하여 올바른 결정을 해야한다.

***그러면 어떤 상황에 어떤 방법을 사용해야 할까?***

1. 모든 정점을 방문하는 경우 : **아무거나 사용해도 무방**하다. 다만, DFS의 경우 재귀를 사용할 경우 RecursionError에 유의해야한다.
2. 최단 경로 : 주로 **BFS를 선호**한다고 한다. 그 이유는 BFS는 인접한 노드들을 우선으로 방문하기에 시작 정점과 목표 정점간의 최단 길이 경로를 보장하지만(가중치가 1일 경우), DFS의 경우 최대 깊이까지 내려갔다가 다시 돌아오기에 최단 길이 경로를 보장하지는 않는다. 하지만, 순회하고자 하는 트리의 구조가 **극심하게 언밸런스하다면, DFS가 첫 방문 노드만 잘 설정하면 훨씬 빠른 경로를 발견할 수도 있다.**
3. 경로의 특징을 저장해야하는 경우 : DFS를 선호한다고 한다. 위 그림에서 볼 수 있듯이 BFS는 경로와 상관없이 인접 노드들을 탐색하지만 DFS는 경로를 따라가며 순회하기에 DFS를 선호한다.

<br>
<br>
<hr>

### *참고자료*

https://ko.wikipedia.org/wiki/%EA%B9%8A%EC%9D%B4_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89<br>https://ko.wikipedia.org/wiki/%EB%84%88%EB%B9%84_%EC%9A%B0%EC%84%A0_%ED%83%90%EC%83%89<br>
https://devuna.tistory.com/32<br>