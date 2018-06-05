# Clustering 


To perform Euclidean Clustering, a k-d tree from the 'cloud_objects' point cloud needs to be constructed.

The k-d tree data structure is used in the Euclidian Clustering algorithm to decrease the computational burden of searching for neighboring points. While other efficient algorithms/data structures for nearest neighbor search exist, PCL's Euclidian Clustering algorithm only supports k-d trees.


 
## 1. K-means

정의 : an interative, unsupervised learning algorithm. K-means clustering algorithm is able to group data points into n groups based on their distance to randomly chosen centroids. 

제약 : However, K-means clustering requires that you know the number of groups to be clustered, which may not always be the case.


동작과정 
- Begin by randomly selecting k initial centroids, 
- then create k clusters by associate each data points with the nearest centroid. 

단점 : However, it may fail due to highly overlapping, complex shapes or highly sensitive to random initialization.

> 분류하려는 그룹의 수를 알고 이어야 가능 I have no idea about the **N**, So not suitable for usual case



## 2. DBSCAN(Density-based spatial cluster of applications with noise)




## 3 CCL(Connected Component Labeling)




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
