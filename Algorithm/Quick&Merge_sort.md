# **퀵 정렬과 병합 정렬**

<br>

퀵 정렬과 병합 정렬은 **분할 정복법**을 사용한 정렬 방법이다. 분할 정복법이란 **하나의 크고 복잡한 문제를 작은 문제들로 나눠서 해결하는 방법**이다. 

<br>

## **1. 퀵 정렬 (Quick sort)**

<br>

![Sorting_quicksort_anim](https://user-images.githubusercontent.com/89771322/151827814-8c6d8fb9-85b6-479b-94aa-d307dc77a57c.gif)
[출처](https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC)

<br>

퀵 정렬은 원소들간의 비교만으로 데이터를 정렬하는 방법이다. 퀵 정렬은 말그대로 빨라서 퀵 정렬이라는 이름으로 불리게 되었다. 퀵 정렬 알고리즘으로 데이터를 정렬하는 방법은 다음과 같다.

1. 데이터들 중 아무 요소나 선택하고 선택한 요소보다 **작은 값들은 왼쪽**에 **큰 값들은 오른쪽**에 위치시킨다. 이때 선택한 요소를 `Pivot`이라고 한다.
2. 이 과정을 피벗을 제외한 왼쪽과 오른쪽에 위치한 데이터들에도 똑같이 적용한다.
3. 주어진 데이터의 길이가 더이상 분할 할 수 없을때 까지 이 과정을 반복한다.
4. 나눠진 데이터들을 하나로 합친다.

위의 1번 과정에서 피벗을 기준으로 작고 큰 요소들이 정렬이 되기 때문에 매 단계에서 **피벗은 올바른 자기 위치를 찾게 된다.** 즉, 매 단계를 진행할때마다 **정렬해야할 데이터의 개수는 줄어드는 것이다.** 이 과정을 그림으로 나타내면 다음과 같다.

<br>

![quick-sort](https://user-images.githubusercontent.com/89771322/151829148-0091d680-da62-48e3-b45d-e3176bb95767.png)
[출처](https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html)

<br>

### ***단점***

퀵 정렬은 중복된 데이터의 순서가 바뀔 수 있는 **불안정 정렬**이고 **성능의 편차**가 심하기 때문에 현업에서 사용하기는 어렵다고 한다. 퀵 소트를 사용했을 때 최악의 경우는 데이터가 매우 불균형적으로 나눠지는 것이다. 예를 들면 아래 그림과 같다.

<br>

![quicksortWorst](https://user-images.githubusercontent.com/89771322/151830167-a55991e8-dc2c-4447-b944-b270f65280bd.png)
[출처](https://www.khanacademy.org/computing/computer-science/algorithms/quick-sort/a/analysis-of-quicksort)

<br>

이런 경우 순환 호출의 깊이가 n이 되고 각 단계에서 평균적으로 n개의 데이터와 피벗을 비교하기 때문에 시간 복잡도는 `O(n^2)`이 된다.

<br>

### ***파이썬으로 구현***

<br>

```python
def quicksort(li):
    if len(li) <= 1: # 더이상 분할 안되면 list 반환
        return li
    else:
        pivot = li[0] # 아무 요소나 피벗으로 설정
        lesser = [element for element in li[1:] if element < pivot]
        greater = [element for element in li[1: if element >= pivot]] # 구현시 중복값을 어디로 보낼지 아무쪽이나 선택해야한다.
        return quicksort(lesser) + [pivot] + quicksort(greater)
```


<br>

## **2. 병합 정렬 (Merge sort)**

<br>

![Merge-sort-example-300px](https://user-images.githubusercontent.com/89771322/151832316-04454c75-725e-4962-9d9e-8c315afa6843.gif)

<br>

병합 정렬은 우선 배열을 반으로 나누고 이 과정을 더이상 나눌 수 없을때 까지 반복한다. 이후에 나눠진 배열을 합치는 과정에서 정렬을 진행하는 방법이다. 자세한 과정은 다음과 같다.

1. 배열을 반으로 나눈다.
2. 더이상 분할되지 않을때까지 반복한다.
3. 새로운 리스트를 만들고 나눠진 배열을 정렬하면서 합친다.
4. 병합하는 과정에서 더 작은 원소를 먼저 새로운 리스트에 추가한다.
5. 나눠진 두 배열 중 어느 한쪽의 배열이 다 옮겨지면 다른 배열의 원소들을 새로운 리스트에 다 추가한다.
6. 나눠진 배열이 하나의 배열로 합쳐질때까지 반복한다.

이를 그림으로 나타내면 다음과 같다.

<br>

![merge-sort-concepts](https://user-images.githubusercontent.com/89771322/151834083-e13062c5-57f4-40be-8154-0a006da5725c.png)
[출처](https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html)

<br>

### ***단점***

<br>

병합 정렬의 경우 위의 과정에서 볼 수 있듯이 배열로 구성되어 있는 경우 새로운 리스트를 추가해야 한다. 즉, 제자리 정렬이 아니라는 단점이 있다.
> 제자리 정렬은 충분히 무시할 만한 저장공간만을 사용하는 정렬 알고리즘을 말한다.

<br>

**하지만, 연결리스트로 구성할 경우 제자리 정렬로 구현할 수 있다고 한다.**

<br>

### ***파이썬으로 구현***

<br>

```python
# 합치면서 정렬하는 함수
def merge(left,right):
    output = []
    i=j=0
    while i < len(left) and j < len(right): # 어느 한쪽의 원소가 새로운 리스트에 다 추가되면 반복 멈춤.
    if left[i] < right[j]:
        output.append(left[i])
        i += 1
    else:
        output.append(right[j])
        j += 1
    # 반복문 남으면 남은 요소들 추가.
    output.extend(left[i:])
    output.extend(right[j:])

def merge_sort(li):
    # 배열 나누는 과정.
    if len(li) == 1:
        return li
    mid = len(li)//2

    left = merge_sort(li[:mid])
    right = merge_sort(li[mid:])
    
    return merge(left, right)
```

<br>
<br>
<hr>

### *참고자료*
https://ko.wikipedia.org/wiki/%ED%80%B5_%EC%A0%95%EB%A0%AC <br>
https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html <br>
https://ko.wikipedia.org/wiki/%EC%A0%95%EB%A0%AC_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98#%EC%A0%9C%EC%9E%90%EB%A6%AC_%EC%A0%95%EB%A0%AC <br>
https://ko.wikipedia.org/wiki/%ED%95%A9%EB%B3%91_%EC%A0%95%EB%A0%AC <br>
https://gmlwjd9405.github.io/2018/05/08/algorithm-merge-sort.html