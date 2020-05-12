# 전역 정합

전역 정합 또는 대략적 정합은 두개의 포인트 클라우드의 점들에서 특징을 계산하고 이를 기반으로 정합을 수행 하는 방법입니다. 

![](http://robotica.unileon.es/images/thumb/7/74/Pairwise_registration.jpg/650px-Pairwise_registration.jpg)

수행 절차는 아래와 같습니다. 

1. 키포인트 선택 : 여러개의 포인트 중에서 데이터를 가장 잘 설명하는 키포인트를 선정 합니다. 
2. 특징 계산 : 각 키포인트별 feature descriptor를 계산 합니다. 
3. 대응점 찾기 : x,y,z위치 정보와 Descriptor 정보를 기반하여 유사도를 계산하여 correspondences를 계산 합니다. 
4. 대응점 선정 : 계산  correspondences 중에서 나쁜 것을 버리게 됩니다. 
5. 변환행렬 계산 : 남은 좋은 correspondences를 이용하여 transformation을 계산 합니다. 

PCL에서 제공하는 다양한 API를 이용하여 단계별로 살펴 보겠습니다. 

1. 키포인트 선택  

두 점군을 비교 할대 사용할 포인트를 선정 하는 단계 입니다. 키포인트 선정시 주요 고려 요소는 다음과 같습니다. 

* **Repeatability**: there should be a good chance of the same points being chosen over several iterations, even when the scene is captured from a different angle.
* **Distinctiveness**: the chosen keypoints should be highly characterizing and descriptive. It should be easy to describe and match them.

PCL에서는 NARF, SIFT, FAST등의 키포인트 추출 방법을 제공하고 있습니다. 또는 간단하게 1장에서 살펴본 샘플링 방법중 하나인 그리드 샘플링을 수행 하여 단순히 수를 줄이는 방법도 있습니다. 



2. 특징 계산 

선택된 각 키포인트에 대하여 특징을 계산 합니다. 정합의 기본 목표는 두 점군의 유사점을 찾아 맵핑 하는 것입니다. 이러한 유사점을 찾기 위해 특징정보를 이용합니다. PCL에서는 NARF, FPFH, BRIEF, SHIF 등의 특징 추출 방법을 제공하고 있습니다. 일부 특징들은 계산시 키포인트의 영향을 받거나 Normal등의 추가 정보가 필요 하기도 합니다. 따라서 특징을 선택 하기 위해서는 키포인트 선택도 중요 합니다. 



3. 대응점 찾기\(Correspondences estimation\)

두 점군의 특징정보를 이용하여 겹치는 부분을 찾는 단계 입니다. 사용되는 특징의 종류에 따라서   brute force matching,kd-tree nearest neighbor search \(FLANN\)방법을 사용할수 있습니다. `pcl::CorrespondenceEstimation`

4. 대응점 선정\(Correspondences rejection\)

위 단계에서 찾은 대응점이 항상 옮지는 않습니다. 잘못된 대응점은 오히려 정합을 어렵게 하거나 실패 할수도 있습니다.따라서 판단 후 제거 되어야 합니다.  판다을 위해서는 RANSAC을 사용할수 있습니다. `Correspondences rejection`



5. 변환행렬 계산 

이제 모든 준비 단계는 끝났습니다. 변환 행렬 계산은 다음과 같습니다. 

* evaluate some error metric based on correspondence
* estimate a \(rigid\) transformation between camera poses \(motion estimate\) and minimize error metric
* optimize the structure of the points
  * Examples: - SVD for motion estimate; - Levenberg-Marquardt with different kernels for motion estimate;
* use the rigid transformation to rotate/translate the source onto the target, and potentially run an internal ICP loop with either all points or a subset of points or the keypoints
* iterate until some convergence criterion is met





ICP의 점기반 정합과는 달리 위 방식에서는 키포인트를 선정하고 해당 키포인트의 특징 정보를 기반으로 정합을 수행 하였기에 특징기반 정합이라고도 합니다. 최소 3개의 대응점을 찾아 내면 변환행렬을 구할수 있다. 정확도를 높이기 위해서는 여러개의 대응값이 필요 하지만 매칭작업이 전체 포인트 클라우드가 아니라 키포인트값으로만 수행 되때문에 ICP보다는 속도가 빠르다. 일반적으로 특징기반으로 러프한 정합을 실시 하고, ICP로 다시 정밀하게 정합을 수행 합니다.



