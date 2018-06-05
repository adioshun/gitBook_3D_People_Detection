# Clustering

## 1. Euclidean Clustering


Euclidean clustering, or DBSCAN. 

전체 클러스터 수는 모르지만 일부 정보`(min_samples, max_dist)`를 알고 있을때 사용 `You may not know how many clusters to expect, but you do know something about how the points should be clustered (min_samples, max_dist). `



Each cluster has 3 levels of members: 
- core member, 
- edge member, 
- outlier.

차이점 : 
- K Means나 Hierarchical 클러스터링의 경우 군집간의 거리를 이용하여 클러스터링을 하는 방법인데, 
- 밀도 기반의 클러스터링은 점이 세밀하게 몰려 있어서 밀도가 높은 부분을 클러스터링 하는 방식이다. 
    - 쉽게 설명하면, 어느점을 기준으로 반경 x내에 점이 n개 이상 있으면 하나의 군집으로 인식하는 방식이다.

###### 예 : minPts = 4, 반경 = epsilon

|![image](https://user-images.githubusercontent.com/17797922/40961916-f2976c78-68de-11e8-9696-aff088b189ce.png)|![image](https://user-images.githubusercontent.com/17797922/40962055-6e81f6fa-68df-11e8-9617-4846be50bfec.png)|![image](https://user-images.githubusercontent.com/17797922/40962080-7ff69e7c-68df-11e8-8ca7-163465efa6ea.png)|
|-|-|-|
|p=코어포인트<br>. 반경안에 점 4개 존재|P2=경계점(Border point)<br>. 반경안에 점 4개 없음<br>. But, Core point에는 속함|P4=Noise Point<br>. 반경안에 점 4개 없음 <br>.And, Core point에 속하지 않음<br>  (어느군집에도 안속함) |
|![image](https://user-images.githubusercontent.com/17797922/40962067-744f3674-68df-11e8-8602-67df0a739c69.png)|![image](https://user-images.githubusercontent.com/17797922/40962073-7afb11d2-68df-11e8-8b3a-81ad25a242fe.png)|![image](https://user-images.githubusercontent.com/17797922/40961898-e1623212-68de-11e8-8cec-c20eaf8bb93b.png)|
|P=코어포인트<br> P3 = 코어포인트|두 코어 포인트를 연결하여 <br> 하나의 군집으로 처리|**정리**|








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







To perform Euclidean Clustering, a k-d tree from the 'cloud_objects' point cloud needs to be constructed.

The k-d tree data structure is used in the Euclidian Clustering algorithm to decrease the computational burden of searching for neighboring points. While other efficient algorithms/data structures for nearest neighbor search exist, PCL's Euclidian Clustering algorithm only supports k-d trees.

```
[3] M. Ester, H.-P. Kriegel, J. Sander, X. Xu, and others. A density-based algorithm for discovering clusters in large spatial databases with noise. In Kdd, volume 96, pages 226–231, 1996.
```



