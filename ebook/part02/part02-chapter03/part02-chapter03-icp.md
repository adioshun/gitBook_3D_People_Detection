# ICP

### ICP \(Iterative closest points\)

ICP는 이름에서 알수 있듯이 반복적인 수행을 통해서 두 점군의 키포인트의 거리가 가까운 **변환행렬**을 추출 하는것을 목적으로 하고 있습니다. 단점은 모든 포인트에 대하여 brute force방식으로 진행 하기 때문에 시간이 많이 소요 됩니다. 또한 반복 횟수가 적으면 정합에 실패 할수도 있어 일반적으로 다른 알고리즘을 이용하여 초기 정합을 수행 하여 대락적으로 정렬된 상태에서 세밀한 정밀을 수행 할때 많이 사용되어 Local Registration 방법. 

키 아이디어는 두 점군의 대응점을 찾고, 이 대응점의 거리가 최소가 되는 변환행렬을 구하는 것이다. 따라서 두 점군은 겹치는 부분이 있어야 합니다.

동작 과정은 아래와 같습니다.

![](../../../.gitbook/assets/image%20%285%29.png)



* 첫째, 점 선택\(point selection\) 단계에서는 알고리즘의 계산량을 줄이기 위하여 노이즈 필터링이나 샘플링을 거쳐 점의 갯수를 줄인다.
* 둘째, 이웃 선택\(neighborhood selection\) 단계에서는 각 점의 주변에 분포하는 점들 중 가까운 점 일부를 선택한다.
* 셋째, 점쌍 매칭\(point pair matching\) 단계에서는 앞서 선택한 주변 점들로 곡면\(surface\)이나 평면\(plane\) 같은 기하학 요소를 만들고, 이를 이용하여 소스 점군 A의 점 에 대응하는\(corresponding\) 타갯 점군 표의 점을 찾는다.
* 넷째, 오정합점 제거 \(outlier rejection\) 단계에서는 소스 점군 A와 타겟 점군 B에서 서로 겹치는 부분, 즉 오버랩 영역\(inlier\)만 남기고, 겹치지 않는 영역\(outlier\) 은 정합에 이용하지 않는다.
* 다섯째, 오차 최소화\(error minimization\) 단계 에서는 소스 점군 A의 오버랩 영역 A′과 타겟 점군 B의 오버랩 영역 B′ 방향으로 이동하는 동안 각 대응점의 거리 오차가 최소가 되도록 한다.
* 여섯째, 변환\(Transformation\) 단계에서 는 소스 오버랩 점군 A′가 타갯 오버랩 점군 B′의 위치로 이동하 는 데에 필요한병진 행렬\(translation matrix\)과 회전 행렬\(rotation matrix\)을 구하여 소스 오버랩 점군 A′에 적용한다.
  * 여기서 거리 오차가 일정한 한계치\(tolerance, τ\) 이하이면 진행을 멈주고 한계치 이상이라면 점쌍 매칭 \(point pair matching\) 단계로 되돌아가서 알고리즘을 다시 수행한다.

> [\[1\]](https://github.com/adioshun/gitBook_Tutorial_PCL/blob/master/part-2/part02-chapter05) 김지건, "[적은 오버랩에서 사용 가능한 3차원 점군 정합 방법](http://journal.cg-korea.org/archive/view_article?pid=jkcgs-24-5-11)", 2018
>
> P. Besl and N. McKay, ”A method for registration of 3-d shapes,” Pattern Analysis and Machine Intelligence, 14 \(2\) pp.239-256, 1992.

* 각 점군에서 키포인트 생성 
* 키포인트에서 feature descriptor 계산 
* 두 점군사이에 유사점을 비교 하여 대응점\(correspondences\) estimate
* 대응점들간의 거리 계산 반복 
* 가장 거리가 가까운 때의 변환 행렬 estimation 

PCL은 다양한 ICP방법을 제공 하고 있다.

* pcl::IterativeClosestPoint 
* pcl::IterativeClosestPointWithNormals
* pcl::IterativeClosestPointNonLinear 
* pcl::JointIterativeClosestPoint
* pcl::NormalDistributionsTransform

여기서는 기본적인 ICP를 살펴 보도록 하겠습니다.



[https://ieeexplore.ieee.org/document/121791](https://ieeexplore.ieee.org/document/121791)

