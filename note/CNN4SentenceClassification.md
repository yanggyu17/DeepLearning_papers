# Convolutional Neural Networks for Sentence Classification
논문 [[pdf]](http://emnlp2014.org/papers/pdf/EMNLP2014181.pdf)

## Abstract
- Word vector와 CNN을 활용한 문장 분류
- 이미 트레이닝된 word vector를 활용(with Word2vec)
- Simple한 CNN 구조 사용
    - 하나의 convolutional layer를 가진다.
    - 2개의 채널(static, non-static)에 대해 convolution과 max pooling을 수행하고 fully connected layer에서 dropout과 softmax를 적용하여 최종 분류를 수행한다.
    - 각 채널 별로 3가지의 filter size[3,4,5]를 구성한 후 계산을 한다.
    - 따라서 3가지의 feature map이 나오고 여기에 max pooling을 수행한다.
- 7개 tasks 중 4 곳에서 가장 높은 정확도

## Introduction

## Model

## Datasets and Experimental Setup
- **CNN-rand** : word2vec을 사용하지 않고 모든 단어를 random 초기화
- **CNN-static** : word2vec으로 pre-train, 모든 word vector를 static하게 유지.(해당 weight에 학습을 진행하지 않음)
- **CNN-non-static** : word2vec으로 pre-train, word vector를 학습하며 fine-tune.(해당 weight에 학습을 진행합니다.)
- **CNN-multichannel** : static 채널과 non-static 채널 모두 사용(static 학습 미허용, non-static 학습 허용)
![result](https://github.com/yanggyu17/DeepLearning_papers/blob/master/images/CNNforSC.png)

## Results and Discussion

## Conslusion
- 단순한 CNN 모델을 사용하였지만 word2vec으로 pre-training 하면 deep learning for NLP에서 높은 성능을 낼 수 있다.