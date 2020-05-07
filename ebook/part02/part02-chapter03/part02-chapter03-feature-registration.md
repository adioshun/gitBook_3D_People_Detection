# Feature Based Registration



### Feature based registration

ICP는 단순한 아이디어로 손쉽게 적용하여 좋은 결과를 뽑아 낼수 있습니다. 하지만 상황에 따라서는 각 단계를 목적에 맞게 수정할 필요가있습니다. 본 실습에서는 PCL에서 제공하는 다양한 API를 이용하여 단계별로 살펴 보겠습니다. 수행시 필요한 모듈과 ‌동작 과정은 다음과 같습니다.

1. 키포인트 선택 : 여러개의 포인트 중에서 데이터를 가장 잘 설명하는 키포인트를 선정 합니다. 
2. 특징 계산 : 각 키포인트별 feature descriptor를 계산 합니다. 
3. 대응점 찾기 : x,y,z위치 정보와 Descriptor 정보를 기반하여 유사도를 계산하여 correspondences를 계산 합니다. 
4. 대응점 선정 : 계산  correspondences 중에서 나쁜 것을 버리게 됩니다. 
5. 변환행렬 계산 : 남은 좋은 correspondences를 이용하여 transformation을 계산 합니다. 



1. 키포인트 선택  

연산 부하를 줄이기 위해서 전체 점군에서 일부 점군을 샘플링 하여 선택하는 단계입니다. 무작위로 선택 할수도 있지만 중요한 의미를 지니는 점인 키포인트를 계산하여 선택 할수도 있습니다 . PCL에서는 NARF, SIFT, FAST등의 키포인트 추출 방법을 제공하고 있습니다. 또는 1장에서 살펴본 샘플링 방법중 하나인 그리드 샘플링을 수행 할수도 있습니다. 



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



