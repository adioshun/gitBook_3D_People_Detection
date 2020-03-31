# 5\_Integral Images

> [적분 영상\(integral image\)](http://kugistory.net/4)이란 쉽게 말해서 다음 픽셀에 이전 픽셀까지의 합이 더해진 영상이다, 적분 영상의 장점은 특정 영역의 픽셀 값의 총합을 매우 쉽게 구할 수 있다  
> ![](https://i.imgur.com/gkpzpvx.png)

> 2016-Monocular 3D Object Detection for Autonomous Driving, Xiaozhi Chen에서도 Feature로 사용







### Normal Estimation 



```cpp
#include <pcl/io/io.h>
#include <pcl/io/pcd_io.h>
#include <pcl/features/integral_image_normal.h>
#include <pcl/visualization/cloud_viewer.h>

// Normal Estimation Using Integral Images
// http://pointclouds.org/documentation/tutorials/normal_estimation_using_integral_images.php


int
main ()
{
     // load point cloud
     pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
     pcl::io::loadPCDFile ("table_scene_mug_stereo_textured.pcd", *cloud);

     // estimate normals
     pcl::PointCloud<pcl::Normal>::Ptr normals (new pcl::PointCloud<pcl::Normal>);

     pcl::IntegralImageNormalEstimation<pcl::PointXYZ, pcl::Normal> ne;
     ne.setNormalEstimationMethod (ne.AVERAGE_3D_GRADIENT);
     ne.setMaxDepthChangeFactor(0.02f);
     ne.setNormalSmoothingSize(10.0f);
     ne.setInputCloud(cloud);
     ne.compute(*normals);

     // visualize normals
     pcl::visualization::PCLVisualizer viewer("PCL Viewer");
     viewer.setBackgroundColor (0.0, 0.0, 0.5);
     viewer.addPointCloudNormals<pcl::PointXYZ,pcl::Normal>(cloud, normals);

     while (!viewer.wasStopped ())
     {
     viewer.spinOnce ();
     }
     return 0;
}
```



