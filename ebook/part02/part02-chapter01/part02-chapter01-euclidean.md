# 3\_Euclidean clustering

```text
  ec.setInputCloud (cloud);       // 입력   
  ec.setConditionFunction  (0.02);  // 2cm  
  ec.setMinClusterSize (100);     // 최소 포인트 수 
  ec.setMaxClusterSize (25000);   // 최대 포인트 수
  ec.setSearchMethod (tree);      // 위에서 정의한 탐색 방법 지정 
  ec.extract (cluster_indices);   // 군집화 적용
```

유클리드 유클리드

가장 간단한 방법의 군집화 방법으로 두 점사이의 거리를 계산 해서 특정 거리 이하일경우 동일한 군집으로 간주 하는 방법입니다. 이때 거리를 계산 하는 방법으로 유클리드 거리계산법을 이용하여 **Euclidean clustering** 라고 합니다. 유클리드 거리 계산법\(=L2\)은 맨하탄 거리 계산법\(=L1\)과 함께 두점의 거리\(=유사도\)를 계산 할때 많이 사용 됩니다. 전자는 그림의 보라색과 같이 같이 실질적으로 x축으로 이동 거리와 y축으로 이동 거리모두 더한것을 의미하며 후자는 그림의 보라색과 같이 우리가 일반적으로 아는 두 점사이의 직선 거리로 삼각형 변의 공식\(??\)을 이용하여 계산 합니다.

![](https://user-images.githubusercontent.com/17797922/40976327-aedac18a-6882-11e8-8f83-6690531e52cd.png)

![](https://user-images.githubusercontent.com/17797922/40978071-87854ef2-6887-11e8-9785-27911ef9936e.png)

점군 계산에서 점 간 거리만 계산하고, 탐색하면 되므로, 처리속도가 다른 클러스터링 알고리즘에 비해 매우 빠르다. 다만, 세밀한 클러스터링을 할 수 없는 단점이 있다.

동작 과정은 다음과 같습니다. 

1. 효율적인 계산을 위하여 점군의 KdTree구조체를 생성 합니다. 

2. 하나의 군집으로 간주한 거리 정보를 설정 합니다. 

3. 이웃 점군을 탐색 하면서 거리 내에 있을경우 하나의 군집으로 설정 합니다.

4. 이웃 점군이 지정된 거리 내에 없을 경우 탐색 하지 않은 새 포인트를 새 군집으로 선택하고 탐색 합니다. 

입력에 사용되는 파라미터는 다음과 같습니다.

```text
  ec.setInputCloud (cloud);       // 입력   
  ec.setClusterTolerance (0.02);  // 2cm  
  ec.setMinClusterSize (100);     // 최소 포인트 수 
  ec.setMaxClusterSize (25000);   // 최대 포인트 수
  ec.setSearchMethod (tree);      // 위에서 정의한 탐색 방법 지정 
  ec.extract (cluster_indices);   // 군집화 적용
```

실행 코드는 다음과 같습니다. 

```cpp
#include <pcl/io/pcd_io.h>
#include <pcl/segmentation/extract_clusters.h>
#include <iostream>

// Euclidean Cluster Extraction
// http://pointclouds.org/documentation/tutorials/cluster_extraction.php#cluster-extraction

int 
main (int argc, char** argv)
{

  pcl::PointCloud<pcl::PointXYZRGB>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZRGB>);
  pcl::PointCloud<pcl::PointXYZRGB>::Ptr cloud_f (new pcl::PointCloud<pcl::PointXYZRGB>);

  // *.PCD 파일 읽기 (https://raw.githubusercontent.com/adioshun/gitBook_Tutorial_PCL/master/Intermediate/sample/RANSAC_plane_true.pcd)
  pcl::io::loadPCDFile<pcl::PointXYZRGB> ("RANSAC_plane_true.pcd", *cloud);

  // 포인트수 출력
  std::cout << "PointCloud before filtering has: " << cloud->points.size () << " data points." << std::endl; //*

  // 탐색을 위한 KdTree 오브젝트 생성 //Creating the KdTree object for the search method of the extraction
  pcl::search::KdTree<pcl::PointXYZRGB>::Ptr tree (new pcl::search::KdTree<pcl::PointXYZRGB>);
  tree->setInputCloud (cloud);  //KdTree 생성 


  std::vector<pcl::PointIndices> cluster_indices;       // 군집화된 결과물의 Index 저장, 다중 군집화 객체는 cluster_indices[0] 순으로 저장 
  // 군집화 오브젝트 생성  
  pcl::EuclideanClusterExtraction<pcl::PointXYZRGB> ec;
  ec.setInputCloud (cloud);       // 입력   
  ec.setClusterTolerance (0.02);  // 2cm  
  ec.setMinClusterSize (100);     // 최소 포인트 수 
  ec.setMaxClusterSize (25000);   // 최대 포인트 수
  ec.setSearchMethod (tree);      // 위에서 정의한 탐색 방법 지정 
  ec.extract (cluster_indices);   // 군집화 적용 

  // 클러스터별 정보 수집, 출력, 저장 
  int j = 0;
  for (std::vector<pcl::PointIndices>::const_iterator it = cluster_indices.begin (); it != cluster_indices.end (); ++it)
  {
    pcl::PointCloud<pcl::PointXYZRGB>::Ptr cloud_cluster (new pcl::PointCloud<pcl::PointXYZRGB>);
    for (std::vector<int>::const_iterator pit = it->indices.begin (); pit != it->indices.end (); ++pit)
     cloud_cluster->points.push_back (cloud->points[*pit]); 
    cloud_cluster->width = cloud_cluster->points.size ();
    cloud_cluster->height = 1;
    cloud_cluster->is_dense = true;

    // 포인트수 출력
    std::cout << "PointCloud representing the Cluster: " << cloud_cluster->points.size () << " data points." << std::endl;

    // 클러스터별 이름 생성 및 저장 
    std::stringstream ss;
    ss << "cloud_cluster_" << j << ".pcd";
    pcl::PCDWriter writer;
    writer.write<pcl::PointXYZRGB> (ss.str (), *cloud_cluster, false); //*
    j++;
  }

  return (0);
}
```

#### Conditional Euclidean Clustering

조건 유클리드 클러스터링은 기존 방법과 동작 과정은 같습니다. 추가적으로 좀더 세밀한 군집화를 위하여 거리 정보외에 사용자가 지정한 조건을 고려 하게 할수도 있습니다. 

동작 과정은 다음과 같습니다. 

1. xxx
2. xxxx

입력에 사용되는 파라미터는 다음과 같습니다.

```cpp
  cec.setInputCloud (cloud_with_normals);                            // 입력 
  cec.setConditionFunction (&customRegionGrowing);                   // 사용자 정의 조건 
  cec.setClusterTolerance (500.0);                                   //K-NN 탐색시 Radius값 (후보 포인트 탐색에 사용)
  cec.setMinClusterSize (cloud_with_normals->points.size () / 1000); //클러스터 최소 포인트 수 (eg. 전체 포인트 수의 0.1% 이하 ) 
  cec.setMaxClusterSize (cloud_with_normals->points.size () / 5);    //클러스터 최대 포인트 수 (eg. 전체 포인트 수의 20% 이상 )
  cec.segment (*clusters);                                           //군집화 실행 
  cec.getRemovedClusters (small_clusters, large_clusters);         
```

```cpp
#include <pcl/point_types.h>
#include <pcl/io/pcd_io.h>
#include <pcl/console/time.h>

#include <pcl/filters/voxel_grid.h>
#include <pcl/features/normal_3d.h>
#include <pcl/segmentation/conditional_euclidean_clustering.h>

//Conditional Euclidean Clustering
//http://pointclouds.org/documentation/tutorials/conditional_euclidean_clustering.php#conditional-euclidean-clustering


bool
customRegionGrowing (const pcl::PointXYZINormal& point_a, const pcl::PointXYZINormal& point_b, float squared_distance)
{
  Eigen::Map<const Eigen::Vector3f> point_a_normal = point_a.getNormalVector3fMap (), point_b_normal = point_b.getNormalVector3fMap ();
  if (squared_distance < 10000)
  {
    if (std::abs (point_a.intensity - point_b.intensity) < 8.0f)
      return (true);
    if (std::abs (point_a_normal.dot (point_b_normal)) < 0.06)
      return (true);
  }
  else
  {
    if (std::abs (point_a.intensity - point_b.intensity) < 3.0f)
      return (true);
  }
  return (false);
}

int
main (int argc, char** argv)
{
  // *.PCD 파일 읽기 
  // https://excellmedia.dl.sourceforge.net/project/pointclouds/PCD%20datasets/Trimble/Outdoor1/Statues_4.pcd.zip
  pcl::PointCloud<pcl::PointXYZI>::Ptr cloud_in (new pcl::PointCloud<pcl::PointXYZI>);
  pcl::io::loadPCDFile <pcl::PointXYZI> ("Statues_4.pcd", *cloud_in);
  // 포인트수 출력
  std::cout << "Loaded :" << cloud_in->points.size () << std::endl;

  // 다운 샘플링 
  pcl::PointCloud<pcl::PointXYZI>::Ptr cloud_out (new pcl::PointCloud<pcl::PointXYZI>);
  pcl::VoxelGrid<pcl::PointXYZI> vg;
  vg.setInputCloud (cloud_in);
  vg.setLeafSize (80.0, 80.0, 80.0);
  vg.setDownsampleAllData (true);
  vg.filter (*cloud_out);

  // Normal 계산후 합치기 (Set up a Normal Estimation class and merge data in cloud_with_normals)
  pcl::PointCloud<pcl::PointXYZINormal>::Ptr cloud_with_normals (new pcl::PointCloud<pcl::PointXYZINormal>);
  pcl::search::KdTree<pcl::PointXYZI>::Ptr search_tree (new pcl::search::KdTree<pcl::PointXYZI>);
  pcl::copyPointCloud (*cloud_out, *cloud_with_normals);
  pcl::NormalEstimation<pcl::PointXYZI, pcl::PointXYZINormal> ne;
  ne.setInputCloud (cloud_out);
  ne.setSearchMethod (search_tree);
  ne.setRadiusSearch (300.0);
  ne.compute (*cloud_with_normals);


  // Set up a Conditional Euclidean Clustering class
  pcl::IndicesClustersPtr clusters (new pcl::IndicesClusters);
  pcl::ConditionalEuclideanClustering<pcl::PointXYZINormal> cec(true);   //True = 작거나(setMinClusterSize) 큰것도(setMaxClusterSize) 수행 
  cec.setInputCloud (cloud_with_normals);                            // 입력 
  cec.setConditionFunction (&customRegionGrowing);                   // 사용자 정의 조건 
  cec.setClusterTolerance (500.0);                                   //K-NN 탐색시 Radius값 (후보 포인트 탐색에 사용)
  cec.setMinClusterSize (cloud_with_normals->points.size () / 1000); //클러스터 최소 포인트 수 (eg. 전체 포인트 수의 0.1% 이하 ) 
  cec.setMaxClusterSize (cloud_with_normals->points.size () / 5);    //클러스터 최대 포인트 수 (eg. 전체 포인트 수의 20% 이상 )
  cec.segment (*clusters);                                           //군집화 실행 
  
  // True로 초기화시 작거나 큰것의 정보를 저장할 곳 
  pcl::IndicesClustersPtr small_clusters (new pcl::IndicesClusters);
  pcl::IndicesClustersPtr large_clusters (new pcl::IndicesClusters);
  cec.getRemovedClusters (small_clusters, large_clusters);           


  // Using the intensity channel for lazy visualization of the output
  for (int i = 0; i < small_clusters->size (); ++i)
    for (int j = 0; j < (*small_clusters)[i].indices.size (); ++j)
      cloud_out->points[(*small_clusters)[i].indices[j]].intensity = -2.0;
  for (int i = 0; i < large_clusters->size (); ++i)
    for (int j = 0; j < (*large_clusters)[i].indices.size (); ++j)
      cloud_out->points[(*large_clusters)[i].indices[j]].intensity = +10.0;
  for (int i = 0; i < clusters->size (); ++i)
  {
    int label = rand () % 8;
    for (int j = 0; j < (*clusters)[i].indices.size (); ++j)
      cloud_out->points[(*clusters)[i].indices[j]].intensity = label;
  }

  // Save the output point cloud

  pcl::io::savePCDFile ("output.pcd", *cloud_out);


  return (0);
}



/*
bool
enforceIntensitySimilarity (const pcl::PointXYZINormal& point_a, const pcl::PointXYZINormal& point_b, float squared_distance)
{
  if (std::abs (point_a.intensity - point_b.intensity) < 5.0f)
    return (true);
  else
    return (false);
}

bool
enforceCurvatureOrIntensitySimilarity (const pcl::PointXYZINormal& point_a, const pcl::PointXYZINormal& point_b, float squared_distance)
{
  Eigen::Map<const Eigen::Vector3f> point_a_normal = point_a.getNormalVector3fMap (), point_b_normal = point_b.getNormalVector3fMap ();
  if (std::abs (point_a.intensity - point_b.intensity) < 5.0f)
    return (true);
  if (std::abs (point_a_normal.dot (point_b_normal)) < 0.05)
    return (true);
  return (false);
}

*/
```

