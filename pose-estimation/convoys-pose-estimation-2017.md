[Vehicle Detection and Pose Estimation in Autonomous Convoys](https://brage.bibsys.no/xmlui/bitstream/handle/11250/2455922/Baardseth_Elisabeth.pdf?sequence=1&isAllowed=y): 2017, Master Thesis



## Abstract

Autonomous convoys, also known as platooning, are defined as a group of vehicles driving after one another autonomously. 

Acting as one single unit the gap between them can be significant reduced and hence the fuel costs compared to conventional driving.

본 연구에서는 반자율 주행을 다루고 있다. `In this thesis a semi-automatic two-vehicle military convoy is studied. `

첫번째 차는 사람이 운전하고, 두번째 차는 첫번째 차의 거리, 위치, 속도 정보에 기반하여 자율 주행 하는 모델 `Assume the first vehicle remain in control by humans. The second vehicle should then follow it’s track autonomously based on information about the relative distance, position and velocity of the first vehicle.`


논문의 목적은 차량 탐지와 자세 추정 이다. `The purpose of this thesis is to find and test methods for vehicle detection and pose estimation.`

활용 센서는 Lidar와 카메라이다. `The methods are mainly based on 3D point cloud data gathered by a LiDAR, but information from cameras are also used. `

센서 선정 사유 : The LiDAR is chosen because of it’s robustness in shifting light and weather conditions, but also because most of today’s research on the field is based on camera vision.

분류기 : Classifiers based on the leading vehicle’s geometrical properties was found suitable. 

2D/3D정보를 합치는 POSIT방식도 성능 좋음 `Also the POSIT method, which combines the imaging coordinates with the corresponding 3D properties provides good results. `

성능 결과 : Distance error was measured to around 15% and orientation deviation to ±4°. 

For driving that not require millimetre precision the conclusion it that the methods used are well suited. 

They also have room for improvements as there was several sources of uncertainty in this project.

