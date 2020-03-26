# tmp



> PCL/OpenNI tutorial 3: Cloud processing \(advanced\)\]

정합은 두개의 점군을 정렬 하는것이다. `Registration is the technique of aligning two point clouds, like pieces of a puzzle.`

좀더 정확히 말하면 두 점군 사이에서 correspondences을 찾아 내는것입니다. 즉, 두 점군은 동일한 부분을 캡쳐한 포인트가 있어야 합니다. `To be precise, the algorithm finds a set of correspondences between them, which would mean that there is an area of the scene that has been captured in both clouds.`

이후 linear transformation을 수행하여 rotation 와 translation정보를 포함하고 있는 변환 행렬을 계산 합니다. `A _linear transformation_ is then computed, which outputs a matrix that contains a rotation and a translation.`

These are the operations that you would apply to one of the clouds so it would get in place with respect to the other, with the intersecting areas overlapping.

비슷한 점군에서 좋은 결과를 얻을수 있습니다. 따라서 심하게 센서를 움직이지 마십시오. `The best results are achieved with clouds that are very similar, so you should try to minimize the transformations between them. Meaning, do not run like crazy with a Kinect in your hands, grabbing frames, and expect PCL to match the clouds. It is better to move the sensor gingerly and at steady intervals. If you use a robotic arm or a rotating tabletop with precise angles, even better.`

정합을 수행하면 full, complete , continous 한 점군을 얻게 된다. `Registration is a very useful technique because it lets you retrieve full, complete and continous models of a scene or object.`

단점은 계산 부하가 크다. `It is also expensive for the CPU. Optimizations have been developed that allow to do cloud matching in real time with the GPU`, like [KinFu](http://pointclouds.org/documentation/tutorials/using_kinfu_large_scale.php) does.

위 도표는 동작 과정을 설명한 것이다. `The previous diagram illustrates how the procedure is done.`

2가지 수행 방법이 있다. `There are mainly two different methods to choose from:`

### ICP registration

ICP stands for Iterative Closest Point.

It is an algorithm that will find the best transformation that minimizes the distance from the source point cloud to the target one.

단점은 모든 포인트에 대하여 brute force방식으로 진행 하기 때문에 시간이 많이 소요 된다. 다운 샘플링을 선행 하는것을 추천 한다. `The problem is that it will do it by associating every point of the source cloud to its "twin" in the target cloud in a linear way, so it can be considered a brute force method. It the clouds are too big, the algorithm will take its time to finish, so try downsampling them first.`

### Feature-based registration

키포인트와 기술자를 생성한후 비교 한다. `the algorithm finds a set of keypoints in each cloud, computes a local descriptor for each, and then performs a search to see if the clouds have keypoints in common.`

최소 3개의 대응점을 찾아 내면 변환행렬을 구할수 있다. `If at least 3 correspondences are found, a transformation can be computed.` 정확도를 높이기 위해서는 여러개의 대응값이 필요 하다. `For accurate results, several correspondences must be found.`

매칭이 키포인트값으로만 수행 되때문에 ICP보다는 속도가 빠르다. This method is \(or should be\) faster than the first, because matching is only done for the keypoints, not the whole cloud.

두점군사이에 차이가 크다면 ICP는 실패 한다. `ICP will probably fail if the difference between the clouds is too big.`

일반적으로 특징기반으로 러프한 정합을 실시 하고, ICP로 다시 정밀하게 정합을 수행 한다. `Usually, you will use features first to perform an initial rough alignment of the clouds, and then use ICP to refine it with precision.`

Feature-based registration is basically identical to the 3D object recognition problem, so we will not look into it here. Features, descriptors and recognition will be dealt with in the next tutorials.

### PCL Class manual

Combining several datasets into a global consistent model is usually performed using a technique called registration.

키 아이디어는 두 점군의 대응점을 찾고, 이 대응점의 거리가 최소가 되는 변환행렬을 구하는 것이다. `The key idea is to identify corresponding points between the data sets and find a transformation that minimizes the distance (alignment error) between corresponding points.`

이 작업은 반복적으로 수행 되는데 대응값을 찾는 것은 This process is repeated, since correspondence search is affected by the relative position and orientation of the data sets.

Once the alignment errors fall below a given threshold, the registration is said to be complete.

The **pcl\_registration** library implements a plethora of point cloud registration algorithms for both organized an unorganized \(general purpose\) datasets.

## 적은 오버랩에서 사용 가능한 3차원 점군 정합 방법

> [https://github.com/adioshun/gitPaper\_SLAM/blob/master/registration/2018-a-modified-method-for-registration-of-3d-point-clouds-with-a-low-overlap-ratio.md](https://github.com/adioshun/gitPaper_SLAM/blob/master/registration/2018-a-modified-method-for-registration-of-3d-point-clouds-with-a-low-overlap-ratio.md)\)

![](https://camo.githubusercontent.com/c2677417e5eb0a40882ab12b7e01dbc89b123477/687474703a2f2f6a6f75726e616c2e63672d6b6f7265612e6f72672f6a6f75726e616c2f6a6b6367732f6a6b6367732d32342d352f6769662f6a6b6367732d32342d352d31312d67322e676966)

![](http://robotica.unileon.es/images/thumb/7/74/Pairwise_registration.jpg/650px-Pairwise_registration.jpg)

![](https://camo.githubusercontent.com/5424b024ea3290017426cc51bcc2052aabbb784d/687474703a2f2f706f696e74636c6f7564732e6f72672f646f63756d656e746174696f6e2f7475746f7269616c732f5f696d616765732f626c6f636b5f6469616772616d5f73696e676c655f697465726174696f6e2e6a7067)

> [출처](https://github.com/adioshun/gitPaper_SLAM/blob/master/registration/2018-a-modified-method-for-registration-of-3d-point-clouds-with-a-low-overlap-ratio.md)

