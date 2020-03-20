# SVM

> [https://www.haidynmcleod.com/3d-robot-perception](https://www.haidynmcleod.com/3d-robot-perception), [3D Perception Project](https://hortovanyi.wordpress.com/2017/11/19/3d-perception-project/)

## Train

[capture\_features.py](https://github.com/chriswernst/Perception-Udacity-RoboticsND-Project3/blob/master/training/capture_features.py)

* 입력 : sample, normal
* 결과물 : [training\_set\_worlds123\_V2.sav](https://github.com/chriswernst/Perception-Udacity-RoboticsND-Project3/blob/master/training/training_set_worlds123_V2.sav)

[train\_svm.py](https://github.com/udacity/RoboND-Perception-Exercises/blob/master/Exercise-3/sensor_stick/scripts/train_svm.py) : [참고](https://github.com/jychstar/NanoDegreeProject/blob/master/RoboND/p3_perception/writeup_perception.md#machine-learning-train_svmpy), This function prints and plots the confusion matrix.

* 입력 : training\_set.sav
* 결과물 : [model\_worlds123\_V2.sav](https://github.com/chriswernst/Perception-Udacity-RoboticsND-Project3/blob/master/training/model_worlds123_V2.sav)

[feature.py](https://github.com/mkhuthir/RoboND-Perception-Project/blob/master/src/sensor_stick/src/sensor_stick/features.py) : compute\_normal\_histograms\(\)

[Training\_helper.py](https://github.com/udacity/RoboND-Perception-Exercises/blob/master/Exercise-3/sensor_stick/src/sensor_stick/training_helper.py)

[model.sav](https://github.com/Heych88/udacity-robond-Perception/blob/master/sensor_stick/scripts/model.sav): 3.23MB

## Test

> [https://gist.github.com/dexter800/d6f5b3c0adf99f338a3baad9a5dc3f11](https://gist.github.com/dexter800/d6f5b3c0adf99f338a3baad9a5dc3f11)

```python
'''
Classifies all the objects in a point cloud
:param: point_cloud, point cloud containing the filtered clusters
:param: white_cloud, points cloud containing only xyz data
:param: cluster_indices, locations of the clusters
:return: the classified objects positions and their labels
'''

def classifyClusters(point_cloud, white_cloud, cluster_indices):
        """
        point_cloud = cloud_filtered.extract(inliers, negative=True)
        white_cloud = XYZRGB_to_XYZ(point_cloud)
        cluster_indices = ec.Extract()
        """
    detected_objects_labels = []
    detected_objects = []

    # Classify each detected cluster
    for index, pts_list in enumerate(cluster_indices):

        # Grab the points for the cluster from the extracted outliers (cloud_objects)
        pcl_cluster = point_cloud.extract(pts_list)

        # convert the cluster from pcl to ROS using helper function
        ros_cluster = pcl_to_ros(pcl_cluster)

​        # Extract the cluster colour feature histogram
        chists = compute_color_histograms(ros_cluster, using_hsv=True, nbins=44)
        normals = get_normals(ros_cluster)

        # Extract the cluster surface normals histogram
        nhists = compute_normal_histograms(normals, nbins=20)
        feature = np.concatenate((chists, nhists))

​        # Make the prediction, retrieve the label for the result
        # and add it to detected_objects_labels list
        # clf is the trained SVM classifier
            """
            # Load model from disk
            model = pickle.load(open('model.sav', 'rb'))
            clf = model['classifier']
            encoder = LabelEncoder()
            encoder.classes_ = model['classes']
            scaler = model['scaler']
            """
        prediction = clf.predict(scaler.transform(feature.reshape(1, -1)))
        label = encoder.inverse_transform(prediction)[0]
        detected_objects_labels.append(label)

​        # store the label to be published into RViz
        label_pos = list(white_cloud[pts_list[0]])
        label_pos[2] += .4
        object_markers_pub.publish(make_label(label, label_pos, index))

​        # Add the detected object to the list of detected objects
        do = DetectedObject()
        do.label = label
        do.cloud = ros_cluster
        detected_objects.append(do)

    return [detected_objects, detected_objects_labels]
```

