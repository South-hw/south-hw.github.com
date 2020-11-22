---
title: "CAM, GRAD-CAM"
categories: [review of paper]
use_math: true
---

## CAM

##### Paper
Zhou, Bolei, et al. "Learning Deep Features for Discriminative Localization", CVPR, 2016

##### Proposition
CAM (Class Activation Map)는 weakly-supervised object localizatoin입니다. Weakly-supervised learning이란 strong supervision이 
아닌 weak supervision으로 네트워크를 학습시키는 것입니다. Segmentation과 같은 localization의 경우 annotation 비용이 classification task에 
비해서 더 많이 든다는 것을 직관적으로 알 수 있습니다. 이 논문에서는 classification task를 통해서 object localization할 수 있는 것을 제안합니다. 
또한 score에 기여한 features와 weights를 이용하여 MAP을 생성하므로 Explainable AI로도 생각할 수 있습니다.

##### Method
![](/assets/imgs/cam.png)

위 그림처엄 네트워크가 "conv - GAP - classifier"인 경우에 해당 score가 나오도록 기여한 weights를 알 수 있습니다. GAP, Score는 다음 식으로 표현할 수 있습니다.
\$\$
F^k = \Sigma_{x,y} f_k(x,y)
\$\$
\$\$
S_c = \underset{k}\Sigma w_k^c \underset{x,y}\Sigma f_k(x,y).
\$\$
논문에서는 두 식을 합성하여 논문에서는 CAM을 다음으로 표현합니다. 
\$\$
M_c (x,y) = \underset{k}\Sigma w_k^c f_k(x,y).
\$\$

## Grad_CAM

##### Paper
Selvaraju, Ramprasaath R., et al. "Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization", ICCV, 2017

##### Proposition
Grad-CAM은 CAM의 generalization version 입니다. CAM은 네트워크 구조가 "conv - global pooling - classifier"에만 적용할 수 있지만 Grad-CAM
은 다양한 네트워크 구조에 적용할 수 있습니다. 그 이유는 Grad-CAM은 Loss로부터 gradient를 계산하기 때문입니다. 논문에서는 Guided Grad-CAM도 소개하지만 
Grad-CAM만 포스팅하겠습니다.

##### Method
CAM에서 $w_k^c$를 importance라고 하며, socre에 기여한 features의 중요도를 나타냅니다. Grad-CAM에서는 importance를 다음과 같이 계산합니다.
\$\$
\alpha_k^c = \frac{1}{Z} \sum_{i} \sum_{j} \frac{\partial y^c}{\partial A^k_{ij}}.
\$\$ 
즉 importance는 output을 features 픽셀마다 각각 gradient를 계산한 후 x,y 좌표로 더해주는 것입니다. importance는 구하고자하는 features의 채널만큼 
나오게 됩니다. 최종적으로 features와 importance를 곱한 후 ReLU function으로 Grad-CAM은 표현됩니다.
\$\$
L^c_{Grad-CAM} = ReLU(\sum_{k} \alpha^c_k A^k).
\$\$

##### etc
이후에 weakly object localization과 관련하여 다양한 논문이 나왔습니다. 다양한 논문에서 서로 비교하고 SOTA가 나왔겠지만, 한 논문에서는 새로운 evaluation metric을 
제시하면서 CAM이 나온 이후로 큰 발전이 없었다고 주장하기도 하였습니다. 즉 CAM과 Grad-CAM은 구현하기도 매우 쉽지만 성능도 매우 우수한 것으로 받아들일 수 있고, 
visual task에서 다양하게 사용될 수 있는 범용성 높은 논문이라고 생각합니다. (논문들에서 네트워크의 성능을 CAM으로 보여주기도 합니다.)  
