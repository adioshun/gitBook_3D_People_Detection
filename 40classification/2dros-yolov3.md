# 2D\_ROS\_YOLOv3

## [YOLO ROS: Real-Time Object Detection for ROS](https://github.com/leggedrobotics/darknet_ros)

```text
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

```text
  camera_reading:
    topic: /camera/image_color
    queue_size: 1
```

라벨 : `~/catkin_ws/src/darknet_ros/darknet_ros/config/yolov3.yaml`

학습 설정 : `~/catkin_ws/src/darknet_ros/darknet_ros/yolo_network_config/cfg/yolov3.cfg`

실행 : `~/catkin_ws/src/darknet_ros/darknet_ros/launch`

* darknet\_ros.launch : YOLOv2-tiny
* yolo\_v3.launch : YOLOv3

> ros.yaml과 yolov3.yaml은 launch파일에서 수정

## [PyTorch-YOLOv3](https://github.com/eriklindernoren/PyTorch-YOLOv3)

```text
@article{yolov3,
  title={YOLOv3: An Incremental Improvement},
  author={Redmon, Joseph and Farhadi, Ali},
  journal = {arXiv},
  year={2018}
}
```

> [PyTorch 로 YOLO v3 구현한 것을 Colaboratory 에서 돌려보자](https://medium.com/@hyunseokjeong/pytorch-%EB%A1%9C-yolo-v3-%EA%B5%AC%ED%98%84%ED%95%9C-%EA%B2%83%EC%9D%84-colaboratory-%EC%97%90%EC%84%9C-%EB%8F%8C%EB%A0%A4%EB%B3%B4%EC%9E%90-5b99e847b420)

wget [https://raw.githubusercontent.com/nicewook/datascience\_exercise/master/PyTorch\_YOLOv3.ipynb](https://raw.githubusercontent.com/nicewook/datascience_exercise/master/PyTorch_YOLOv3.ipynb)

뭐 구현하기 따라 다른데 GPU를 최대한 활용하시려면 실시간 이미지를 여러장 모아서 배치 하나 만드셔서 forward하시면되고 아니면 한장한장 하셔야되고요. Dimension 첫번째에 1 추가하셔서 만드셔야해요. 1추가 한후에 torch.cat으로 합쳐서 batch로 만들어서 쓰셔도되고요

unsqueeze였던거같은데 dimension추가하는 함수가 있습니다.

x = torch.from\_numpy\(transform\(img\)\[0\]\).permute\(2, 0, 1\) x = Variable\(x.unsqueeze\(0\)\)

