# NDP

앞서 살펴본 ICP는 간단한 구현방법과 상대적으로 높은 성능으로 오랫동안 사용되어 왔습니다. 하지만, 모든 포인트에 대하여 연산 작업을 진행 하므로 계산 시간이 큽니다. 이를 해결하기 위한 많은 알고리즘 중에서 NDT가 있습니다. NDT는 2003 Biber et al. \[11\]에 의해 제안된 방식으로  거리 센서로 수집한 점군이 주어졌을 때, NDT는 일정한 크기의 격자를 설정하고, 각 격자 속의 점의 위치 벡터에 대해 정규분포로 변환후 이 정보를 활용하여 정합을 수행 합니다. NDT는 점들의 개수에 비해 월등히 적은 분포로 공간을 표현하기 때문에 메모리 효율이 높고, 대응쌍 검색의 소요시간이 짧다는 장점이 있습니다. 



```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/registration/ndt.h> // NormalDistributionsTransform

/* ---[ */
int
main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_in (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_out (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ> Final;   

  pcl::io::loadPCDFile ("bun0.pcd", *cloud_in);
  pcl::io::loadPCDFile ("bun4.pcd", *cloud_out);

  pcl::NormalDistributionsTransform<pcl::PointXYZ, pcl::PointXYZ> reg;
  //reg.setStepSize (0.05);
  reg.setResolution (0.025f);
  reg.setInputCloud (cloud_in);
  reg.setInputTarget (cloud_out);
  //reg.setMaximumIterations (50);
  //reg.setTransformationEpsilon (1e-8);
  reg.align(Final);

  std::cout << "has converged:" << reg.hasConverged() << " score: " <<   // 정확히 정합되면 1(True)
  reg.getFitnessScore() << std::endl;
  
  Eigen::Matrix4f transformation = reg.getFinalTransformation ();
  std::cout << transformation << std::endl;                // 변환 행렬 출력 

 return (0);
}
```







> 최초 제안은 2D Range Scan을 기반으로 하고 있으며 \[2\]에 위해서 3D로 확장 되었다.



\[2\] M. Magnusson, R. Elsrud, L.-E. Skagerlund and T. Duckett, "3D Modelling for Underground Mining Vehicles," in Conference on Modeling and Simulation for Public Safety, Linkping, Sweden, 2005.

