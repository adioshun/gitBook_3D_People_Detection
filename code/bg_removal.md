# 배경 제거 

## 1. 베이스라인 생성 

### 1.1 pickle로 생성 

```python 

import sensor_msgs.point_cloud2 as pc2
import numpy as np
import rosbag
import os 
import pickle
import argparse


# ROSBAG SETTINGS
#data_dir = "/workspace/_rosbag/"
#bag_name = 'cafeteria_baseline_2018-11-16-03-24-16.bag'
#bag_file = os.path.join(data_dir, bag_name)


parser = argparse.ArgumentParser()
parser.add_argument("--baseline", help="Use baseline rosbag to remove background")

args = parser.parse_args()

bag_file = args.baseline



bag = rosbag.Bag(bag_file, "r")
print("")
print("Load rosbag : {}".format(bag_file))

messages = bag.read_messages(topics=['/velodyne_points'])
n_lidar = bag.get_message_count(topic_filters=["/velodyne_points"])


baseline = np.zeros((0,5),dtype=np.float32)




for i in range(50):#n_lidar):
    # READ NEXT MESSAGE IN BAG
    topic, msg, t = messages.next()  #print(dir(msg))

    # CONVERT MESSAGE TO A NUMPY ARRAY OF POINT CLOUDS
    # creates a Nx5 array: [x, y, z, reflectance, ring]
    lidar = pc2.read_points(msg)
    lidar = np.array(list(lidar),  dtype=np.float32)
    baseline = np.concatenate((baseline, lidar),axis=0)
    print("ID : {}-{}".format(i, baseline.shape[0]))

baseline = baseline[:,0:3]
#save_file = bag_name + ".npy"
#np.save(save_file, lidar)

save_file = bag_file + ".pkl"
output = open(save_file, 'wb')
pickle.dump(baseline, output)
output.close()

print("")
print("File saved : {}".format(save_file))
print("")


```

## 2. 배경 제거 

```python 

#!/usr/bin/env python3
# coding: utf-8

import sys
sys.path.append("/workspace/include")

from ddynamic_reconfigure import DDynamicReconfigure 
from ddynamic_reconfigure import DyConfigure

import rospy
from sensor_msgs.msg import PointCloud2
import sensor_msgs.point_cloud2 as pc2

import numpy as np
import pcl
import pcl_msg

import pcl_helper
import filter_helper
import time

import argparse

def background_removal(daytime, nighttime):

    resolution = 1.0#0.8  # 값이 커지면 missing 존재, noise도 존재 
    #배경 포인트 
    octree = nighttime.make_octreeChangeDetector(resolution)
    octree.add_points_from_input_cloud ()
    octree.switchBuffers () #Switch buffers and reset current octree structure.

    # 입력 포인트 #cloudB cloudA
       
    daytime = pcl_helper.XYZRGB_to_XYZ(daytime)

    octree.set_input_cloud(daytime)
    octree.add_points_from_input_cloud ()
    newPointIdxVector = octree.get_PointIndicesFromNewVoxels ()
    daytime.extract(newPointIdxVector)


    #배경 제거 포인트 
    result = np.zeros((len(newPointIdxVector)+1, 3), dtype=np.float32)
    for i in range(0, len(newPointIdxVector)):
        result[i][0] = daytime[newPointIdxVector[i]][0]
        result[i][1] = daytime[newPointIdxVector[i]][1]
        result[i][2] = daytime[newPointIdxVector[i]][2]
    #print("Shape of Result : {}".format(result.shape))

    pc = pcl.PointCloud(result)
    
    # 노이즈 제거 
    #pc = filter_helper.do_statistical_outlier_filtering(pc,10,0.01) #(pc,10,0.001)


    cloud = pcl_helper.XYZ_to_XYZRGB(pc,[255,255,255])
    
    return cloud











def callback(input_ros_msg):
    
    pcl_xyzrgb = pcl_helper.ros_to_pcl(input_ros_msg) #ROS 메시지를 PCL로 변경
    pcl_xyz = pcl_helper.XYZRGB_to_XYZ(pcl_xyzrgb)

    pcl_xyzrgb = background_removal(pcl_xyz, searchPoint)
    bg_ros_msg = pcl_helper.pcl_to_ros(pcl_xyzrgb) #PCL을 ROS 메시지로 변경 
    pub_bg = rospy.Publisher("/velodyne_bg", PointCloud2, queue_size=1)
    pub_bg.publish(bg_ros_msg)




if __name__ == "__main__":

    rospy.init_node('node_bg_removal', anonymous=True)
    E = DyConfigure()
    #leaf_size = E.leaf_size
    leaf_size = 0.5  #0.5는 missing은 없지만, 잔상 존재, 0.1은 잔상은 없지만 missing존재 0.8은 속도 저하 

    parser = argparse.ArgumentParser()
    parser.add_argument("--baseline", help="Use baseline rosbag to remove background")

    args = parser.parse_args()
    if args.baseline:
        import pickle
        print("serchPointNP Loaded : {}".format(args.baseline))
        pkl_file = open(args.baseline, 'rb')   
        arr = pickle.load(pkl_file)

        searchPoint = pcl.PointCloud()
        
        searchPoint.from_array(arr)   
        print("Normal Baseline point {}".format(searchPoint))
        searchPoint = filter_helper.do_voxel_grid_downssampling(searchPoint,leaf_size)
        print("Voxeled Basedline point {}".format(searchPoint))

 
    
    rospy.Subscriber('/velodyne_points', PointCloud2, callback) 
    rospy.spin()
```
