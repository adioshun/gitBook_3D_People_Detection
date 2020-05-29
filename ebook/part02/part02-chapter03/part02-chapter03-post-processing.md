# 후처리

지금까지 살펴본 정합 알고리즘들의 목표는 입력\(소스\) 점군이 목적\(타겟\) 점군과 비교 하여 어느정도 이동 및 회전 하였는지에 대한 변환행렬 정보를 계산 하는 것입니다. 우리의 목적은 두 점군을 하나로 합치는 것입니다. 이를 위해서는 입력\(소스\) 점군을 변환행렬을 이용하여 변경하고 변경된 점군을 목적 점군과 합치면 됩니다. 점군간의 병합은 간단한 덧셈 산술연산으로 가능합니다. 점군을 변경하기 위해서 PCL의 3가지 방법을 이용하여 수행 할 수 있습니다. 



### 'align' 사용 

icp, 





```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/registration/icp.h>

// Using a matrix to transform a point cloud
// http://pointclouds.org/documentation/tutorials/matrix_transform.php#matrix-transform

int
 main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr source (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr target (new pcl::PointCloud<pcl::PointXYZ>);
  

  pcl::io::loadPCDFile ("bun0.pcd", *source);
  pcl::io::loadPCDFile ("bun4.pcd", *target);

  pcl::IterativeClosestPoint<pcl::PointXYZ, pcl::PointXYZ> icp;
  icp.setInputSource(source);
  icp.setInputTarget(target);

  icp.setMaximumIterations (50);
  icp.setTransformationEpsilon (1e-8);
  icp.setMaxCorrespondenceDistance (0.15);

  pcl::PointCloud<pcl::PointXYZ> source_transformed;   
  icp.align(source_transformed);

  std::cout << icp.getFinalTransformation() << std::endl;                // 변환 행렬 출력 
  pcl::io::savePCDFile<pcl::PointXYZ>("source_transformed.pcd", source_transformed); // 산술 연산자로 합치기 
  //pcl_viewer bun4.pcd source_transformed.pcd
  pcl::io::savePCDFile<pcl::PointXYZ>("concated.pcd", source_transformed + *target); // 산술 연산자로 합치기 
  


  // 변환 하기 #1
  Eigen::Matrix4f transform = icp.getFinalTransformation();

	pcl::PointCloud<pcl::PointXYZ>::Ptr output (new pcl::PointCloud<pcl::PointXYZ>);
	pcl::transformPointCloud (*source, *output, transform);
	pcl::io::savePCDFileBinary ("source_transformed_get.pcd", *output);
  //pcl::io::savePCDFile<pcl::PointXYZ>("result1.pcd", Final + *target); // 산술 연산자로 합치기 
  //pcl_viewer bun4.pcd source_transformed.pcd 

  // 변환 하기 #2
  /*
   0.880453   0.0369574   -0.472695   0.0344352
 -0.0240292    0.999157   0.0333579 -0.00150679
   0.473528  -0.0180111    0.880596   0.0411298
          0           0           0           1
  */

  // Define a translation of 2.5 meters on the x axis.
  Eigen::Matrix4f transform_1 = Eigen::Matrix4f::Identity();
  transform_1 (0,0) = 0.880453;transform_1 (0,1) = 0.0369574;transform_1 (0,2) = -0.472695;transform_1 (0,3) = 0.0344352;
  transform_1 (1,0) = -0.0240292;transform_1 (1,1) = 0.999157;transform_1 (1,2) = 0.0333579;transform_1 (1,3) = -0.00150679;
  transform_1 (2,0) = 0.473528;transform_1 (2,1) = -0.0180111;transform_1 (2,2) = 0.880596;transform_1 (2,3) = 0.0411298;
  transform_1 (3,0) = 0.0;transform_1 (3,1) = 0.0;transform_1 (3,2) = 0.0;transform_1 (3,3) = 1.0;  
  std::cout << transform_1 << std::endl;

  
	pcl::PointCloud<pcl::PointXYZ>::Ptr output_value (new pcl::PointCloud<pcl::PointXYZ>);
	pcl::transformPointCloud (*source, *output_value, transform_1);
	pcl::io::savePCDFileBinary ("source_transformed_value.pcd", *output_value);
  //pcl_viewer bun4.pcd source_transformed_value.pcd 



  // 변환 하기 #3
  Eigen::Affine3f transform_2 = Eigen::Affine3f::Identity();
  
  transform_2.translation() << 2.5, 0.0, 0.0; // Define a translation of 2.5 meters on the x axis. 
  float theta = M_PI/4; // The angle of rotation in radians
  transform_2.rotate (Eigen::AngleAxisf (theta, Eigen::Vector3f::UnitZ())); // The same rotation matrix as before; theta radians around Z axis
  std::cout << transform_2.matrix() << std::endl;

  pcl::PointCloud<pcl::PointXYZ>::Ptr output_Affine (new pcl::PointCloud<pcl::PointXYZ>);
	pcl::transformPointCloud (*source, *output_Affine, transform_2);
	pcl::io::savePCDFileBinary ("source_transformed_Affine.pcd", *output_Affine);
  //pcl_viewer bun4.pcd source_transformed_Affine.pcd 

  

 return (0);
}


```

