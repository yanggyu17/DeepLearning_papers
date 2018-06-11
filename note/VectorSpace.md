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
    - E : 훈련 Epoch의 수 -보통 3 ~ 50
    - T : 훈련 셋에 있는 단어의 수 -보통 10억
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
- 계층적 softmax를 사용하거나 정규화 되지 않은 모델에서 정규 모델을 사용하지 않음으로써 이를 회피할 수 있다.
- 그러므로 **가장 큰 복잡도는 N x D x H**에 의해 생긴다.
- 이 논문에선 어휘가 Huffman 이진 트리로 표현되고 계층적 softmax를 사용하는 모델을 사용했다.

### Recurrent Neural Net Language Model(RNNLM)
- input, hidden and output layers를 갖는 모델
    - 시간의 흐름에 따른 연결을 위해 recurrent matrix를 hidden layer에서 자기 자신으로 연결한다.
    - 이것은 반복적인 모델이 현재의 입력과 과거의 hidden 층의 상태를 기반으로 hidden 층의 상태를 갱신하는 것으로 과거의 정보를 표현하는 짧은 기간의 기억을 가능하게 한다.
- RNN 모델 훈련 계산 복잡도 : Q = H x H + H x V
- H x V 항은 계층적인 softmax를 이용해 효율적으로 H x log2(V)로 줄어든다. 따라서 **가장 큰 복잡도는 H x H**

### Parallel Training of Neural Networks
- 큰 데이터 셋으로 모델을 훈련시키기 위해서 'DistBelief'라는 분산 framework 기반으로 NNLM과 논문에서 제시한 모델을 실행
- 이 framework로 같은 모델을 병렬로 여러개의 복제본을 실행 할 수 있다.
    - Adagrad라고 불리는 Adaptive learning rate 방식을 기반으로 하는 mini-batch 비동기식 gradient descent를 사용
## New Log-linear Models
- 계산 복잡도를 최소화 하기 위한 두가지 새로운 모델 제안
- 비선형 hidden layer가 가장 큰 계산 복잡도의 원인!
    - 신경망 구조가 매력적이지만 간단한 모델을 시도

### Continuous Bag-of-Words Model
- 첫번째 구조는 비선형 hidden 층이 사라지고 projection 층이 모든 단어들에서 공유되는 것을 제외하고 feedforward NNLM과 유사하다.
- 이 구조를 단어의 순서가 projection에 영향을 주지 않는 것으로 bag-of-words 모델이라고 한다. 여기서 더 나아가 앞으로 나올 단어들 까지 사용해서 CBOW라고 표현(앞뒤로 4개씩의 단어를 입력, log선형 분류기를 이용하여 최고의 성능을 얻었다.)
- 훈련 복잡도 : Q = N x D + D x log2(V)
- NNLM에서와 같이 입력과 projection 층 사이의 가중치 행렬은 모든 단어들이 공유한다.

### Continuous Skip-gram Model
- 두번째 구조는 CBOW와 비슷하지만 문맥을 통해서 현재 단어를 예측하는 것 대신 같은 문장에서의 다른 단어들을 기반으로 단어의 분류를 극대화한다.
- 각각의 현재 단어를 연속적인 projection layer와 함께 log선형 분류기 입력으로 사용
- 멀리 떨어진 단어일수록 보통 현재 단어와 적은 연관을 갖기 때문에 거리가 있는 단어에 적은 샘플링 하는 것으로 적은 가중치를 두었다.
- 훈련 복잡도 : Q = C x (D + D x log2(V))
    - C : 단어의 최대 거리(실험에서 10을 사용)

![cbow_skipgram](https://github.com/yanggyu17/DeepLearning_papers/blob/master/images/CBOW_SkipGram.png)

## Results
- 
