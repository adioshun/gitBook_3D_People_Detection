# tmp

## 특징과 기술자

특징\(=관심점/관심영역\)들의 주변 또는 내부를 들여다 보고 풍부한 정보를 추출 한다.

* 기술자 = 추출된 정보 / 추출하는 알고리즘 
* 기술자 = 특징의 성질/특성을 기술 하는것 
* 기술자 =벡터로 표현 = 특징 벡터..라고 불리기도 함 \(패턴인식 분야\)
  * 컴퓨터 비젼 분야는 특징벡터 보다 기술자라는 표현을 선호 

관심점에서 기술자 추출 하기

* SIFT 
* 이진 기술자 

관심영역에서 기술자 추출 하기

* 모멘트 : 화소/화소의 명암값의 통계적 분포 
* 모양\(면적/둘레길이\) 
* 푸리에 기술자 : 푸리에 변환 후 주파수 공간에서 기술자 추출 
* 텍스처\(일정한 패턴의 반복\) 추출 : \(이 분류에 속하는지 확인 필요\) :

차원을 축소 하여 기술자 추출 하기

* PCA : 고차원 벡터에서 정보 손실을 최소화 하여 저차원의 벡터를 추출 = 특징 벡터와 비슷 
* PCA-SIFT : 3,042차원을 -&gt; 20차원의 특징 벡터로 줄임 

### 1. 특징점\(Keypoint\)

image의 특징을 잘 나타내줄 수 있는 부분 \(eg. 꼭지점, 끝점\)

#### 1.1 꼭지점 특징점을 찾는 방법 : Corner Detector

가우시안 필터와 미분값 사용

![](https://t1.daumcdn.net/cfile/tistory/233F574354EA880E27)

> a의 필터를 b에 적용하면 코너를 찾을수 없으므로 크기\(Scale\)를 크게 하여 C처럼 탐지 가능

**A . 방법 1 : 필터 사이즈 키워서 Scale을 크게 - SURF Descriptor**

**B.  방법 2 : 이미지를 축소 하여서 Scale을 크게  - SIFT Descriptor**

### 2. 특징\(Feature\)  = 특징 기술자\(Feature Descriptor\)

특징점의 지역적 특성을 설명

특징점간 비교를 가능하게 함

특징 기술자에게 필요한 특성

* 분별력 : 서로 다른 점의 기술자는 분별 가능해야 함 
* 불변 : 회전, 축소, 변형 등이 발생해도 변하지 않는 성질 
* 크기 : 데이터의 크기가 작을수록 좋음

영상 Feature : SIFT, HOG, Haar, Ferns, LBP, MCT

[https://darkpgmr.tistory.com/116](https://darkpgmr.tistory.com/116)

[http://166.104.231.121/ysmoon/mip2017/lecture\_note/%EC%A0%9C6%EC%9E%A5-%EC%B6%94%EA%B0%80.pdf](http://166.104.231.121/ysmoon/mip2017/lecture_note/%EC%A0%9C6%EC%9E%A5-%EC%B6%94%EA%B0%80.pdf)

{% embed url="http://www.pointclouds.org/assets/uploads/cglibs13\_features.pdf" %}







