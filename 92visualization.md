# Visualization 

## 1. PCD

[샘플 PCD Download](https://github.com/PointCloudLibrary/data/blob/master/tutorials/table_scene_lms400.pcd), [PCD File Format](http://www.jeffdelmerico.com/wp-content/uploads/2014/03/pcl_tutorial.pdf): slide 12

### 1.1 PCL viewer

`/usr/bin/pcl_viewer/{  }.pcd`

> [PCL Visualization overview](http://pointclouds.org/documentation/overview/visualization.php), [Youtube](https://www.youtube.com/watch?v=BZBQXcBvHW0)



### 1.2 Jupyter

[point cloud visualization with jupyter/pcl-python/and potree](https://www.youtube.com/watch?v=s2IvpYvB7Ew)


### 1.3 Mayavi 이용 

[Mayavi 홈페이지](http://docs.enthought.com/mayavi/mayavi/)


설치 
- 설치 스크립트 : [Install Mayavi on Ubuntu](https://gist.github.com/ronrest/d778ee5d49c026ccee1dbec6bd5b3988)
- 패키지 설치 : 
    - python2 : `conda install -c anaconda mayavi`
    - python3 : `- conda install -c clinicalgraphics vtk=7.1.0; pip install mayavi`


> ImportError: Could not import backend for traits 
> - conda install -c conda-forge pyside=1.2.4 
> - {OR} conda install pyqt=4




```bash
sudo apt-get install vtk6 python-vtk
python -c "import vtk"
# cp -r /usr/lib/python2.7/dist-packages/vtk /opt/anaconda3/envs/python2_gpu/lib/python2.7/site-packages/
pip install mayavi
import mayavi.mlab as mlab
```

실행 코드 

```python
# ==============================================================================
#                                                                     VIZ_MAYAVI
# Input : kitti Raw Dataset 
# ==============================================================================
def viz_mayavi(points, vals="distance"):
    x = points[:, 0]  # x position of point
    y = points[:, 1]  # y position of point
    z = points[:, 2]  # z position of point
    # r = lidar[:, 3]  # reflectance value of point
    d = np.sqrt(x ** 2 + y ** 2)  # Map Distance from sensor

    # Plot using mayavi -Much faster and smoother than matplotlib
    import mayavi.mlab

    if vals == "height":
        col = z
    else:
        col = d

    fig = mayavi.mlab.figure(bgcolor=(0, 0, 0), size=(640, 360))
    mayavi.mlab.points3d(x, y, z,
                         col,          # Values used for Color
                         mode="point",
                         colormap='spectral', # 'bone', 'copper', 'gnuplot'
                         # color=(0, 1, 0),   # Used a fixed (r,g,b) instead
                         figure=fig,
                         )
    mayavi.mlab.show()
```


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

> Simple Version : [Cloud Viewer](http://pointclouds.org/documentation/tutorials/cloud_viewer.php#cloud-viewer)



### 1.6 Matplotlib이용



- 설치가 쉽지만, 느리고 3D를 충분히 표현하지 못한다. [[KITTI Data Demo]](https://github.com/hunjung-lim/awesome-vehicle-datasets/blob/master/vehicle/kitti/KITTI%2BDataset%2BExploration.ipynb)


```python
from mpl_toolkits.mplot3d import Axes3D

f2 = plt.figure()
ax2 = f2.add_subplot(111, projection='3d')
# Plot every 100th point so things don't get too bogged down
velo_range = range(0, third_velo.shape[0], 100)
ax2.scatter(third_velo[velo_range, 0],
            third_velo[velo_range, 1],
            third_velo[velo_range, 2],
            c=third_velo[velo_range, 3],
            cmap='gray')
ax2.set_title('Third Velodyne scan (subsampled)')

plt.show()
```




In order to prevent matplotlib from crashing your computer, it is recomended to only view a subset of the point cloud data. 

For instance, if you are visualizing LIDAR data, then you may only want to view one in every 25-100 points. Below is some sample code to get you started.

```python
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

skip = 100   # Skip every n points

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')
point_range = range(0, points.shape[0], skip) # skip points to prevent crash
ax.scatter(points[point_range, 0],   # x
           points[point_range, 1],   # y
           points[point_range, 2],   # z
           c=points[point_range, 2], # height data for color
           cmap='spectral',
           marker="x")
ax.axis('scaled')  # {equal, scaled}
plt.show()
```
























