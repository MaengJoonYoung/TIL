## **Segmentation & Object Detection/Recognition**

<br>

## **1. Segmentation**

<br>

Segmentation은 하나의 이미지에서 **픽셀 단위로 레이블을 분류**하여 **같은 의미를 가지고 있는 부분을 구분**하는 Task이다.

<br>

![segmentation](https://user-images.githubusercontent.com/89771322/147926157-37e4fcd7-5f8e-49d5-a6df-6696a46b7198.png)


<br>

그림을 보면 픽셀 단위로 사람, 가방, 식물 등으로 레이블이 구분되어 있는 걸 볼 수 있다. Segementation에는 **Semantic Segmentation** 과 **Instance Segmentation**이 있다.

<br>

### ***Semantic Segmentation vs Instance Segmentation***

<br>

* Semantic Segmentation
  * 의미적 분할(Semantic Segmentation)은 같은 의미를 가진 모든 개체를 하나의 레이블로 구분하는 것이다.
  * 예를 들어 사람 A, 사람 B, 사람 C가 한 이미지 안에 있다면, 모두 
다 사람이라는 같은 레이블로 구분된다는 것이다.

<br>

* Instance Segmentation
  * Instance Segmentation은 같은 의미를 가진 개체라도 따로 따로 분류하는 것이다.
  * 즉, 사람 A, 사람 B, 사람 C를 다른 레이블로 분류한다.

<br>

![semantic](https://user-images.githubusercontent.com/89771322/147927263-67ea6320-a308-41fa-8cf2-698bb5d4441e.png)

<br>

그림을 보면 Semantic의 경우 **의자를 모두 같은 레이블로 구분**했고 Instance의 경우 의자를 **각각 다른 레이블로 구분**한 걸 볼 수 있다.

<br>
<br>

## **1-2. FCN**

<br>

위에서 봤듯이 Segmentation은 **픽셀 단위로 분류가 이뤄지기 때문에 해당 픽셀이 어디서 왔는지를 알아야 한다.** 즉, **픽셀의 위치정보가 보존**되어야 한다. 

하지만 기존 CNN 모델은 합성곱 층과 풀링 층을 거치면서 **이미지 데이터가 Downsampling 되고** 이를 분류기인 완전신경망의 입력하기에 **위치정보가 손실되어** Segmentation에는 적절하지 않았다.

***FCN은 기존 CNN 모델에서 분류기 부분을 완전 신경망을 사용하지 않고 합성곱 층으로 대체하여 이러한 문제를 해결한 모델이다.***

<br>

### ***Upsampling***

<br>

앞에서 말한 것 처럼 Segmentation은 픽셀의 위치 정보가 보존되어야 하기 때문에 Downsampling된 이미지 데이터를 **원본 이미지 크기와 비슷하게 키우는 Upsampling을 진행**한다.

Upsampling은 **Transpose Convolution**을 적용하여 이미지 크기를 키워나간다.

<br>

### ***Transpose Convolution***

<br>

![transposed](https://user-images.githubusercontent.com/89771322/147927990-3f4f2dd0-565d-45e6-b42a-0c9df2c50302.gif)

<br>

Transpose Convolution은 작아진 인풋값 하나인 **스칼라에 필터를 곱하고 이 출력값을 Output 영역에 넣는다.** 이후 슬라이딩하여 반복하고 **출력값이 겹치는 영역은 서로 더해준다.** 이런 식으로 이미지 크기를 늘리는 것이다.

<br>

### ***U-net***

<br>

![u-net](https://user-images.githubusercontent.com/89771322/147929508-9cea1a81-6dde-486c-bde0-972d8a82be9c.png)

<br>

U-net의 구조를 도식화 하면 이렇게 U자 모양이다.

U-net도 마찬가지로 Downsampling과 Upsampling 과정을 거친다. Downsampling은 합성곱과 풀링 층을 거치면서 **이미지의 특징을 뽑아낸다**고 볼 수 있고, Upsampling은 **위치정보를 가져온다고 할 수 있다.** 

또한 **Upsampling 과정도 Transpose Convolution을 사용하기에 학습**을 진행한다. 여기에 더해서 **Downsampling 과정에서 출력된 Feature map을 적당한 크기로 잘려서 Concatanate 하여 추가 데이터로 사용한다.**

<br>
<br>

## **2. 객체 탐지/인식(Object Detection/Recognition)**

<br>

객체 탐지/인식은 이미지에서 객체 주변에 **Bounding box**를 만들고 해당 box안에 있는 **객체가 어떤 클래스에 속하는지** 분류하는 Task이다.

객체 탐지 결과는 아래 그림과 같다.

<br>

![객체 탐지](https://user-images.githubusercontent.com/89771322/147931059-c21d3b91-6f17-4581-8749-48479bbda312.jpeg)

<br>

그림처럼 네모난 Bounding box를 만들고 박스 안에 객체가 어떤 클래스인지 분류를 진행한다. 

***즉, 객체 탐지는 Classification 과 Localization 두가지 Task를 수행하는 것이다.***

<br>

### ***Classification & Localization***

<br>

* Localization
  * Localization은 회귀 문제로 4개의 숫자를 예측하는 Task이다.
  * 박스의 좌표인 x, y와 박스의 크기인 w, h를 예측한다.

<br>

* Classification
  * Classification은 박스 안에 객체가 어떤 클래스인지 분류하는 Task이다.

<br>

### ***IoU(Intersection over Union)***

<br>

IoU는 객체 탐지의 결과를 평가하는 평가지표이다. 

정답에 해당하는 Bounding box를 Ground-truth하고 예측한 Bounding box를 Predicted Bounding box라고 한다. 이 **두 박스를 사용하여 객체 탐지의 결과를 평가한다.**

<br>

![Iou](https://user-images.githubusercontent.com/89771322/147931863-fb435da9-c24f-4cd4-9797-41c762b27d40.png)

<br>

그림처럼 두 박스의 교집합을 분자로 합집합을 분모로 사용한다. 즉, Bounding box가 정답에 가까울 수록 1에 가까워진다. 이러한 평가 지표를 사용하여 **Bounding box의 범위가 커지는 문제를 해결**할 수 있다.

<br>


### ***Two Stage Detector & One Stage Detector***

<br>

객체 탐지는 2-stage와 1-stage 방식으로 나눌 수 있다.

<br>

* Two Stage Detector
  * Two Stage Detector는 특정 알고리즘으로 객체가 있을 것이라는 지역을 추천받는다.
  * 이후 추천 받은 지역인 RoI(Region of Interest)에 대해 분류를 수행한다.
  * 대표적인 모델로 RNN 계열이 있다.

<br>

* One Stage Detector
  * One Stage Detector는 특정 지역을 추천받지 않고 이미지를 격자 형태로 나눈 뒤 나눈 공간을 탐색하며 분류를 진행한다.
  * 대표적인 모델로 SSD 계열과 YOLO 계열이 있다.