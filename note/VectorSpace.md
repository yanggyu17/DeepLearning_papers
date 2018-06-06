# Efficient Estimation of Word Representations in Vector Space

논문 [[pdf]](https://arxiv.org/pdf/1301.3781.pdf)

## Abstract
- 아주 적은 계산 복잡도로 매우 큰 정확도 향상 관측
- 16억 단어 데이터 셋으로 부터 단어 벡터를 학습하는데 하루도 걸리지 않는다.
- 이 벡터들이 구문론 측정이나 의미론적 단어 유사성에서 최신의 성능을 제공한다.

## Introduction
- 이 전까지는 단어 간의 유사도에 대한 사고 없이 단어를 표현
    - 장점 : simplicity, robustness, observation
- 엄청난 양의 데이터로 훈련된 간단한 모델이 적은 데이터로 훈련된 복잡한 모델의 성능을 능가한다.
- 통계학적인 언어 모델링을 위한 모델인 N-gram이 있지만 많은 한계가 존재
- 최근 머신러닝 기술의 발전으로 복잡한 모델을 매우 대량의 데이터 셋으로 학습이 가능해 졌고 간단한 모델의 성능을 능가한다.

### Goals of the Paper
- 이 논문의 목표는 수십억 단어와 vocabulary안의 수백만의 단어들로 이루어진 매우 큰 데이터 셋으로 부터 높은 수준의 단어 벡터 학습을 하는 기술을 소개하는 것
- 비슷한 단어가 서로 근접하는 것 뿐만 아니라 **복수의 유사도**를 갖도록 한다.
- word offset technique를 이용해 단어 벡터의 대수적 연산을 실행 가능
    - **King - Man + Woman = Queen**
- 단어의 선형 규칙들을 보존하는 새로운 모델 구조를 개발함으로써 벡터 계산의 정확도를 높이려한다.

## Model Architectures
- LSA(Latent Semantic Anlysis)와 LDA(Latent Dirichlet Allocation)등 연속적인 단어의 표현들을 추정하는 다양한 타입의 모델들이 제안되었다.
- 신경망 구조에 의해 학습된 단어의 분포 표현에 집중한다.
- 훈련 복잡도 정의
    - O = E x T x Q
    - E : 훈련 Epoch의 수 보통 3 ~ 50
    - T : 훈련 셋에 있는 단어의 수 보통 10억
    - Q : 각각 모델 구조를 위해 정의
- 모든 모델들은 SGD와 Backpropagation을 사용해서 학습

### Feedforward Neural Net Language Model(NNLM)
- input, projection, hidden and output layers를 갖는 모델
- 매 훈련마다의 계산 복잡도 : Q = N x D + N x D x H + H x V
    - V : 사전의 사이즈
    - P : projection layer
    - H : hidden layer size
    - N x D : projection layer 차원
    - **H x V : 영향력이 가장 큰 부분**
- 계층적 softmax를 사용하거나 정규화 되지 않는 모델에서 정규 모델을 사용하지 않음으로써 이를 회피할 수 있다.
- 그러므로 가장 큰 복잡도는 **N x D x H**에 의해 생긴다.
