# [Object detection in 3D point clouds](https://www.mi.fu-berlin.de/inf/groups/ag-ki/Theses/Completed-theses/Master_Diploma-theses/2016/Damm/Master-Damm.pdf)


## 2. Ground Segmentation

지면 포인트들을 그룹화  
배경을 제거 하기 위한 작업

### 2.1 RANSAC plane filtering

RANSAC은 주어진 데이터에서 반복적으로 샘플을 취하여 전체 데이터를 분석하는 방법론이다\[7,8\]. 이는 모든 데이터를 이용하지 않기 때문에 비교적 빠르고, 잡음에 강한 모습을 보인다

* 센서에서 측정한 데이터로부터 벽면과 같은 직선을 특징으로 뽑아내기 위해 RANSAC을 용용

* 지면을 제거하게 되면 각각의 오브젝트 들이 서로 연결되지 않고 떨어지기때문에 segmentation이 쉬워집

* 제거를 위해서 바닥은 평평\(`even plane`\)하거나, 약간의 경사가 있다고 가정 한다. \(`small elevations like curbside`\)

By modeling the table as a plane, we can remove it from the point cloud.

* Segmentation of the table from everything else 
* to identify the table.

RANSAC은 포인트들이 특정 Model에 속하는지 아닌지 식별 한다. `Random Sample Consensus \(RANSAC\) is used to identify points in the dataset that belong to a particular model.`

RANSAC은 포인트들은 두 종류로 구분 된다고 가정 한다: inlier, outlier ` It assumes that all of the data in a dataset is composed of both inliers and outliers,`
- where inliers can be defined by a particular model with a specific set of parameters, 
- Outliers if that model does not fit then it gets discarded.


---

# [RANSAC 알고리즘을 이용한 지상 라이다 포인트 클라우드의 세그먼테이션](http://www.dbpia.co.kr/journal/articleDetail?nodeId=NODE01558668&language=ko_KR)

> RANSAC 알고리즘을 이용한 지상 라이다 포인트 클라우드의 세그먼테이션, 2009, 정성수 (파라미터 T 구하는법 기술)

## 2. 이론적 배경 및 실험 방법

### 2.1 RANSAC 알고리즘

Fischler(1981)에 의해 제안된 RANSAC(RANdom Sample And Consensus) 알고리즘은 노이즈나 이상 치(outlier)가 많은 데이터로부터 특정한 모델에 부합하는 정상치(inlier)를 찾는 알고리즘이다. 

이 알고리즘은 크게 두 단계로 구성된다. 
- 첫 단계는 샘플링을 통한 가설 설정 단계로서, 
    - 전체 데이터에서 모델의 파라메터 계산에 필요한 최소한의 개수의 데이터인 MSS(Minimal Sample Set)를 추출하여 모델을 결정하고 
    - 이 모델 과의 오차가 일정 수준 이하에 있는 데이터를 추출한다. 추출된 데이터는 잠재적으로 정상치로 가정한다.
- 위의 단계에서 추출된 정상치 들은 평가 단계를 거치게 되는데 추출된 데이터들을 평가함수를 이용해 그 데이터가 얼마나 정상치로서 적합한지 평가한다. 
    -  일반적으로 평가함수는 정상치 내 포인트의 개수를 함수로 서 사용한다. 
    - 새로 추출된 정상치의 평가 결과가 이 전 단계까지 정상치로 간주되었던 데이터의 결과보다 좋으면 이 데이터를 새로운 정상치로 지정하며 평가값이 낮을 경우는 추출된 데이터를 기각한다. 
    
이 두 단계를 보다 높은 평가값을 갖는 데이터 들을 찾을 확률이 일정 수준 이하가 될 때 까지 반복한다.

> [cpp 코드 ](http://blog.daum.net/pg365/242)

---

# 다중 저채널 라이다와 카메라의 센서 융합에 의한 차량 객체 검출 알고리즘

### 2.2 지면 모델

모든 영역을 센서 중심으로 2차원 극좌표 격자로 나눈 뒤, 각 격자에 속하는점들 중 최저점을 뽑아낸다. 반지름 방향으로 각 격자에서 뽑힌 최저점들과 여러 조건을 바탕으로 지면 모델을 결정하게 되고, 이 지면 모델로부터 일정 거리 이내의 점들을 지면점, 그렇지 않은 점들을 비 지면점으로구분

```
[8] H. Himmelsbach, Felix v. Hundelshausen and H.-J. Wuensche, “Fast Segmentation of 3D Point Clouds for Ground Vehicles,” Intelligent Vehicles Symposium, San Diego, CA, USA, June 2010.
```

지면 제거 및 클러스터링은 Yani의 DoN\(Difference of Normals\) 알고리 즘\[2\]을 적용했다. DoN 알고리즘에 모든 포인트 클라우드에 적용한다면 양이 워낙 많기 때  
문에 처리 속도가 빠르지 못한다. 따라서 실시간에는 적합하지 않지만 본논문은 전방에 대해서만 객체 검출을 진행 했고, 또한 근거리는 제외 대상 이고 일반적으로 객체는 높은 곳에 있지 않기 때문에 많은 양의 포인트 클라우드를 제외시킬 수 있어 실시간으로 사용 가능하다.



---

### 2.3 RGB filter

Color filtering is one of the examples where the presence of RGB sensors
in Kinect® v2 is expressed, in a robust and effective way, certain color
spots can be removed by defining the correct RGB range.
However, it is important to keep in mind that objects of interest are not of
the same color as the background because the filtration will be the same.
It is recommended that the background be black or white because only
the narrow RGB intervals are excluded, leaving a larger RGB space for
objects of interest.
As a part of the work, it was decided to remove the background with RGB
filter for faster processing, stable action on each image and without
affecting the amount of objects on the background.


