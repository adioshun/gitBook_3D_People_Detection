# Clustering 

> RoboND-Perception-Exercises/Exercise-2/ : [Euclidean Clustering with ROS and PCL](https://github.com/udacity/RoboND-Perception-Exercises/tree/master/Exercise-2)

## 1. ~~K-means~~

K-means clustering algorithm is able to group data points into n groups based on their distance to randomly chosen centroids. However, K-means clustering requires that you know the number of groups to be clustered, which may not always be the case.

> I have no idea about the **N**, So not suitable for usual case

## 2. Euclidean clustering (=DBSCAN)

Density-based spatial cluster of applications with noise

### 2.1 Definition 

creates clusters by grouping data points that are within some threshold **distance from their nearest neighbor**.


- pros.: Don't need to know how many clusters to expect in the data. 

- cons.: You do need to know something about the density of the data points being clustered.


### 2.2 Process

1. converting the XYZRGB point cloud to a XYZ point cloud, 
2. making a k-d tree (decreases the computation required), 
3. preparing the Euclidean clustering function, 
4. extracting the clusters from the cloud. 

This process generates a list of points for each detected object.

---

## 3. Clustering

비 지면 포인트들을 그룹화

> 상황에 따라 지표면에서의 높이가 2m 이하인 것들만 Clustering 대상으로 삼기도 한다. (사람은 일반적으로 2m 이하 )

### 3.1 A range search algorithm based on the euclidean distance

가정 :  dense points represents the same object


### 3.2 CCL(Connected Component Labeling)

CCL 알고리즘은 이미지를 일정한 구역들로 나누고 인접한 구역들끼리의 유사성을 판단하여 같은 label로 묶음
- OpenCV에 구현되어 있음??

논문[8]에서는 데카르트 좌표계에서 2차원 격자로 나눠 CCL을 적용



> 본 연구에서는 앞서 지면 추출 단계에서 생성된 2차원 극좌표 격자에 CCL을 적용하여 기존 결과를 재사용하는 방법을 취하였다. 센서특성에 더 적합한 방법이며, 재사용성을 이용해 메모리나 연산시간 면에서 더 효과적이다.

```
[8] H. Himmelsbach, Felix v. Hundelshausen and H.-J. Wuensche, “Fast Segmentation of 3D Point Clouds for Ground Vehicles,” Intelligent Vehicles Symposium, San Diego, CA, USA, June 2010.
```
