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

Basically, previous work mainly focuses on feature transformation
or clustering independently. Data are usually
mapped into a feature space and then directly fed into a
clustering algorithm. In recent years, deep embedding clustering
(DEC) [11] was proposed and followed by other novel
methods [12]–[18], making deep clustering become a popular
research field. Recently, an overview of deep clustering was
proposed in [19] to review most remarkable algorithms in this
field. Specifically, it presented some key elements of deep
clustering and introduce related methods. However, this paper
mainly focuses on methods based on autoencoder [20], and it
was incapable of generalizing many other important methods,
e.g., clustering based on deep generative model. What is
worse, some up-to-date progress is also missing. Therefore,
it is meaningful to conduct a more systematic survey covering
the advanced methods in deep clustering.