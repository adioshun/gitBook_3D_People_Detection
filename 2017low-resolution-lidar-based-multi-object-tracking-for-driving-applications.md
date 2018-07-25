# Low resolution lidar-based multi-object tracking for driving applications

http://www.iri.upc.edu/files/scidoc/1924-Low-resolution-lidar-based-multi-object-tracking-for-driving-applications.pdf

# Abstract

we developed a lidar-based system that uses a Convolutional Neural Network (CNN), to perform pointwise vehicle detection using PUCK data, and Multi-Hypothesis Extended Kalman Filters (MH-EKF), to estimate the actual position and velocities of the detected vehicles. 

# 1.  Introduction

기존 Geometrical 접근 법 `Lidar point clouds have been traditionally processed following geometrical approaches like in [20].`


최근 딥러닝 방식으로 좋은 성과 보임 `However, recent works [8], [5] are pointing at Deep Learning techniques as powerful tools to extract information from point clouds, expanding their applicability beyond image processing tasks. `

저자는 HDL-64로도 비슷한 연구를 진행 하였음 `In previous works [26], we developed a vehicle lidar-based tracking system that used a Fully Convolutional Network (FCN) to perform per-point data segmentation using a Velodyne HDL64 sensor. `