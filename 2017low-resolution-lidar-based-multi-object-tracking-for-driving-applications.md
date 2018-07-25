# Low resolution lidar-based multi-object tracking for driving applications

http://www.iri.upc.edu/files/scidoc/1924-Low-resolution-lidar-based-multi-object-tracking-for-driving-applications.pdf

# Abstract

we developed a lidar-based system that uses a Convolutional Neural Network (CNN), to perform pointwise vehicle detection using PUCK data, and Multi-Hypothesis Extended Kalman Filters (MH-EKF), to estimate the actual position and velocities of the detected vehicles. 

# 1.  Introduction

기존 Geometrical 접근 법 `Lidar point clouds have been traditionally processed following geometrical approaches like in [20].`

```python
20. Petrovskaya, A., Thrun, S.: Model based vehicle detection and tracking for autonomous urban driving. Autonomous Robots 26(2-3), 123–139 (2009)
```

최근 딥러닝 방식으로 좋은 성과 보임 `However, recent works [8], [5] are pointing at Deep Learning techniques as powerful tools to extract information from point clouds, expanding their applicability beyond image processing tasks. `

```
8. Engelcke, M., Rao, D., Wang, D.Z., Tong, C.H., Posner, I.: Vote3deep: Fast object detection in 3d point clouds using efficient convolutional neural networks. In: Robotics and Autom. (ICRA), 2017 IEEE Int. Conf. on, pp. 1355–1361. IEEE (2017)
5. Chen, X., Ma, H., Wan, J., Li, B., Xia, T.: Multi-view 3d object detection network for autonomous driving. arXiv preprint arXiv:1611.07759 (2016)
```

저자는 HDL-64로도 비슷한 연구를 진행 하였음 `In previous works [26], we developed a vehicle lidar-based tracking system that used a Fully Convolutional Network (FCN) to perform per-point data segmentation using a Velodyne HDL64 sensor. `

```
26. Vaquero, V., del Pino, I., Moreno-Noguer, F., Sol`a, J., Sanfeliu, A., Juan, A.C.: Deconvolutional networks for point-cloud vehicle detection and tracking in driving scenarios. In: Mobile Robotics, 2017. (ECMR-2017). Eur. Conf. on. IEEE (2017)
```

# 2. Related Work



