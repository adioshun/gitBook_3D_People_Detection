
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

## 2. 