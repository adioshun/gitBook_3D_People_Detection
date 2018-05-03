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
