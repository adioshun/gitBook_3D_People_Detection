# ICP

### ICP \(Iterative closest points\)

ICP는 이름에서 알수 있듯이 반복적인 수행을 통해서 두 점군의 키포인트의 거리가 가까운 **변환행렬**을 추출 하는것을 목적으로 하고 있습니다. 단점은 모든 포인트에 대하여 brute force방식으로 진행 하기 때문에 시간이 많이 소요 됩니다. 또한 반복 횟수가 적으면 정합에 실패 할수도 있습니다.

키 아이디어는 두 점군의 대응점을 찾고, 이 대응점의 거리가 최소가 되는 변환행렬을 구하는 것이다. 따라서 두 점군은 겹치는 부분이 있어야 합니다.

동작 과정은 아래와 같습니다.

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

