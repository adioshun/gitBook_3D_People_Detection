---
description: >-
  Aleksey Golovinskiy, Thomas Funkhouser, Min-Cut Based Segmentation of Point
  Clouds, ICCV 2009
---

# 2\_MIN-CUT

민컷 알고리즘은 이진 세분화\(binary segmentation\)기 입니다. 즉, 전체 포인트클라우드를 2개의 클러스터로 나눕니다. 구분 기준은 사용자의 지정 포인트 입니다. 지정된 포인트에 속한 물체는 전경\( \(foreground points\)으로, 속하지 못한 포인트는 배경\(background points\)으로  세분화 합니다. 민컷은 그래프 기반 군집화 기법인 Spectral Clustering을 이용하는 특징을 가지고 있습니다. 

In order to do that, the algorithm uses a vertex graph, where each vertex \(node of the graph\) represents a point, plus two additional vertices called source and sink that will be connected to the others with edges with different penalties \(weights\). Then, edges are also established between neighboring points, with a weight value each that depends on the distance that separates them. The greater the distance, the higher the probability of the edge being cut. The algorithm also needs as input a point that is known to be the center of the object, and the radius \(size\). After everything has been set, the minimum cut will be found, and we will have a list of foreground points.

동작 과정은 아래와 같습니다. 

1. 각 포인트에 대하여 
2. ...
3. ...
4. ...

> [https://app.gitbook.com/@adioshun/s/-1/unsupervised-learning/clustering/graph-based/spectral-clustering](https://app.gitbook.com/@adioshun/s/-1/unsupervised-learning/clustering/graph-based/spectral-clustering)
>
> [https://github.com/adioshun/gitBook\_Tutorial\_PCL/blob/master/Intermediate/Part02-Chapter01-Min-Cut-PCL-Cpp.md](https://github.com/adioshun/gitBook_Tutorial_PCL/blob/master/Intermediate/Part02-Chapter01-Min-Cut-PCL-Cpp.md)



사용되는 파라미터는 아래와 같습니다.  

| 파라미터  | 설명  |
| :--- | :--- |
| setInputCloud  | 입력   |
| setIndices | \(옵션\)indices |
| setForegroundPoints |  |
| setSigma |  |
| setRadius |  |
| setNumberOfNeighbours |  |
| setSourceWeight |  |
| extract |  |

```cpp
#include <iostream>
#include <vector>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/segmentation/min_cut_segmentation.h>

// Min-Cut Based Segmentation
// http://pointclouds.org/documentation/tutorials/min_cut_segmentation.php#min-cut-segmentation
// Commnets : Hunjung, Lim (hunjung.lim@hotmail.com)

int main (int argc, char** argv)
{

  // *.PCD 파일 읽기 
  // https://raw.githubusercontent.com/PointCloudLibrary/data/master/tutorials/min_cut_segmentation_tutorial.pcd
  pcl::PointCloud <pcl::PointXYZ>::Ptr cloud (new pcl::PointCloud <pcl::PointXYZ>);
  pcl::io::loadPCDFile <pcl::PointXYZ> ("min_cut_segmentation_tutorial.pcd", *cloud);

  //중심점 선정 
  pcl::PointCloud<pcl::PointXYZ>::Ptr foreground_points(new pcl::PointCloud<pcl::PointXYZ> ());
  pcl::PointXYZ point; //(필수) 입력 파라미터로 사용할 중간 포인트
  point.x = 68.97;
  point.y = -18.55;
  point.z = 0.57;
  foreground_points->points.push_back(point);

  pcl::MinCutSegmentation<pcl::PointXYZ> seg;
  seg.setInputCloud (cloud);
  //seg.setIndices (indices);  //indices 기반으로도 동작 함 (eg. PassThrough 필터 )
  seg.setForegroundPoints (foreground_points);
  seg.setSigma (0.25);          //smooth cost calculation을 위해 필요한 값 
  seg.setRadius (3.0433856);    //smooth cost calculation을 위해 필요한 값 
  seg.setNumberOfNeighbours (14);  // 그래프 생성시 탐색할 이웃 포인트 수 
  seg.setSourceWeight (0.8);       //전경 패널티 값 

  std::vector <pcl::PointIndices> clusters;
  seg.extract (clusters);         // 군집화 실행 

  //graph cut시 사용되는 값 출력 
  std::cout << "Maximum flow is " << seg.getMaxFlow () << std::endl;

  // 생성된 포인트클라우드 저장 
  pcl::PointCloud <pcl::PointXYZRGB>::Ptr colored_cloud = seg.getColoredCloud ();
  pcl::io::savePCDFile<pcl::PointXYZRGB>("result_min_cut_segmentation.pcd", *colored_cloud);
  return (0);
}
```

