# 지역 정합

### ICP \(Iterative closest points\)

대표적인 지역 정합 알고리즘으로는  ICP가 있습니다. ICP는 모든 포인트들을 비교 하면서 대응점간의 거리가 최소화 될때가지 반복적으로 정합을 수행 하는 알골리즘 입니다. 가장 직관적이면서 좋은 성능을 보여 많은 분야에서 사용되고 있습니다. 하지만 모든 포인트에 대하여 brute force방식으로 진행 하기 때문에 시간이 많이 소요 되어 kd-tree를 이용하여 지연을 줄이는 방법도 활용 되고 있습니다. 

동작 과정은 아래와 같습니다.

![](../../../.gitbook/assets/image%20%286%29.png)



* 첫째, 점 선택\(point selection\) 단계에서는 알고리즘의 계산량을 줄이기 위하여 노이즈 필터링이나 샘플링을 거쳐 점의 갯수를 줄인다.
* 둘째, 이웃 선택\(neighborhood selection\) 단계에서는 각 점의 주변에 분포하는 점들 중 가까운 점 일부를 선택한다.
* 셋째, 점쌍 매칭\(point pair matching\) 단계에서는 앞서 선택한 주변 점들로 곡면\(surface\)이나 평면\(plane\) 같은 기하학 요소를 만들고, 이를 이용하여 소스 점군 A의 점 에 대응하는\(corresponding\) 타갯 점군 표의 점을 찾는다.
* 넷째, 오정합점 제거 \(outlier rejection\) 단계에서는 소스 점군 A와 타겟 점군 B에서 서로 겹치는 부분, 즉 오버랩 영역\(inlier\)만 남기고, 겹치지 않는 영역\(outlier\) 은 정합에 이용하지 않는다.
* 다섯째, 오차 최소화\(error minimization\) 단계 에서는 소스 점군 A의 오버랩 영역 A′과 타겟 점군 B의 오버랩 영역 B′ 방향으로 이동하는 동안 각 대응점의 거리 오차가 최소가 되도록 한다.
* 여섯째, 변환\(Transformation\) 단계에서 는 소스 오버랩 점군 A′가 타갯 오버랩 점군 B′의 위치로 이동하 는 데에 필요한병진 행렬\(translation matrix\)과 회전 행렬\(rotation matrix\)을 구하여 소스 오버랩 점군 A′에 적용한다.
  * 여기서 거리 오차가 일정한 한계치\(tolerance, τ\) 이하이면 진행을 멈주고 한계치 이상이라면 점쌍 매칭 \(point pair matching\) 단계로 되돌아가서 알고리즘을 다시 수행한다.

> [\[1\]](https://github.com/adioshun/gitBook_Tutorial_PCL/blob/master/part-2/part02-chapter05) 김지건, "[적은 오버랩에서 사용 가능한 3차원 점군 정합 방법](http://journal.cg-korea.org/archive/view_article?pid=jkcgs-24-5-11)", 2018
>
> P. Besl and N. McKay, ”A method for registration of 3-d shapes,” Pattern Analysis and Machine Intelligence, 14 \(2\) pp.239-256, 1992.

```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/registration/icp.h>


// How to use iterative closest point
// http://pointclouds.org/documentation/tutorials/iterative_closest_point.php#iterative-closest-point

int
 main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_in (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_out (new pcl::PointCloud<pcl::PointXYZ>);

  pcl::io::loadPCDFile ("bun0.pcd", *cloud_in);
  pcl::io::loadPCDFile ("bun4.pcd", *cloud_out);

  pcl::IterativeClosestPoint<pcl::PointXYZ, pcl::PointXYZ> icp;
  icp.setInputSource(cloud_in);
  icp.setInputTarget(cloud_out); 
  pcl::PointCloud<pcl::PointXYZ> Final;   
  icp.align(Final);

  std::cout << "has converged:" << icp.hasConverged() << " score: " <<   // 정확히 정합되면 1(True)
  icp.getFitnessScore() << std::endl;
  std::cout << icp.getFinalTransformation() << std::endl;                // 변환 행렬 출력 

 return (0);
}

```



[https://github.com/PointCloudLibrary/pcl/blob/master/test/registration/test\_registration.cpp](https://github.com/PointCloudLibrary/pcl/blob/master/test/registration/test_registration.cpp)

알고리즘의 종료 조건은 아래와 같습니다. `The algorithm has several termination criteria:`

1. Number of iterations has reached the maximum user imposed number of iterations \(via [setMaximumIterations](http://docs.pointclouds.org/trunk/classpcl_1_1_registration.html#a3844d186f7a99d15464368e0f25635ed)\)
2. The epsilon \(difference\) between the previous transformation and the current estimated transformation is smaller than an user imposed value \(via [setTransformationEpsilon](http://docs.pointclouds.org/trunk/classpcl_1_1_registration.html#aec74ab878cca8d62fd1be9942685a8c1)\)
3. The sum of Euclidean squared errors is smaller than a user defined threshold \(via [setEuclideanFitnessEpsilon](http://docs.pointclouds.org/trunk/classpcl_1_1_registration.html#aeb0bb4577dbe144bd467d4a9632b84d8)\)

```cpp
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/registration/icp.h>

// How to use iterative closest point
// http://pointclouds.org/documentation/tutorials/iterative_closest_point.php#iterative-closest-point

int
 main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_in (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud_out (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::PointCloud<pcl::PointXYZ> Final;   

  pcl::io::loadPCDFile ("bun0.pcd", *cloud_in);
  pcl::io::loadPCDFile ("bun4.pcd", *cloud_out);

  pcl::IterativeClosestPoint<pcl::PointXYZ, pcl::PointXYZ> reg;
  reg.setInputSource(cloud_in);
  reg.setInputTarget(cloud_out); 
  //종료 조건 지정 
  //reg.setMaximumIterations (50);              // 최대 수행 횟수 Set the maximum number of iterations (criterion 1)
  //reg.setTransformationEpsilon (1e-8);        // 이전 Transformation과의 최대 변화량 Set the transformation epsilon (criterion 2)
  //reg.setMaxCorrespondenceDistance (0.05);    // Set the max correspondence distance to 5cm 
                                               // (e.g., correspondences with higher distances will be ignored)
  //reg.setEuclideanFitnessEpsilon (1);       // Set the euclidean distance difference epsilon (criterion 3)
  reg.align(Final);

  std::cout << "has converged:" << reg.hasConverged() << " score: " <<   // 정확히 정합되면 1(True)
  reg.getFitnessScore() << std::endl;
  
  Eigen::Matrix4f transformation = reg.getFinalTransformation ();
  std::cout << transformation << std::endl;                // 변환 행렬 출력 

 return (0);
}
```







PCL은 다양한 ICP방법을 제공 하고 있다.

### IterativeClosestPointWithNormals

By default, this implementation uses the traditional point to plane objective and computes point to plane distances using the normals of the target point cloud. It also provides the option \(through setUseSymmetricObjective\) of using the symmetric objective function of \[Rusinkiewicz 2019\]. This objective uses the normals of both the source and target point cloud and has a similar computational cost to the traditional point to plane objective while also offering improved convergence speed and a wider basin of convergence.

Note that this implementation not demean the point clouds which can lead to increased numerical error. If desired, a user can demean the point cloud, run iterative closest point, and composite the resulting ICP transformation with the translations from demeaning to obtain a transformation between the original point clouds.

> Radu B. Rusu, Matthew Cong

### IterativeClosestPointNonLinear 

IterativeClosestPointNonLinear is an ICP variant that uses Levenberg-Marquardt optimization backend.

The resultant transformation is optimized as a quaternion.

The algorithm has several termination criteria:

1. Number of iterations has reached the maximum user imposed number of iterations \(via [setMaximumIterations](http://docs.pointclouds.org/trunk/classpcl_1_1_registration.html#a3844d186f7a99d15464368e0f25635ed)\)
2. The epsilon \(difference\) between the previous transformation and the current estimated transformation is smaller than an user imposed value \(via [setTransformationEpsilon](http://docs.pointclouds.org/trunk/classpcl_1_1_registration.html#aec74ab878cca8d62fd1be9942685a8c1)\)
3. The sum of Euclidean squared errors is smaller than a user defined threshold \(via [setEuclideanFitnessEpsilon](http://docs.pointclouds.org/trunk/classpcl_1_1_registration.html#aeb0bb4577dbe144bd467d4a9632b84d8)\)

> Radu B. Rusu, Michael Dixon

#### JointIterativeClosestPoint

JointIterativeClosestPoint extends ICP to multiple frames which share the same transform.

This is particularly useful when solving for camera extrinsics using multiple observations. When given a single pair of clouds, this reduces to vanilla ICP.



---

![](https://i.imgur.com/PfjFNzE.png)

ICP 알고리즘은 서로 다른 두 matrix A와 B 간의 거리를 최소화하는 알고리즘입니다.

transposed A와 B를 곱하고 그것을 Singular Value Decomposition\(SVD\) 해서 U, S, Vt를 구하고 transposed Vt와 transposed U를 곱한 행렬이 회전 행렬\(Transformation matrix\)이 됩니다.







[https://ieeexplore.ieee.org/document/121791](https://ieeexplore.ieee.org/document/121791)

