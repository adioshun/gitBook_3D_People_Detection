---
description: 하나의 물체에 대해 서로다른 위치에서 획득한 두개 이상의 지역 좌표계를 가진 점군을 하나의 표준 좌표계로 정렬하는것
---

# 04-Registration

휴대폰의 기능 중 파노라마 사진찍기가 있습니다. 동일한 사물을 다양한 위치에서 찍은 여러 장의 사진을 스티칭\(Stitching\)라는 기술을 이용하여 중복되는 부분을 붙이면 한장의 파노라마 사진을 얻을수 있습니다. 이와 유사하게다양한 각도와 위치의 사진을 Sfm\(Structure from Motion\)라는 기술을 이용하여 3D 이미지로 합칠수도 있습니다.  두 기술의 기본 원리는 각 사진들에서 중복되는 부분을 찾아 서로 매칭 시키는 것입니다.

!\[[위키피디아](https://en.wikipedia.org/wiki/Image_stitching#/media/File:Rochester_NY.jpg)\]\([https://i.imgur.com/xK8yde5.jpg](https://i.imgur.com/xK8yde5.jpg)\)

!\[\]\([https://i.imgur.com/JZBN2a1.png](https://i.imgur.com/JZBN2a1.png)\)

이와 비슷하게 포인트 클라우드에서는 여러 각도에서 획득한 포인트에서 중복되는 부분을 찾아 매칭 하여 하나의 완성된 포인트 클라우드로 만드는 정합\(Registration\)기술을 이용하여 얻을수 있습니다. 먼저 물체의 3D 포인트 클라우드를 생성하는 방법은 물체의 앞에서 스캔하여 점군을 얻고, 뒷면을 스캔하여 점군을 얻은 후 이 둘을 합칩니다. 하지만 여러 방향에서 데이터를 얻게 되면 점군을 획득한 위치가 다르기 때문에 서로 다른 지역 좌표계\(local coordinate system\)에서 얻은 데이터이기에 그림 \(b\)와 같이 됩니다. 따라서 기준이 되는 하나의 전역 좌표계\(global coordinate system\) 변경하여야  하나의 물체처럼  표현하여야 합니다. 이렇둣, 여러 점군 데이터의 여러 지역 좌표계를 점군 데이터 세트 하나의 전역 좌표계로 변환 하기 위한 정보를 얻는 과정을 정합이라고 한다. 정합 알고리즘의 최종 목적은 전역 좌표계로의 지역 좌표계 이동 및 회전량을 위한 **변환행렬** 추출 입니다. 변환행렬을 이용하여 정합을 수행하면 완벽한 형태의 점군을 얻게 되어 이후에 진행되는 특징 추출에 좋은 결과를 얻게 됩니다. 정합 기술은 SLAM 이라는 지도생성 작업에서도 활용 됩니다.

 ![](https://www.spiedigitallibrary.org/ContentImages/Journals/JARSC4/10/4/045024/WebImages/JARS_10_4_045024_f024.png)

![](https://storage.googleapis.com/groundai-web-prod/media/users/user_301676/project_403379/images/icp-example.jpg)

Improved algorithm for point cloud registration based on fast point feature histograms, 2016, \[Peng Li\]

![](https://storage.googleapis.com/groundai-web-prod/media/users/user_301676/project_403379/images/registration-intro.png) [https://www.groundai.com/project/target-less-registration-of-point-clouds-a-review/1](https://www.groundai.com/project/target-less-registration-of-point-clouds-a-review/1)

본문에서는 정합 기법을  전역적\(Global\)방법과 지역적\(Local\)방법으로 나누었습니다. 구분 기준은 포인트 클라우드의 특징 정보를 기반으로 정밀도는 약하지만 빠른 정합을 수행하는 방법을 전역적 방법으로 하였으며, 지역적 방법은 포인트 클라우드의 각 포인트를 기반으로 목적함수를 최적화 하는 방식으로 정밀도는 높지만 반복적으로 수행 으로 시간이  걸리는 방식을 지역적 방법으로 구분 하였습니다. 지역적 정합은 초기 위치값을 잘못 잡게 되면 Local Minima에 빠지게 되므로 중요 합니다. 이를 해결 하기 위하여 초기 위치값 선정시 IMU나 GPS등의 외부 센서를 이용하여 구할수도 있고, 전역적 방법으로 구할수도 있습니다. 일반적으로는 전역적 방법으로 대략적인 정합을 수행하고, 지역적 방법으로 세밀한 조정을 하여 정합 정확도를 높히게 됩니다. 따라서 일부 논문에서는 전자를 rough 정합, 후자를 정밀정합법이라고 부르기도 합니다. 각 정합 방법을 위해 PCL에서는 제공하는 방법을 나누어 보면 다음과 같습니다. 

* 전역적 정합 : API\(keypoint, Feature \)기반 정합, SAC-IA 정합 등 
* 지역적 정합 : ICP  정합, NDP 정합 

