# [hdl_people_tracking](https://github.com/koide3/hdl_people_tracking)



hdl_people_tracking is a ROS package for real-time people tracking using a 3D LIDAR. 
- 클러스터링 : It first performs Haselich's clustering technique to detect human candidate clusters, 
    - Confidence-Based Pedestrian Tracking in Unstructured Environments Using 3D Laser Distance Measurements
    - [깃북 정리](https://legacy.gitbook.com/book/adioshun/paper-3d-object-detection-and-tracking/edit#/edit/master/Tracking/2014-confidence-based-pedestrian-tracking-in-unstructured-environments-using-3d-laser-distance-measurements.md?_k=4bfxq2)
- 분류 : and then applies Kidono's person classifier to eliminate false detections. 
    - Pedestrian Recognition Using High-definition LIDAR
    - [깃북정리](https://legacy.gitbook.com/book/adioshun/paper-3d-object-detection-and-tracking/edit#/edit/master/2011-pedestrian-recognition-using-high-definition-lidar.md?_k=1m2iwy)
- 추적 : The detected clusters are tracked by using Kalman filter with a contant velocity model.


---

## 부모 프로젝트 

Kenji Koide, Jun Miura, and Emanuele Menegatti, A Portable 3D LIDAR-based System for Long-term and Wide-area People Behavior Measurement, Advanced Robotic Systems, 2019

https://github.com/koide3/hdl_graph_slam