# [A Survey of Clustering With Deep Learning: From the Perspective of Network Architecture](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=8412085)


```
Clustering is a fundamental problem in many data-driven application domains, and clustering
performance highly depends on the quality of data representation. 

Hence, linear or non-linear feature transformations have been extensively used to learn a better data representation for clustering. 

In recent years, a lot of works focused on using deep neural networks to learn a clustering-friendly representation, resulting in a significant increase of clustering performance. 

In this paper, we give a systematic survey of clustering with deep learning in views of architecture. 

Specifically, 
- we first introduce the preliminary knowledge for better understanding of this field. 
- Then, a taxonomy of clustering with deep learning is proposed and some representative methods are introduced. 
- Finally, we propose some interesting future opportunities of clustering with deep learning and give some conclusion remarks.
```


## I. INTRODUCTION

군집화 목적 : The goal of clustering is to categorize similar data into one cluster based on some similarity measures (e.g., Euclidean distance). 

기존 방식은 고차원 데이터에 대하여 좋지 않은 성능을 보임 `Although a large number of data clustering methods have been proposed [1]–[5], conventional clustering methods usually have poor performance on high-dimensional data, due to the inefficiency of similarity measures used in these methods. Furthermore, these methods generally suffer from high computational complexity on large-scale datasets. `

이러한 이유로 차원 축소나 Raw데이터를 다른 형태로 특징을 변환하는 방식이 많이 사용되었다. `For this reason, dimensionality reduction and feature transformation methods have been extensively studied to map the raw data into a new feature space, where the generated data are easier to be separated by existing classifiers.`

변환기의 예는 다음과 같다. `Generally speaking, existing data transformation methods include `
- **linear transformation** like Principal component analysis (PCA) [6] and 
- **non-linear transformation** such as kernel methods [7] and spectral methods [8].

그럼에도 불구 하고 고차원 데이터는 아직도 연구 대상이다. `Nevertheless, a highly complex latent structure of data is still challenging the effectiveness of existing clustering methods.`

딥러닝의 발전 덕분에 이런 문제는 많이 해결 될수 있다. `Owing to the development of deep learning [9], deep neural networks (DNNs) can be used to transform the data into more clustering-friendly representations due to its inherent property of highly non-linear transformation. `

이전 방식은 특징 변환과 군집화를 별개로 생각 하였다. 그래서 특징 변환을 하고 바로 군집 알고리즘에 적용 시키는 형식이었다. `Basically, previous work mainly focuses on feature transformation or clustering independently. Data are usually mapped into a feature space and then directly fed into a clustering algorithm. `

최근들어 DEC기법이 제안 되면서 딥클러스터링 연구가 시작 되었다. `In recent years, deep embedding clustering (DEC) [11] was proposed and followed by other novel methods [12]–[18], making deep clustering become a popular research field. `

```
[11] J. Xie, R. Girshick, and A. Farhadi, ‘‘Unsupervised deep embedding for clustering analysis,’’ in Proc. Int. Conf. Mach. Learn., 2016, pp. 478–487.
[12] F. Li, H. Qiao, B. Zhang, and X. Xi. (2017). ‘‘Discriminatively boosted image clustering with fully convolutional auto-encoders.’’ [Online]. Available: https://arxiv.org/abs/1703.07980
[13] B. Yang, X. Fu, N. D. Sidiropoulos, and M. Hong. (2016). ‘‘Towards K-means-friendly spaces: Simultaneous deep learning and clustering.’’ [Online]. Available: https://arxiv.org/abs/1610.04794
[14] K. G. Dizaji, A. Herandi, C. Deng, W. Cai, and H. Huang, ‘‘Deep clustering via joint convolutional autoencoder embedding and relative entropy minimization,’’ in Proc. IEEE Int. Conf. Comput. Vis. (ICCV), Oct. 2017, pp. 5747–5756.
[15] Z. Jiang, Y. Zheng, H. Tan, B. Tang, and H. Zhou. (2016). ‘‘Variational deep embedding: An unsupervised and generative approach to clustering.’’[Online]. Available: https://arxiv.org/abs/1611.05148
[16] J. Yang, D. Parikh, and D. Batra, ‘‘Joint unsupervised learning of deep representations and image clusters,’’ in Proc. IEEE Conf. Comput. Vis. Pattern Recognit., Jun. 2016, pp. 5147–5156.
[17] C.-C. Hsu and C.-W. Lin, ‘‘CNN-based joint clustering and representation learning with feature drift compensation for large-scale image data,’’ IEEE Trans. Multimedia, vol. 20, no. 2, pp. 421–429, Feb. 2018.
[18] Z. Wang, S. Chang, J. Zhou, M. Wang, and T. S. Huang, ‘‘Learning a taskspecific deep architecture for clustering,’’ in Proc. SIAM Int. Conf. Data Mining, 2016, pp. 369–377.
```

[19] 논문에서 딥클러스터링 기법을 overview로 다루었다. `Recently, an overview of deep clustering was proposed in [19] to review most remarkable algorithms in this field. Specifically, it presented some key elements of deep clustering and introduce related methods. `

```
[19] E. Aljalbout, V. Golkov, Y. Siddiqui, and D. Cremers. (2018). ‘‘Clustering with deep learning: Taxonomy and new methods.’’ [Online]. Available: https://arxiv.org/abs/1801.07648
```

하지만 본 논문에서는 오토인코더에 초점을 두어 overview하였다. `However, this paper mainly focuses on methods based on autoencoder [20], and it was incapable of generalizing many other important methods, e.g., clustering based on deep generative model. What is worse, some up-to-date progress is also missing. Therefore, it is meaningful to conduct a more systematic survey covering the advanced methods in deep clustering.`

기존 군집 알고리즘은 아래와 같이 분류 된다. `Classical clustering methods are usually categorized as `
- partition-based methods [21], 
- density-based methods [22], 
- hierarchical methods [23] 
- and so on. 

딥클러스터가 representation를 학습 하는것을 목적으로 하기에 Loss 기법으로 나눈건 불적합 하다. 대신, 네트워크 구조로 나누어야 한다. `However, since the essence of deep clustering is to learning a clustering-oriented representation, it is not suitable to classify methods according to the clustering loss, instead, we should focus on the network architecture used for clustering. `

> In this paper, we make a survey of deep clustering from the perspective of network architecture.

### 1.1 autoencoder (AE)

The first category uses the autoencoder (AE) to obtain a feasible feature space. 

An autoencoder network provides a non-linear mapping function through learning an encoder and a decoder, where the encoder is a mapping function to be trained, and the decoder is required to be capable to reconstruct the original data from those features generated by the encoder. 

### 1.2 Clustering DNN

The second category is based on feed-forward networks trained only by specific clustering loss, thus we refer to this type of DNN as Clustering DNN (CDNN). 

The network architecture of this category can be very deep and networks pre-trained on large-scale image datasets can further boost its clustering performance. 

### 1.3 GAN and VAE

The third and fourth categories are based on Generative Adversarial Network (GAN) [24] and Variational Autoencoder (VAE) [25] respectively, which are the most popular deep generative models in recent years.

They can not only perform clustering task, but also can generate new samples from the obtained clusters. 



본 논문에서는 구조에 따라 좀더 살펴 보겠다. To be more detailed, 
- we present a taxonomy of existing deep clustering methods based on the network architecture. 
- We introduce the representative deep clustering methods and 
- compare the advantages and disadvantages of different architectures and methods. 
- Finally, some directions are suggested for future development of this field

논문의 구조는 아래와 같다. `The rest of this paper is organized as follows:`
- Section II reviews some preliminaries of deep clustering. 
- Section III presents a taxonomy of existing deep clustering algorithms and introduces some representative methods. 
- Finally, Section IV provides some notable trends of deep clustering and gives conclusion remarks.


## II. PRELIMINARIES