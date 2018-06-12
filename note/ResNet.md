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
- depth가 깊은 상태에서 학습을 이미 많이 진행한 경우 weight들의 분포가 균등하지 않고, 역전파시 기울기가 충분하지 않아 학습을 안정적으로 진행할 수 없는 문제가 있다. 
- over-fitting 문제라고 착각할 수도 있겠지만 실제로 그것이 원인은 아니다.
