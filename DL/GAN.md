# **GAN (Generative Adversarial Networks)**


<br>

GAN은 **생성모델**이다. 즉, **랜덤하게 노이즈를 준 샘플을 인풋으로 받아 이미지를 잘 생성**하는데 목적이 있는 모델이다. GAN을 한국어로 해석하면 생성적 적대 신경망이라고 해석할 수 있는데, 적대라는 말이 붙은 이유는 **생성자와 판별자가 서로 경쟁하면서 발전**하기 때문이다.

<br>

## **1. 생성자(Generator) vs 판별자(Discriminator)**

<br>

생성자는 랜덤하게 노이즈를 준 샘플을 바탕으로 **실제와 가까운 이미지를 만들기 위해 노력**하고 판별자는 **생성된 데이터가 진짜인지 가짜인지 잘 구분하기 위해 노력**한다.

생성자와 판별자는 위조지폐 예시를 많이 사용한다. 생성자는 위조지폐를 그럴듯하게 만들고 싶어하는 **위조지폐범**이고 판별자는 이 위조지폐가 진짜인지 아닌지 구분하기 위해 노력하는 **탐정**이라고 볼 수 있다.

<br>

위에서 판별자는 데이터가 진짜인지 가짜인지 잘 구분하기 위해 노력한다고 했는데, 이는 **이진분류**를 진행한다는 것이다. 그렇기에 성능이 좋은 GAN은 판별자가 생성된 데이터가 진짜인지 가짜인지 **잘 구분하지 못하게 된다.**

<br>

## **2. 손실함수**

<br>

GAN은 생성자와 판별자 서로의 성능이 개선되는 방향으로 학습이 진행된다. 그렇다면 손실함수는 어떻게 될까?

<br>

*생성자의 경우 손실함수는 다음과 같이 정의할 수 있다.*
```python
import tensorflow as tf

cross_entropy = tf.keras.losses.BinaryCrossentropy(from_logits=True)

def generator_loss(fake_output):
    
    "fake_output : 생성자가 만들어낸 가짜 이미지"

    return cross_entropy(tf.ones_like(fake_output), fake_output) 
```

> *생성자는 가짜 이미지를 진짜 처럼 느끼게 해야되기 때문에 (진짜 이미지 : 1, 가짜 이미지 : 0) 전체가 1인 행렬과 fake_output을 비교한다.*

<br>

*판별자의 경우 다음과 같이 정의할 수 있다.*

```python
def discriminator(real_output, fake_output):
    """
    real_output : 진짜 이미지 (훈련 데이터에 있는)
    fake_output : 생성자가 만들어낸 가짜 이미지
    """

    real_loss = cross_entropy(tf.ones_like(real_output), real_output)
    fake_loss = cross_entropy(tf.zeros_like(fake_output), fake_output)

    total_loss = real_loss + fake_loss

    return total_loss
```

> *판별자는 진짜와 가짜를 맞춰야하기 때문에 진짜 이미지와 전체가 1인 행렬을 비교하는 real_loss와 가짜 이미지와 전체가 0인 행렬을 비교하는 fake_loss를 합친 loss를 사용한다.*

<br>

## **3. 학습**

<br>

GAN의 학습은 손실이 계산되면 이를 바탕으로 기울기 값을 구해 경사하강법을 진행하여 가중치를 갱신하는 방식으로 이루어진다. 이를 코드로 구현하면 다음과 같다.

<br>

```python
def train_step(images):

  """
  매 iteration 마다 가중치를 갱신.
  loss 구해서 gradient 구하고 경사하강법 실시.
  """
  
  # noise 생성 (generator의 input)
  noise = tf.random.normal([BATCH_SIZE, noise_dim])

  with tf.GradientTape() as gen_tape, tf.GradientTape() as disc_tape:
    generated_image = generator(noise, training=True)

    real_output = discriminator(images, training=True)
    fake_output = discriminator(generated_image, training=True)
    
    # loss 구하기.
    gen_loss = generator_loss(fake_output)
    disc_loss = discriminator_loss(real_output, fake_output)
  
  # gradient 구하기.
  # generator.trainable_variables -> generator 가중치
  # discriminator.trainable_variables -> discriminator 가중치
  gradients_of_generator = gen_tape.gradient(gen_loss, generator.trainable_variables)
  gradients_of_discriminator = disc_tape.gradient(disc_loss, discriminator.trainable_variables)
  
  # 경사하강법 진행.
  generator_optimizer.apply_gradients(zip(gradients_of_generator, generator.trainable_variables))
  discriminator_optimizer.apply_gradients(zip(gradients_of_discriminator, discriminator.trainable_variables))
```

<br>

## 4. CycleGAN

<br>

CycleGAN은 **Style Transfer**이다. 즉, **특정 이미지의 스타일 (도메인 특성)을 다른 이미지에 적용하는 작업**을 하는 것이다. 예를 들어, 갈색 말을 얼룩말로 바꾼다던지, 여름 사진을 겨울 사진으로 바꾸는 것들을 할 수 있다.

<br>

![cyclegan](https://user-images.githubusercontent.com/89771322/148221993-11ccad32-b525-4f16-a381-62839564eed8.png)

<br>

### ***CycleGAN의 장점***

CycleGAN과 비슷한 일을 진행하는 Pix2Pix의 경우 훈련 데이터를 구성할때 레이블에 해당하는 데이터를 무조건 짝을 맞춰줘야 된다. 예를 들어 흑백 사진을 컬러 사진을 바꾸고 싶다면, 흑백 사진과 매칭되는 컬러 사진을 다 준비해야 된다.

하지만 CycleGAN은 **변환하고 싶은 두 스타일의 이미지를 따로 구하더라도 좋은 성능을 보인다**는 장점이 있다. 이로 인해 **학습 데이터를 마련하기가 훨씬 수월해졌다.**

<br>

### ***CycleGAN 원리***

<br>

![68747470733a2f2f692e696d6775722e636f6d2f65726a415841762e6a7067](https://user-images.githubusercontent.com/89771322/148222438-fc554be2-6f87-4e49-9c23-02072949cd6d.jpeg)

<br>

위의 그림은 갈색 말을 얼룩말로 변환하는 CycleGAN이 학습하는 방식을 보여준다.

<br>

그림에서 볼 수 있듯이 CycleGAN은 **갈색 말 → 얼룩말을 진행하는 생성자**와 **얼룩말 → 갈색말을 진행하는 생성자**, 총 **2개의 생성자**가 필요하다.

판별자도 마찬가지로 **가짜 얼룩말이 진짜인지 아닌지 판별하는 판별자**와 **가짜 갈색 말이 진짜인지 아닌지 판별하는 판별자**, 총 **2개의 판별자**가 필요하다.

<br>

위에 사진을 이해하면 아래 사진도 이해할 수 있다.

* 윗 부분
  * 갈색 말 → 얼룩말 생성자를 통해 가짜 얼룩말을 만든 후 가짜 얼룩말을 판별하는 판별자를 통해 판별한다.
  * 생성된 가짜 얼룩말을 얼룩말 → 갈색 말 생성자를 통해 인풋과 같은 형태로 생성할 수 있도록 한다.
> *가짜 얼룩말을 다시 인풋과 같은 형태로 생성하는 이유는 갈색 말 → 얼룩말, 얼룩말 → 갈색 말로 변환하는 과정에서 인풋 정보가 유실되어 원본 이미지의 특성을 잃어버릴 수 있기 때문이다. 즉, 원본 데이터로 돌아갈 수 있을 정도로만 변환을 진행하기 위해서 이렇게 Cycle 과정을 거치는 것이다.*

<br>

아랫 부분은 얼룩말 → 갈색 말로만 바뀌고 위와 똑같이 이루어진다.
