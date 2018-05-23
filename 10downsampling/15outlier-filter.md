## 5. Outlier 필터 

- 노이즈 데이터 제거 

### 5.1 Statistical Outlier Filtering

Statistical Outlier Filtering is use to remove outlieres using number of neighboring points of 10 and standard deviation threshold of 0.001

```python 
# Statistical Outlier Filtering Code

def do_statistical_outlier_filtering(pcl_data,mean_k,tresh):
    '''
    :param pcl_data: point could data subscriber
    :param mean_k:  number of neighboring points to analyze for any given point
    :param tresh:   Any point with a mean distance larger than global will be considered outlier
    :return: Statistical outlier filtered point cloud data
    '''
    outlier_filter = pcl_data.make_statistical_outlier_filter()
    outlier_filter.set_mean_k(mean_k)
    outlier_filter.set_std_dev_mul_thresh(tresh)
    return outlier_filter.filter()

# Convert ROS msg to PCL data
cloud = ros_to_pcl(pcl_msg)

# Statistical Outlier Filtering
cloud = do_statistical_outlier_filtering(cloud,10,0.001)
```


Mithi 작성 코드 
```python
import pcl

##################################################################################
# This pipeline reduces the statistical noise of the scene 

# port of http://pointclouds.org/documentation/tutorials/statistical_outlier.php
# download http://svn.pointclouds.org/data/tutorials/table_scene_lms400.pcd

point_cloud = pcl.load("point_clouds/table_scene_lms400.pcd")

noise_filter = point_cloud.make_statistical_outlier_filter()

# Set the number of neighboring points to analyze for any given point
noise_filter.set_mean_k(50)

# Any point with a mean distance larger than global (mean distance+x*std_dev)
# will be considered outlier
noise_filter.set_std_dev_mul_thresh(1.0)

pcl.save(noise_filter.filter(), "point_clouds/table_scene_lms400_inliers.pcd")

noise_filter.set_negative(True)
pcl.save(noise_filter.filter(), "point_clouds/table_scene_lms400_outliers.pcd")
```
