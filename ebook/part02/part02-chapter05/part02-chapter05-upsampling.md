# Upsampling

업샘플리은 앞장에서 살펴본 다운 샘플링과 반대로 포인트 클라우드의 수를 늘리는 것을 의미 합니다. RGB-D 센서 데이터는 이미지에 데이터 대비 저밀도\(Sparse\)한 특징을 가지고 있습니다\[1\]. 또한, 빔의 확산\(divergence\)으로 인해 원거리 물체는 근거리 대비 상대적으로 적은 포인트가 형성 됩니다\[2\]. 이때 포인트 보간\(interpolating\)을 통해 포인트수를 늘는 업샘플링이 수행 합니다. 가능합니다. 보간이란  기존의 존재 하는 포인트 사이를 다른 포인트로 채워넣어 좀더 조밀한\(Dense\) 포인트클라드를 만들어 원래 표면을 회복\(recover\) 하는  방식입니다. 

![](../../../.gitbook/assets/image%20%283%29.png)

PLC에서는 MLS를 이용하여 업샘플링을 수행 할수 있습니다. 



| 파라미터 | 설명 |
| :--- | :--- |
| setInputCloud |  |
| setSearchMethod |  |
| setSearchRadius |  |
| setUpsamplingMethod | SAMPLE\_LOCAL\_PLANE,  RANDOM\_UNIFORM\_DENSITY |
| setUpsamplingRadius |  |
| setUpsamplingStepSize |  |
| process |  |

```cpp
#include <pcl/io/pcd_io.h>
#include <pcl/surface/mls.h>

// Upsampling
// http://robotica.unileon.es/index.php/PCL/OpenNI_tutorial_2:_Cloud_processing_(basic)#Upsampling

int
main(int argc, char** argv)
{
	// Object for storing the point cloud.
	pcl::PointCloud<pcl::PointXYZ>::Ptr input(new pcl::PointCloud<pcl::PointXYZ>);
	pcl::PointCloud<pcl::PointXYZ>::Ptr output(new pcl::PointCloud<pcl::PointXYZ>);

	// Read a PCD file from disk.
	pcl::io::loadPCDFile<pcl::PointXYZ>("bunny.pcd", *input);


	// Filtering object.
	pcl::MovingLeastSquares<pcl::PointXYZ, pcl::PointXYZ> upsampling;
	upsampling.setInputCloud(input);
	// Object for searching.
	pcl::search::KdTree<pcl::PointXYZ>::Ptr kdtree;
	upsampling.setSearchMethod(kdtree);
	// Use all neighbors in a radius of 3cm.
	upsampling.setSearchRadius(0.03);
	// Upsampling method. Other possibilites are DISTINCT_CLOUD, RANDOM_UNIFORM_DENSITY
	// and VOXEL_GRID_DILATION. NONE disables upsampling. Check the API for details.
	upsampling.setUpsamplingMethod(pcl::MovingLeastSquares<pcl::PointXYZ, pcl::PointXYZ>::SAMPLE_LOCAL_PLANE);
	// Radius around each point, where the local plane will be sampled.
	upsampling.setUpsamplingRadius(0.03);
	// Sampling step size. Bigger values will yield less (if any) new points.
	upsampling.setUpsamplingStepSize(0.02);

	upsampling.process(*output);
	
	pcl::io::savePCDFile ("bunny-upsample.pcd",*output);
}
```



