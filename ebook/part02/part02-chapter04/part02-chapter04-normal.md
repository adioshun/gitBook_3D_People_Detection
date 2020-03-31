# 2\_Normal

### 1. Normal Estimation



{% embed url="https://github.com/adioshun/gitBook\_PCL/blob/master/Tutorial/Feature/Normal-Estimation.md" %}

[https://github.com/adioshun/gitBook\_PCL/blob/master/Tutorial/Feature/Estimating-surface-normals-in-a-pointcloud.md](https://github.com/adioshun/gitBook_PCL/blob/master/Tutorial/Feature/Estimating-surface-normals-in-a-pointcloud.md)

점군에서 구할수 있는 Feature중에 가장 간단한 Surface Normal에 대하여 살펴 보도록 하겠습니다. 먼저 Normal은 삼차원 공간에서는 공간에 있는 평면 위의 한 점을 지나면서 그 평면에 수직인 직선을 의미합니다. Normal은 크게 꼭지점 법\(Vertex Normals\)과 평면 법선\(Face/surface Normals\)로 나누어 집니다. 여기서는 평면 법선만을 다루고 있으며, 줄여서 Normal이라고 표기 하였습니다.

| [![](https://camo.githubusercontent.com/ed34621376868f248806a326cae13b9801b986ad/68747470733a2f2f692e696d6775722e636f6d2f654d68464763682e706e67)](https://camo.githubusercontent.com/ed34621376868f248806a326cae13b9801b986ad/68747470733a2f2f692e696d6775722e636f6d2f654d68464763682e706e67) | [![](https://camo.githubusercontent.com/a6534f330d9d4e4a4d997f10e34be05617c5e895/68747470733a2f2f692e696d6775722e636f6d2f69366e337945612e706e67)](https://camo.githubusercontent.com/a6534f330d9d4e4a4d997f10e34be05617c5e895/68747470733a2f2f692e696d6775722e636f6d2f69366e337945612e706e67) |
| :--- | :--- |
| Normal 종류 | 3D Surface Normal |

정의 : The normal of a plane is an unit vector that is perpendicular to it

Normal Estimation은 샘플링 된 값들로부터 방향 정보를 복원해 내는 작업을 의미 합니다. 여기서 중요한 것은 샘플링된 값들입니다. 한점의 보만으로는 법선 벡터를 구할수 없습니다. 그래서 구하려고 하는 대상 점의 이웃한 점들이 가지고 있는 값들을 이용하면 샘플링하기 전에 그 점을 포함하고 있던 면의 법선 벡터를 근사적으로 추정 할수 있다. 이렇게 대상점을 중심으로 한 국소적인 정보로 부터 구해 낸 법선 벡터를 추정 법선 벡터\(estimated normal\)라 합니다. PCL에서는 샘플링 방법으로 이전장에서 살펴본 Octree Search를 사용합니다.

[![image](https://user-images.githubusercontent.com/17797922/41693140-e87b4298-753e-11e8-8d66-0c1ca989e531.png)](https://user-images.githubusercontent.com/17797922/41693140-e87b4298-753e-11e8-8d66-0c1ca989e531.png)

> Integral images도 normal estimation방법 중 하나입니다. 하지만 RGB-D센서등에서 얻은 organized clouds를 대상으로 하고 있어 여기서는 다루지 않습니다.

목적에 따라 3가지의 코드 [https://github.com/adioshun/gitBook\_PCL/blob/master/Tutorial/Feature/how-3d-features-work-in-pcl.md](https://github.com/adioshun/gitBook_PCL/blob/master/Tutorial/Feature/how-3d-features-work-in-pcl.md)

```text
  ne.setInputCloud (cloud);
  ne.setSearchMethod (tree);  
  //ne.setIndices () //?? 
  ne.setRadiusSearch (0.03); // Use all neighbors in a sphere of radius 3cm  
  ne.compute (*cloud_normals); // Compute the features
```



```cpp
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/features/normal_3d.h>
#include <pcl/visualization/pcl_visualizer.h>

// How 3D Features work in PCL
// http://pointclouds.org/documentation/tutorials/how_features_work.php

int
main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>); // 입력 포인트 클라우드 저장할 오브젝트 
  pcl::PointCloud<pcl::Normal>::Ptr cloud_normals (new pcl::PointCloud<pcl::Normal>); // 계산된 Normal을 저장할 오브젝트 

  // *.PCD 파일 읽기 (https://raw.githubusercontent.com/adioshun/gitBook_Tutorial_PCL/master/Beginner/sample/tabletop.pcd)
  pcl::io::loadPCDFile<pcl::PointXYZ> ("tabletop.pcd", *cloud);
  
  // Create the normal estimation class, and pass the input dataset to it
  pcl::NormalEstimation<pcl::PointXYZ, pcl::Normal> ne;
  ne.setInputCloud (cloud);
  pcl::search::KdTree<pcl::PointXYZ>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZ> ());
  ne.setSearchMethod (tree);  
  ne.setRadiusSearch (0.03); // Use all neighbors in a sphere of radius 3cm  
  ne.compute (*cloud_normals); // Compute the features

  // 포인트수 출력  
  std::cout << "Filtered " << cloud_normals->points.size ()  << std::endl;

  // Visualize them.
	boost::shared_ptr<pcl::visualization::PCLVisualizer> viewer(new pcl::visualization::PCLVisualizer("Normals"));
	viewer->addPointCloud<pcl::PointXYZ>(cloud, "cloud");
	// Display one normal out of 20, as a line of length 3cm.
	viewer->addPointCloudNormals<pcl::PointXYZ, pcl::Normal>(cloud, cloud_normals, 20, 0.03, "normals");
	while (!viewer->wasStopped())
    {
      viewer->spinOnce(100);
      boost::this_thread::sleep(boost::posix_time::microseconds(100000));
    }
}
```





Normal Estimation Using Integral Images 

[https://github.com/adioshun/gitBook\_PCL/blob/master/Tutorial/Feature/Normal-Estimation-Using-Integral-Images.md](https://github.com/adioshun/gitBook_PCL/blob/master/Tutorial/Feature/Normal-Estimation-Using-Integral-Images.md)

