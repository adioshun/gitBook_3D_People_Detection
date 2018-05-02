# Visualization 

## 1. PCD

[샘플 PCD파일](https://raw.github.com/PointCloudLibrary/data/master/tutorials/table_scene_lms400.pcd)



### 1.1 PCL viewer


### 1.2 Jupyter

[point cloud visualization with jupyter/pcl-python/and potree](https://www.youtube.com/watch?v=s2IvpYvB7Ew)



### 1.4 Paraview 

[ParaView/PCL Plugin](https://www.paraview.org/Wiki/ParaView/PCL_Plugin)
- apt-get install paraview


### 1.5 3D Viewer

> [How to visualize a range image](http://pointclouds.org/documentation/tutorials/range_image_visualization.php#range-image-visualization)


Code Download : [range_image_visualization.cpp](https://gist.githubusercontent.com/adioshun/1ae4197af17f79f01f1ec3ec7c8f4bcb/raw/6ec7e03db63f0477688a66ae580c14795dec0803/range_image_visualization.cpp), [CMakeLists.txt](https://gist.githubusercontent.com/adioshun/1ae4197af17f79f01f1ec3ec7c8f4bcb/raw/6ec7e03db63f0477688a66ae580c14795dec0803/CMakeLists.txt)



```
mkdir range_image_visualization; cd range_image_visualization
vi range_image_visualization.cpp
vi CMakeLists.txt
mkdir build;cd build
cmake ..
make

# Test
wget https://raw.github.com/PointCloudLibrary/data/master/tutorials/table_scene_lms400.pcd
./range_image_visualization table_scene_lms400.pcd 

```

---

## 2. 활용

예제

```python
import pcl
import numpy as np
p = pcl.PointCloud(np.array([[1, 2, 3], [3, 4, 5]], dtype=np.float32))
seg = p.make_segmenter()
seg.set_model_type(pcl.SACMODEL_PLANE)
seg.set_method_type(pcl.SAC_RANSAC)
indices, model = seg.segment()
```

# 시각화 툴

## PCD 시각화 \(Jupyter + Potree\)

* [point cloud visualization with jupyter/pcl-python/and potree](https://www.youtube.com/watch?v=s2IvpYvB7Ew) : Youtube

[PCD File Format](http://www.jeffdelmerico.com/wp-content/uploads/2014/03/pcl_tutorial.pdf): slide 12

[ROS/pcl/Tutorials](http://wiki.ros.org/pcl/Tutorials)



# PCD Viewer

`/usr/bin/pcl_viewer/{  }.pcd`

