# A New Upsampling Method for Mobile LiDAR Data

> Ruisheng Wang, Jeff Bach, Jane Macfarlane

## 1.1. Related Work

There appears to be relatively little work in using co-registered intensity images to upsample range data, at least in comparison to pure image-based super-resolution, e.g.,[9, 10, 11]. 

```
[9] S. Farsiu, M. Robinson, M. Elad, and P. Milanfar. Fast and robust multiframe super resolution. IEEE Transactions on Image Processing, 13(10):1327–1344, Oct. 2004.
[10] S. Borman and R. L. Stevenson. Super-resolution from image sequences - a review. Proc. Midwest Symp. Circuits and Systems, 5, 1998.
[11] M. Irani and S. Peleg. Improving resolution by image registration. CVGIP: Graph. Models Image Process., 53(3):231--239, 1991.
```

초창기 연구 중 하나는 MRF를 이용한 방법이다. `One of the first attempts reported was basedon Markov Random Fields (MRFs) [1, 2, 3, 4]. `

- 가정사항 : 색이나 밝기 변화로 깊이 단절이 종종 발생한다. 문제는 깊이 단절은 색 채널에서는 보이지 않는다. `A common assumption here is that depth discontinuities in a scene often co-occur with color or brightness changes within the associated camera images. The problem occurs when a depth discontinuity is not visible in the color channel.`


```

[1] Luz A. Torres-Méndez and Gregory Dudek. RangeSynthesis for 3D Environment Modeling In 2002 IEEEWorkshop on Applications of Computer Vision, pp. 231--236, Orlando, FL, USA
[2] Luz Abril Torres-Méndez and Gregory Dudek.Reconstruction of 3D models from intensity images andpartial depth. Proceeding American Association forArtificial Intelligence (AAAI), 2004, pp. 476-481.
[3] J. Diebel and S. Thrun. An application of markov randomfields to range sensing. In Proceedings of Conference onNeural Information Processing Systems (NIPS),Cambridge, MA, 2005. MIT Press.
[4] Zhaoyin Jia, Yao-Jen Chang, Tzung-Han Lin, and TsuhanChen, "Dense 3D-Point Estimation Based on SurfaceFitting and Color Information," 2009 Western New YorkImage Processing Workshop, Henrietta, NY, USA, Sept.25, 2009.

```


Yang et al. [5] proposed an **iterative bilateral filtering method** for enhancing the resolution of range images. 

- The authors also compared their approach with MRF, showing that this method allows for sub-pixel accuracy. 

```
[5] Q. Yang, R. Yang, J. Davis, and D. Nist´er. Spatial-depth super resolution for range images. In CVPR, 2007.
```

Andreasson et al. [6] compared **five different interpolation schemes** with the MRF method [3] and summarized four different metrics for confidence measures of interpolated range data. (5개의 일반적 방식이 뭔지 참고 필요)
- 가정사항 : The assumption in their approach is similar to that of the MRF method which uses color similarity as an indication of depth similarity. 

```
[6] H. Andreasson, R. Triebel, and A. J. Lilienthal. Noniterative Vision-based Interpolation of 3D Laser Scans, volume 76 of Studies in Computational Intelligence, pages 83–90. Springer, Germany, Aug 14 2007.
```

In [8] points are projected onto segmented color images, and bilinear interpolation is used to compute the depth value of the grid samples that belong to the same region. 

```
[8] V. Garro, C. Dal Mutto, P. Zanuttigh, and G. M. Cortelazzo. A novel interpolation scheme for range data with side information. In Proc. of CVMP Conf., London, UK, November
```

[7] addresses the problem of upsampling range data in dynamic environments based on a Gaussian framework.

```
[7] Dolson, J.; Jongmin Baek; Plagemann, C.; Thrun, S.Upsampling range data in dynamic environments. IEEE Conference on Computer Vision and Pattern Recognition (CVPR), 2010.
```


슈퍼 리솔루션 방법 이요 `There is some work that does super resolution from depth data only. `
- 기본적이르 이 방식은 다른 뷰 포인트를 가지는 스트레오 카메라의 depth map을 이용한다. `Basically the goal is to enhance the resolution by using depth maps of a static scene that were acquired from slightly displaced viewpoints [12, 13].`


--- 

# Upsampling

## 1. List

- 깃북 : [칼만필터](https://adioshun.gitbooks.io/deep_drive/content/introfusion.html)

## 2. Paper

- [LiDAR Based Real Time Multiple Vehicle Detection and Tracking](https://waset.org/publications/10004678/lidar-based-real-time-multiple-vehicle-detection-and-tracking): 3.D Data Association and Tracking

- ~~A New Upsampling Method for Mobile LiDAR Data~~, Ruisheng Wang, Jeff Bach, Jane Macfarlane, NAVTEQ Corporation
    - 카메라 + Lidar연계, upsample mobile LiDAR data using panoramic images


- Upsampling Range Data in Dynamic Environments, Jennifer Dolson
    - 카메라 + Lidar 연계 


- LidarBoost: Depth Superresolution for ToF 3D Shape Scanning, Sebastian Schuon(스탠포드)
    - 스트레오 카메라활용 combines several low resolution noisy depth images of a static scene from slightly displaced viewpoints, and merges them into a high-resolution depth image


- [Semantically Guided Depth Upsampling](https://arxiv.org/abs/1608.00753), Nick Schneider, 2016
    - 이지미 정보 활용, Lidar 업샘플링 기법 upsampling of sparse depth(Lidar) data, guided by high-resolution **imagery**.


- DenseLidarNet : 카메라 + Lidar, [[깃허브]](https://github.com/345ishaan/DenseLidarNet)

- [Extended Object Tracking: Introduction, Overview and Applications](https://arxiv.org/abs/1604.00970): arXiv2017

- [3D-LIDAR Multi Object Tracking for Autonomous Driving](https://repository.tudelft.nl/islandora/object/uuid%3Af536b829-42ae-41d5-968d-13bbaa4ec736): 석사 학위 2017

- [Robust Real-Time Tracking Combining 3D Shape, Color, and Motion](http://davheld.github.io/DavidHeld_files/ijrr_tracking.pdf): 2015, 28page



## 3. Article (Post, blog, etc.)



## 3. Tutorial (Series, )



## 4. Youtube



## 6. Material (Pdf, ppt)



## 7. Implementation (Project)

[Object (e.g Pedestrian, vehicles) tracking by Extended Kalman Filter (EKF), with fused data from both lidar and radar sensors.](https://github.com/JunshengFu/tracking-with-Extended-Kalman-Filter)


## 8. Research Group / Conference 

--- 

이미지 기반 트래킹 

- [A Review on Object Detection and Tracking Methods](https://ijrest.net/downloads/volume-2/issue-1/pid-21201506.pdf): 2015





- [A Survey on Object Detection and Tracking Methods](https://pdfs.semanticscholar.org/25a6/c5dff9a7019475daa81cd5a7f1f2dcdb5cf1.pdf): 2014

    ![image](https://user-images.githubusercontent.com/17797922/40040388-0ac64296-5855-11e8-8b14-5b15cc508410.png)


- [A Review of Object Detection and Tracking Methods](http://www.ijaerd.com/papers/finished_papers/A%20Review%20of%20Object%20Detection%20and%20Tracking%20Methods-IJAERDV04I1045913.pdf):2017

    ![image](https://user-images.githubusercontent.com/17797922/40040689-303d1cce-5856-11e8-86c5-07293af6f9ec.png)

- [Moving Object Tracking of Vehicle Detection”: A Concise Review](http://docplayer.net/16497156-Moving-object-tracking-of-vehicle-detection-a-concise-review.html): 2015
    - Tracking Methods
        - Region-based tracking methods
        - contour tracking methods
        - 3D Model based tracking methods
        - Feature based tracking methods
        - Color and Pattern based tracking methods
    


