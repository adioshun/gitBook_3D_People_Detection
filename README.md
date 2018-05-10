# 3D People Detection 


![](https://i.imgur.com/rZiLqLR.png)
> 출처 : [Object detection in 3D point clouds](https://www.mi.fu-berlin.de/inf/groups/ag-ki/Theses/Completed-theses/Master_Diploma-theses/2016/Damm/Master-Damm.pdf)


- A Survey of Human-Sensing: Methods for Detecting Presence, Count, Location, Track, and Identity, THIAGO TEIXEIRA, Yale University,2010

- [Ten Years of Pedestrian Detection,What Have We Learned?](https://arxiv.org/pdf/1411.4304.pdf): arXiv2014, Camera기반 탐지 기법 서베이 논문

$\sum$



---

안녕하세요. 말씀 드렸던 참고 하실만한 논문을 보내드립니다.

 

모델 기반 Detection은 아래 논문을 참고 하면 되는데 사람에 대한 모델링은 아닙니다. 모델링은 따로 하셔야 할 것 같습니다.

Model Based Vehicle Tracking for AutonomousDriving in Urban Environments

 

Camera-Lidar Calibration은 Kitti에서 나온 논문을 참고 하시면 되는데 다양한 논문들이 있지만 데이터 가공에 대한 내용이 조금씩

다를 뿐 핵심적인 내용은 크게 다르지 않습니다. 

Automatic Camera and Range Sensor Calibration using a single Shot

 

3D Perception은 아래 논문을 참고 하시면 되며 네트워크의 구성은 거의 동일 하며 두번째는 좀 더 발전된 행태로

3D BBox의 Regression을 더 잘 하기 위해 데이터를 가공하는 단계가 추가 되었습니다. 세번째는 Lidar 만을 사용 합니다.

Multi-View 3D Object Detection Network for Autonomous Driving

Joint 3D Proposal Generation and Object Detection from View Affregation

VoxelNet: End-to-End Learning for Point Cloud Based 3D Object Detection

 

Point Cloud를 다루는 Library는 PCL을 보편적으로 사용하고 Algorithm, Visualization도 포함하고 있습니다.

http://pointclouds.org/

Python 에서는 Mayavi를 사용 하시면 Visualization을 조금 더 쉽게 쓸 수 있습니다.

http://docs.enthought.com/mayavi/mayavi/
