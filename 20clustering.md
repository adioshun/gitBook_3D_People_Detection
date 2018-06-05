# Clustering 

> RoboND-Perception-Exercises/Exercise-2/ : [Euclidean Clustering with ROS and PCL](https://github.com/udacity/RoboND-Perception-Exercises/tree/master/Exercise-2)

참고자료 : [Clustering Algorithms:K-means and DBSCAN](https://docs.google.com/presentation/d/1o_rTjzkK7_q672rociNBu11R5dEDlACtrWrfR34FQ3s/edit#slide=id.p): ppt, Python코드 포함

 
## 1. K-means

정의 : an interative, unsupervised learning algorithm. K-means clustering algorithm is able to group data points into n groups based on their distance to randomly chosen centroids. 

제약 : However, K-means clustering requires that you know the number of groups to be clustered, which may not always be the case.


차이점 : 
 K Means나 Hierarchical 클러스터링의 경우 군집간의 거리를 이용하여 클러스터링을 하는 방법인데, 밀도 기반의 클러스터링은 점이 세밀하게 몰려 있어서 밀도가 높은 부분을 클러스터링 하는 방식이다. 

쉽게 설명하면, 어느점을 기준으로 반경 x내에 점이 n개 이상 있으면 하나의 군집으로 인식하는 방식이다.



출처: http://bcho.tistory.com/1205 [조대협의 블로그]


동작과정 
- Begin by randomly selecting k initial centroids, 
- then create k clusters by associate each data points with the nearest centroid. 

단점 : However, it may fail due to highly overlapping, complex shapes or highly sensitive to random initialization.

> 분류하려는 그룹의 수를 알고 이어야 가능 I have no idea about the **N**, So not suitable for usual case



## 2. DBSCAN(Density-based spatial cluster of applications with noise)


Euclidean clustering, or DBSCAN. 

전체 클러스터 수는 모르지만 일부 정보`(min_samples, max_dist)`를 알고 있을때 사용 `You may not know how many clusters to expect, but you do know something about how the points should be clustered (min_samples, max_dist). `

Each cluster has 3 levels of members: 
- core member, 
- edge member, 
- outlier.


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


## 4. EMST(Euclidean Minimum Spanning Tree) 

> 레이저스캐너 기반 도심 도로 환경 차량 인지/추적 알고리즘 개발, 김선욱, 서울대, 2017

```
Wang D.Z., Posner I., Newman P., 2012, "What could move? Finding cars, pedestrians and bicyclists in 3D laser data." Robotics and Automation (ICRA), IEEE International Conference.
```

## 5. RNNN(Redially Bounded nearest neighbor)


```
Lidar based real time multiple vehicle detection and tracking, Zhongzhen Luo, 2016
```

---

## 9. etc

비 지면 포인트들을 그룹화 

> 상황에 따라 지표면에서의 높이가 2m 이하인 것들만 Clustering 대상으로 삼기도 한다. (사람은 일반적으로 2m 이하 )


---

# Clustering 

## 1. List



## 2. Paper

- [Vehicle detection from airborne LiDAR point clouds based on a decision tree algorithm with horizontal and vertical features](https://www.tandfonline.com/doi/abs/10.1080/2150704X.2016.1278310?journalCode=trsl20): 엄준호교수, 2017, 필터링, 세그먼트 추출, OBPCA(Object-Based Point Cloud Analysis)기법


- [Fast Segmentation of 3D Point Clouds for Ground Vehicles](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=5548059): Himmelsbach2010,[깃허브_ROS](https://github.com/lorenwel/linefit_ground_segmentation)

- 레이저스캐너 기반 도심 도로 환경 차량 인지/추적 알고리즘 개발: 서울대 김선욱 2017, seonwook2017surrounding

## 3. Article (Post, blog, etc.)



## 3. Tutorial (Series, )



## 4. Youtube



## 6. Material (Pdf, ppt)



## 7. Implementation (Project)


## 8. Research Group / Conference 
