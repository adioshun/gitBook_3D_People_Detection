# Clustering 

## 1. List



## 2. Paper

- [Vehicle detection from airborne LiDAR point clouds based on a decision tree algorithm with horizontal and vertical features](https://www.tandfonline.com/doi/abs/10.1080/2150704X.2016.1278310?journalCode=trsl20): 엄준호교수, 2017, 필터링, 세그먼트 추출, OBPCA(Object-Based Point Cloud Analysis)기법


- [Fast Segmentation of 3D Point Clouds for Ground Vehicles](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=5548059): Himmelsbach2010,[깃허브_ROS](https://github.com/lorenwel/linefit_ground_segmentation)

- 레이저스캐너 기반 도심 도로 환경 차량 인지/추적 알고리즘 개발: 서울대 김선욱 2017, seonwook2017surrounding

- [A Survey of Clustering With Deep Learning: From the Perspective of Network Architecture](https://www.semanticscholar.org/paper/A-Survey-of-Clustering-With-Deep-Learning%3A-From-the-Min-Guo/d9e9ef4c91134a90704f2fe0722fbec8995734ab): 2018

- [Learning Neural Models for End-to-End Clustering](https://www.semanticscholar.org/paper/Learning-Neural-Models-for-End-to-End-Clustering-Meier-Elezi/54a9ed950458f4b7e348fa78a718657c8d3d0e05):2018


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
