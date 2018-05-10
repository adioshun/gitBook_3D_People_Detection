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


```python

# Import PCL module
import pcl

# Load Point Cloud file
cloud = pcl.load_XYZRGB('tabletop.pcd')

##############################################################################
# Voxel Grid filter
# Downsamples the data by taking a spatial average of the points in the cloud
#   confined by each voxel. The set of points which lie within the bounds of
#   a voxel are assigned to that voxel and are statistically combined into
#   one output point.

# Create a Voxel Grid filter object for the input point cloud
vox = cloud.make_voxel_grid_filter()

# Choose a voxel size (also known as leaf size) of n cubic meters per voxel
LEAF_SIZE = 0.01

# Set the voxel (or leaf) size; can adjust size along each dimension
vox.set_leaf_size(LEAF_SIZE, LEAF_SIZE, LEAF_SIZE)

# Call the filter function to obtain the resultant downsampled point cloud
cloud_filtered = vox.filter()
filename = 'voxel_downsampled.pcd'
pcl.save(cloud_filtered, filename)

##############################################################################
# PassThrough filter
# Allows a 3D point cloud to be cropped by specifying an axis with cut-off
#   values along that axis. The region allowed to 'pass through' is often
#   called the 'region of interest.'

# Create a pass through filter objeect
passthrough = cloud_filtered.make_passthrough_filter()

# Assign axis and range to the passthrough filter object
filter_axis = 'z'
passthrough.set_filter_field_name(filter_axis)
axis_min = 0.6
axis_max = 1.1
passthrough.set_filter_limits(axis_min, axis_max)

# Use the filter function to obtain the resultant point cloud
cloud_filtered = passthrough.filter()
filename = 'pass_through_filtered.pcd'
pcl.save(cloud_filtered, filename)

##############################################################################
# RANSAC plane segmentation
# Random Sample Consensus (RANSAC) is used to identify points in the dataset
#   that belong to a particular model. It assumes that all of the data in a
#   dataset is composed of both inliers and outliers, where inliers can be
#   defined by a particular model with a specific set of parameters, where
#   outliers don't and can be discarded.

# Create the segmentation object
seg = cloud_filtered.make_segmenter()

# Set the model to be fitted
seg.set_model_type(pcl.SACMODEL_PLANE)
seg.set_method_type(pcl.SAC_RANSAC)

# Max distance for a point to be considered fitting the model
max_distance = 0.01
seg.set_distance_threshold(max_distance)

# Call segment function to obtain set of inlier indices and model coefficients
inliers, coefficients = seg.segment()

##############################################################################
# Extract inliers
# Extract the points from the point cloud from using the inliers list of
#   indicies. This will isolate the table from the point cloud.

# Setting the negative flag to False filters for the table
extracted_inliers = cloud_filtered.extract(inliers, negative=False)
filename = 'extracted_inliers.pcd'

# Save pcd for table
pcl.save(extracted_inliers, filename)


# Extract outliers
# Setting the negative flag to True filters for the objects
extracted_outliers = cloud_filtered.extract(inliers, negative=True)
filename = 'extracted_outliers.pcd'

# Save pcd for tabletop objects
pcl.save(extracted_outliers, filename)

##############################################################################
# Outlier removal filter
# Outlier filters can remove outliers from the point cloud. These outliers are
#   often caused by external factors like dust in the environment, humidity in
#   the air, or presence of various light sources. One such filter is
#   PCL's StaticsticalOutlierRemoval fileter that computes the distance to
#   each point's neighbors, then calculates a mean distance. Any points whose
#   mean distances are outside a defined interval are removed.

## Create a filter object
#outlier_filter = cloud_filtered.make_statistical_outlier_filter()
#
## Set the number of neighboring points to analyze for any given point
#outlier_filter.set_mean_k(50)
#
## Any point with a mean distance larger than global (mean distance + x*std_dev)
##   will be considered an outlier
#outlier_filter.set_std_dev_mul_thresh(1.0)
#
## Call the filter function
#outlier_removal_filter = outlier_filter.filter()
#filename = 'outlier_removal_filter.pcd'
#pcl.save(outlier_removal_filter, filename)


```


# Downsampling

## 1. List



## 2. Paper



## 3. Article (Post, blog, etc.)

- [CNN for Very Fast Ground Segmentation in Velodyne LiDAR Data](https://arxiv.org/abs/1709.02128): ICRA 2018

## 3. Tutorial (Series, )



## 4. Youtube



## 6. Material (Pdf, ppt)



## 7. Implementation (Project)


## 8. Research Group / Conference 
