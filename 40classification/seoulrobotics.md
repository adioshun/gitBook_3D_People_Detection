# Train 



```python

model.fit_generator(             # generator로 생성한 데이터로 학습시 (보통은 model.fit())
                   generator, # 훈련데이터셋을 제공할 제네레이터를 지정합니다. 
                   steps_per_epoch=steps_per_epoch, # 한 epoch에 사용한 스텝 수를 지정합니다. 
                                                   #총 45개의 훈련 샘플이 있고 배치사이즈가 3이므로 15 스텝으로 지정합니다.
                   epochs=5, # 전체 훈련 데이터셋에 대해 학습 반복 횟수를 지정합니다. 
                             #5번을 반복적으로 학습시켜 보겠습니다.
                   callbacks=[checkpointer, logger])
                   #validation_data : 검증데이터셋을 제공할 제네레이터를 지정합니다. 
                                       #본 예제에서는 앞서 생성한 validation_generator으로 지정합니다.
                   #validation_steps : 한 epoch 종료 시 마다 검증할 때 사용되는 검증 스텝 수를 지정합니다. 
                                       #총 15개의 검증 샘플이 있고 배치사이즈가 3이므로 5 스텝으로 지정합니다.

generator=train_batch_generator(list_of_lidar, list_of_gtbox, batch_size = batch_size, data_augmentation = True, width = 256, height = 64,
                    car_index = car_index, undersample = undersample, percent_noncar = percent_noncar)



```


# gt_boxes3d.npy / gt_label.npy 생성 

## 1. object 생성 (`kitti_data/io.py`)

object = read_objects(tracklet)
- 입력 : tracklet = kitti raw데이터에서 배포하는 xml 타입의 데이터 
- 출력 : object
    - box = cornerPosInVelo.transpose() = np.dot(rotMat, trackletBox) + np.tile(translation, (8,1)).T
    - type = tracklet.objectType
    - tracket_id 

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

## 2. gt_boxes3d, gt_labels = obj_to_gt_boxes3d(object)

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