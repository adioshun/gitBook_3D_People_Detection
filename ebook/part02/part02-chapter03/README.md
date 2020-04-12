---
description: 하나의 물체에 대해 서로다른 위치에서 획득한 두개 이상의 지역 좌표계를 가진 점군을 하나의 표준 좌표계로 정렬하는것
---

# 04-Registration





여러개의 각도에서 찍은 포인트클라우들을 정렬하여 하나의 완성된 포인트 클라우드를 생성한느것을 정합\(Registration\) 이라고 합니다. 물체의 3D 포인트 클라우드를 생성할때 방법은 물체의 앞에서 스캔하여 점군을 얻고, 뒷면을 스캔하여 점군을 얻은 후 이 둘을 합치면 됩니다. 하지만 여러 방향에서 데이터를 얻게 되면 찍은 위치가 다르기 때문에 서로 다른 지역 좌표계\(local coordinate system\)에서 얻은 데이터를 원래 물체처럼 하나로 나타내기 위해 하나의 전역 좌표계\(global coordinate system\) 변경하여 표현하여야 합니다. 이렇둣, 여러 점군 데이터의 여러 지역 좌표계를 점군 데이터 세트 하나의 전역 좌표계로 표현하는 과정을 정합이라고 한다. 정합 알고리즘의 최종 목적은 전역 좌표계로의 지역 좌표계 이동 및 회전량을 위한 **변환행렬** 추출 입니다. 변환행렬을 이용하여 정합을 수행하면 full, complete , continous 한 점군을 얻게 되어 이후에 진행되는 특징 추출에 좋은 결과를 얻게 됩니다. SLAM 이라는 알고리즘에서 지도생성 작업에서도 활용 됩니다.

 ![](https://www.spiedigitallibrary.org/ContentImages/Journals/JARSC4/10/4/045024/WebImages/JARS_10_4_045024_f024.png)

![](https://storage.googleapis.com/groundai-web-prod/media/users/user_301676/project_403379/images/icp-example.jpg)

Improved algorithm for point cloud registration based on fast point feature histograms, 2016, \[Peng Li\]

![](https://storage.googleapis.com/groundai-web-prod/media/users/user_301676/project_403379/images/registration-intro.png) [https://www.groundai.com/project/target-less-registration-of-point-clouds-a-review/1](https://www.groundai.com/project/target-less-registration-of-point-clouds-a-review/1)

PCL에서는 다음 알고리즘을 제공 합니다.

* ICP 기반 
* Feature based registration

