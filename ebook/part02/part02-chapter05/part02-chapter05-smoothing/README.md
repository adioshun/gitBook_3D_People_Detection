# Smoothing

3D 센서는 앞에서 살펴본 조밀하지 못한 단점외에도 센서의 성능으로 인해 측정 에러나, 잡음, 빈공간 등이 발생 할수 있습니다.  아래 그림은 Smoothing전후의 Normal을 시각화 한것입니다. Normal값이 보정되어 정합이나 Normal을 이용한 알고리즘이나 밀집도 기반 알고리즘의 성능 향상도 가능합니다. Smoothing역시 PCL에서는 Moving Least Squares \(MLS\) 알고리즘을 이용합니다

파라미터는 아래와 같습니다.

| 파라미터 | 설명 |
| :--- | :--- |
| setComputeNormals |  |
| setInputCloud |  |
| setPolynomialOrder |  |
| setSearchMethod |  |
| setSearchRadius |  |
| setPolynomialFit |  |
| process |  |



```text
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/kdtree/kdtree_flann.h>
#include <pcl/surface/mls.h>

int
main (int argc, char** argv)
{
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ> ());
  pcl::io::loadPCDFile ("bunny.pcd", *cloud);

  pcl::search::KdTree<pcl::PointXYZ>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZ>);

  // Output has the PointNormal type in order to store the normals calculated by MLS
  pcl::PointCloud<pcl::PointNormal>::Ptr smoothedCloud(new pcl::PointCloud<pcl::PointNormal>);

  // Init object (second point type is for the normals, even if unused)
  pcl::MovingLeastSquares<pcl::PointXYZ, pcl::PointNormal> mls; 
  mls.setComputeNormals (true); //optional
  mls.setInputCloud (cloud);
  mls.setPolynomialOrder (2);
  mls.setSearchMethod (tree);
  mls.setSearchRadius (0.03); // Use all neighbors in a radius of 3cm.
    //filter.setPolynomialFit(true); //If true, the surface and normal are approximated using a polynomial estimation
                                   // (if false, only a tangent one).
  // Reconstruct
  mls.process (*smoothedCloud);

  // Save output
  pcl::io::savePCDFile ("bunny-mls.pcd",*smoothedCloud);
}
```







