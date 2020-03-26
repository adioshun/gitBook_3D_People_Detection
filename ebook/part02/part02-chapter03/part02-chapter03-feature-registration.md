# Feature Based Registration



### Feature based registration

특징 기반의 정합은 키포인트와 기술자를 생성한후 이를 비교 하는 방법 입니다. 최소 3개의 대응점을 찾아 내면 변환행렬을 구할수 있다. 정확도를 높이기 위해서는 여러개의 대응값이 필요 하지만 매칭작업이 전체 포인트 클라우드가 아니라 키포인트값으로만 수행 되때문에 ICP보다는 속도가 빠르다. 일반적으로 특징기반으로 러프한 정합을 실시 하고, ICP로 다시 정밀하게 정합을 수행 합니다.

동작 과정은 다음과 같습니다.

* use SIFT Keypoints \(pcl::SIFT…something\)
* use FPFH descriptors \(pcl::FPFHEstimation\) at the keypoints
  * see our tutorials for that,  [like](http://www.pointclouds.org/media/rss2011.html)
* get the FPFH descriptors and estimate correspondences using  `pcl::CorrespondenceEstimation`
* reject bad correspondences using one or many of the  `pcl::CorrespondenceRejectionXXX methods`
* finally get a transformation as mentioned above

