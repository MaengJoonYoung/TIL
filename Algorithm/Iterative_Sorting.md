# **반복 정렬**

<br>

정렬은 정렬하고자 하는 조건에 맞춰 숫자들을 비교하여 조건에 맞게 순서를 바꿔주는 작업을 말한다. **반복 정렬**은 **데이터에서 특정 값들을 선택 → 비교 → 스왑 하는 과정을 반복**하는 것을 말한다.

<br>

## **1. 선택 정렬 (Selection Sort)**

<br>

![selectionsort](https://user-images.githubusercontent.com/89771322/151697166-3b76404b-ab8a-4a43-9f58-60ed3f7f695c.png)

<br>

선택 정렬은 위 그림처럼 주어진 **데이터에서 최솟값을 찾아서 위치를 바꿔주는식**으로 정렬을 진행하는데 자세한 과정은 다음과 같다.

1. 데이터에서 최솟값을 찾는다.
2. 찾은 최솟값과 가장 앞에 있는 값의 위치를 변경한다.
3. 정렬되지 않은 데이터에서 위의 과정을 반복한다.


이러한 과정을 아래와 같이 나타낼 수 있다.

<br>

![Selection_sort_animation](https://user-images.githubusercontent.com/89771322/151697382-3d966d71-13ec-4ec9-9803-a8cddcef861b.gif)

<br>

선택 정렬 단점 중 하나는 **불안정 정렬**인데, 불안정 정렬인 이유는 다음과 같다. 

**B, b, a, c**의 순서로 입력된 데이터를 선택 정렬로 정렬하면 첫번재로 **최솟값인 a와 B가 교환**될 것이다. 그 결과는 **a, b, B, c**가 될 것이고 그 다음 과정에서부터는 **정렬이 잘 되어 있기에 더 이상 위치가 바뀌지 않아서 a, b, B, c가 최종 결과가 될 것이다.** 이렇게 되면 입력되었을때는 **중복값인 B와 b 중 B가 더 앞에 있었지만 정렬 후에는 b가 B의 앞에 오게 된다.** 그렇기에 선택 정렬은 불안정 정렬이다.

***그렇다면 불안정 정렬이 왜 안좋은 걸까?*** 

위와 같은 경우에서 안 좋은 이유를 생각해보자면, 입력값에도 의도한 우선 순위가 있을 수 있는데 불안정 정렬은 이를 무시하기 때문에 단점이라고 생각할 수 있다.

> 안정 정렬 : 중복된 값을 입력된 순서와 동일하게 정렬 <br>
> 불안정 정렬 : 중복된 값을 인력된 순서와 동일하지 않게 정렬할 수 있음.

<br>

### ***파이썬으로 구현***

<br>

```python
def selection_sort(li):
    
    for i in range(0, len(li)-1): # 최소값과 가장 앞에 있는 인덱스 위치 변경하는 반복문
        min_idx = i
        for j in range(i+1, len(li)): # 최솟값을 탐색하는 반복문
            if li[min_idx] > li[j]:
                min_idx = j
            else:
                min_idx = min_idx
        li[min_idx], li[i] = li[i], li[min_idx]

    return li
```


<br>



## **2. 삽입 정렬 (Insertion Sort)**

<br>

![insertionpass](https://user-images.githubusercontent.com/89771322/151698307-66bc20f9-e740-4512-9547-4b3a5dafd231.png)

<br>

삽입 정렬은 **이미 정렬된 데이터들과 정렬되지 않은 데이터를 비교하면서 적절한 위치에 삽입**하는 정렬을 말한다. 

위의 그림의 경우 31을 앞에 정렬된 배열들과 하나씩 비교하면서 위치를 바꾸는 과정을 보여주는 그림이다. 최종적으로 26이 31보다 작기 때문에 31은 26의 오른쪽에 위치하게 된다.

이 처럼 삽입 정렬은 **정렬된 데이터들의 범위가 하나씩 늘어난다.** 즉, 정렬하고자 하는 데이터와 **비교해야할 데이터의 범위가 늘어나기 때문에 소량의 데이터를 정렬할때 효율적인 방법 중 하나이다.**

<br>

### ***파이썬으로 구현***

<br>

```python
def insertion_sort(li):
    for i in range(1,len(li)): # i가 점점 커짐 => 비교할 범위 늘어남.
        for j in range(i, 0, -1): # 값을 비교하고 위치를 바꾸는 반복문.
            if li[j] < li[j-1]:
                li[j], li[j-1] = li[j-1], li[j]
    return li
```

<br>

## 3. 버블 정렬 (Bubble Sort)

<br>

![Bubble_sort_animation](https://user-images.githubusercontent.com/89771322/151699116-e616232e-8cb0-4599-bca3-44875f2c8964.gif)

<br>

버블 정렬은 **서로 인접한 원소들을 비교하여 정렬하는 방법**이다.  위 그림 처럼 거품이 수면위로 올라오는 듯한 모습을 보이기에 버블 정렬이라고 이름을 지었다고 한다.

버블 정렬로 정렬하는 과정은 다음과 같다.

1. 첫번째 데이터와 두번째 데이터를 비교해서 두번째 데이터가 더 작으면 위치를 변경.
2. 두번째 데이터와 세번째 데이터 비교해서 세번째 데이터가 더 작으면 위치를 변경.
3. 위 과정을 정렬될때까지 반복.

이러한 과정을 거치기 때문에 버블 정렬은 데이터가 정렬되어 있더라도 모든 데이터를 검사해야 한다. 또한 위의 과정을 거치면 **가장 큰 값이 제일 끝으로 가게 되기 때문에 시행할때마다 정렬해야할 데이터의 범위가 줄어든다.**

<br>

### ***파이썬으로 구현***

<br>

```python
def bubble_sort(li):
    for i in range(len(li)-1): # 반복해야할 범위를 점점 줄인다.
        for j in range(len(li)-1-i): # 값을 비교하고 위치를 변경하는 반복문.
            if li[j] > li[j+1]:
                li[j], li[j+1] = li[j+1], li[j]
    return li
```

<br>
<br>
<hr>

### ***참고자료***

https://www.daleseo.com/sort-bubble/ <br>
https://ko.wikipedia.org/wiki/%EA%B1%B0%ED%92%88_%EC%A0%95%EB%A0%AC <br>
https://runestone.academy/ns/books/published//pythonds/SortSearch/TheInsertionSort.html <br>
https://ko.wikipedia.org/wiki/%EC%82%BD%EC%9E%85_%EC%A0%95%EB%A0%AC <br>
https://hongl.tistory.com/9 <br>
https://stackoverflow.com/questions/4601057/why-is-selection-sort-not-stable <br>
https://godgod732.tistory.com/13 <br>
https://ko.wikipedia.org/wiki/%EC%84%A0%ED%83%9D_%EC%A0%95%EB%A0%AC