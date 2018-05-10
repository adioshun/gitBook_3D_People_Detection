# Clustering 

> RoboND-Perception-Exercises/Exercise-2/ : [Euclidean Clustering with ROS and PCL](https://github.com/udacity/RoboND-Perception-Exercises/tree/master/Exercise-2)

## 1. ~~K-means~~

K-means clustering algorithm is able to group data points into n groups based on their distance to randomly chosen centroids. However, K-means clustering requires that you know the number of groups to be clustered, which may not always be the case.

> I have no idea about the **N**, So not suitable for usual case

## 2. DBSCAN(Density-based spatial cluster of applications with noise)

> Euclidean clustering 의 한 종류?

```
[3] M. Ester, H.-P. Kriegel, J. Sander, X. Xu, and others. A density-based algorithm for discovering clusters in large spatial databases with noise. In Kdd, volume 96, pages 226–231, 1996.
```

It is a density-based algorithm designed on the concepts of density reachability and density-connection:
1. density-reachable: a point p is density reachable from a point q if there is a chain of points p1, ..., pn, p1 = p, pn = q such that pi+1 is directly density-reachable from pi. A
point p is directly density-reachable if the point p is included
in the area defined by a circle centered on q of radius
EPS.
2. density-connected: a point p is density connected to a point q if there is a point o such that both, p and q are density-reachable from o.

The algorithm visits all points once and for each p aggregates all density-reachable points according to the parameters EPS and MinPts. 

MinPts defines the minimum number of points that a cluster should contain, otherwise the group is considered as noise. 

EPS is a parameter that defines the maximum allowed distance between two density-reachable points. 

By projecting the clusters into the image space, we generate the candidates for detection (see Figure 3a).




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



## 3 CCL(Connected Component Labeling)

CCL 알고리즘은 이미지를 일정한 구역들로 나누고 인접한 구역들끼리의 유사성을 판단하여 같은 label로 묶음
- OpenCV에 구현되어 있음??

논문[8]에서는 데카르트 좌표계에서 2차원 격자로 나눠 CCL을 적용



> 본 연구에서는 앞서 지면 추출 단계에서 생성된 2차원 극좌표 격자에 CCL을 적용하여 기존 결과를 재사용하는 방법을 취하였다. 센서특성에 더 적합한 방법이며, 재사용성을 이용해 메모리나 연산시간 면에서 더 효과적이다.

```
[8] H. Himmelsbach, Felix v. Hundelshausen and H.-J. Wuensche, “Fast Segmentation of 3D Point Clouds for Ground Vehicles,” Intelligent Vehicles Symposium, San Diego, CA, USA, June 2010.
```
---

## 9. etc

비 지면 포인트들을 그룹화 

> 상황에 따라 지표면에서의 높이가 2m 이하인 것들만 Clustering 대상으로 삼기도 한다. (사람은 일반적으로 2m 이하 )


---

# Clustering 

## 1. List



## 2. Paper

- [OBPCA(Object-Based Point Cloud Analysis)](https://www.tandfonline.com/doi/abs/10.1080/2150704X.2016.1278310?journalCode=trsl20): 엄준호교수, 2017, 필터링, 세그먼트 추출
기법


- [Fast Segmentation of 3D Point Clouds for Ground Vehicles](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=5548059): Himmelsbach2010,[깃허브_ROS](https://github.com/lorenwel/linefit_ground_segmentation)

## 3. Article (Post, blog, etc.)



## 3. Tutorial (Series, )



## 4. Youtube



## 6. Material (Pdf, ppt)



## 7. Implementation (Project)


## 8. Research Group / Conference 
