# Smoothing

Smoothing은 포인트 보간\(interpolating\)을 통해 가능합니다. 보간이란  기존의 존재 하는 포인트 사이를 다른 포인트로 채워넣어 좀더 조밀한\(Dense\) 포인트클라드를 만드는 방식입니다. PCL에서는 Moving Least Squares \(MLS\) 알고리즘을 이용합니다. 아래 그림은 Smoothing전후의 Normal을 시각화 한것입니다. Normal값이 보정되어 정합이나 Normal을 이용한 알고리즘이나 밀집도 기반 알고리즘의 성능 향상도 가능합니다.

파라미터는 아래와 같습니다.

| 파라미터 | 설명 |
| :--- | :--- |
| setComputeNormals |  |
| setInputCloud |  |
| setPolynomialOrder |  |
| setSearchMethod |  |
| setSearchRadius |  |
| setPolynomialFit |  |
| setUpsamplingMethod | SAMPLE\_LOCAL\_PLANE,  RANDOM\_UNIFORM\_DENSITY |
| process |  |

활용

Smoothing은 업샘플링으로도 볼수 있습니다. 업샘플리은 앞장에서 살펴본 다운 샘플링과 반대로 포인트 클라우드의 수를 늘리는 것을 의미 합니다. D 센서 데이터는 이미지에 데이터 대비 저밀도\(Sparse\)한 특징을 가지고 있습니다\[1\]. 또한, 빔의 확산\(divergence\)으로 인해 원거리 물체는 근거리 대비 상대적으로 적은 포인트가 형성 됩니다\[2\]. 물체를 표현하는 점군이 적을경우 특징 추출이 어려워 인식 성능이 떨어 집니다. 일반적으로 업샘플링은 탐지/식별 정확도 향상과 물체의 형태를  등의 목적으로 수행 합니다.



![](../../../../.gitbook/assets/image%20%286%29.png)

