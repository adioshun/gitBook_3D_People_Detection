# 03-Octree-kdTree



## 탐색 모듈을 이용한

2D 이미지 처리는 픽셀 단위 영상처리와 마스크 기반 처리로 나눌수 있습니다. 전자는 픽섹 하나의 값을 변경하여 영상반전이나 이진영상등을 얻을때 사용하며, 후자는 주변의 픽셀의 정보를 활용 하여 영상을 선명\(샤프닝\)하게 하거나 꼭지점이나 선등의 특징을 찾을수 있습니다. 3D 데이터 처리에서도 주변 포인의들의 정보 활용하여 많은 연산을 수행 합니다. 하지만 2D와 3D 데이터의 주변 픽셀 또는 포인트들을 찾는 방법에서 큰 차이가 있습니다. 그림 \[2x-a\]는 이미지와 이를 테이터화 표현한것입니다. 특정한 픽셀\(x,y\)을 기준으로 주변의 픽셀을 구하려면  상하좌우\(x-1, y\), \(x-1, y\),\(x-1, y\),\(x-1, y\)의 순서 정보를 보고 픽셀을 구할수 있습니다. 예를 들어 \(5,5\) 주변의 픽셀은 \(5,5\)\(5,5\)\(5,5\)\(5,5\)입니다. 그림 \[2x-b\]는 포인트 클라으드와 이를 데이터로 표현한것입니다. 특정한 점\(x,y,z\)을 기준으로 순서만으로는 주변의 포인트를 구할수 없습니다. 예를 들어 L14에 있는 점의 위아래 L13과 L15가 반듯이 L15 주변에 있다고 확정 할수 없습니다. 

![](../../.gitbook/assets/image%20%285%29.png)

가장 간단한 방법은 list에 저장된 모든 위치 정보를  거리계산 알고리즘을 이용하여 탐색해서 찾을 수도 있습니다. 하지만 매 순간 매 포인트에 대하여 검색 할경우 시간이 많이 걸립니다. 따라서 공간을 분활하여 구조화하여 탐색을 하면 좋습니다. Spatial partitioning 효율적으로 수행하기 위해 space-partitioning data structure를 생성하면 됩니다. spatial data structure는 위치 정보를 저장하고 있는 데이터 구조체 입니다. 이 형태로 데이터를 저장하게 되면 효율적으로 레인지 서치와 근접 이웃 탐색이 가능 합니다. 주변점은 이전에 살펴 보았던 filters외에도 향후, surface, features, registration등도 탐색 기능에 바탕을 두고 있기에 중요합니다. PCL에서는 다음과 같은 탐색 방법을 제공하고 있습니다.

* BruteForce :simple brute force search algorithm 
* OrganizedNeighbor : earest neigbhor search in organized point clouds \[코드,
* KdTree
* Octree
* FlannSearch

![](https://i.imgur.com/LggWFmz.png)

가장 간단한 spatial partitions 방법은 앞에서 배운 그리드\(2D\)/복셀\(3D\)을 이용하는 것입니다. 격자를 통한 접근법은 골고루 분산되어 있는 점에 대해서 구현하기 쉬운 해법이지만, 적절한 크기를 선정하는 방법과 빈공간으로 인한 메모리 낭비와 계산 효율성이 떨어 집니다. 공간을 조금 더 적절히 나누는 방법은 공간분할 트리\(Space-partitioning trees\)를 사용하는 것입니다. 대표적인 방법은 아래와 같습니다.

* k-d trees
* Octrees\(3D\)

### k-d trees

BST\(Binary Search Tree\)를 다차원 공간으로 확장한 것, 각 노드의 데이터가 공간의 K 차원 포인트인 이진 검색 트리입니다. 기본 구조와 알고리즘은 BST와 유사하지만 트리의 레벨 차원을 번갈아 가며 비교한다는 점이 다릅니다. 아래 그림은 BST와 2D, 3D kd-tree 를 표현한 것입니다.

![](https://i.imgur.com/ImyyICy.png)

기본적인 원리는 같습니다. BST의 경우 값이 크고/작음에 따라서 나뉘고, 2d-tree는 좌우/상하에 따라서 나눕니다. 3d-tree는 좌우/상하/앞뒤\(?\)에 따라서 나눕니다. 자세한 설명을 위해서 2d-tree를 기준으로 설명 드리겠습니다.

* 먼저 전체 공간의 중앙값\(1\)을 root  노드 선택 합니다. \(보통 중앙값을 선택 합니다. \) 
* x의 값이 중앙값\(1\)보다 작은 점\(6,3,5,4\)들은 모두 왼쪽에 둡니다. x의 값이 중앙값\(1\)보다 큰 점\(8,9,2,7,10\)은 오른쪽에 둡니다. 
* 왼쪽점중에서 중앙값\(3\)을 선택 합니다. 오른쪽점중에서 중앙값 2를 선택 합니다. 
* y의 값이 왼쪽 중앙값\(3\)보다 작은점\(5,4\)을 왼쪽에 둡니다.  y의 값이 왼쪽 중앙값\(3\)보다 큰점\(6\)을 오른쪽에 둡니다. 
* y의 값이 오른쪽 중앙값\(2\)보다 작고/큼에 따라서 위와 같이 구분 합니다. 
* 왼쪽 상단의 중앙값\(6\)을 선택 합니다. 왼쪽 하단의 중앙값\(4\)을 선택 합니다. 
* 각 공간에 하나의 포인만 위치 할때까지 반복 합니다. `The partition is encoded by one of the points in the data set, and is selected by the median of the points in that subpartition.`

k-d tree에서 노드를 검색하는 방법은 주로 두 가지 방법을 사용합니다. 첫번째가 **range search** 방법으로 찾고자 하는 키 값의 범위를 정하고 이 범위에 포함되는 노드를 찾게 됩니다. 이 탐색 방법도 binary search tree에서 탐색 방법과 유사합니다. 두번째는 **nearest neighbour search** 방법으로 주어진 키 값에 가장 근접한 노드를 찾는 것입니다.

> kdtree\_flann : FLANN \(Fast Library for Approximate Nearest Neighbors\) is a library for performing fast approximate nearest neighbor searches, Marius Muja, David G. Lowe. Fast Approximate Nearest Neighbors with Automatic Algorithm Configuration, 2009

### Octree

옥트리는 트리 구조를 이용하여 공간/볼륨을 표현하는 방법입니다`Use a tree structure to represent an area/volume of space`. 앞서 살펴본 이진트리란 자식노드가 최대 두 개인 노드들로 구성된 트리라면 옥트리는 최대 8개의 노드들로 구성된 트리로 3D 구조를 표현 할때 사용됩니다. 참고로 QuadTree는 4개의 노드들로 구성된 트리로 2D 구조를 표현할때 사용됩니다. kd-tree에서 각 포인트 정보를 기준으로 트리를 생성하였다면 옥트리는 공간을 기준으로 트리를 생성해 나갑니다. 따라서 트리 크기를 사용자가 정할수 있습니다.

| ![](https://www.researchgate.net/profile/Arjan_Egges/publication/221588530/figure/fig11/AS:393966462750731@1470940332380/On-the-left-we-see-part-of-a-quadtree-a-two-dimensional-octree-with-several.png) | ![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Octree2.svg/1200px-Octree2.svg.png) |
| :--- | :--- |
|  |  |

At every level, it becomes subdivided by a factor of 2 which results in an increased voxel resolution

생성 과정은 아래와 같습니다.

* \(1\). 최대 깊이를 지정 합니다. `Set the maximum recursion depth.`
* \(2\). 공간의 최대 크기를 구하고 이를 첫번째 큐드로 지정 합니다. `Find the maximum size of the scene and build the first cube with this size.` ?? 전체 공간을 루트 노드로 지정 아닌가? 
* \(3\). Throw the unit element elements into cubes that can be included and have no children. 
* \(4\). If the maximum recursion depth is not reached, subdivide the octaves and then carry all the unit elements of the cube to the eight sub-cubes. 
* \(5\). If the number of unit elements assigned to the subcube is not zero and is the same as the parent cube, then the subcube stops subdividing because of the allocation of the subdivided space according to the spatial partition theory. There must be fewer, if it is the same number, then how to cut the number is still the same, it will cause infinite cutting. 
* \(6\). Repeat 3 until the maximum recursion depth is reached.

kdtree와 달리 옥트리의 리프노드는 목적에 따라 여러 가지가 사용 가능 합니다.

* OctreePointCloudPointVector \(equal to OctreePointCloud\): This octree can hold a list of point indices at each leaf node.
* OctreePointCloudSinglePoint: This octree class hold only a single point indices at each leaf node. Only the most recent point index that is assigned to the leaf node is stored.
* OctreePointCloudOccupancy: This octree does not store any point information at its leaf nodes. It can be used for spatial occupancy checks.
* OctreePointCloudDensity: This octree counts the amount of points within each leaf node voxel. It allows for spatial density queries.
* 
옥트리는 노드 검색 외에도 다양한 부가 연산이 가능 합니다.

* Search operations \(neighbor search, radius search, voxel search\) 
  * `octree.voxelSearch`  : 사용자 정의 기준점이 속해 있는 Voxel내 모든 점군을 검색 합니다.
  * \`octree.nearestKSearch: 사용자 정의 기준점에서 가까운 N\(사용자 정의\)개의 점들을 검색 합니다.
  * `octree.radiusSearch`  : 사용자 정의 기준 기준점에서 반경\(사용자 정의\)내 점들을 검색 합니다.
* Downsampling \(Voxel-grid / Voxel-centroid filter\) 
* Point cloud compression 
* Spatial change detection : based on XOR comparison of octree structure
* Spatial point density analysis
* Occupancy checks/maps 
* Collision detection

kd-tree의 KNN처러 옥트리의 성능 향상을 위해서는 octree double buffering implementation \( Octree2BufBase class \).를 활용 가능 합니다. 빠른 속도를 원한다면, If octrees needs to be created at high rate, please have a look at the octree double buffering implementation \( Octree2BufBase class \).

* This class keeps two parallel octree structures in the memory at the same time.
* 배경탐지에도 활용된다. In addition to search operations, this also enables spatial change detection.
* Furthermore, an advanced memory management reduces memory allocation and deallocation operations during the octree building process.
* The double buffering octree implementation can be assigned to all OctreePointCloud classes via the template argument “OctreeT”.

All octrees support serialization and deserialization of the octree structure and the octree data content.

### 비교

Octree : split along pre-defined planes, 노드는 복셀, 루트 노드는 전체 공간  
Kd-Tree : split along planes parallel to coordinate axes, so as to split up the objects nicely, 노드는 점

### 옥트리의 octree structure 비교를 통한 배경 제거 방법

본 챕터에서에서는 Octee 구조체를 이용한 배경제거에 대하여 알아 보겠습니다. 배경은 벽, 기둥, 바닥, 나무, 전봇대, 책상 등 고정된 물체라고 보면 됩니다. 1장에서 살펴본 바닥제거는 배경제거의 한 부분입니다. 바닥제거에서는 일반적으로 평면으로 가정하고 RANSAC의 모델을 이용하여 제거 하거나, Z좌표에서 최하단 부분을 관심영역 필터링으로 제거 할 수 있습니다. 하지만, 나무나 책상 등은 이러한 기법을 이용하여 제거 하기 어렵습니다.

가장 이상적인 배경제거는 배경에 해당하는 물체를 모델링 하거나 인식기술을 사용하는 것입니다. 하지만 여기에서는 Octree를 학습을 목표로 하기 때문에 약간의 편법을 통해 배경을 제거해 보겠습니다.

기본적인 아이디어는 간단 합니다. 탐지 하고자 하는 물체가 없는 상태 즉, 빈방에 있는 모든것을 **배경 점군**으로 정의 합니다. 그리고 탐지 하려고 하는 물체가 추가 된다면 여기에서 **배경 점군**을 찾아내어 제거 하면 됩니다.

* [PCL::Search](http://www.pointclouds.org/assets/rss2011/06_search.pdf): ppt, PCL Search
* [Spatial data structures: Octrees, BSP, and k-d trees](https://observablehq.com/@2talltim/spatial-data-structures-octrees-bsp-and-k-d-trees)
* [Geometric Application of BSTs](https://rottk.tistory.com/entry/Geometric-Application-of-BSTs): 한글
* Geometric Search : [kd trees](https://algs4.tistory.com/68), [Range Search](https://algs4.tistory.com/67) : 한글
* \[라온피플\] 포인트 클라우드 누가 누가 빠른가 : [KD Tree](https://m.blog.naver.com/laonple/221207919855), [Octree](https://m.blog.naver.com/laonple/221361446531)
* [BTS단점](https://stackoverflow.com/questions/99796/when-to-use-binary-space-partitioning-quadtree-octree)
* [KD-tree representation](https://ko.coursera.org/lecture/ml-clustering-and-retrieval/kd-tree-representation-S0gfp): 코세라
* [Using k-d trees to efficiently calculate nearest neighbors in 3D vector space](https://blog.krum.io/k-d-trees/)

