# 08-Recognition



## PCL-Three-dimensional Object Recognition Based on Corresponding Grouping

[http://pointclouds.org/documentation/tutorials/correspondence\_grouping.php\#correspondence-grouping](http://pointclouds.org/documentation/tutorials/correspondence_grouping.php#correspondence-grouping) [https://blog.csdn.net/weixin\_41835977/article/details/87626852](https://blog.csdn.net/weixin_41835977/article/details/87626852)

### 1. 키포인트 선정

전체 포인트 클라우드에서 어떤 포인트를 키포인트로 선정 할지를 정해야 함

* 다운 샘플링후 결과를 키 포인트로 간주 
* NARF keypoint detector 등을 이용하여 키 포인트 선택 

### 2. descriptors 계산

선별된 키 포인트의 모든 descriptor 계산 \(`The next step is to compute the desired local descriptor(s) for every keypoint.`\)

* PFH
* NARF 

### 3. matching

미리 저장된 object database의 값과 맞는것을 찾아내기\(`find all point correspondences between the cloud and a model:`\)

* K-dtree를 이용하여 유사성 거리 계산 하여 탐색 

결과물 : Right now, all we have is a list of correspondences between keypoints in the scene and keypoints from some object\(s\) in the database.

### 4. Correspondence grouping

특징이 비슷하여 잘못 매칭된 Correspondence가 있을수 있다. 이경우 geometrically정보를 보고 \(eg. 서로의 위치가 너무 멀다\) 제거 할수 있다.

이러한 작업을 Correspondence grouping이라고 한다. \(`it groups correspondences that are geometrically consistent`\)

* Geometric consistency : 가장 간단한 방법 `pcl::GeometricConsistencyGrouping`
* Hough voting 

### 5. Pose estimation

> 위에서 언급한 correspondence grouping 알고리즘에 내장되어 있으므로 메뉴얼/추가 보정을 수행할때만 진행

* RANSAC 
* pcl::SampleConsensusPrerejective  : SAC\_plane등의 모델이 아니라 Descriptors를 입력으로 받아 비교 
* registration활용 가능 
* [Robust pose estimation of rigid objects](http://pointclouds.org/documentation/tutorials/alignment_prerejective.php)
* [Aligning object templates to a point cloud](http://pointclouds.org/documentation/tutorials/template_alignment.php)





---

3D Object Recognition in Clutter with the Point Cloud Library : 

Tutorial: Point Cloud Library: Three-Dimensional Object Recognition and 6 DOF Pose Estimation : [https://ieeexplore.ieee.org/document/6299166?arnumber=6299166](https://ieeexplore.ieee.org/document/6299166?arnumber=6299166) , 2012







