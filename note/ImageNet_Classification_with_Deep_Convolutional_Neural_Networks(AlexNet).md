# AlexNet
[pdf](http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)
## Abstract
- ILSVRC(ImageNet Large-Scale Visual Recognition Challenge) 2012년 우승 모델(제프리 힌튼 교수팀)
- top-5 test error 기준 15.3% 달성!! 2위(26.2%)와는 큰 차이로 1위를 하였다.

## Introduction
- LeNet5와 구조적으로 비슷하지만 GPU를 사용하여 엄청난 량의 계산 문제를 해결 
- 이 후로 CNN구조 설계시 GPU사용이 대세가 되었다.

## The Dataset
- ImageNet
    - 이미지넷은 1500만개의 이미지와 22000개의 categories로 이루어져있다.
    - Crowd-sourcing을 이용하여 사람이 분류하였다.
- ILSVRC는 ImageNet에서 1000개의 categories와 1000개의 images를 사용하여 test한다.
- training images로 120만장, validation images로 5만장, testing images로 15만장을 사용

## The Architecture
- 5개의 convolutional layers + 3개의 fully-connected layers
    - 맨마지막 FC layer는 1000개의 category 분류를 위해 활성 함수로 softmax 함수를 사용
- activation function으로 Relu 사용
- 2개의 GPU사용
    - 엔비디아사의 GTX580 3GB 사용
- 파라미터의 수
    - 11 x 11 x 3 x 48 +  48 bias
