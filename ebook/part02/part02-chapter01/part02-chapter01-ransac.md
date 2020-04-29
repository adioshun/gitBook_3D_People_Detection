---
description: >-
  Straight line detection using Hough transform
  https://blog.csdn.net/qq_40335930/article/details/100162201
---

# 1\_RANSAC



## 2.  RANSAC  

RANSAC은Random Sample Consensus의  약어로 sample consensus 모듈에서 제공하는 여러 SAC계열 방식중에 하나로 잡음등에 강건하여 많이 사용 되고있습니다.  1981년 Martin A에  의해  제안된  방법입니다\(Fischler & Bolles, 1981\). 점군  안에  선, 원통, 평면  등과  같은  특정 Model이  있다는  가정하에  해당  모델의  파라미터를  추정  하고  이  후 cloud point가  이 model에  속하는지  아닌지를  무작위\(Random\)로 point들을  선별\(sample\)하여  일치\(Consensus\)하는지  테스트  하는  방법을  이야기  합니다. RANSAC의 장점은 모델 파라미터에 대해 강건한 예측수행이 가능합니. 즉, outliers이 많이 포함되어 있어도 높은 정확도를 보인다. 

단점으로는 계산 부하가 크고 각 대상에 따라 사용자가 직접 thresholds를 설정해 주어야 합니다. 또한, 하나의 데이터셋에 하나의 모델만 적용이 가능하다는 단점도 있습니다. 



![](../../../.gitbook/assets/image%20%289%29.png)

왼쪽 이미지의 회색은 입력 데이터로 사용되는 Point cloud입니다. 오른쪽 이미지에서 파란색 Line은 사용하고자 하는 Model입니다. RANSAC에서는 모델+파라미터에 맞으면 Inlier라 하고 맞지 않으면 outlier라고 합니다. 즉, 빨간색 점은 Outlier이고, 파란색 점은 Inlier입니다. 

동작과정은 다음과 같습니다\(오일석, 2014\).  

1. A model is fitted to the hypothetical inliers, i.e. all free parameters of the model are reconstructed from the inliers. 
2. All other data are then tested against the fitted model and, if a point fits well to the estimated model, also considered as a hypothetical inlier. 
3. The estimated model is reasonably good if sufficiently many points have been classified as hypothetical inliers. 
4. The model is reestimated from all hypothetical inliers, because it has only been estimated from the initial set of hypothetical inliers. 
5. Finally, the model is evaluated by estimating the error of the inliers relative to the model. 

1.

사용되는 파라미터는 아래와 같습니다.  

| 파라미터  | 설명  |
| :--- | :--- |
| setInputCloud  | 입력   |
| setModelType  | 적용 모델  |
| setMethodType  | 적용 방법  |
| setMaxIterations  | 최대 실행 수  |
| setDistanceThreshold  | inlier로 처리할 거리 정보  |
| setOptimizeCoefficients  | \(옵션\)  |
| segment  | 출력, 추정된 파라미터   |

RANSAC 알고리즘을 설명하기 전에, 먼저 inlier와 outlier 개념을 정확히 잡고 넘어갔으면 한다. RANSAC과 관련된 여러 미묘한 문제들은 결국 inlier와 outlier의 구분 문제이기 때문이다.

[Wikipedia](http://en.wikipedia.org/wiki/Outlier)에 보면 Outlier\(이상점\)는 데이터의 분포에서 현저하게 벗어나 있는 관측값으로 정의되어 있다. 현실 세계에서 관측된 데이터들이 완벽한 경우는 거의 없으며 약간씩의 오차, 편차, 노이즈 등이 있는 경우가 대부분이다. 그렇다면, 오차\(에러\)가 크면 outlier, 그렇지 않으면 inlier, 이렇게 구분하는 것일까? 대답은 NO! 아니라고 생각한다. 단지 편의상, 또는 별다른 방법이 없기 때문에 그렇게 할 뿐이다.





```cpp
#include <iostream>
#include <pcl/ModelCoefficients.h>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/sample_consensus/method_types.h>
#include <pcl/sample_consensus/model_types.h>
#include <pcl/segmentation/sac_segmentation.h>
#include <pcl/filters/extract_indices.h>

//Plane model segmentation
//http://pointclouds.org/documentation/tutorials/planar_segmentation.php#planar-segmentation

int
main (int argc, char** argv)
{

  pcl::PointCloud<pcl::PointXYZRGB>::Ptr cloud (new pcl::PointCloud<pcl::PointXYZRGB>), 
                                        cloud_p (new pcl::PointCloud<pcl::PointXYZRGB>), 
                                        inlierPoints (new pcl::PointCloud<pcl::PointXYZRGB>),
                                        inlierPoints_neg (new pcl::PointCloud<pcl::PointXYZRGB>);

  // *.PCD 파일 읽기 (https://raw.githubusercontent.com/adioshun/gitBook_Tutorial_PCL/master/Beginner/sample/tabletop_passthrough.pcd)
  pcl::io::loadPCDFile<pcl::PointXYZRGB> ("tabletop_passthrough.pcd", *cloud);

  // 포인트수 출력
  std::cout << "Loaded :" << cloud->width * cloud->height  << std::endl;
  
  // Object for storing the plane model coefficients.
  pcl::ModelCoefficients::Ptr coefficients (new pcl::ModelCoefficients ());
  pcl::PointIndices::Ptr inliers (new pcl::PointIndices ());


  // 오프젝트 생성 Create the segmentation object.
  pcl::SACSegmentation<pcl::PointXYZRGB> seg;
  seg.setOptimizeCoefficients (true);       //(옵션) // Enable model coefficient refinement (optional).
  seg.setInputCloud (cloud);                 //입력 
  seg.setModelType (pcl::SACMODEL_PLANE);    //적용 모델  // Configure the object to look for a plane.
  seg.setMethodType (pcl::SAC_RANSAC);       //적용 방법   // Use RANSAC method.
  seg.setMaxIterations (1000);               //최대 실행 수
  seg.setDistanceThreshold (0.01);          //inlier로 처리할 거리 정보   // Set the maximum allowed distance to the model.
  //seg.setRadiusLimits(0, 0.1); 	// cylinder경우, Set minimum and maximum radii of the cylinder.
  seg.segment (*inliers, *coefficients);    //세그멘테이션 적용 


  //추정된 평면 파라미터 출력 (eg. ax + by + cz + d = 0 ).
  std::cerr << "Model coefficients: " << coefficients->values[0] << " " 
                                      << coefficients->values[1] << " "
                                      << coefficients->values[2] << " " 
                                      << coefficients->values[3] << std::endl;

  pcl::copyPointCloud<pcl::PointXYZRGB>(*cloud, *inliers, *inlierPoints);
  pcl::io::savePCDFile<pcl::PointXYZRGB>("SACSegmentation_result.pcd", *inlierPoints);


 //[옵션]] 바닥 제거 결과 얻기 
 //Extracting indices from a PointCloud
 //http://pointclouds.org/documentation/tutorials/extract_indices.php
 pcl::ExtractIndices<pcl::PointXYZRGB> extract;
 extract.setInputCloud (cloud);
 extract.setIndices (inliers);
 extract.setNegative (true);//false
 extract.filter (*inlierPoints_neg);
 pcl::io::savePCDFile<pcl::PointXYZRGB>("SACSegmentation_result_neg.pcd", *inlierPoints_neg);

  return (0);

 
}
```

실행 결과

```text
Loaded :72823
Model coefficients: 3.88368e-05 0.000606678 1 -0.773654
```

시각화 & 결과

```text
$ pcl_viewer tabletop_passthrough.pcd 
$ pcl_viewer SACSegmentation_result.pcd  
$ pcl_viewer SACSegmentation_result_neg.pcd
```

| [![](https://camo.githubusercontent.com/d60875d7e7bd5503e4a5e86557f6fe7eba61e3e0/68747470733a2f2f692e696d6775722e636f6d2f7168637a5266572e706e67)](https://camo.githubusercontent.com/d60875d7e7bd5503e4a5e86557f6fe7eba61e3e0/68747470733a2f2f692e696d6775722e636f6d2f7168637a5266572e706e67) | [![](https://camo.githubusercontent.com/6733c3b8ab6504daa00549d9b8f4077c27639527/68747470733a2f2f692e696d6775722e636f6d2f55706f37425a4b2e706e67)](https://camo.githubusercontent.com/6733c3b8ab6504daa00549d9b8f4077c27639527/68747470733a2f2f692e696d6775722e636f6d2f55706f37425a4b2e706e67) | [![](https://camo.githubusercontent.com/1ec4e739b475d193469af8126b8c53915140c76e/68747470733a2f2f692e696d6775722e636f6d2f6a36486f4a42792e706e67)](https://camo.githubusercontent.com/1ec4e739b475d193469af8126b8c53915140c76e/68747470733a2f2f692e696d6775722e636f6d2f6a36486f4a42792e706e67) |
| :--- | :--- | :--- |
| 원본`tabletop_passthrough.pcd` | 결과`setNegative (false)` | 결과 `setNegative (true)` |

RANSAC과 평면 모델을 이용하면 지면과 물체를 세분화 할수 있습니다. 탐지 하려는 대부분의 물체는 지표면에 닿아 있습니다. 물건은 선반/책상위에, 차량은 도로 위에 존재 합니다1. 지면을 제거하게 되면 각각의 오브젝트 들이 서로 연결되지 않고 떨어지기 때문에 구분이 쉬워집니다. 



Performance Evaluation of RANSAC Family, [http://www.bmva.org/bmvc/2009/Papers/Paper355/Paper355.pdf](http://www.bmva.org/bmvc/2009/Papers/Paper355/Paper355.pdf)



Model Fitting and Shape Matching : [http://montefiore.ulg.ac.be/~nvecoven/ir/files/10-fitting.pdf](http://montefiore.ulg.ac.be/~nvecoven/ir/files/10-fitting.pdf)

