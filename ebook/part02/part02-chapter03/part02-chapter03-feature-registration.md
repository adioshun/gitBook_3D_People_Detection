# 전역 정합

전역 정합 또는 대략적 정합은 두개의 포인트 클라우드의 점들에서 특징을 계산하고 이를 기반으로 정합을 수행 하는 방법입니다. 

![](http://robotica.unileon.es/images/thumb/7/74/Pairwise_registration.jpg/650px-Pairwise_registration.jpg)

수행 절차는 아래와 같습니다. 

1. 키포인트 선택 : 여러개의 포인트 중에서 데이터를 가장 잘 설명하는 키포인트를 선정 합니다. 
2. 특징 계산 : 각 키포인트별 feature descriptor를 계산 합니다. 
3. 대응점 찾기 : x,y,z위치 정보와 Descriptor 정보를 기반하여 유사도를 계산하여 correspondences를 계산 합니다. 
4. 대응점 선정 : 계산  correspondences 중에서 나쁜 것을 버리게 됩니다. 
5. 변환행렬 계산 : 남은 좋은 correspondences를 이용하여 transformation을 계산 합니다. 

PCL에서 제공하는 다양한 API를 이용하여 단계별로 살펴 보겠습니다. 

1. 키포인트 선택  

두 점군을 비교 할대 사용할 포인트를 선정 하는 단계 입니다. 키포인트 선정시 주요 고려 요소는 다음과 같습니다. 

* **Repeatability**: there should be a good chance of the same points being chosen over several iterations, even when the scene is captured from a different angle.
* **Distinctiveness**: the chosen keypoints should be highly characterizing and descriptive. It should be easy to describe and match them.

PCL에서는 NARF, SIFT, FAST등의 키포인트 추출 방법을 제공하고 있습니다. 예시에서는 ISS 키포인트를 사용 하였습니다. 

```cpp
// ISS keypoint detector object.
pcl::ISSKeypoint3D<pcl::PointXYZ, pcl::PointXYZ> detector_src;
detector_src.setInputCloud(src);
pcl::search::KdTree<pcl::PointXYZ>::Ptr kdtree_src(new pcl::search::KdTree<pcl::PointXYZ>);
detector_src.setSearchMethod(kdtree_src);
double resolution_src = computeCloudResolution(src);
std::cout << "resolution: "<< resolution_src << std::endl;    
// Set the radius of the spherical neighborhood used to compute the scatter matrix.
detector_src.setSalientRadius(6 * resolution_src);
// Set the radius for the application of the non maxima supression algorithm.
detector_src.setNonMaxRadius(4 * resolution_src);
// Set the minimum number of neighbors that has to be found while applying the non maxima suppression algorithm.
detector_src.setMinNeighbors(5);
// Set the upper bound on the ratio between the second and the first eigenvalue.
detector_src.setThreshold21(0.975);
// Set the upper bound on the ratio between the third and the second eigenvalue.
detector_src.setThreshold32(0.975);
// Set the number of prpcessing threads to use. 0 sets it to automatic.
detector_src.setNumberOfThreads(4);
detector_src.compute(*keypoints_src);
```



또는 간단하게 1장에서 살펴본 샘플링 방법중 하나인 그리드 샘플링을 수행 하여 단순히 수를 줄이는 방법도 있습니다. 

```cpp
pcl::UniformSampling<PointXYZ> uniform;
uniform.setRadiusSearch (1);  // 1m
uniform.setInputCloud (src);
uniform.filter (*keypoints_src);
```



2. 특징 계산 

선택된 각 키포인트에 대하여 특징을 계산 합니다. 정합의 기본 목표는 두 점군의 유사점을 찾아 맵핑 하는 것입니다. 이러한 유사점을 찾기 위해 특징정보를 이용합니다. PCL에서는 NARF, FPFH, BRIEF, SHIF 등의 특징 추출 방법을 제공하고 있습니다. 일부 특징들은 계산시 키포인트의 영향을 받거나 Normal등의 추가 정보가 필요 하기도 합니다. 따라서 특징을 선택 하기 위해서는 키포인트 선택도 중요 합니다. 

```cpp
// Compute normals for all points keypoint
PointCloud<Normal>::Ptr normals_src (new PointCloud<Normal>), 
                         normals_tgt (new PointCloud<Normal>);
pcl::NormalEstimation<PointXYZ, Normal> normal_est;
normal_est.setInputCloud (src);
normal_est.setRadiusSearch (0.5);  // 50cm
normal_est.compute (*normals_src);
normal_est.setInputCloud (tgt);
normal_est.compute (*normals_tgt);
//print_info ("- Estimated %lu and %lu normals for the source and target datasets.\n", normals_src->points.size (), normals_tgt->points.size ());
std::cout << "normal_est" << std::endl;
// Compute FPFH features at each keypoint
PointCloud<FPFHSignature33>::Ptr fpfhs_src (new PointCloud<FPFHSignature33>), 
                              fpfhs_tgt (new PointCloud<FPFHSignature33>);
pcl::FPFHEstimation<PointXYZ, Normal, FPFHSignature33> fpfh_est;
fpfh_est.setInputCloud (keypoints_src);
fpfh_est.setInputNormals (normals_src);
fpfh_est.setRadiusSearch (1); // 1m
fpfh_est.setSearchSurface (src);
fpfh_est.compute (*fpfhs_src);

fpfh_est.setInputCloud (keypoints_tgt);
fpfh_est.setInputNormals (normals_tgt);
fpfh_est.setSearchSurface (tgt);
fpfh_est.compute (*fpfhs_tgt);
```





3. 대응점 찾기\(Correspondences estimation\)

두 점군의 특징정보를 이용하여 겹치는 부분을 찾는 단계 입니다. 사용되는 특징의 종류에 따라서   brute force matching,kd-tree nearest neighbor search \(FLANN\)방법을 사용할수 있습니다. 

```cpp
// Find correspondences between keypoints in FPFH space
CorrespondencesPtr all_correspondences (new Correspondences);
pcl::registration::CorrespondenceEstimation<FPFHSignature33, FPFHSignature33> est;
est.setInputCloud (fpfhs_src);
est.setInputTarget (fpfhs_tgt);
est.determineReciprocalCorrespondences (*all_correspondences);
```



4. 대응점 선정\(Correspondences rejection\)

위 단계에서 찾은 대응점이 항상 옮지는 않습니다. 잘못된 대응점은 오히려 정합을 어렵게 하거나 실패 할수도 있습니다.따라서 판단 후 제거 되어야 합니다.  판다을 위해서는 RANSAC을 사용할수 있습니다. `Correspondences rejection`

```cpp
// Reject correspondences based on their XYZ distance
CorrespondencesPtr good_correspondences (new Correspondences);
pcl::registration::CorrespondenceRejectorDistance rej;
rej.setInputCloud<PointXYZ> (keypoints_src);
rej.setInputTarget<PointXYZ> (keypoints_tgt);
rej.setMaximumDistance (1);    // 1m
rej.setInputCorrespondences (all_correspondences);
rej.getCorrespondences (*good_correspondences);
```



5. 변환행렬 계산 

이제 모든 준비 단계는 끝났습니다. 변환 행렬 계산은 다음과 같습니다. 

* evaluate some error metric based on correspondence
* estimate a \(rigid\) transformation between camera poses \(motion estimate\) and minimize error metric
* optimize the structure of the points
  * Examples: - SVD for motion estimate; - Levenberg-Marquardt with different kernels for motion estimate;
* use the rigid transformation to rotate/translate the source onto the target, and potentially run an internal ICP loop with either all points or a subset of points or the keypoints
* iterate until some convergence criterion is met

```cpp
// Obtain the best transformation between the two sets of keypoints given the remaining correspondences
Eigen::Matrix4f transform;
pcl::registration::TransformationEstimationSVD<PointXYZ, PointXYZ> trans_est;
trans_est.estimateRigidTransformation (*keypoints_src, *keypoints_tgt, *good_correspondences, transform);
std::cerr << transform << std::endl;
```



6. SAC-IA를 이용하여 자동으로 하기 

Sample Consensus Initial Alignment을 이용하여 각 점군의 특징을 입력으로 받아 들여 대응점 찾기와 변환 행렬 계산을 한번에 할수도 있습니다. 

```cpp
// Initialize Sample Consensus Initial Alignment (SAC-IA)
pcl::SampleConsensusInitialAlignment<PointXYZ, PointXYZ, FPFHSignature33> reg;
reg.setMinSampleDistance (0.05f);
reg.setMaxCorrespondenceDistance (0.2);
reg.setMaximumIterations (1000);

reg.setInputCloud (src);
reg.setInputTarget (tgt);
reg.setSourceFeatures (fpfhs_src);
reg.setTargetFeatures (fpfhs_tgt);

// Register
pcl::PointCloud<pcl::PointXYZ> Final;   
reg.align (Final);

std::cout << "has converged:" << reg.hasConverged() << " score: " <<   // ?�확???�합?�면 1(True)
reg.getFitnessScore() << std::endl;

Eigen::Matrix4f transformation = reg.getFinalTransformation ();
std::cout << transformation << std::endl;                // 변???�렬 출력 
```







전체 코드는 아래와 같습니다.  



```cpp
#include <pcl/console/parse.h>
#include <pcl/point_types.h>
#include <pcl/point_cloud.h>
#include <pcl/point_representation.h>

#include <pcl/io/pcd_io.h>
#include <pcl/conversions.h>
#include <pcl/filters/uniform_sampling.h>
#include <pcl/features/normal_3d.h>
#include <pcl/features/fpfh.h>

#include <pcl/registration/registration.h>


#include <pcl/kdtree/impl/kdtree_flann.hpp>
#include <pcl/registration/ia_ransac.h>
#include <pcl/registration/pyramid_feature_matching.h>
#include <pcl/features/ppf.h>
#include <pcl/registration/ppf_registration.h>
#include <pcl/registration/ndt.h>
#include <pcl/registration/correspondence_estimation.h>
#include <pcl/registration/correspondence_rejection_distance.h>
#include <pcl/registration/transformation_estimation_svd.h>
#include <pcl/registration/transformation_estimation_point_to_plane_lls.h>
#include <pcl/registration/transformation_estimation_point_to_plane.h>
#include <pcl/registration/transformation_validation_euclidean.h>

#include <pcl/keypoints/iss_3d.h>
using namespace std;
using namespace pcl;
using namespace pcl::io;
using namespace pcl::console;
using namespace pcl::registration;
PointCloud<PointXYZ>::Ptr src, tgt;


// https://github.com/otherlab/pcl/blob/master/test/registration/test_registration.cpp
// pcd_data : open3d.ord


double
computeCloudResolution(const pcl::PointCloud<pcl::PointXYZ>::ConstPtr& cloud)
{
	double resolution = 0.0;
	int numberOfPoints = 0;
	int nres;
	std::vector<int> indices(2);
	std::vector<float> squaredDistances(2);
	pcl::search::KdTree<pcl::PointXYZ> tree;
	tree.setInputCloud(cloud);

	for (size_t i = 0; i < cloud->size(); ++i)
	{
		if (! pcl_isfinite((*cloud)[i].x))
			continue;

		// Considering the second neighbor since the first is the point itself.
		nres = tree.nearestKSearch(i, 2, indices, squaredDistances);
		if (nres == 2)
		{
			resolution += sqrt(squaredDistances[1]);
			++numberOfPoints;
		}
	}
	if (numberOfPoints != 0)
		resolution /= numberOfPoints;

	return resolution;
}


int
main (int argc, char** argv)
{
pcl::PointCloud<pcl::PointXYZ>::Ptr src (new pcl::PointCloud<pcl::PointXYZ>);
pcl::PointCloud<pcl::PointXYZ>::Ptr tgt (new pcl::PointCloud<pcl::PointXYZ>);
pcl::io::loadPCDFile<pcl::PointXYZ> ("bun0.pcd", *src);
pcl::io::loadPCDFile<pcl::PointXYZ> ("bun4.pcd", *tgt);

// Get an uniform grid of keypoints  //?�운 ?�플�?
PointCloud<PointXYZ>::Ptr keypoints_src (new PointCloud<PointXYZ>), 
                         keypoints_tgt (new PointCloud<PointXYZ>);


/*				 
pcl::UniformSampling<PointXYZ> uniform;
uniform.setRadiusSearch (1);  // 1m
uniform.setInputCloud (src);
uniform.filter (*keypoints_src);
uniform.setInputCloud (tgt);
uniform.filter (*keypoints_tgt);
//print("- Found %lu and %lu keypoints for the source and target datasets.\n", keypoints_src->points.size (), keypoints_tgt->points.size ());

std::cout << "Downsample" << std::endl;
*/

// ISS keypoint detector object.
pcl::ISSKeypoint3D<pcl::PointXYZ, pcl::PointXYZ> detector_src;
detector_src.setInputCloud(src);
pcl::search::KdTree<pcl::PointXYZ>::Ptr kdtree_src(new pcl::search::KdTree<pcl::PointXYZ>);
detector_src.setSearchMethod(kdtree_src);
double resolution_src = computeCloudResolution(src);
std::cout << "resolution: "<< resolution_src << std::endl;    
// Set the radius of the spherical neighborhood used to compute the scatter matrix.
detector_src.setSalientRadius(6 * resolution_src);
// Set the radius for the application of the non maxima supression algorithm.
detector_src.setNonMaxRadius(4 * resolution_src);
// Set the minimum number of neighbors that has to be found while applying the non maxima suppression algorithm.
detector_src.setMinNeighbors(5);
// Set the upper bound on the ratio between the second and the first eigenvalue.
detector_src.setThreshold21(0.975);
// Set the upper bound on the ratio between the third and the second eigenvalue.
detector_src.setThreshold32(0.975);
// Set the number of prpcessing threads to use. 0 sets it to automatic.
detector_src.setNumberOfThreads(4);

detector_src.compute(*keypoints_src);



pcl::ISSKeypoint3D<pcl::PointXYZ, pcl::PointXYZ> detector_tgt;
detector_tgt.setInputCloud(tgt);
pcl::search::KdTree<pcl::PointXYZ>::Ptr kdtree_tgt(new pcl::search::KdTree<pcl::PointXYZ>);
detector_tgt.setSearchMethod(kdtree_tgt);
double resolution_tgt = computeCloudResolution(tgt);
std::cout << "resolution: "<< resolution_tgt << std::endl;    
// Set the radius of the spherical neighborhood used to compute the scatter matrix.
detector_tgt.setSalientRadius(6 * resolution_tgt);
// Set the radius for the application of the non maxima supression algorithm.
detector_tgt.setNonMaxRadius(4 * resolution_tgt);
// Set the minimum number of neighbors that has to be found while applying the non maxima suppression algorithm.
detector_tgt.setMinNeighbors(5);
// Set the upper bound on the ratio between the second and the first eigenvalue.
detector_tgt.setThreshold21(0.975);
// Set the upper bound on the ratio between the third and the second eigenvalue.
detector_tgt.setThreshold32(0.975);
// Set the number of prpcessing threads to use. 0 sets it to automatic.
detector_tgt.setNumberOfThreads(4);

detector_tgt.compute(*keypoints_tgt);



std::cout << "keypoints" << std::endl;

// Compute normals for all points keypoint
PointCloud<Normal>::Ptr normals_src (new PointCloud<Normal>), 
                         normals_tgt (new PointCloud<Normal>);
pcl::NormalEstimation<PointXYZ, Normal> normal_est;
normal_est.setInputCloud (src);
normal_est.setRadiusSearch (0.5);  // 50cm
normal_est.compute (*normals_src);
normal_est.setInputCloud (tgt);
normal_est.compute (*normals_tgt);
//print_info ("- Estimated %lu and %lu normals for the source and target datasets.\n", normals_src->points.size (), normals_tgt->points.size ());
std::cout << "normal_est" << std::endl;
// Compute FPFH features at each keypoint
PointCloud<FPFHSignature33>::Ptr fpfhs_src (new PointCloud<FPFHSignature33>), 
                              fpfhs_tgt (new PointCloud<FPFHSignature33>);
pcl::FPFHEstimation<PointXYZ, Normal, FPFHSignature33> fpfh_est;
fpfh_est.setInputCloud (keypoints_src);
fpfh_est.setInputNormals (normals_src);
fpfh_est.setRadiusSearch (1); // 1m
fpfh_est.setSearchSurface (src);
fpfh_est.compute (*fpfhs_src);

fpfh_est.setInputCloud (keypoints_tgt);
fpfh_est.setInputNormals (normals_tgt);
fpfh_est.setSearchSurface (tgt);
fpfh_est.compute (*fpfhs_tgt);

std::cout << "fpfh_est" << std::endl;


// Find correspondences between keypoints in FPFH space
CorrespondencesPtr all_correspondences (new Correspondences);
pcl::registration::CorrespondenceEstimation<FPFHSignature33, FPFHSignature33> est;
est.setInputCloud (fpfhs_src);
est.setInputTarget (fpfhs_tgt);
est.determineReciprocalCorrespondences (*all_correspondences);

// Reject correspondences based on their XYZ distance
CorrespondencesPtr good_correspondences (new Correspondences);
pcl::registration::CorrespondenceRejectorDistance rej;
rej.setInputCloud<PointXYZ> (keypoints_src);
rej.setInputTarget<PointXYZ> (keypoints_tgt);
rej.setMaximumDistance (1);    // 1m
rej.setInputCorrespondences (all_correspondences);
rej.getCorrespondences (*good_correspondences);

for (int i = 0; i < good_correspondences->size (); ++i)
std::cerr << good_correspondences->at (i) << std::endl;

// Obtain the best transformation between the two sets of keypoints given the remaining correspondences
Eigen::Matrix4f transform;
pcl::registration::TransformationEstimationSVD<PointXYZ, PointXYZ> trans_est;
trans_est.estimateRigidTransformation (*keypoints_src, *keypoints_tgt, *good_correspondences, transform);
std::cerr << transform << std::endl;


// Transform the data and write it to disk
PointCloud<PointXYZ> output;
transformPointCloud (*src, output, transform);
 
savePCDFileBinary ("source_transformed.pcd", output);

//--------------------------------------------------------------------------------------------------
// Initialize Sample Consensus Initial Alignment (SAC-IA)
pcl::SampleConsensusInitialAlignment<PointXYZ, PointXYZ, FPFHSignature33> reg;
reg.setMinSampleDistance (0.05f);
reg.setMaxCorrespondenceDistance (0.2);
reg.setMaximumIterations (1000);

reg.setInputCloud (src);
reg.setInputTarget (tgt);
reg.setSourceFeatures (fpfhs_src);
reg.setTargetFeatures (fpfhs_tgt);

// Register
pcl::PointCloud<pcl::PointXYZ> Final;   
reg.align (Final);

std::cout << "has converged:" << reg.hasConverged() << " score: " <<   // ?�확???�합?�면 1(True)
reg.getFitnessScore() << std::endl;

Eigen::Matrix4f transformation = reg.getFinalTransformation ();
std::cout << transformation << std::endl;                // 변???�렬 출력 




 return (0);
}
```



ICP의 점기반 정합과는 달리 위 방식에서는 키포인트를 선정하고 해당 키포인트의 특징 정보를 기반으로 정합을 수행 하였기에 특징기반 정합이라고도 합니다. 최소 3개의 대응점을 찾아 내면 변환행렬을 구할수 있다. 정확도를 높이기 위해서는 여러개의 대응값이 필요 하지만 매칭작업이 전체 포인트 클라우드가 아니라 키포인트값으로만 수행 되때문에 ICP보다는 속도가 빠르다. 일반적으로 특징기반으로 러프한 정합을 실시 하고, ICP로 다시 정밀하게 정합을 수행 합니다.



