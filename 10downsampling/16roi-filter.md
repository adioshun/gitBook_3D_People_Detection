## 6. RoI 필터 

- 관심영역 정의 (eg. 센서로 부터 50m)
- cropping tool (When we have prior information about the location of a target)

### 6.1 Pass Through Filter

- Getting the region of interest 
- isolate the table and objects

The passthrough filter allows a 3D point cloud to be cropped by specifying an axis with cut-off values along that axis. The region allowed to pass through is often called the region of interest.

The PR2 robot simulation required passthrough filters for both the Y and Z axis (global). This prevented processing values outside the area immediately in front of the robot. For the Y axis, I used a range of -0.4 to 0.4, and for the Z axis, I used a range of 0.61 to 0.9.




```python 
# PassThrouh Filter Code
def do_passthrough(pcl_data,filter_axis,axis_min,axis_max):
    '''
    Create a PassThrough  object and assigns a filter axis and range.
    :param pcl_data: point could data subscriber
    :param filter_axis: filter axis
    :param axis_min: Minimum  axis to the passthrough filter object
    :param axis_max: Maximum axis to the passthrough filter object
    :return: passthrough on point cloud
    '''
    passthrough = pcl_data.make_passthrough_filter()
    passthrough.set_filter_field_name(filter_axis)
    passthrough.set_filter_limits(axis_min, axis_max)
    return passthrough.filter()
    
# Convert ROS msg to PCL data
cloud = ros_to_pcl(pcl_msg)

# PassThrough Filter
filter_axis ='z'
axis_min = 0.44
axis_max =0.85
cloud = do_passthrough(cloud,filter_axis,axis_min,axis_max)

filter_axis = 'x'
axis_min = 0.33
axis_max = 1.0
cloud = do_passthrough(cloud, filter_axis, axis_min, axis_max)    
```


Mithi 작성 코드 

```python 
# Returns only the point cloud information at a specific range of a specific axis
def do_passthrough_filter(point_cloud, name_axis = 'z', min_axis = 0.6, max_axis = 1.1):
  pass_filter = point_cloud.make_passthrough_filter()
  pass_filter.set_filter_field_name(name_axis);
  pass_filter.set_filter_limits(min_axis, max_axis)
  return pass_filter.filter()

# Load the point cloud in memory
cloud = pcl.load_XYZRGB('point_clouds/tabletop.pcd')

# Get only information in our region of interest, as we don't care about the other parts
filtered_cloud = do_passthrough_filter(point_cloud = cloud, 
                                    name_axis = 'z', min_axis = 0.6, max_axis = 1.1)
```

