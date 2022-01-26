# **트리 구조**

<br>

![tree-terms](https://user-images.githubusercontent.com/89771322/151170829-523ea607-d919-44d3-a2e2-eca28cf25b40.png)

<br>

트리는 위 그림처럼 node들과 이를 연결하는 가지들로 이루어진 자료구조로 사이클을 이루지 않도록 구성되어 있다. 트리는 비선형적 자료구조로 각 노드는 부모 노드와 자식 노드로 연결되어 있어 계층적인 관계를 표현한다.

<br>

## **1. 트리 구조 용어**

<br>

* 루트
  * 가장 위에 있는 노드를 루트 노드라고 부른다. 트리별로 하나씩만 가질 수 있다.
* 서브트리
  * 자식 노드이면서 부모 노드의 역할도 하는 노드가 있는 트리
* 차수
  * 노드가 갖고 있는 자식 노드의 최대 갯수 (위의 그림의 경우 2차수)
* 리프
  * 자식 노드를 갖고 있지 않는 노드로 한 트리 내에 여러개가 존재할 수 있다.
* 레벨
  * 루트 노드에서 얼마나 떨어져 있는지를 나타낸다. (루트는 0)
* 높이
  * 루트 노드에서 가장 멀리 떨어진 노드와의 거리 (위의 경우 3)
* 형제 노드
  * 부모가 같은 노드

<br>

## **2. 이진 탐색 트리**

<br>

이진 트리는 **자식 노드를 최대 2개로 갖는 트리**를 말한고 이진 탐색 트리(Binary Search Trees)는 이진 트리에서 **빠르게 노드를 탐색하고 정렬하기 위한** 이진 트리이다.

이를 위해 이진 탐색 트리는 **'값 크기 조건'** 을 갖는데, 값 크기 조건은 다음과 같다.

1. 중복되는 값이 없어야 한다.
2. 왼쪽 자식 노드의 값은 부모 노드의 값 보다 작다.
3. 오른쪽 자식 노드의 값은 부모 노드의 값 보다 크다.

![이진 탐색 트리](https://user-images.githubusercontent.com/89771322/151174165-8829206c-3fbf-4901-acd4-815512ca505e.png)

위 그림처럼 조건을 지킨 이진 탐색 트리는 **탐색이 일반 이진 트리에 비해 빠르기 때문에 노드를 삽입하는 게 수월하다.**

<br>

## **3. 이진 탐색 트리 삽입**

<br>

이진 탐색 트리에서 노드를 삽입하는 과정은 다음과 같다.

<br>

*우선 삽입하고자 하는 노드의 값과 루트 노드 값의 대소를 비교한다. → 조건에 맞는 노드로 이동한다. → 이후 위 과정을 반복한다. → 이러한 과정에서 노드가 비어있다면 해당 노드에 삽입하고자 하는 노드를 삽입한다.*

<br>

### ***구현***

<br>

```python
class node:
  def __init__(self, value):
    self.value = value
    self.left = None
    self.right = None

class binary_search_tree:
    def __init__(self, head):
        # head는 node의 인스턴스
        self.head = head 


    def insert_node(self, value):
        # 루트 노드에서 부터 탐색 시작.
        self.base_node = self.head # base_node는 현재 node의 위치를 나타내는 커서
        while True:
            if value < self.base_node.value: # value가 node의 value보다 작으면 왼쪽으로 이동
                if self.base_node.left != None:
                    self.base_node = self.base_node.left
                else: # node.left가 None이면 node 왼쪽에 추가
                    self.base_node.left = node(value)
                    break
            elif value > self.base_node.value: # value가 node의 value보다 크면 오른쪽으로 이동
                if self.base_node.right != None:
                    self.base_node = self.base_node.right
                else: # node.right가 None이면 node 오른쪽에 추가
                    self.base_node.right = node(value) 
                    break
```

<br>

## **4. 이진 탐색 트리 검색**

<br>

이진 탐색 트리에서 검색은 다음과 같이 이뤄진다.

<br>

루트 노드의 값과 찾으려는 값이 같은지 비교. → 같지 않다면 대소 관계를 파악하여 적절한 위치로 이동. → 위 과정을 반복하여 값이 같은 노드를 찾는다.

<br>

### ***구현***

<br>

```python
class node:
  def __init__(self, value):
    self.value = value
    self.left = None
    self.right = None

class binary_search_tree:
    def __init__(self, head):
        # head는 node의 인스턴스
        self.head = head 

    def search_node(self, value): # 있으면 True, 없으면 False
        self.base_node = self.head
        while self.base_node : # node가 None이면 반복문 탈출
            if value < self.base_node.value: # value가 node의 value보다 작으면 왼쪽으로 이동
                self.base_node = self.base_node.left
            elif value > self.base_node.value: # value가 node의 value보다 크면 오른쪽으로 이동
                self.base_node = self.base_node.right
            else: # 탐색하려는 값 찾으면 break
                return True
        return False # 반복문을 탈출 했으면 value가 없는 것이니 False 반환.
            
```

<br>

## **5. 블군형 이진 트리**

<br>

이진 탐색 트리의 경우 찾고자 하는 값의 범위를 반으로 좁혀가며 탐색하기에 Big O 시간복잡도로 표현하면 `O(log n)`)의 시간이 걸린다.

***하지만, 만약 삽입되는 node의 값들이 오름차순이나 내림차순으로 정렬된 값이라면 어떻게 될까?***

이 경우에 트리는 왼쪽이나 오른쪽으로만 자식 노드들을 생성한 아래와 같은 불균형 이진 트리가 될 것이다.

<br>

![불균형](https://user-images.githubusercontent.com/89771322/151178755-5789201e-5fca-4494-b9ff-dee76b599760.png)

<br>

위의 그림과 같은 경우에 4라는 value를 검색하려면 루트 노드부터 모든 노드를 지나야 찾을 수 있을 것이다. 즉 `O(n)`만큼의 시간이 걸리게 되는 것이다.

*그렇기에 BST의 효율을 높이기 위해선, 불균형 트리를 균형 트리로 바꿔주는 작업을 진행해야 된다고 한다.*

<br>
<br>
<hr>

### *참고자료*
https://m.blog.naver.com/PostView.nhn?blogId=qbxlvnf11&logNo=221371437794&proxyReferer=https:%2F%2Fwww.google.com%2F <br>
https://www.fun-coding.org/Chapter10-tree.html