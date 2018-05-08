## 2. Ground Segmentation

지면 포인트들을 그룹화
배경을 제거 하기 위한 작업 (Downsampling의 한 분류??)

### 2.1 random sample consensus algorithm (RANSAC)
제거를 위해서 바닥은 평평(`even plane`)하거나, 약간의 경사가 있다고 가정 한다. (`small elevations like curbside`)
```
1981 by Martin A. Fischler and Robert C. Bolles [FB81].
```
Modifications of RANSAC : MLESAC algorithm, local optimized RANSAC (LO-RANSAC), randomized RANSAC algorithm (RRANSAC)


> 상세한 내용은 [Object detection in 3D point clouds](https://www.mi.fu-berlin.de/inf/groups/ag-ki/Theses/Completed-theses/Master_Diploma-theses/2016/Damm/Master-Damm.pdf)의 20page참고

### 2.2 지면 모델

모든 영역을 센서 중심으로 2차원 극좌표 격자로 나눈 뒤, 각 격자에 속하는점들 중 최저점을 뽑아낸다. 반지름 방향으로 각 격자에서 뽑힌 최저점들과 여러 조건을 바탕으로 지면 모델을 결정하게 되고, 이 지면 모델로부터 일정 거리 이내의 점들을 지면점, 그렇지 않은 점들을 비 지면점으로구분

```
[8] H. Himmelsbach, Felix v. Hundelshausen and H.-J. Wuensche, “Fast Segmentation of 3D Point Clouds for Ground Vehicles,” Intelligent Vehicles Symposium, San Diego, CA, USA, June 2010.
```



지면 제거 및 클러스터링은 Yani의 DoN(Difference of Normals) 알고리 즘[2]을 적용했다. DoN 알고리즘에 모든 포인트 클라우드에 적용한다면 양이 워낙 많기 때
문에 처리 속도가 빠르지 못한다. 따라서 실시간에는 적합하지 않지만 본논문은 전방에 대해서만 객체 검출을 진행 했고, 또한 근거리는 제외 대상 이고 일반적으로 객체는 높은 곳에 있지 않기 때문에 많은 양의 포인트 클라우드를 제외시킬 수 있어 실시간으로 사용 가능하다.

> 다중 저채널 라이다와 카메라의 센서 융합에 의한 차량 객체 검출 알고리즘
