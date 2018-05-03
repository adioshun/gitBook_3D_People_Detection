## Voxel Grid Filter
- Downsampling 


## passthrough filter

- Getting the region of interest 
- isolate the table and objects

## RANSAC Plane Fitting
- Segmentation of the table from everything else 
- to identify the table.

## statistical outlier filter
- Reducing noise 


## ExtractIndices Filter
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