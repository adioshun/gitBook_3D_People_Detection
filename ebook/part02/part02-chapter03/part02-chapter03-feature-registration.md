# Feature Based Registration



### Feature based registration

ICP는 단순한 아이디어로 손쉽게 적용하여 좋은 결과를 뽑아 낼수 있습니다. 하지만 상황에 따라서는 각 단계를 목적에 맞게 수정할 필요가있습니다. 본 실습에서는 PCL에서 제공하는 다양한 API를 이용하여 단계별로 살펴 보겠습니다. 수행시 필요한 모듈과 ‌동작 과정은 다음과 같습니다.

![](../../../.gitbook/assets/image%20%288%29.png)

1. 데이터 획득 : 센서 A와 B에서 각각 포인트 클라우드\_A와 포인트 클라우드\_B 를 획득 합니다. 
2. 키포인트 선택 : 여러개의 포인트 중에서 데이터를 가장 잘 설명하는 키포인트를 선정 합니다. 
3. Descriptor 계산 : 각 키포인트별 feature descriptor를 계산 합니다. 
4. correspondences 계산 : x,y,z위치 정보와 Descriptor 정보를 기반하여 유사도를 계산하여 correspondences를 계산 합니다. 
5. correspondences선정 : 계산  correspondences 중에서 나쁜 것을 버리게 됩니다. 
6. 변환행렬 계산 : 남은 좋은 correspondences를 이용하여 transformation을 계산 합니다. 



ICP의 점기반 정합과는 달리 위 방식에서는 키포인트를 선정하고 해당 키포인트의 특징 정보를 기반으로 정합을 수행 하였기에 특징기반 정합이라고도 합니다. 최소 3개의 대응점을 찾아 내면 변환행렬을 구할수 있다. 정확도를 높이기 위해서는 여러개의 대응값이 필요 하지만 매칭작업이 전체 포인트 클라우드가 아니라 키포인트값으로만 수행 되때문에 ICP보다는 속도가 빠르다. 일반적으로 특징기반으로 러프한 정합을 실시 하고, ICP로 다시 정밀하게 정합을 수행 합니다.

* * use SIFT Keypoints \(pcl::SIFT…something\)
* use FPFH descriptors \(pcl::FPFHEstimation\) at the keypoints
  * see our tutorials for that,  [like](http://www.pointclouds.org/media/rss2011.html)
* get the FPFH descriptors and estimate correspondences using  `pcl::CorrespondenceEstimation`
* reject bad correspondences using one or many of the  `pcl::CorrespondenceRejectionXXX methods`
* finally get a transformation as mentioned above

