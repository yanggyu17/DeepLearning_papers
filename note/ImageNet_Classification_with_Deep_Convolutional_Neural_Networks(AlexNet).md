# AlexNet
저자 : Alex Krizhevsky(University of Toronto), Ilya Sutskever(University of Toronto), Geoffrey E. Hinton(University of Toronto)

논문 [[pdf]](http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)

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
- image size를 256 x 256으로 맞춤 

## The Architecture
- 5개의 convolutional layers + 3개의 fully-connected layers
    - 맨마지막 FC layer는 1000개의 category 분류를 위해 활성 함수로 softmax 함수를 사용
- activation function으로 Relu 사용 *
    - tanh함수를 썼을때 보다 6배 빨라졌다.
- 2개의 GPU사용하여 병렬로 처리 *
    - 엔비디아사의 GTX580 3GB 사용
- 첫번째 convolutional layer
    - 224 x 224 x 3 input image size
    - 11 x 11 x 3 비교적 큰 kernel size를 사용(입력 이미지의 크기가 크기때문)
    - stride size 4를 적용하여 96개의 feature map이 생성
    - 55 x 55 x 96 = 290,400개의 뉴런이 만들어진다.
    - 96개의 feature map을 위아래로 48개 씩 나누어서 학습을 진행

- 두번째 convolutional layer
    - 5 x 5 x 48 크기의 kernel을 사용
    - response normalization과 pooling과정을 거쳐 이미지의 크기가 27 x 27 x 256으로 줄어들게 된다.
- 세번째 convolutional layer
    - 3 x 3 x 256 크기의 kernel을 사용 384개의 feature map을 얻는다.
    - GPU-1과 GPU-2의 결과를 모두 섞어서 사용한다.
    - response normalization과 pooling과정을 거쳐 13 x 13 x 384 크기의 이미지 출력
- 네번째 convolutional layer
    - 3 x 3 x 192 크기의 384개의 kernel 
- 다섯번째 convolutional layer
    - 3 x 3 x 192 크기의 256개의 kernel
    - pooling과정을 거쳐 4096개의 fully connected net에 연결
- 첫번째, 두번째 fully connected
    - GPU-1과 GPU-2의 결과를 모두 섞어서 사용한다.
- 세번째 fully connected(마지막)
    - 1000개의 category에서 결과를 낼 수 있도록 softmax 함수를 적용하여 출력

- Local Response Normalization *
    - sigmoid나 tanh의 경우는 saturation 되는 구간이 있기 때문에 overfitting을 피하기 위하여 입력에 대한 정규화(normalization)를 수행한다.
    - ReLU를 사용했을 때는 입력의 normalization이 필요가 없다. 
    - 출력은 입력에 비례하여 그대로 증가하기 때문에 여러 feature map에서의 결과를 normalization을 시키면 생물학적 뉴런에서의 lateral ingibition(강한 자극이 주변의 약한 자극이 전달되는 것을 막는 효과)과 같은 효과를 얻을 수 있기 때문에 generalization 관점에서는 훨씬 좋아진다.
    - 논문에서는 첫번째 두번째 convolution을 거친 결과에 ReLU를 수행하고 max pooling을 수행하기에 앞서 response normalization을 수행하였다.
    - 이를 통해 top-1과 top-5 에러율을 각각 1.4%와 1.2% 향상된 결과를 얻었다.
- Overlapping Pooling *
    - pooling size 3과 stride size 2를 사용하여 최초로 cnn에서 겹치는 max pooling을 시도하였다.
    - overfitting의 가능성을 줄여준다.

## Reducing Overfitting
### Data Augmentation
- AlexNet에서 작은 연산만으로 학습 데이터를 늘리는 2가지 방법을 적용하였다.
- 첫번쨰 방법
    - ILSVRC의 256 x 256 크기의 원래 이미지로 부터 무작위로 224 x 224 크기의 이미지를 가져오는 것이다. 이것을 통해 1장의 학습 이미지로 부터 2048개의 다른 이미지를 얻을 수 있다.
    - 상하좌우와 중앙에 위치하는 이미지5개와 이것의 반전 이미지 5개 총 10개로 부터 softmax 출력을 평균하는 방법으로 택하였다.
- 두번째 방법
    - 각 학습 이미지로부터 RGB 채널의 값을 변화 시키는 방법을 택했다.
    - 학습 이미지의 RGB픽셀 값에 PCA(주성분 분석)를 수행하고 여기에 평균 0, 표준편차 0.1 크기를 갖는 랜덤 변수를 곱하고 이것을 원래 픽셀 값에 더해주는 방식으로 컬러의 채널 값을 바꾸어 다양한 이미지를 얻었다.

### Dropout
- Dropout은 요즘 대부분의 CNN구조에서 학습시간을 단축시키고 overfitting 문제도 해결한다.
- AlexNet에서는 fully connected layer의 처음 2개 layer에 대해서 적용했고 비율은 50%를 사용하였다.

## Results
- GPU-1 에서는 주로 color와 상관없는 정보를 추출하기 위한 kernel이 학습이 되고(top) GPU-2 에서는 주로 color와 관련된 정보를 추출하기 위한 kernel이 학습된다.(bottom)
