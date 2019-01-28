|논문명 |Efficient Online Segmentation for Sparse 3D Laser Scans |
| --- | --- |
| 저자\(소속\) | IGOR BOGOSLAVSKY\(\) |
| 학회/년도 | PFG2017, [논문](http://www.ipb.uni-bonn.de/pdfs/bogoslavskyi16pfg.pdf) |
| Citation ID / 키워드 | Range-image, VPL-16, Depth-clustering|
| 데이터셋(센서)/모델 | |
| 관련연구|[Fast Range Image-Based Segmentation of Sparse 3D Laser Scans for Online Operation, 2016](http://www.ipb.uni-bonn.de/pdfs/bogoslavskyi16iros.pdf)|
| 참고 | |
| 코드 |[Youtube](https://www.youtube.com/watch?v=xAAz3Zgkmy80), [깃허브](https://github.com/PRBonn/depth_clustering)|


# Efficient Online Segmentation for Sparse 3D Laser Scans


```
인지 시스템에서 사전-세그멘테이션은 이후 분석 작업을 위해 필수적인 요소 이다. `In most perception cues, a pre-segmentation of the current image or laser scan into individual objects is the first processing step before a further analysis is performed.`

본 논문에서는 바닦제거 + 물체 세그멘테이션을 다루고 있다. `In this paper, we present an effective method that first removes the ground from the scan and then segments the 3D data in a range image representation into different objects.`

본 논문에서는 2.5D range image를 이용하고 있다. `We explicitly avoid the computation of the 3D point cloud and operate directly on a 2.5D range image, which enables a fast segmentation for each 3D scan`
```


## 1 Introduction

인지 시스템에서 사전-세그멘테이션은 중요 하다. `Object segmentation from raw sensor data is especially relevant when mapping or operating in dynamic environments. In busy streets with cars and pedestrians, for example, the maps can be influenced by wrong data associations caused by the dynamic nature of the environment. A key step to enable a better reasoning about such objects and to potentially neglect dynamic objects during scan registration and mapping is the segmentation of the 3D range data into different objects so that they can be tracked separately, see (DEWAN et al., 2016)`

저채널(16ch)라이다 사용의 제약 `Sparser point clouds lead to an increased Euclidean distance between neighbouring points even if they stem from the same object. Thus, these sparse 3D points render it more difficult to reason about segments. The situation becomes even harder with the increase in distance between the object and the sensor.`

본 논문의 기여점 `The contribution of this paper is a robust method for separating ground from the rest of the scene and a fast and effective segmentation approach for 3D range data obtained from modern laser range finders such as Velodyne scanners.`

제안 바닥 제거 방법의 특징 : 곡선이 있거나, 평평하지 않아도 괜찮음, sub-sampling방법을 사용 안함 `To achieve the final segmentation, we first perform a robust ground separation which can detect ground fast and reliably. In contrast to several other approaches, the ground can have slight curvature and does not necessarily have to be entirely flat. We also do not use any kind of sub-sampling and decide for each pixel of the range image whether it belongs to ground or not. `

기존 연구에 바닥제거를 추가 하였음 `This paper extends our recently published conference paper on 3D range data segmentation (BOGOSLAVSKYI & STACHNISS, 2016). In this work, we added the robust ground removal and provide an extended experimental evaluation.`


## 2 Related Work

### 2.1 Segmenting objects from 3D point clouds 

There is substantial amount of work that targets acquiring a global point cloud and segmenting it off-line. 

Examples for such approaches are the works by ABDULLAH et al. (2014); ENDRES et al. (2009); GOLOVINSKIY
& FUNKHOUSER (2009); HEBEL & STILLA (2008); WANG & SHAN (2009). 

These segmentation methods have been used on a variety of different data produced by 3D range sensors or 2D lasers in push-broom mode. 

VELIZHEV et al. (2012) focus on learning the classes of the objects and detecting them in huge point clouds via a voting-based method. 

These point clouds can be large, and the work by HACKEL et al. (2016) targets the runtime along with the quality of classification. 

본논문의 차별점 : In contrast with these works, we focus on the segmentation of range data that comes from a 3D laser scanner such as a Velodyne that provides a 360 degree field of view in a single scan and is used for online operation on a mobile robot. 
- Additionally, we target segmentation of a scene without the knowledge about the objects in it and without any prior
learning and not using complex features. 


분석 논문 추천 : For a comprehensive analysis of methods that perform supervised scene segmentation we refer the reader to WEINMANN et al. (2015).


### 2.2 Ground removal

Ground removal is an often used pre-processing step and is therefore well-discussed in the literature. 

RANSAC기반 방법 : There is a number of papers that use RANSAC for fitting a plane to the ground and removing points that are near this plane such as the work by OSEP ˇ et al. (2016). 

시멘틱 세그멘테이션 방법 : Another prominent method of ground detection is a side-product of full semantic segmentation of the scene, where all parts of the scene get a semantic label. 
- 바닥도 하나의 label로 분류됨 : The ground is then segmented as one class; for more details we refer the reader to the papers by HERMANS et al. (2014) and BANSAL et al. (2009). 

높이 정보 이용 : A couple of approaches use a 2D-grid and analyse the heights of the points that fall into its bins, taking decisions about points being parts of the ground based on this information. 

The decisions can be taken based on the inclination of lines between consecutive cells as in works by PETROVSKAYA &
THRUN (2008); LEONARD et al. (2008) or by analysing the height above the lowest local point as in works by GORTE et al. (2015); BEHLEY et al. (2013).

### 2.3 Segmentation techniques 

Segmentation techniques for single scans without requiring additional information can be divided into three groups. 


#### A. 3D domain

The first group, represented by the works by DOUILLARD et al. (2011,2014), performs the segmentation in the 3D domain by defining sophisticated features that explain the data in 3D or by removing the ground plane and segmenting the clouds with a variant of a nearest neighbour approach as shown by CHOE et al. (2012) and KLASING et al. (2008). 

Feature-based approaches, while allowing for accurate segmentation, are often comparably time-consuming and may limit the application for online applications to a robot with substantial computational resources.

#### B. projecting 3D points onto a 2D grid positioned


The second group focuses on projecting 3D points onto a 2D grid positioned on the ground plane. 

The segmentation is then carried out on occupied grid cells as in BEHLEY et al. (2013); HIMMELSBACH et al. (2010); KORCHEV et al. (2013); STEINHAUSER et al. (2008). 

These algorithms are fast and suitable to run online. 

Quite often, however, they have a slight tendency to under-segment the point clouds, i.e. multiple objects may be grouped as being one object if they are close to each other. 

This effect often depends on the choice of the grid discretisation, so that the grid width may need to be tuned for individual environments. 

Additionally, some of these approaches can suffer from under-segmenting objects in the vertical direction.


### C. range image

The third group of approaches performs the segmentation on a range image and our approach belongs to this group of techniques. 

For example, MOOSMANN et al. (2009) and MOOSMANN (2013) use a range image to compute local convexities of the points in the point cloud. 

In contrast to that, our approach avoids computing complex features and, thus, is easier to implement, runs very fast and produces comparable results. 

We therefore believe that our approach is a valuable contribution to a vast and vibrant field of 3D point cloud segmentation, and consequently we will contribute our approach to the open source ROS community by providing the source code for our implementation.


### D. 그외 - RGBD

There are also several works (PYLVANAINEN et al., 2010; STROM et al., 2010) that perform segmentation on RGBD data acquired from a LIDAR registered with a camera. 

Registering one or multiple cameras with the laser scanner requires a more sophisticated setup and the segmentation
becomes more demanding. 

Using both cues may improve the results but it is seldom possible at speeds faster than the frame rate. 

Therefore, we focus on segmenting unknown objects from pure 3D range data not requiring any additional visual or intensity information.


Visual information is not the only information that aids segmentation. Temporal information and tracking are also shown to be useful to enhance the segmentation performance by FLOROS & LEIBE (2012) and TEICHMAN & THRUN (2012). While the benefit of using the information about the moving objects is clear, we show that it is possible to perform a fast and meaningful segmentation on single scans even without relying on temporal integration.










