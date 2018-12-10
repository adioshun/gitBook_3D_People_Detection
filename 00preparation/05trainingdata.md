# Open Data
## 1. Lidar



## 2. image

[Indoor Multi-Camera Pedestrian Datasets](https://www.tugraz.at/institute/icg/research/team-bischof/lrs/downloads/MultiCam/)

[Apollo Scape](http://apolloscape.auto/index.html): [논문](https://arxiv.org/abs/1803.06184), [정리](https://medium.com/syncedreview/baidu-apollo-releases-massive-self-driving-dataset-teams-up-with-berkeley-deepdrive-5e785ab4053b)

---


# 툴 활용 방안

## 1. 3D ANNOTATION TOOL


## 2. 시뮬레이터 툴

### 2.1 Velodyne Simulator

- [Velodyne Simulator](https://github.com/LCAS/velodyne_simulator)

### 2.2 Google 3D Warehouse

[Object Recognition in 3D Point Clouds Using Web Data and Domain Adaptation](https://pdfs.semanticscholar.org/ecc2/ea05877d720b725fb89bc3b0586a51cabdc7.pdf): Lai, K, 2011


## 9. IDEA

- [People font](https://flowingdata.com/2018/02/09/people-font/)를 학습용 데이터로 사용하는것은 어떨까?


---

# 업체 


![image](https://user-images.githubusercontent.com/17797922/39740494-c7ecef8a-52d0-11e8-9c18-7de1aae07b69.png)


|구분|이름|특징|
|-|-|-|
|Tool(2D)|[LanelImg](https://github.com/tzutalin/labelImg)|TFRecord로 변경 가능 |
|Tool(2D)|[FIAT (Fast Image Data Annotation Tool)](https://github.com/christopher5106/FastAnnotationTool)||
|Tool(2D)|[Yolo_mark](https://github.com/AlexeyAB/Yolo_mark)|YOLO용, Windows & Linux GUI|
|Tool(2D)|sloth||
|Took(2D)|[Supervisely-Smart Tool](https://supervise.ly/smart-tool/)|웹기반|
|Tool(2D)|[The Human Annotation Tool](https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/shape/hat/)|사람 Part별 라벨링, 2011|
|Tool(3D)|[SceneNN](http://people.sutd.edu.sg/~saikit/projects/sceneNN/)|RGB-D, 세그멘테이션, 라벨링|
|Tool(3D)|[3D ANNOTATION TOOL](http://strands.readthedocs.io/en/latest/annotation_tool_kth/annotation-tool.html)|ROS, PCD, XML, 2015|
|Tool(3D)|[L-CAS](https://github.com/yzrobot/cloud_annotation_tool)|**추천 방안**|
|Tool(3D)|[]()||
|Service|[CrowdFlower](https://www.crowdflower.com/)|해외업체, |
|Service|[CrowdAI ](https://crowdai.com/)|해외업체, 항공사진등에서 물체, 도로 라벨링|
|Service|[Amazon’s Mechanical Turk](https://www.mturk.com/mturk/welcome)|해외업체|
|Service|[supervise.ly](https://supervise.ly/company)|해외업체|
|Service|[crowdworks](http://www.crowdworks.kr)|국내업체|
|Service|[playment](https://playment.io/image-annotation/)|해외업체|
|Service|[Mighty AI](https://mty.ai/)||

- [List of manual image annotation tools](https://en.wikipedia.org/wiki/List_of_manual_image_annotation_tools): 위키 정리
- [Annotating Large Datasets with the TensorFlow Object Detection API](http://andrew.carterlunn.co.uk/programming/2018/01/24/annotating-large-datasets-with-tensorflow-object-detection-api.html): 딥러닝 기 라벨링 기법

# 협력 업체 확용 방안

### CrowdFlower

- 데이터 수집, 2D 라벨링
- 3D 라벨링 문의 메일 발송

### Amazon’s Mechanical Turk

- 데이터 수집, 2D 라벨링
- 3D 라벨링 문의 메일 발송

### supervise.ly

- 데이터 수집, 2D 라벨링
- 3D 라벨링 문의 메일 발송 -> 현재 지원 안함, 향후 지원 예정

### crowdworks

- 중계 업체 (사용자-요청자) : 요청자가 가격을 제시, 사용자가 원하는 작업 선택 진행
- 성별 판단의 경우 약 200,000이미지를 112명이 3,300,000원에 17일간 작업 진행 (이미지당 5원)
- 강남삼성사옥 건너편 강남역 2번출구 메리츠타워 16층입니다


