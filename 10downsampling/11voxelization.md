

```python
# Import PCL module
import pcl
'''
A Voxel Grid filter allows us to down sample the data by taking a spatial average of the points in the could confined by
each Voxel. We can adjust the sampling size by settings the Voxel size along each dimension. The set of points which lie
within the bounds of a Voxel are assigned to that Voxel and statistically combined into one output point.

 Generally downsampling to a smaller Voxel size retain more information about the original point cloud.

 After experimenting a bit, 0.01 is considered as a reasonable voxel(leaf) size for the tabletop.pcd data set.
'''


# Load Point Cloud file
cloud = pcl.load_XYZRGB('tabletop.pcd')


'''
Voxel Grid filter
'''
# Create a VoxelGrid filter object for our input point cloud
vox = cloud.make_voxel_grid_filter()

# Choose a voxel (also known as leaf) size
LEAF_SIZE = 0.01

# Set the voxel (or leaf) size
vox.set_leaf_size(LEAF_SIZE, LEAF_SIZE, LEAF_SIZE)

# Call the filter function to obtain the resultant downsampled point cloud
cloud_filtered = vox.filter()
filename = 'voxel_downsampled.pcd'
pcl.save(cloud_filtered, filename)


# Save pcd for tabletop objects
cloud_filtered = vox.filter()
filename = 'voxel_downsampled.pcd'
pcl.save(cloud_filtered, filename)



```