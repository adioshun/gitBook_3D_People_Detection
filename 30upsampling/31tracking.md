## 4. Obstacle tracking

vehicle odometry 정보 추가적으로 활용

연속적으로 얻어지는 인식 결과를 바탕으로 이러한 오탐을 줄일 수 있는데, 이룰 추적기법(Tracking)이라고 한다.
- 이전 프레임의 포인트를 현재 프레임의 물체에 자동으로 누적하여 물체 해상도 및 보행자 분류 성능을 향상

### 4.1 단일 칼만 필터 기반

일반적으로 단일 객체 추적에 적용

Teichman의 연구[7]에선 단일 칼만 필터기반 추적기법을 이용하여 데이터를 누적시켜 분류의 성능을 높이고자 했다.

하지만 물체가 몇 개 없는 단조로운 시나리오에서 전후 프레임 간에 일치되는 물체관계를 이미 알고 있다고 가정하여, 특별한 데이터 연계 방법 없이 해당 물체 정보들을 단일 칼만 필터만 사용해추적 및 누적하여 포인트 수 증가를 유도하였다.

```
[7] A. Teichman, J. Levinson and S. Thrun, “Towards 3D Object Recognition via Classification of Arbitrary Object Tracks,” International Conference on Robotics and Automation, Shanghai, China, May 2011.
```

### 4.2 GM-PHD 필터

Data Associateion까지 고려한 다중 객체 추적에 적용

이전 프레임의 포인트를 현재 프레임의 물체에 자동으로 누적하여 물체 해상도 및 보행자 분류 성능을 향상

```
이연주, 서승우, "GM-PHD 필터를 이용한 보행자 탐지 성능 향상 방법", 서울대, 2015 
```
