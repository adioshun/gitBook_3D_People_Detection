# [Seoul Robotics Didi Challenge 결과물](https://github.com/hb0702/Didi_challenge_2017_Python)

> [ROS버젼: Didi_Challenge_2017_ROS](https://github.com/hb0702/Didi_Challenge_2017_ROS)

## 1. Train 

```python

model.fit_generator(             # generator로 생성한 데이터로 학습시 (보통은 model.fit())
                   generator, # 훈련데이터셋을 제공할 제네레이터를 지정합니다. (하단 추가설명)
                   steps_per_epoch=steps_per_epoch, # 한 epoch에 사용한 스텝 수를 지정합니다. 
                                                   #총 45개의 훈련 샘플이 있고 배치사이즈가 3이므로 15 스텝으로 지정합니다.
                   epochs=5, # 전체 훈련 데이터셋에 대해 학습 반복 횟수를 지정합니다. 
                             #5번을 반복적으로 학습시켜 보겠습니다.
                   callbacks=[checkpointer, logger])
                   #validation_data : 검증데이터셋을 제공할 제네레이터를 지정합니다. 
                                       #본 예제에서는 앞서 생성한 validation_generator으로 지정합니다.
                   #validation_steps : 한 epoch 종료 시 마다 검증할 때 사용되는 검증 스텝 수를 지정합니다. 
                                       #총 15개의 검증 샘플이 있고 배치사이즈가 3이므로 5 스텝으로 지정합니다.

generator=train_batch_generator(
                    list_of_lidar, 
                    list_of_gtbox, 
                    batch_size = batch_size, 
                    data_augmentation = True, 
                    width = 256, 
                    height = 64,
                    car_index = car_index, 
                    undersample = False, 
                    percent_noncar = 0.097)

```

필요 데이터 
- list_of_lidar : 1.1에서 다룸 
- list_of_gtbox : 1.1에서 다룸 
- car_index : 1.2에서 다룸 



### 1.1 gt_boxes3d.npy / gt_label.npy 생성 

#### A. object 생성 (`kitti_data/io.py`)

object = read_objects(tracklet)
- 입력 : tracklet = kitti raw데이터에서 배포하는 xml 타입의 데이터 
- 출력 : object
    - box = 3D박스의 8개 꼭지점 
    - type = tracklet.objectType
    - tracket_id 

> - trackletbox는 (0,0,0)기준으로 angle이 0도일때의 3d box이니 계산식은 회전변환으로 z축기준으로 회전시킨 후에 centet x,y,z를 더해서 실제 3d box의 8개의 꼭지점을 구하는 식으로 보면 될것 같습니다
> - 기본적으로 데이터 측정시 각 프레임의 기준 좌표계(원점 및 방향)는 velodyne sensor coordinate를 기준으로 하기 때문에 데이터 및 바운딩 박스를 해당 좌표계에서 바로 표시하면 너무 제각각입니다. 이는 여러 응용에 사용되는 benchmark로써 제한이 될 수 있기 때문에 bounding box들을 원점 좌표로 옮기고 이를 bird'eye view(z축 positive) 시점에서 데이터를 보는 방식으로 표현한 것입니다. 그래서 앞서 말씀하신것 처럼 z축 기준으로 rotMat을 이용해서 회전시키고, translation 시키면 각 프레임의 센서 좌표계로 다시 변환 할 수 있습니다.

```python     
tracklets = parseXML(tracklet_file)

h,w,l = tracklet.size

translation, rotation, state, occlusion, truncation, amtOcclusion, amtBorders, absoluteFrameNumber in tracklet.__iter__()
- translation = cornerPosInVelo생성시 사용 -> cornerPosInVelo는 box생성시 사용 
- rotation = yaw생성시 사용 -> yaw는 rowMat생성시 사용 
- 

trackletBox = np.array([ 
    [-l/2, -l/2,  l/2, l/2, -l/2, -l/2,  l/2, l/2], \
    [ w/2, -w/2, -w/2, w/2,  w/2, -w/2, -w/2, w/2], \
    [ 0.0,  0.0,  0.0, 0.0,    h,     h,   h,   h]])

 rotMat = np.array([\
              [np.cos(yaw), -np.sin(yaw), 0.0], \  # yaw = rotation[2] 
              [np.sin(yaw),  np.cos(yaw), 0.0], \
              [        0.0,          0.0, 1.0]])
 
```

#### B. gt_boxes3d, gt_labels  (`convert_kiti_to_numpy.py`)

gt_boxes3d, gt_labels = obj_to_gt_boxes3d(object)

- gt_boxes3d : object의 box정보 , np.zeros((num,8,3)
    - box = cornerPosInVelo.transpose() = np.dot(rotMat, trackletBox) + np.tile(translation, (8,1)).T

- gt_labels : 현재는 1로 고정 

```python

def obj_to_gt_boxes3d(objs):
    num        = len(objs)
    gt_boxes3d = np.zeros((num,8,3),dtype=np.float32)
    gt_labels  = np.zeros((num),    dtype=np.int32)

    for n in range(num):
        obj = objs[n]
        b   = obj.box
        label = 1 #<todo>

        gt_labels [n]=label
        gt_boxes3d[n]=b

    return  gt_boxes3d, gt_labels


```

### 1.2 car_index (`data_exploration.ipynb`)

frame별 차량의 수 

eg. `2.0: [108]` : 108번 프레임에는 차량이 2대 

```

{2.0: [108],
 3.0: [0,  1,  2,  3,  4,  6, ..., 112],
 4.0: [5, 113, 114, 115, 116],
 5.0: [117, 118, 119],
 6.0: [120, 122, 123],
 7.0: [121, 124, 125],
 9.0: [126, 127, 128, 129],
 10.0: [130, 131, ... , 153],
 11.0: [144, 145, 146, 147, 148, 149],
 12.0: [138, 139, 140, 141, 142, 143]}
```







---


# Generate tracklet file (`python-seoul-robotics/generate_tracklet-.ipynb`)

```python 

generate_tracklet(pred_model=model, input_folder='/workspace/_test_data/lidar_npy',
                  output_file='/workspace/_test_data/tracklet_sync_cs_model_no_merge_0531.xml', 
                  fixed_size=None, # [4.241800, 1.447800, 1.574800], # fixed box size: None or [l, w, h]
                  no_rotation=False, 
                  cluster=True, seg_thres=0.07, cluster_dist = 1.8, min_dist = 1., neigbor_thres = 7,
                  ver_fov=(-24.4, 15.), v_res=1., num_hor_seg=2,
                  merge=False)

#Frame 0: 6 boxes detected
#Frame 1: 6 boxes detected
#Frame 2: 6 boxes detected
#Exported tracklet to /workspace/_test_data/tracklet_sync_cs_model_no_merge_0531.xml


"""
count,18
item_version,1


objectType,Car
h,1.471019
w,2.456383
l,2.362085
first_frame,-19
count,1
item_version,2
tx,-3.301863
ty,-21.410707
tz,-0.070407
rx,0.000000
ry,0.000000
rz,-3.141593
state,1
occlusion,-1
occlusion_kf,-1
truncation,-1
amt_occlusion,0.0
amt_occlusion_kf,-1
amt_border_l,0.0
amt_border_r,0.0
amt_border_kf,-1
finished,1
"""
 
 
 
 ```
 
 






