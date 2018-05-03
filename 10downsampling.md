## 1. statistical outlier filter
- Reducing noise 

These outliers are often caused by external factors like dust in the environment, humidity in the air, or presence of various light sources. One such filter is PCL's StatisticalOutlierRemoval filter that computes the distance to each point's neighbors, then calculates a mean distance. Any points whose mean distances are outside a defined interval are removed.


For the PR2 simulation, I found that a mean k value of 20 and a standard deviation threshold of 0.1 provided the optimal outlier filtering. Here is the cloud after performing the outlier removal filter.

## 2. Voxel Grid Filter
- Downsampling 

A voxel grid filter downsamples the data by taking a spatial average of the points in the cloud confined by each voxel. The set of points which lie within the bounds of a voxel are assigned to that voxel and are statistically combined into one output point.

I used an X, Y, and Z voxel grid filter leaf size of 0.01. This was a good compromise of leaving enough detail while minimizing processing time.



## 3. passthrough filter(= ROI Filter)

- Getting the region of interest 
- isolate the table and objects

The passthrough filter allows a 3D point cloud to be cropped by specifying an axis with cut-off values along that axis. The region allowed to pass through is often called the region of interest.

The PR2 robot simulation required passthrough filters for both the Y and Z axis (global). This prevented processing values outside the area immediately in front of the robot. For the Y axis, I used a range of -0.4 to 0.4, and for the Z axis, I used a range of 0.61 to 0.9.


## 4. RANSAC Plane Fitting
- Segmentation of the table from everything else 
- to identify the table.


Random Sample Consensus (RANSAC) is used to identify points in the dataset that belong to a particular model. It assumes that all of the data in a dataset is composed of both inliers and outliers, where inliers can be defined by a particular model with a specific set of parameters, and outliers don't.

I used a RANSAC max distance value of 0.01.


## ?? ExtractIndices Filter
- To create new point clouds containing the table and objects separately







> [Python Sample Code](https://github.com/mithi/point-cloud-filter)







```python

# Import PCL module
import pcl

# Load Point Cloud file
cloud = pcl.load_XYZRGB('tabletop.pcd')


# Voxel Grid filter


# PassThrough filter


# RANSAC plane segmentation


# Extract inliers

# Save pcd for table
# pcl.save(cloud, filename)


# Extract outliers


# Save pcd for tabletop objects

```