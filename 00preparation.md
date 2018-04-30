PCL 설치 : https://adioshun.gitbooks.io/system_setup/content/08a-pcl-setup.html

Sample PCD : [table_scene_lms400.pcd](https://github.com/PointCloudLibrary/data/blob/master/tutorials/table_scene_lms400.pcd)

## PCD 읽기

```python
import pcl
p = pcl.PointCloud()
p.from_file("table_scene_lms400.pcd")
fil = p.make_statistical_outlier_filter()
fil.set_mean_k (50)
fil.set_std_dev_mul_thresh (1.0)
fil.filter().to_file("inliers.pcd")
```


---

[Python_PCL API](http://strawlab.github.io/python-pcl/)


[ [RoboND_Sensors] brief course notes #robotics ](https://gist.github.com/kor01/84b4c1c590583533811781a9209f243e): By `kor01`


[Udacity Robotics Nanodegree](https://github.com/camisatx/RoboticsND)
- [install the Anaconda environment](https://github.com/camisatx/RoboticsND/blob/master/doc/configure_via_anaconda.md)
- [RANSAC.py](https://github.com/camisatx/RoboticsND/blob/master/projects/perception/Exercise-1/RANSAC.py)
- [voxel_grid_downsampling.py](https://github.com/fouliex/RoboND-Perception-Exercises/blob/master/Exercise-1/voxel_grid_downsampling.py): [Addiotional Code /w Sample](https://github.com/fouliex/RoboticPerception), [sample code](https://gist.github.com/kor01/84b4c1c590583533811781a9209f243e#file-009_voxel_grid_downsampling-py)


[Project: Perception Pick & Place](https://github.com/tony7126/perception_project)
: `Pipeline in depth` includes most of algorithms

[3D Robot Perception and Object Classification](https://www.haidynmcleod.com/3d-robot-perception)


[ParaView/PCL Plugin](https://www.paraview.org/Wiki/ParaView/PCL_Plugin)


[point cloud visualization with jupyter/pcl-python/and potree](https://www.youtube.com/watch?v=s2IvpYvB7Ew)
