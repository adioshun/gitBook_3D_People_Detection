# Clustering 

> 컴퓨터 비젼 알고리즘으로 2D 이미지 대상 설명임 

## 2. Connected-component labeling

CCL 알고리즘은 이미지를 일정한 구역들로 나누고 인접한 구역들끼리의 유사성을 판단하여 같은 label로 묶음

인접 기준 : 픽실 기준 
- 4연결 : 좌우상하
- 8연결 : 좌우상하 + 각 대각선 



연결요소라벨링(connected component labeling)는 크게 두가지 알고리즘이 존재
- 재귀알고리즘
- 반복알고리즘 

### 2.1 동작 과정 

#### A. 재귀 알고리즘 
TBD..

#### B. 반복 알고리즘 

절차 
- 1차(위 - 아래) : 객체에 라벨을 부여하고 라벨에 대응하는 등가표(eqivalent table)를 작성 
- 2차(왼쪽 - 오른쪽) :등가표를 적당히 조정하고(resolve) 이에 맞추어서 이미지의 객체에 부여하는 라벨 번호도 조정

##### 가. 1차 상세 





논문[8]에서는 데카르트 좌표계에서 2차원 격자로 나눠 CCL을 적용



> 본 연구에서는 앞서 지면 추출 단계에서 생성된 2차원 극좌표 격자에 CCL을 적용하여 기존 결과를 재사용하는 방법을 취하였다. 센서특성에 더 적합한 방법이며, 재사용성을 이용해 메모리나 연산시간 면에서 더 효과적이다.




---

## 참고 

- [A Review of World’s Fastest Connected Component Labeling Algorithms: Speed and Energy Estimation](https://hal.inria.fr/hal-01081962/document): 2014

- [8] H. Himmelsbach, Felix v. Hundelshausen and H.-J. Wuensche, “Fast Segmentation of 3D Point Clouds for Ground Vehicles,” Intelligent Vehicles Symposium, San Diego, CA, USA, June 2010.


- 코드 : https://www.codeproject.com/Articles/336915/Connected-Component-Labeling-Algorithm

- [[VB.Net 영상처리] 일지 10 : Connected component Labeling....영상 인식의 세번째](http://m.blog.daum.net/shksjy/198?np_nil_b=2): 추천 


- Connected Component Labelling
    - [Pixel neighbourhoods and connectedness](http://aishack.in/tutorials/pixel-neighbourhoods-connectedness/)
    - [Connected Component Labelling](http://aishack.in/tutorials/connected-component-labelling/)
    - [Labelling connected components - Example](http://aishack.in/tutorials/labelling-connected-components-example/)


- [Connected component labeling](https://blogs.mathworks.com/steve/2007/05/11/connected-component-labeling-part-5/): 매트랩