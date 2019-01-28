|논문명 |Efficient Online Segmentation for Sparse 3D Laser Scans |
| --- | --- |
| 저자\(소속\) | IGOR BOGOSLAVSKY\(\) |
| 학회/년도 | 2017, [논문](http://www.ipb.uni-bonn.de/pdfs/bogoslavskyi16pfg.pdf) |
| Citation ID / 키워드 | Range-image, VPL-16, Depth-clustering|
| 데이터셋(센서)/모델 | |
| 관련연구||
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

