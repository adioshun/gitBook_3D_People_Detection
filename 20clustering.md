# Clustering 

[Octree-based region growing for point cloud segmentation](https://www.researchgate.net/publication/274645446_Octree-based_region_growing_for_point_cloud_segmentation): 2015
1. Model fitting-based methods
2. Region growing-based methods
3. Clustering feature-based methods


## 1. Model fitting-based methods(=parameter-based approach)



### 1.1 Hough Transform (HT) (Ballard, 1981) 

### 1.2 the Random sample consensus (RANSAC) approach proposed by Fischler and Bolles (1981). 

The HT is used to detect 
- planes (Vosselman et al., 2004), 
- cylinders (Tarsha-Kurdi,2007), 
- spheres (Rabbani and Heuvel, 2005). 

속도/신뢰성 향상을 위해 파라미터 선별시 여러 Step 으로 나누어 진행 `determined the parameters of the objects through several separate steps. `

For example, plane identification employed two steps: 
- (1) determination of the plane normal vector and 
- (2) establishment of the distance from the plane to the origin.



---

## 1. K-means

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

[clustering valuation(군집 모델 평가하기)](http://woolulu.tistory.com/50)

## 3. Tutorial (Series, )



## 4. Youtube



## 6. Material (Pdf, ppt)



## 7. Implementation (Project)


## 8. Research Group / Conference 


My best guess from your description is that you have 100 data streams, all providing x,y,z data from different sources?
What do you mean by 'cluster them'? Do you mean group data with similar x,y,x values? Or do want similar x,y,z values at a 'similar time' - in this case it is simply 4D data, time is just another dimension. Also, do you want hyper-elliptical clusters, or arbitary shapes of connected similairty?
Considerations:
You first have to consider what you mean by 'cluster'? What do you define as a cluster?
You then have to consider how many clusters you want? A fixed number, or an appropriate number based on the data spread or clusters of fixed sizes?
You then have to consider the importance of outliers - do you want to force them into the nearest custer, discard them or are they actually the most important data points?
Then consider when you want to cluster? Offline after data collection, on-line during data collection or at discreet time intervals?
If on-line how do you want the clusters to behave? To grow, move and be created based on all data so far or to fully evolve and allow clusters that are no longer receiving data to 'fade and die out'.
Depending on your answers to these questions different techniques are more appropriate, e.g.
```
Hierarchical - not really any use for anything except small, offline data sets as it takes waaaay too much memory
Subtractive - offline, finds an appropriate number of clusters, but slow
k-means - only useful if you have a fixed number of clusters and don't mind data being forced into clusters it doesn't belong in
DBScan - has a number of variants, offline will find arbitrary shaped clusters
ELM - online evolving hyper-elliptical clusters
My techniques:
DDC - very fast offline approach that will find an appropriate number of clusters
DDCAR - very fast offline and will find appropriate clusters with no user input whatsoever but is limited to hyper-spherical clusters
DDCAS - DDC adapted for arbitrary shaped clusters (unpublished as yet so you can't have any source code but I can run it for you)
CODAS - very fast, online and will find the required number of clusters and clusters of arbitrary shape (unpublished as yet so you can't have any source code but I can run it for you)
```

All of these are available for Matlab:
hierarchical, subtractive and k-means are built in
an implementation of DBScan is available online, I forget where
DDC & DDCAR I have almost finished tidying up to a releasable version
DDCAS & CODAS - I can run for you on test data
There are also various implementations of other technqieus around including Matlab Central File Exchange

---
# 비교 방법 
![](http://scikit-learn.org/stable/_images/sphx_glr_plot_cluster_comparison_001.png)
[Comparing different clustering algorithms on toy datasets](http://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html#sphx-glr-auto-examples-cluster-plot-cluster-comparison-py)
