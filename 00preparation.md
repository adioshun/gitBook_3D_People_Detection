PCL 설치 : https://adioshun.gitbooks.io/system_setup/content/08a-pcl-setup.html



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