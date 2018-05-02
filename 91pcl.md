# PCL

[Community](http://www.pcl-users.org/), [설치(gitbook)](https://adioshun.gitbooks.io/system_setup/content/08a-pcl-setup.html)



![image](https://user-images.githubusercontent.com/17797922/39513568-cc50661e-4dc2-11e8-8e9e-0e9e1e5747ad.png)

> PCL API Documentation [Html](http://docs.pointclouds.org/trunk/), [pdf](http://www.pointclouds.org/assets/pdf/pcl_icra2011.pdf)

Module common : common data structures, computing distances/norms, means and covariances, angular conversions, geometric transformations

[Module filters](http://docs.pointclouds.org/trunk/group__filters.html): outlier and noise removal mechanisms (eg. PassThrough, voxel grid)
- [How 3D Features work in PCL](http://pointclouds.org/documentation/tutorials/how_features_work.php)

[Module features](http://docs.pointclouds.org/trunk/group__features.html): data structures and mechanisms for 3D feature estimation from point cloud data

[Module keypoints](http://docs.pointclouds.org/trunk/group__keypoints.html): keypoint detection algorithms 

[Module registration](http://docs.pointclouds.org/trunk/group__registration.html):  Combining several datasets into a global consistent model is usually performed using a technique called **registration**

[Module kdtree](http://docs.pointclouds.org/trunk/group__kdtree.html): kd-tree data-structure, Allows for fast nearest neighbor searches. 

[Module octree](http://docs.pointclouds.org/trunk/group__octree.html): efficient methods for creating a hierarchical tree data structure from point cloud data
- The `pcl_octree` implementation provides efficient nearest neighbor search routines, such as 
    - "Neighbors within Voxel Search”, 
    - “K Nearest Neighbor Search” 
    - “Neighbors within Radius Search”. 

[Module segmentation](http://docs.pointclouds.org/trunk/group__segmentation.html):  algorithms for segmenting a point cloud into distinct clusters
- [Euclidean Cluster Extraction](http://pointclouds.org/documentation/tutorials/cluster_extraction.php#cluster-extraction)

[Module sample_consensus](http://docs.pointclouds.org/trunk/group__sample__consensus.html): SAmple Consensus (SAC) methods like `RANSAC` / models like `planes` and `cylinders`
- [How to use Random Sample Consensus model](http://pointclouds.org/documentation/tutorials/random_sample_consensus.php#random-sample-consensus)


[Module surface](http://docs.pointclouds.org/trunk/group__surface.html): deals with reconstructing the original surfaces from 3D scans(eg. hull, a mesh representation or a smoothed/resampled)

[Module recognition](http://docs.pointclouds.org/trunk/group__recognition.html): algorithms used for Object Recognition 

[Module io](http://docs.pointclouds.org/trunk/group__io.html): reading and writing point cloud data (PCD) files
- The PCD (Point Cloud Data) file format
- Reading PointCloud data from PCD files
- Writing PointCloud data to PCD files
- The OpenNI Grabber Framework in PCL
- Grabbing point clouds from Ensenso cameras

[Module visualization](http://docs.pointclouds.org/trunk/group__visualization.html): visualize the results


Module Range Image : depth map


Module common : common data structures, computing distances/norms, means and covariances, angular conversions, geometric transformations

---
## PCD 읽기




```python
import pcl
import numpy as np
p = pcl.PointCloud(np.array([[1, 2, 3], [3, 4, 5]], dtype=np.float32))
seg = p.make_segmenter()
seg.set_model_type(pcl.SACMODEL_PLANE)
seg.set_method_type(pcl.SAC_RANSAC)
indices, model = seg.segment()
```


```python
import pcl
p = pcl.PointCloud()
p.from_file("table_scene_lms400.pcd")
fil = p.make_statistical_outlier_filter()
fil.set_mean_k (50)
fil.set_std_dev_mul_thresh (1.0)
fil.filter().to_file("inliers.pcd")
```


---

[Python_PCL API](http://strawlab.github.io/python-pcl/)


[ [RoboND_Sensors] brief course notes #robotics ](https://gist.github.com/kor01/84b4c1c590583533811781a9209f243e): By `kor01`


[Udacity Robotics Nanodegree](https://github.com/camisatx/RoboticsND)
- [install the Anaconda environment](https://github.com/camisatx/RoboticsND/blob/master/doc/configure_via_anaconda.md)
- [RANSAC.py](https://github.com/camisatx/RoboticsND/blob/master/projects/perception/Exercise-1/RANSAC.py)
- [voxel_grid_downsampling.py](https://github.com/fouliex/RoboND-Perception-Exercises/blob/master/Exercise-1/voxel_grid_downsampling.py): [Addiotional Code /w Sample](https://github.com/fouliex/RoboticPerception), [sample code](https://gist.github.com/kor01/84b4c1c590583533811781a9209f243e#file-009_voxel_grid_downsampling-py)


[Project: Perception Pick & Place](https://github.com/tony7126/perception_project)
: `Pipeline in depth` includes most of algorithms

[3D Robot Perception and Object Classification](https://www.haidynmcleod.com/3d-robot-perception)



---

# The Point Cloud Data

- [The PCD (Point Cloud Data) file format](http://pointclouds.org/documentation/tutorials/pcd_file_format.php)

```
# .PCD v.7 - Point Cloud Data file format
VERSION .7
FIELDS x y z rgb
SIZE 4 4 4 4
TYPE F F F F
COUNT 1 1 1 1
WIDTH 213
HEIGHT 1
VIEWPOINT 0 0 0 1 0 0 0
POINTS 213
DATA ascii
0.93773 0.33763 0 4.2108e+06
0.90805 0.35641 0 4.2108e+06
0.81915 0.32 0 4.2108e+06
0.97192 0.278 0 4.2108e+06
0.944 0.29474 0 4.2108e+06
0.98111 0.24247 0 4.2108e+06
0.93655 0.26143 0 4.2108e+06
0.91631 0.27442 0 4.2108e+06
0.81921 0.29315 0 4.2108e+06
0.90701 0.24109 0 4.2108e+06
0.83239 0.23398 0 4.2108e+06
0.99185 0.2116 0 4.2108e+06
0.89264 0.21174 0 4.2108e+06
```

## 1. 개요 

> [Ronny Restrepo](http://ronny.rest/tutorials/module/pointclouds_01/point_cloud_data/)

|The Point Cloud Data|이미지 데이터와 비교 |
|-|-|
|![](http://i.imgur.com/Bc13san.png)|![](http://i.imgur.com/smzFU5N.png)|



- 포인트 클라우드 데이터는 보통 `N x 3` Numpy 배열로 표현 된다. 
    - 각 N 줄은 하나의 점과 맵핑이 되며 
    - 3(x,y,z) 정보를 가지고 있다. 

- Lidar 센서에서 수집한 정보의 경우는 `reflectance`라는 정보가 추가되어 `N x 4` Numpy 배열이 된다. 
    - reflectance : 반사 시간 정보로 보면 된다. 


이미지 데이터
- 항상 양수 이다. 
- 기준점은 왼쪽 위부터 이다. 
- 좌표값은 정수(integer)이다. 

포인트 클라우드 데이터 
- 양수/음수 이다. 
- 좌표값은 real numbered이다. 
- The positive x axis represents forward.
- The positive y axis represents left.
- The positive z axis represents up.

## 2. Creating Birdseye View of Point Cloud Data

> 참고 : Height의 Level별 값 추출 (Height as Channels), [Creating Height Slices of Lidar Data](http://ronny.rest/blog/post_2017_03_27_lidar_height_slices/)

In order to create a birds eye view image, the relevant axes from the point cloud data will be the x and y axes.

![](http://i.imgur.com/cHsb48Y.png)

조심해야 할점 
- the x, and y axes mean the opposite thing.
- The x, and y axes point in the opposite direction.
- You have to shift the values across so that (0,0) is the smallest possible value in the image.


|- [Creating Birdseye View of Point Cloud Data 코드 및 설명(python)](http://ronny.rest/blog/post_2017_03_26_lidar_birds_eye/), [gist](https://gist.github.com/adioshun/12873804f472080c612e506310674797)|
|-|

> [참고] cpp로 작성한 코드 : [mjshiggins's github](https://github.com/mjshiggins/ros-examples)



## 3. Creating 360 degree Panoramic Views

- Project the `points in 3D` space into `cylindrical surface`

- LiDAR센서의 특징에 따라 설정값이 달라 진다. 
    - `h_res`: Horizontal resolution
    - `v_res`: vertical resolution

```python
# KTTI dataset = Velodyne HDL 64E 
## A vertical field of view of 26.9 degrees, at a resolution of 0.4 degree intervals. The vertical field of view is broken up into +2 degrees above the sensor, and -24.9 degrees below the sensor.
## A horizontal field of view of 360 degrees, at a resolution of 0.08 - 0.35 (depending on the rotation rate)
## Rotation rate can be selected to be betwen 5-20Hz.
## http://velodynelidar.com/docs/datasheet/63-9194%20Rev-E_HDL-64E_S3_Spec%20Sheet_Web.pdf

# Resolution and Field of View of LIDAR sensor
h_res = 0.35         # horizontal resolution, assuming rate of 20Hz is used 
v_res = 0.4          # vertical res
v_fov = (-24.9, 2.0) # Field of view (-ve, +ve) along vertical axis
v_fov_total = -v_fov[0] + v_fov[1] 
```

|- [Creating 360 degree Panoramic Views코드 및 설명(matplotlib)](http://ronny.rest/blog/post_2017_03_25_lidar_to_2d/)<br>- [Creating 360 degree Panoramic Views코드 및 설명(numpy)](http://ronny.rest/blog/post_2017_04_03_point_cloud_panorama/)|
|-|












