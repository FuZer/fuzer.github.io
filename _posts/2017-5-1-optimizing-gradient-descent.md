---
layout: post
title: (번역) An overview of gradient descent optimization algorithms
category: deeplearning
---
![](http://sebastianruder.com/content/images/2016/09/loss_function_image_tumblr.png)

경사하강법은 최적화를 수행하는 가장 보편적인 알고리즘 중 하나이며 뉴럴 네트워크를 최적화하는 가장 일반적인 방법입니다.
그 와 동시에 모든 state-of-the-art 딥러닝 라이브러리에는 경사하강법
(예 :  [lasagne](http://lasagne.readthedocs.io/en/latest/modules/updates.html),
[caffe](http://caffe.berkeleyvision.org/tutorial/solver.html), 그리고
[keras](https://keras.io/optimizers/) 문서)을 최적화 하기 위한 다양한 알고리즘 구현이 들어 있습니다.
그러나 이러한 알고리즘은 장점과 단점에 대한 명확한 설명이 힘들기 때문에 블랙 박스 최적화 도구로 사용됩니다.

이 포스트는 여러 경사하강법이 동작하는 모습을 직관적으로 보여주어,
경사하강법를 최적화하여 사용하도록 도와주는 것을 목표로 합니다.
먼저 경사하강법의 다양한 변형을 살펴볼 것입니다.
그런 다음 트레이닝 과정에서 해결해야 할 문제를 간략하게 요약 해 보겠습니다.
그 뒤에는 이러한 문제를 해결하기 위해 어떻게 업데이트 규칙을 유도하는지 보여줌으로써
가장 일반적인 최적화 알고리즘을 소개합니다.
또한, 병렬 및 분산 처리 환경에서 경사하강법을 최적화하는
알고리즘 및 아키텍처에 대해 간략히 살펴 보겠습니다.
마지막으로 경사하강법을 최적화하는 데 도움이 되는 추가 방법을 소개할 것입니다.

경사하강법은 목적 함수 J(θ)의 θ 에 대한 미분 값의 반대 방향으로 θ 를 업데이트하여  
목적 함수 J(θ) 를 최소화하는 방법입니다. (θ는 θ ∈ R^d 로 파타미터 화)
러닝 레이트  η 는 (로컬) 최소값에 도달하기 위해 기울기의 반영 비율을 결정합니다.
즉, 우리는 계곡 아래에 도달 할 때까지 내리막 길목에 의해 만들어진 표면의 경사 방향을 따라갑니다.
경사하강법에 익숙하지 않은 경우 [여기](http://cs231n.github.io/optimization-1/)에서
신경망을 최적화하는 방법에 대한 소개를 찾을 수 있습니다.

# Gradient descent variants
경사하강법의 세 가지 변형이 있는데, 목적 함수의 기울기를 계산하는 데 사용하는 데이터의 양이 다릅니다.
데이터의 양에 따라 매개 변수 업데이트의 정확성과 업데이트를 수행하는 데 걸리는 시간 사이에
트레이드-오프( trade-off )가 있습니다.

## batch gradient descent
기본적인 경사하강법 ( 일명 배치 경사하강법 ) 은 전체 훈련 데이터 세트에 대한 파라미터 θ 에 대한
손실 함수의 기울기를 계산합니다:

![](/images/2017-5-1-optimizing-gradient-desent/vanilla.png)  

하나의 업데이트만 수행하기 위해 전체 데이터 세트에 대한 기울기를 계산해야하므로
배치 경사하강법은 매우 느리고 보유한 메모리 보다 큰 데이터 세트의 경우 다루기가 어렵습니다.
배치 경사하강법은 우리가 온라인으로 모델을 업데이트하는 것을 허용하지 않습니다. 즉,
즉석에서 새로운 데이터로 모델을 업데이트하는 것을 허용하지 않습니다.

코드에서 배치 경사하강법은 다음과 같습니다:

```python
for i in range(nb_epochs):
  params_grad = evaluate_gradient(loss_function, data, params)
  params = params - learning_rate * params_grad
```

사전 정의 된 epoch 수 만큼, 매개 변수 벡터 `params` 에 관련해
전체 데이터 세트에 대한 손실 함수의 기울기 벡터 `params_grad` 를 먼저 계산합니다.

[참고] state-of-the-art 딥러닝 라이브러리는 파라미터에 대해 기울기를 효율적으로 계산하는
자동 미분 기능을 제공합니다. 기울기를 직접 유도 한다면 기울기 검사가 좋은 생각입니다.
( 기울기를 검사하는 방법에 대한 정보는 [여기](http://cs231n.github.io/neural-networks-3/)
를 참조하십시오. )

그런 다음 러닝 레이트 을 이용해 기울기 방향으로 매개 변수를 업데이트합니다.
배치 경사하강법은 convex 한 손실 표면에 대한 글로벌 미니멈과
non-convex 한 표면에 대한 로컬 미니멈으로 수렴되는 것이 보장됩니다.

## Stochastic gradient descent
확률적 경사하강법( SGD )는 각 학습 데이터  x (i) 및 레이블 y (i) 에 대한 파라미터 업데이트를 수행합니다.

![](/images/2017-5-1-optimizing-gradient-desent/stochastic.png)

배치 경사하강법은 각 파라미터를 업데이트하기 전에 유사한 샘플의 기울기를 다시 계산하므로
대규모 데이터 세트에 대한 중복 계산을 수행합니다.
SGD는 한 번에 하나의 업데이트 만 수행하여이 중복성을 제거합니다.
따라서 대개 훨씬 빠르며 온라인 학습에 사용될 수도 있습니다.

SGD는 [그림 1] 에서와 같이 목적 함수가 크게 변동하는 높은 분산을 자주 업데이트합니다.

![](http://sebastianruder.com/content/images/2016/09/sgd_fluctuation.png)
<font color="gray">
[그림 1] Image 1: SGD fluctuation
(Source: <a href="https://upload.wikimedia.org/wikipedia/commons/f/f3/Stogra.png"> Wikipedia </a>)
</font>

배치 경사하강법은 파라미터가 시작된 곳의 로컬 미니멈으로 수렴하는 동안
SGD는 새롭고 잠재적으로 더 나은 로컬 미니멈으로 점프 할 수 있게 합니다.
그러나 SGD는 계속 널뛰기 (overshooting) 하므로, 궁극적으로 수렴을 어렵게 만듭니다.

[참고]
러닝 레이트를 서서히 낮추면 SGD는 배치 경사하강법과 동일한 수렴 동작을 나타내며
convex  및 non-convex 표면에서 로컬 또는 글로벌 미니멈으로 거의 수렴합니다.

다음 코드는 트레이닝 예제에 루프를 추가하고 각 데이터에 대한 그라디언트를 평가합니다.  
[여기](http://sebastianruder.com/optimizing-gradient-descent/index.html#shufflingandcurriculumlearning) 에서 설명한대로 매 epoch 마다 훈련 데이터를 섞는다.

```python
for i in range(nb_epochs):
  np.random.shuffle(data)

  for example in data:
    params_grad = evaluate_gradient(loss_function, example, params)
    params = params - learning_rate * params_grad
```

## Mini-batch gradient descent
미니 배치 경사하강법은 앞서 나온 방법들의 장점을 최대한 살리며 n개의 트레이닝 데이터의
모든 미니 배치에 대한 업데이트를 수행합니다.

![](/images/2017-5-1-optimizing-gradient-desent/minibatch.png)

이 방법은 a) 파라미터 업데이트의 분산을 줄여보다 안정적인 수렴을 유도 할 수 있습니다.
b) 기울기 계산을 가능하게하는 state-of-the-art 딥러닝 라이브러리에 공통적으로
최적화 된 행렬 최적화를 사용할 수 있습니다.
일반적인 미니 배치 크기의 범위는 50 ~ 256 이지만 상황 마다 다를 수 있습니다.
미니 배치 경사하강법은 일반적으로 신경망을 학습 할 때 선택되는 알고리즘이며
SGD라는 용어는 일반적으로 미니 배치에 사용되는 경우에도 사용됩니다.

[참고] 이 문서 뒷 부분 부터 SGD를 수정하여 간단하게 파라미터를 생략했습니다.

코드에서는 데이터를 루프도는 대신 크기가 50 인 미니 배치를 반복합니다.

```python
for i in range(nb_epochs):
  np.random.shuffle(data)
  for batch in get_batches(data, batch_size=50):
    params_grad = evaluate_gradient(loss_function, batch, params)
    params = params - learning_rate * params_grad
```
