# 4\_Region-Growing

이미지 분석에서 부터 많이 사용하는 리전 그로잉은 시드라는 기준 픽셀 포인트를 정하고 기준 픽셀과 이웃 픽셀이 비슷한 속성을 갖는다면 동일한 영역\(리전\)으로 확장\(그로잉\)해 가는 알고리즘이다. 더이상 같은 속성을 갖는 것이 없으면 확장을 멈춘다. 

예를 들어 아래 흑백 그림에서 번개만 추출 하고자 할때 리전 그로잉을 적용 할수 있습니다. 먼저 시드픽셀을 정하고 속성 정보로 밝기 정보를 활용하여 임계치 밝기 155~255이내일경우 동일한 그룹으로 간주 한다면 이웃 픽셀을 비교 하면서 번개만 추출 할수 있다. 포인트인경우 노멀의 각도 정보를 속성값으로 활용 할수 있습니다. 



This type of segmentation \(which is also a greedy-like, flood fill approach like the Euclidean one\) groups together points that check a smoothness constraint. The angle between their normals and the difference of curvatures are checked to see if they could belong to the same smooth surface.

Think about a box laying on a table: with Euclidean segmentation, both would be considered to be in the same cluster because they are "touching". With region growing segmentation, this would not happen, because there is a 90° \(with ideal normal estimation, that is\) difference between the normals of a point in the table and another one in the box's lateral.

리전 그로잉을 수행하게 되면 결과적으로 유사한 표면속성을 가지는 물체끼리 군집화 됩니다. 이전에 살펴본 유클리드 방식과 비교 하여 바닥이 제거 되지 않았다면 모두 하나의 물체로 인식 하지만, 리전그잉이느 테이블과 물체의 노멀 정보가 다르므로 분류가 가능 합니다. 

피시엘에서는 추가적으로 시드 포인트를 선택 할때 곡률 정보를 활용하고 있습니다. 곡률 정보가 가장 낮은곳은 평면이므로 이곳에서부터 확장을 하면 전체 군집의 수를 줄일수 있기 때문입니다.\(??\) 

[https://en.wikipedia.org/wiki/Region\_growing](https://en.wikipedia.org/wiki/Region_growing)

동작 과정은 아래와 같습니다. 

1. 모든 포인트 곡률정보를 
2. ....





```cpp
reg.setMinClusterSize (50);           // 클러스터 최소 포인트 수, 기본값 = 1(모두 수행)
reg.setMaxClusterSize (1000000);      // 클러스터 최대 포인트 수, 기본값 = 1(모두 수행)
reg.setSearchMethod (tree);           // 내부적으로 사용하는 K-nearest search의 탐색 방법 지정 
reg.setNumberOfNeighbours (30);       // 내부적으로 사용하는 K-nearest search의 탐색 이웃 갯수 지정 
reg.setInputCloud (cloud);
//reg.setIndices (indices);          //index정보 이용 가능 (eg. PassThrough)
reg.setInputNormals (normals);
reg.setSmoothnessThreshold (3.0 / 180.0 * M_PI);   // angle in radians that will be used as the allowable range for the normals deviation
reg.setCurvatureThreshold (1.0);                  // If two points have a small normals deviation then the disparity between their curvatures is tested. 
                                                    // And if this value is less than curvature threshold then the algorithm will continue the growth of the cluster using new added point.

```



```cpp
#include <iostream>
#include <vector>
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/search/search.h>
#include <pcl/search/kdtree.h>
#include <pcl/features/normal_3d.h>
#include <pcl/segmentation/region_growing.h>

// Region growing segmentation
// http://pointclouds.org/documentation/tutorials/region_growing_segmentation.php#region-growing-segmentation
// Commnets : Hunjung, Lim (hunjung.lim@hotmail.com)

int
main (int argc, char** argv)
{
  // *.PCD 파일 읽기 
  // https://github.com/adioshun/gitBook_Tutorial_PCL/blob/master/Intermediate/sample/RANSAC_plane_true.pcd
  pcl::PointCloud<pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZ>);
  pcl::io::loadPCDFile <pcl::PointXYZ> ("tabletop_passthrough.pcd", *cloud);

  // 알고리즘에서 사용하는 Surface Normal 계산 
  pcl::search::Search<pcl::PointXYZ>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZ>);
  pcl::PointCloud <pcl::Normal>::Ptr normals (new pcl::PointCloud <pcl::Normal>);
  pcl::NormalEstimation<pcl::PointXYZ, pcl::Normal> normal_estimator;
  normal_estimator.setSearchMethod (tree);
  normal_estimator.setInputCloud (cloud);
  normal_estimator.setKSearch (50);
  normal_estimator.compute (*normals);

  pcl::RegionGrowing<pcl::PointXYZ, pcl::Normal> reg;
  reg.setMinClusterSize (50);           // 클러스터 최소 포인트 수, 기본값 = 1(모두 수행)
  reg.setMaxClusterSize (1000000);      // 클러스터 최대 포인트 수, 기본값 = 1(모두 수행)
  reg.setSearchMethod (tree);           // 내부적으로 사용하는 K-nearest search의 탐색 방법 지정 
  reg.setNumberOfNeighbours (30);       // 내부적으로 사용하는 K-nearest search의 탐색 이웃 갯수 지정 
  reg.setInputCloud (cloud);
  //reg.setIndices (indices);          //index정보 이용 가능 (eg. PassThrough)
  reg.setInputNormals (normals);
  reg.setSmoothnessThreshold (7.0 / 180.0 * M_PI); // 7 degrees.   Set the angle in radians, it will be used as the allowable range for the normals deviation
  reg.setCurvatureThreshold (1.0);                  // If two points have a small normals deviation then the disparity between their curvatures is tested. 
                                                    // And if this value is less than curvature threshold then the algorithm will continue the growth of the cluster using new added point.
                                                    // Set the curvature threshold. The disparity between curvatures will be
	                                                  // tested after the normal deviation check has passed.

  std::vector <pcl::PointIndices> clusters;
  reg.extract (clusters);

  std::cout << "Number of clusters is equal to " << clusters.size () << std::endl;
  std::cout << "First cluster has " << clusters[0].indices.size () << " points." << std::endl;
  std::cout << "These are the indices of the points of the initial" <<
  std::endl << "cloud that belong to the first cluster:" << std::endl;
  
	// 각 클러스터들 모두 생성 및 저장  
	int currentClusterNum = 1;
	for (std::vector<pcl::PointIndices>::const_iterator i = clusters.begin(); i != clusters.end(); ++i)
	{
		// ...add all its points to a new cloud...
		pcl::PointCloud<pcl::PointXYZ>::Ptr cluster(new pcl::PointCloud<pcl::PointXYZ>);
		for (std::vector<int>::const_iterator point = i->indices.begin(); point != i->indices.end(); point++)
			cluster->points.push_back(cloud->points[*point]);
		cluster->width = cluster->points.size();
		cluster->height = 1;
		cluster->is_dense = true;

		// ...and save it to disk.
		if (cluster->points.size() <= 0)
			break;
		std::cout << "Cluster " << currentClusterNum << " has " << cluster->points.size() << " points." << std::endl;
		std::string fileName = "cluster" + boost::to_string(currentClusterNum) + ".pcd";
		pcl::io::savePCDFileASCII(fileName, *cluster);

		currentClusterNum++;
	}

  //색상 정보로 표현 및 저장 
  pcl::PointCloud <pcl::PointXYZRGB>::Ptr colored_cloud = reg.getColoredCloud ();
  pcl::io::savePCDFile<pcl::PointXYZRGB>("region_growing_segmentation_result.pcd", *colored_cloud);
  

  return (0);
}
```



#### Color-based

위 방식과 기본 아이디어는 같습니다. 하지만 Normal과 curve정보 대신 색상 정보를 이용하여 작업을 진행합니다. 색상 정보를 이용하기 때문에 "PointXYZRGB" 점군에서만 동작 합니다. 또한 Normal 정보를 사용하지않기 때문에 Normal 계산 작업이 불 필요 합니다. 

동작 방법은 아래와같습니다. 

* 색상의 비슷함을 임계치 정보를 이용하여 검사 합니다. 
* 비슷한 색상의 이웃 점군까리 군집화를 진행 합니다. 
* 최소 크기 요구치에 부합되지 않으면 이웃 점군과 통합 됩니다. 
* 
```cpp
  reg.setInputCloud (cloud);         // 입력 
  reg.setSearchMethod (tree);        // 탐색 방법 
  reg.setDistanceThreshold (10);     // 이웃(Neighbor)으로 지정되는 거리 정보  
  reg.setPointColorThreshold (6);    // 동일 Cluter여부를 테스트 하기 위해 사용 (cf. Just as angle threshold is used for testing points normals )
  reg.setRegionColorThreshold (5);   // 동일 Cluter여부를 테스트 하기 위해 사용, 통합(merging)단계에서 사용됨 
  reg.setMinClusterSize (600);       // 최소 포인트수, 지정 값보다 작으면 이웃 포인트와 통합 됨 
```

```cpp
#include <iostream>
#include <thread>
#include <vector>
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/search/search.h>
#include <pcl/search/kdtree.h>
#include <pcl/visualization/cloud_viewer.h>
#include <pcl/filters/passthrough.h>
#include <pcl/segmentation/region_growing_rgb.h>

// Color-based region growing segmentation
// http://pointclouds.org/documentation/tutorials/region_growing_rgb_segmentation.php#region-growing-rgb-segmentation

int
main (int argc, char** argv)
{
  // *.PCD 파일 읽기 
  // https://github.com/adioshun/gitBook_Tutorial_PCL/blob/master/Intermediate/sample/region_growing_rgb_passthrough.pcd
  pcl::PointCloud <pcl::PointXYZRGB>::Ptr cloud (new pcl::PointCloud <pcl::PointXYZRGB>);
  pcl::io::loadPCDFile <pcl::PointXYZRGB> ("region_growing_rgb_passthrough.pcd", *cloud);

  // 알고리즘에서 사용하는 Surface Normal 계산  : Normal 정보 대신 색상 정보를 사용하므로 Normal 계산 작업이 불필요 함 
  
  //오프젝트 생성 
  pcl::search::Search <pcl::PointXYZRGB>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZRGB>);
  pcl::RegionGrowingRGB<pcl::PointXYZRGB> reg;
  reg.setInputCloud (cloud);         // 입력 
  reg.setSearchMethod (tree);        // 탐색 방법 
  reg.setDistanceThreshold (10);     // 이웃(Neighbor)으로 지정되는 거리 정보  
  reg.setPointColorThreshold (6);    // 동일 Cluter여부를 테스트 하기 위해 사용 (cf. Just as angle threshold is used for testing points normals )
  reg.setRegionColorThreshold (5);   // 동일 Cluter여부를 테스트 하기 위해 사용, 통합(merging)단계에서 사용됨 
  reg.setMinClusterSize (600);       // 최소 포인트수, 지정 값보다 작으면 이웃 포인트와 통합 됨 

  std::vector <pcl::PointIndices> clusters;
  reg.extract (clusters);

  //색상 정보로 표현 및 저장 
  pcl::PointCloud <pcl::PointXYZRGB>::Ptr colored_cloud = reg.getColoredCloud ();
  pcl::io::savePCDFile<pcl::PointXYZRGB>("region_growing_rgb_result.pcd", *colored_cloud);

  return (0);
}
```

