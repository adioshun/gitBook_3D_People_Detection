# Clustering

## 1. EuclideanClustering


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



--- 

> 중요 : colorlist is a static variable you should define it outside the function getcolorlist, I defined it after if _name == ‘__main‘: get_color_list.color_list = []

```python

# Eucliean Cluster Extraction Code
def do_euclidean_clustering(white_cloud):
    '''
    :param cloud_objects:
    :return: cluster cloud and cluster indices
    '''
    tree = white_cloud.make_kdtree()

    # Create Cluster-Mask Point Cloud to visualize each cluster separately
    ec = white_cloud.make_EuclideanClusterExtraction()
    ec.set_ClusterTolerance(0.015)
    ec.set_MinClusterSize(50)
    ec.set_MaxClusterSize(20000)
    ec.set_SearchMethod(tree)
    cluster_indices = ec.Extract()
    cluster_color = get_color_list(len(cluster_indices))

    color_cluster_point_list = []

    for j, indices in enumerate(cluster_indices):
        for i, indice in enumerate(indices):
            color_cluster_point_list.append([white_cloud[indice][0],
                                             white_cloud[indice][1],
                                             white_cloud[indice][2],
                                             rgb_to_float(cluster_color[j])])

    cluster_cloud = pcl.PointCloud_PointXYZRGB()
    cluster_cloud.from_list(color_cluster_point_list)
    return cluster_cloud,cluster_indices
    
# Euclidean Clustering
get_color_list.color_list = []
white_cloud = pcl.load("sample_table.pcd") #white_cloud= XYZRGB_to_XYZ(cloud_objects)
white_cloud = XYZRGB_to_XYZ(white_cloud)
cluster_cloud,cluster_indices = do_euclidean_clustering(white_cloud)
```


> 중요 : colorlist is a static variable you should define it outside the function getcolorlist, I defined it after if _name == ‘__main‘: get_color_list.color_list = []


---


Yuchao's blogspot
```python
# Euclidean Clustering
white_cloud = XYZRGB_to_XYZ(white_cloud) # <type 'pcl._pcl.PointCloud'>
tree = white_cloud.make_kdtree() # <type 'pcl._pcl.KdTree'>
ec = white_cloud.make_EuclideanClusterExtraction()
ec.set_ClusterTolerance(0.02) # for hammer
ec.set_MinClusterSize(10)
ec.set_MaxClusterSize(250)
ec.set_SearchMethod(tree)
cluster_indices = ec.Extract() # indices for each cluster (a list of lists)
# Assign a color to each cluster
cluster_color = get_color_list(len(cluster_indices))
color_cluster_point_list = []
for j, indices in enumerate(cluster_indices):
  for i, indice in enumerate(indices):             
  color_cluster_point_list.append([white_cloud[indice][0], white_cloud[indice][1],  white_cloud[indice][2], rgb_to_float(cluster_color[j])])
# Create new cloud containing all clusters, each with unique color
cluster_cloud = pcl.PointCloud_PointXYZRGB()
cluster_cloud.from_list(color_cluster_point_list)
# publish to cloud
ros_cluster_cloud = pcl_to_ros(cluster_cloud)
# save to local
pcl.save(cluster_cloud, 'cluster.pcd')
```

분리수행

```python
# Euclidean Clustering
def euclid_cluster(cloud):
    white_cloud = XYZRGB_to_XYZ(cloud) # Apply function to convert XYZRGB to XYZ
    tree = white_cloud.make_kdtree()
    ec = white_cloud.make_EuclideanClusterExtraction()
    ec.set_ClusterTolerance(0.015)
    ec.set_MinClusterSize(20)
    ec.set_MaxClusterSize(3000)
    ec.set_SearchMethod(tree)
    cluster_indices = ec.Extract()

    return cluster_indices, white_cloud
    
def cluster_mask(cluster_indices, white_cloud):
    # Create Cluster-Mask Point Cloud to visualize each cluster separately
    #Assign a color corresponding to each segmented object in scene
    cluster_color = get_color_list(len(cluster_indices))

    color_cluster_point_list = []

    for j, indices in enumerate(cluster_indices):
        for i, indice in enumerate(indices):
            color_cluster_point_list.append([
                                            white_cloud[indice][0],
                                            white_cloud[indice][1],
                                            white_cloud[indice][2],
                                            rgb_to_float( cluster_color[j] )
                                           ])

    #Create new cloud containing all clusters, each with unique color
    cluster_cloud = pcl.PointCloud_PointXYZRGB()
    cluster_cloud.from_list(color_cluster_point_list)

    return cluster_cloud
```

