# Classification (=Object Recognition)

> The Udacity RoboND use an RGB-D sensor , eg. color. It does not fit to our problems.  
> - Pick list.yaml : label, color
> - outpul.yaml : position(x,y,z)
> RoboND-Perception-Exercises/Exercise-3/ : [Object Recognition with Python, ROS and PCL](https://github.com/udacity/RoboND-Perception-Exercises/tree/master/Exercise-3)



## 1. SVM

### 1.1 Capture Object Features

### 1.2 Training 





---

## 5. Classification

군집화된 포인트들을 사람, 자동차, 가로수 등으로 구분

### 5.1 Machine learning based approach

RBF (Radial Basis Function) 커널을 사용한 SVM (Support Vector Machine)


> [GM-PHD 필터를 이용한 보행자 탐지 성능 향상 방법(2015)](http://www.dbpia.co.kr/Journal/ArticleDetail/NODE06594856)

### 5.2 Deep learning based approach

![](https://github.com/BichenWuUCB/SqueezeSeg/raw/master/readme/pr_0005.gif)
[SqueezeSeg: Convolutional Neural Nets with Recurrent CRF for Real-Time Road-Object Segmentation from 3D LiDAR Point Cloud](https://github.com/BichenWuUCB/SqueezeSeg)

---

# Classification

## 1. List



## 2. Paper

- [추천] SqueezeSeg demo: CNN for LiDAR point cloud segmentation : [Youtube](https://www.youtube.com/watch?v=Xyn5Zd3lm6s), [[깃허브]](https://github.com/BichenWuUCB/SqueezeSeg)

- [Tracking People in 3D Using a Bottom-Up Top-Down Detector](http://www2.informatik.uni-freiburg.de/~spinello/ICRA2011.html): ICRA2011

- [DepthCN: Vehicle Detection Using 3D-LIDAR and ConvNet](http://home.isr.uc.pt/~cpremebida/files_cp/DepthCN_preprint.pdf)

- Deep Semantic Classification for 3D LiDAR Data, Ayush Dewan

- [3D Object Recognition based on Correspondence Grouping](http://www.pointclouds.org/documentation/tutorials/correspondence_grouping.php#correspondence-grouping): This tutorial aims at explaining how to perform 3D Object Recognition based on the pcl_recognition module.

- [LiDAR 센서 정보를 활용한 데이터 마이닝 기법 기반의 수상함정 표적 식별기법 제안](http://www.dbpia.co.kr/Journal/ArticleDetail/NODE07207161): 2017, 엄준호

- [A General Purpose Feature Extractor for Light Detection and Ranging Data](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3230992/): 2010

- [Multiple Sensor Fusion and Classification for Moving Object Detection and Tracking](https://hal.archives-ouvertes.fr/hal-01241846/document): 2015

- [Multisensor Online Transfer Learning for 3D LiDAR-based Human Detection with a Mobile Robot](https://arxiv.org/pdf/1801.04137.pdf): 2018, L-CAS

- [LIDAR-based 3D Object Perception](http://www.velodynelidar.com/lidar/hdlpressroom/pdf/papers/journal_papers/LIDAR-based%203D%20Object%20Perception.pdf): 2015

- [Instant Object Detection in Lidar Point Clouds](http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7927715): 2017

- [Lidar Based Object Detection Near Vehicle](http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7973852): 2017


- [Self-supervised online learning of appearance for 3D tracking](https://ieeexplore.ieee.org/abstract/document/8206373/): 2017

- [3D Lidar-based Static and Moving Obstacle Detection in Driving Environments:an approach based on voxels and multi-region ground planes](http://home.isr.uc.pt/~cpremebida/files_cp/3D%20Lidar-based%20static%20and%20moving%20obstacle%20detection%20in%20driving%20environments_Preprint.pdf): 2016

- [A real-time LIDAR and vision based pedestrian detection system for unmanned ground vehicles](https://ieeexplore.ieee.org/abstract/document/7486580/): 2015

- [Object detection in 3D point clouds](https://www.mi.fu-berlin.de/inf/groups/ag-ki/Theses/Completed-theses/Master_Diploma-theses/2016/Damm/Master-Damm.pdf): 2016, 3장 참고
  - Downsampling
  - ground Segmentation
  - Clustering
  - Tracking / Kalman filter
  
- [Object Classification using CNN-Based Fusion of Vision and LIDAR in Autonomous Vehicle Environment](https://ieeexplore.ieee.org/document/8331162/#full-text-section): 2018

- [Laser-Based Detection and Tracking of Dynamic Objects](http://www.robots.ox.ac.uk/~mobile/Theses/WangThesis.pdf): 2014, 옥스포드, 183p

- [No Blind Spots: Full-Surround Multi-Object Tracking for Autonomous Vehicles using Cameras & LiDARs](https://arxiv.org/pdf/1802.08755.pdf): 2018

- [추천][3D-LIDAR Multi Object Tracking for Autonomous Driving](https://repository.tudelft.nl/islandora/object/uuid:f536b829-42ae-41d5-968d-13bbaa4ec736/datastream/OBJ/download): 2017, 140page, 석사학위 논문

- [Object tracking and state estimation in outdoor scenes based on 3D laser scanner](https://ieeexplore.ieee.org/document/7888334/): 2016

- Real-Time Deep ConvNet-Based Vehicle Detection Using 3D-LIDAR Reflection Intensity Data : 2017

- [3D Scanning: A Comprehensive Survey](https://arxiv.org/pdf/1801.08863.pdf): 2018


- [원(Raw) LiDAR 자료 구조를 이용한 LiDAR 자료의 분리(Segmentation)에 관한 연구](http://www.dbpia.co.kr/Journal/ArticleDetail/NODE01354500): 2005, 서울대 유기윤, 지구환경시스템공학부

- [깊이 영상 기반의 보행자 검증을 적용한 보행자 검출 성능 분석](http://www.dbpia.co.kr/Journal/ArticleDetail/NODE02432113): 2014, DGIST 임영철, 강민

- ~~[GM-PHD 필터를 이용한 보행자 탐지 성능 향상 방법](http://www.dbpia.co.kr/Journal/ArticleDetail/NODE06594856)~~: 2015, 서울대 서승우
  - 기반 논문 : [Towards 3D Object Recognition via Classification of Arbitrary Object Tracks](https://cs.stanford.edu/people/teichman/papers/icra2011.pdf): 추천,Alex Teichman 2011
  - Alex Teichman 박사 학위 논문 : [USER-TRAINABLE OBJECT RECOGNITION SYSTEMS VIA GROUP INDUCTION](http://www.alexteichman.com/files/dissertation.pdf)

```
[3] D. Prokhorov, “A Convolutional Learning System for Object Classification in 3-D Lidar Data,” IEEE Trans. Neural Networks, Vol. 21, No. 5, pp. 858-863, May 2010.
[4] S. Awan, M. Muhamad, K.Kusevic, P. Mrstik and M. Greenspan, “Object Class Recognition in Mobile Urban Lidar Data Using Global Shape
Descriptors,” 2013 International Conference on 3D Vision, Seattle, WA, USA, June 2013.
```

Premebida[5]의 연구에서는 불완전한 3D 센서인 4채널 LIDAR를 이용하여 도로 환경에서 보행자 탐지 알고리즘을 제안하였다. 데이터의 선형성과 원형성을 바
탕으로 한 특징을 사용하여 보행자 분류를 하였지만, 포인트 수가 적어 가까운 지역에서만 인식 가능하며, 완전한 3차원 데이터에 이 방법을 적용시키기에는 계산시간이 오래 걸린다는 한계가 있다.

```
[5] C. Premebida, O. Ludwig and U. Nunes, “Exploiting LIDAR-based Features on Pedestrian Detection in Urban Scenarios,” 12th International
IEEE Conference on Intelligent Transportation Systems, St. Louis, MO, USA, Oct. 2009.
```

Navaro-Serment[6]는 각 물체에 해당하는 3D 포인트 클라우드(Pointcloud)를 두 다리와 몸통에 해당하는 3가지 부분으로 나눠 각 부분의 분산행렬을 특징으로 분류하는 방법을 제시했다. 하지만 먼 거리의 사람데이터는 포인트 수가 부족하여 다리와 몸통을 구분하기 어렵다는 문제로 인해 인식 거리가 짧고 정확하지 않다는 문제점이 있다.
```
[6] L. E. Navarro-Serment, C. Mertz, and M. Hebert, “Pedestrian Detection and Tracking Using Three-Dimensional LADAR Data,” International Conference on Field and Service Robotics, 2009.
```




## 3. Article (Post, blog, etc.)



## 3. Tutorial (Series, )



## 4. Youtube



## 6. Material (Pdf, ppt)



## 7. Implementation (Project)

- ROS **obstacle detection** for 3D point clouds using a height map algorithm
    - [ROS velodyne_height_map](http://wiki.ros.org/velodyne_height_map)
    - [깃허브](https://github.com/jack-oquin/velodyne_height_map)





## 8. Research Group / Conference 