```python
#!/usr/bin/env python3
# coding: utf-8

import sys
sys.path.append("/workspace/include")

import rospy
from sensor_msgs.msg import PointCloud2
import sensor_msgs.point_cloud2 as pc2

import numpy as np
import pcl
import pcl_msg



import pcl_helper
import filter

def roi_filter(cloud):
    filter_axis = 'x'
    axis_min = 1.0
    axis_max = 20.0
    cloud = filter.do_passthrough(cloud, filter_axis, axis_min, axis_max)

    filter_axis = 'y'
    axis_min = -7.0
    axis_max = 5.5
    cloud = filter.do_passthrough(cloud, filter_axis, axis_min, axis_max)

    filter_axis = 'z'
    axis_min = -1.2
    axis_max = 10.0
    #cloud = filter.do_passthrough(cloud, filter_axis, axis_min, axis_max)
    return cloud


def callback(input_pcl_msg):
    
    pcl_data = pcl_helper.ros_to_pcl(input_pcl_msg) #ROS 메시지를 PCL로 변경
    roi_pcl = roi_filter(pcl_data) # 탐지 영역 설정 
    pcl_XYZRGB = pcl_helper.XYZ_to_XYZRGB(roi_pcl, pcl_helper.random_color_gen()) #PCL_XYZ를 PCL_XYZRGB로 변경



    #cloud = np.asarray(pcl_data)
    #print("3. cloud : {}".format(type(cloud)))


    output_msg = pcl_helper.pcl_to_ros(pcl_XYZRGB) #PCL을 ROS 메시지로 변경 
    pub = rospy.Publisher("/pcl_objects", PointCloud2, queue_size=1)
    pub.publish(output_msg)

if __name__ == "__main__":
    rospy.init_node('myopen3d_node', anonymous=True)
    rospy.Subscriber('velodyne_points',
                     PointCloud2, callback)

    rospy.spin()
    
```