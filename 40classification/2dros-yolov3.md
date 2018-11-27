# [YOLO ROS: Real-Time Object Detection for ROS](https://github.com/leggedrobotics/darknet_ros)

```
  @misc{bjelonicYolo2018,
    author = {Marko Bjelonic},
    title = {{YOLO ROS}: Real-Time Object Detection for {ROS}},
    howpublished = {\url{https://github.com/leggedrobotics/darknet_ros}},
    year = {2016--2018},
  }
```
 
```python   
cd catkin_ws/src/
git clone --recursive https://github.com/leggedrobotics/darknet_ros.git
cd  ..
catkin_make -DCMAKE_BUILD_TYPE=Release

```

설정 파일 

입력 topic : `~/catkin_ws/src/darknet_ros/darknet_ros/config/ros.yaml`
```
  camera_reading:
    topic: /camera/image_color
    queue_size: 1
```

라벨 : `~/catkin_ws/src/darknet_ros/darknet_ros/config/yolov3.yaml`


학습 설정 : `~/catkin_ws/src/darknet_ros/darknet_ros/yolo_network_config/cfg/yolov3.cfg`

실행 : `~/catkin_ws/src/darknet_ros/darknet_ros/launch`
- darknet_ros.launch : YOLOv2-tiny
- yolo_v3.launch : YOLOv3

> ros.yaml과 yolov3.yaml은 launch파일에서 수정 

