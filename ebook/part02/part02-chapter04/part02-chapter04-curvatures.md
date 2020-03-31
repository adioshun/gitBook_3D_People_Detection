# 9\_curvatures\_tmp

코드 동작 안함, 추후 검토 

```cpp
#include <pcl/point_cloud.h>
#include <pcl/features/normal_3d.h>
#include <pcl/features/principal_curvatures.h>
#include <pcl/io/pcd_io.h>

using namespace pcl;
using namespace pcl::io;
using namespace std;

int
main (int argc, char** argv)
{
  //https://github.com/PointCloudLibrary/pcl/blob/master/test/features/test_curvatures_estimation.cpp

  pcl::PointCloud <pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud <pcl::PointXYZ>);
  pcl::PointCloud<pcl::Normal>::Ptr normals (new pcl::PointCloud<pcl::Normal>); // 계산된 Normal을 저장할 오브젝트 

  pcl::io::loadPCDFile <pcl::PointXYZ> ("bun0.pcd", *cloud);

  float pcx, pcy, pcz, pc1, pc2;




// Create the normal estimation class, and pass the input dataset to it
  pcl::NormalEstimation<pcl::PointXYZ, pcl::Normal> ne;
  ne.setInputCloud (cloud);
  pcl::search::KdTree<pcl::PointXYZ>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZ> ());
  ne.setSearchMethod (tree);  
  ne.setRadiusSearch (0.03); // Use all neighbors in a sphere of radius 3cm  
  ne.compute (*normals); // Compute the features




  PrincipalCurvaturesEstimation<PointXYZ, Normal, PrincipalCurvatures> pc;
  pc.setInputNormals (normals);
  int indices_size = static_cast<int> (indices.size ());    // computePointPrincipalCurvatures (indices)
  pc.computePointPrincipalCurvatures (*normals, 0, indices, pcx, pcy, pcz, pc1, pc2);
  pc.setInputCloud (cloud.makeShared ());
  pc.setIndices (indicesptr);
  pc.setSearchMethod (tree);
  pc.setKSearch (indices_size);
  PointCloud<PrincipalCurvatures>::Ptr pcs (new PointCloud<PrincipalCurvatures> ());
  pc.compute (*pcs);

  }
```

