This dataset provides part segmentation to a subset of ShapeNetCore models, containing ~16K models from 16 shape categories. The number of parts for each category varies from 2 to 6 and there are a total number of 50 parts.
The dataset is based on the following work:

@article{yi2016scalable,
  title={A scalable active framework for region annotation in 3d shape collections},
  author={Yi, Li and Kim, Vladimir G and Ceylan, Duygu and Shen, I and Yan, Mengyan and Su, Hao and Lu, ARCewu and Huang, Qixing and Sheffer, Alla and Guibas, Leonidas and others},
  journal={ACM Transactions on Graphics (TOG)},
  volume={35},
  number={6},
  pages={210},
  year={2016},
  publisher={ACM}
}

You could find the initial mesh files from the released version of ShapeNetCore.
An mapping from synsetoffset to category name could be found in "synsetoffset2category.txt"
```
The folder structure is as below:
	-synsetoffset
		-points : *.pts , x,y,z(??)
			-uniformly sampled points from ShapeNetCore models
		-point_labels : *.seg (1~3)
			-per-point segmentation labels
		-seg_img : *.png
			-a visualization of labeling
	-train_test_split
		-lists of training/test/validation shapes shuffled across all categories (from the official train/test split of ShapeNet)

    Airplane	02691156
    Bag	02773838
    Cap	02954340
    Car	02958343
    Chair	03001627
    Earphone	03261776
    Guitar	03467517
    Knife	03624134
    Lamp	03636649
    Laptop	03642806
    Motorbike	03790512
    Mug	03797390
    Pistol	03948459
    Rocket	04099429
    Skateboard	04225987
    Table	04379243
```


-  pts handling : [pts_loader](https://github.com/albanie/pts_loader)

- pts Visualization : [A library for visualization and creative-coding ](https://github.com/williamngan/pts)

- open3d지원한가고 하나 포인트수가 0