# Deep Residual Learning for Image Recognition

논문 [[pdf]](https://arxiv.org/pdf/1512.03385.pdf)
## Abstract
- Residual Learning 프레임워크를 제시하여 네트워크가 더 깊어지고 커져도 학습하기 쉽게 도와준다. 
- 실험 결과를 통해 이 ResNet(Residual Networks)이 optimize하기 쉽고, depth가 증가해도 정확도가 더 높은 모습을 증명
- 152 layers까지 구성
- VGG넷 보다 8배 깊지만 복잡성은 낮다.
- 2015년 ImageNet detection, ImageNet localization,
COCO detection, and COCO segmentation 에서 1위 한 모델

## Introduction
- **degradation problem** : 망이 깊어지면 vanishing/exploding gradient 때문에 학습이 어려워지는데 이를 degradation problem이라고 한다.
- depth가 깊은 상태에서 학습을 이미 많이 진행한 경우 weight들의 분포가 균등하지 않고, 역전파시 기울기가 충분하지 않아 학습을 안정적으로 진행할 수 없는 문제가 있다. (overfitting문제가 원인은 아님)
- **important concepts**
    - H(x)가 target function이면 F(x) := H(x) - x처럼 output과 input의 차이인 F(x)를 학습하는 것이 residual representation의 주요 개념이다.
    - (H(x) = x)layer들을 쌓아서 학습시키는 것 보단 F(x)를 0으로 학습시키는 것이 더 용이하다는 뜻
    - H(x) = F(x) + x는 또한 "shortcut connection"으로도 이해가 가능한데, 1개 또는 그 이상의 layer들을 파라미터 없이 바로 skip하여 연결하는 개념이다.

![resnetFigure](https://github.com/yanggyu17/DeepLearning_papers/blob/master/images/resnet2.png)
- 관점을 조금 바꿨을 뿐이지만 많은효과를 가져온다. 최적의 경우가 F(x) = 0 되어야 하기 때문에 학습의 방향이 미리 결정이 되어, 이것이 pre-conditioning 구실을 하게 된다.
- F(x)가 거의 0이 되는 방향으로 학습을 하게 되면 입력의 작은 움직임을 쉽게 검출할 수 있게 된다. 즉 작은 움직임, 나머지(residual)를 학습한다는 관점에서 residual learning이라고 불리게 된다.  

## Related Work

## Deep Residual Learning
### Residual Learning
- H(x)를 기존의 네트워크라고 할때(x는 input) F(x) := H(x) - x 로 바꿔 F(x) + x를 H(x)에 근사하도록 하는 것(Residual mapping)이 더 쉽다고 가정한다.

### Identity Mapping by Shortcuts

### Network Architecture
