# Going Deeper with Convolutions(GoogLeNet)
논문[[arXiv]](https://arxiv.org/abs/1409.4842)

## Motivation
- DNN(Deep Neural Network)의 성능은 사이즈를 크게함으로써 향상될 수 있다.
    - 더 깊은 네트워크: 네트워크의 레이어(layer)를 많이 쌓는다.
    - 더 넓은 네트워크 : 각 레이어의 유닛(unit) 수를 늘린다.
- 네트워크의 크기가 커지면 파라미터의 개수가 증가하여 오버피팅이 더 잘 발생할 수 있고, 필요한 컴퓨팅 리소스도 증가한다.
- 이런 문제는 fully connected layer들을 sparse connected layer로 대체함으로써 해결할 수 있다.
- Arora at al. (2013) 에서는 activation의 상관성이 높은(highly correlated) 유닛들을 클러스터링하여 다음 레이어를 구성하면 최적의 네트워크 구조가 됨을 보여주고 있다.

## Architectural Details
- 네트워크에서 초반의 레이어의 유닛들은 인풋 이미지의 특정 영역과 대응하고, 이 유닛들은 필터 뱅크(filter banks)로 그룹지어지어져 있다고 가정했다.
- 인풋에 가까운 레이어에서 서로 상관이 있는 유닛들은 국소 영역(local region)에 모여있을 것이다.
    - 한 영역(single region)에 모여있는 클러스터는 Lin et al. (2013)에서 제시된 1X1 컨볼루션으로 커버
    - 좀 더 넓은 영역에 퍼져 있는 클러스터들을 커버하기 위해 3×3, 5×5 크기의 컨볼루션 연산도 수행
- 최근 컨볼루션 네트워크에서 좋은 성능을 내는 pooling layer도 사용했다.
- 이렇게 1x1, 3x3, 5x5, max pooling 연산이 결합된 것이 하나의 **인셉션 모듈(Inception module)** 이다.

![inception(a)](https://github.com/yanggyu17/DeepLearning_papers/blob/master/images/inception(a).png)
- 인셉션 모듈이 여러 개 결합되어 네트워크가 깊어질 수록, 필터의 개수가 늘어나고 그에 따라 연산량도 증가한다.
- 이에 대한 해결책으로 3x3, 5x5 컨볼루션 연산 전에 1x1 컨볼루션 연산을 수행하여 인풋의 차원을 축소시켰다.
![inception(b)](https://github.com/yanggyu17/DeepLearning_papers/blob/master/images/inception(b).png)